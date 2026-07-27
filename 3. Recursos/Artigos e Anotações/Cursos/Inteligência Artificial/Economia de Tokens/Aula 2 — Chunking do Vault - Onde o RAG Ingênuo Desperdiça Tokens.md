---
tags:
  - IA
  - programação
  - inovação
---
# Aula 2 (expandida) — Chunking do vault: mecânica completa
---

### 1. Chunking de tamanho fixo: o dano é estrutural, não cosmético

O problema não é só "corta no meio de uma ideia" — é que isso corrompe o **embedding** do chunk.

Pensa assim: um embedding de um chunk é (grosseiramente) uma média ponderada semântica de todos os tokens daquele texto. Se um chunk de 512 tokens contém o final do conceito "Strategy Pattern" e o início de um conceito completamente não relacionado (ex: "Roteamento de agentes"), o vetor resultante é uma mistura dos dois — não representa bem nenhum dos dois. Isso é **contaminação semântica de fronteira** (boundary contamination).

Consequência prática na busca: quando você faz uma query sobre "Strategy Pattern", esse chunk contaminado tem similaridade de cosseno _mais baixa_ com sua query do que teria um chunk limpo só sobre o assunto — porque parte do vetor está "puxado" na direção do assunto vizinho. Resultado: **recall pior** (você perde chunks relevantes no top-k) sem nem perceber, porque o sistema não erra de forma óbvia — ele só retorna resultados um pouco piores silenciosamente.

**Exemplo concreto no seu vault**: se uma nota `conceito` sobre RAG tem, no mesmo arquivo, uma seção sobre "chunking" seguida de uma seção sobre "custo de embeddings", um corte de tamanho fixo pode gerar um chunk que é 60% chunking + 40% custo. Uma query específica sobre "overlap token waste" vai competir contra esse vetor híbrido e pode perder pra um chunk pior mas mais "puro" de outro arquivo.

---

### 2. Chunking semântico: como o algoritmo realmente funciona

Não é mágica — é um pipeline de 4 passos:

```python
import tiktoken
from sentence_transformers import SentenceTransformer
import numpy as np

def semantic_chunk(text: str, model: SentenceTransformer, 
                     percentile_threshold: float = 95) -> list[str]:
    # 1. Divide em unidades atômicas (sentenças, não tokens)
    sentences = split_into_sentences(text)  # regex ou spaCy
    
    # 2. Embedda cada sentença individualmente
    embeddings = model.encode(sentences)  # shape: (n_sentences, dim)
    
    # 3. Calcula similaridade de cosseno entre sentenças ADJACENTES
    distances = []
    for i in range(len(embeddings) - 1):
        sim = np.dot(embeddings[i], embeddings[i+1]) / (
            np.linalg.norm(embeddings[i]) * np.linalg.norm(embeddings[i+1])
        )
        distances.append(1 - sim)  # distância = 1 - similaridade
    
    # 4. Detecta breakpoints: onde a distância é um outlier (alta)
    threshold = np.percentile(distances, percentile_threshold)
    breakpoints = [i for i, d in enumerate(distances) if d > threshold]
    
    # Reconstrói chunks nos breakpoints detectados
    chunks = []
    start = 0
    for bp in breakpoints:
        chunks.append(" ".join(sentences[start:bp+1]))
        start = bp + 1
    chunks.append(" ".join(sentences[start:]))
    
    return chunks
```

O ponto-chave é o **passo 4**: você não define um threshold fixo arbitrário — usa o percentil da distribuição de distâncias _daquele texto específico_. Isso adapta o corte ao estilo de escrita de cada nota (uma nota mais "densa" com muitas transições de assunto vai gerar mais chunks pequenos; uma nota mais linear gera poucos chunks grandes).

**Variante mais robusta**: em vez de percentil fixo (95%), usar desvio padrão (breakpoint = média + 1.5×desvio_padrão) — mais resistente a outliers em textos muito curtos, onde percentil de poucas amostras é instável. Pro seu vault com 826 arquivos de tamanhos variados, desvio padrão tende a generalizar melhor.

**Custo desse pipeline**: é 100% offline, rodando localmente com `sentence-transformers` (grátis, sem API). Você paga isso uma vez na indexação, não a cada query — é exatamente o tipo de custo fixo que vale a pena pagar adiantado pra economizar no recorrente.

---

### 3. Overlap: quantificando o desperdício

Vamos formalizar. Se você tem chunk size `C` e overlap `O`:

```
número_de_chunks ≈ (N - O) / (C - O)
tokens_totais_armazenados = número_de_chunks × C
tokens_redundantes = tokens_totais_armazenados - N
```

Com `C = 512` e `O = 50` (valores comuns), a fração de redundância é aproximadamente `O / C ≈ 9.8%` — quase 10% do seu índice vetorial é literalmente texto duplicado. Isso não é custo de API (embedding é local), mas _é_ custo de input se dois chunks adjacentes com overlap forem ambos recuperados no mesmo top-k — você paga por aquele trecho duplicado **duas vezes** no prompt final.

Com chunking semântico e overlap zero (fronteiras já são "limpas" por natureza), você elimina essa redundância inteiramente. A troca é: mais chunks pequenos e heterogêneos em tamanho, mas cada um semanticamente íntegro.

---

### 4. Positional encoding: o que realmente acontece na concatenação

Isso merece mais rigor do que dei antes. Modelos como o Claude usam variantes de **RoPE (Rotary Position Embeddings)** ou mecanismos relativos similares — a atenção entre dois tokens é modulada pela _distância relativa_ entre suas posições na sequência, não pela posição absoluta.

Quando você concatena chunks recuperados do seu vault num único prompt, o modelo atribui posições sequenciais 0, 1, 2, ..., n **à sequência concatenada**, não às posições originais dentro do documento-fonte. Isso tem uma implicação direta:

- O último token do Chunk A e o primeiro token do Chunk B ficam em posições _adjacentes_ (distância relativa = 1)
- RoPE (e mecanismos como ALiBi, que penalizam distância) fazem o modelo **atribuir peso de atenção artificialmente alto** entre esses dois tokens, como se fossem contíguos no texto original — mas semanticamente eles não têm relação nenhuma

Isso é o motivo técnico exato (não só "boa prática") pelo qual o separador explícito importa: ele não é só um lembrete visual pro modelo, ele **quebra a proximidade posicional** que gera esse artefato de atenção espúria, e o texto do separador ("Fonte: ...") dá ao modelo um sinal textual explícito de fronteira de documento que compensa a ausência de um sinal posicional nativo.

**Bônus — "Lost in the Middle" (Liu et al.)**: modelos de linguagem tendem a atender melhor a informação no início e no fim do contexto, e pior no meio. Implicação prática pro seu RAG: **ordene os chunks recuperados por relevância, colocando o mais relevante primeiro ou por último no prompt** — não deixe o chunk mais importante enterrado no meio de uma lista de 5-10 chunks.

---

### 5. Retrieval em duas camadas: pre-filtering vs. post-filtering

Isso também merece mais precisão. Bancos vetoriais modernos (Chroma, Weaviate, Qdrant) oferecem duas estratégias de filtro por metadata, com trade-offs diferentes:

**Post-filtering** (ingênuo): roda a busca ANN (Approximate Nearest Neighbor, geralmente via HNSW — Hierarchical Navigable Small World graph) sobre o índice inteiro, pega os top-k, e _depois_ descarta os que não batem com o filtro de metadata. Problema: se o filtro for restritivo (ex: só notas do domínio "IA"), você pode acabar com menos de k resultados úteis, porque o ANN já gastou o "orçamento" de busca em candidatos que serão descartados.

**Pre-filtering** (o que você quer): restringe o espaço de busca _antes_ de rodar o ANN — o grafo HNSW é percorrido já considerando só os nós que satisfazem o filtro de metadata. Isso é mais eficiente (menos comparações de vetor) e garante que você realmente recupera k resultados relevantes, não k-menos-descartados.

Pro seu vault: se seu schema de metadata (`dominio`/`subdominio`/`tier`) estiver bem populado (o que você já projetou no `guia_mestre_metadados.md`), pre-filtering por `dominio = "IA"` antes da busca vetorial elimina candidatos de Business/Marketing/Branding logo de cara — reduzindo drasticamente o espaço de comparação vetorial.

---

### 6. Retrieval hierárquico (small-to-big) — isso conecta direto com seu schema de vault

Essa é uma técnica que seu Second Brain já está estruturalmente pronto para usar, dado o desenho MOC → HUB → nota que você fez:

**Ideia**: indexe chunks _pequenos_ (parágrafo ou seção) para busca de alta precisão, mas quando um chunk pequeno é recuperado, **expanda o contexto injetado no prompt para incluir o "pai" dele** — a seção inteira, ou até a nota completa, ou o HUB que a referencia.

Por quê isso funciona: chunks pequenos têm embeddings mais "puros" (menos contaminação semântica, como vimos na seção 1) — então a busca é mais precisa. Mas chunks pequenos sozinhos podem faltar contexto pro modelo gerar uma resposta completa. A solução é desacoplar **granularidade de indexação** de **granularidade de injeção**: busca fina, contexto grosso.

Isso é literalmente o que sua hierarquia `dominio` → `subdominio` → `sub_subdominio` já viabiliza: o chunk pequeno carrega metadata apontando pro pai, e na hora de montar o prompt você sobe um nível na hierarquia antes de injetar.

---

### Tabela atualizada

|Decisão técnica|Mecanismo|Eixo afetado|
|---|---|---|
|Chunking semântico vs. fixo|Evita contaminação de embedding na fronteira|Input (recall melhor) + Output (menos reconstrução)|
|Overlap zero com chunking semântico|Elimina redundância `O/C` no índice|Input (menos tokens duplicados no prompt)|
|Separador explícito entre chunks|Quebra artefato de proximidade posicional (RoPE)|Output (evita mistura de fontes)|
|Ordenação por relevância (não por posição arbitrária)|Mitiga "lost in the middle"|Output (melhor uso do contexto relevante)|
|Pre-filtering por metadata antes do ANN|Restringe espaço de busca no grafo HNSW|Custo computacional local (não API), mas evita chunk irrelevante entrar no prompt|
|Retrieval hierárquico (small-to-big)|Desacopla granularidade de busca vs. injeção|Input (contexto certo, nem fragmentado nem excessivo)|

---

**Checagem**: dado o mecanismo de RoPE que expliquei na seção 4 — se você tivesse que decidir a _ordem_ dos separadores de metadata (colocar "Fonte: X" _antes_ ou _depois_ do conteúdo do chunk), qual você escolheria e por quê, pensando em como o modelo processa a sequência da esquerda pra direita?