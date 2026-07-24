---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# O problema não é "chunkar", é chunkar errado

Chunking ruim quebra a unidade semântica do texto, e isso te penaliza nos dois eixos do que vimos na Aula 1: infla o input (recupera lixo/redundância) e infla o output (o modelo precisa reconstruir contexto que foi fragmentado).

## Estratégia 1: chunking por tamanho fixo (o que a maioria faz, e é ruim)

```
chunk_size = 512 tokens, overlap = 50 tokens
```

Problema: corta no meio de uma ideia. Se sua nota do Obsidian tem um parágrafo sobre "arquitetura Strategy Pattern" que atravessa o limite de 512 tokens, metade do conceito fica num chunk, metade no outro. Na hora do retrieval, você recupera só a metade — e o modelo "acha" que entendeu, mas está trabalhando com informação incompleta. Isso gera **output extra** (ele tenta preencher a lacuna com raciocínio, às vezes alucinando).

## Estratégia 2: chunking semântico (o que você quer)

Corta por unidade de sentido, não por contagem de tokens:

- **Por heading/seção** (se seu vault usa Markdown com `##`, cada seção é um chunk natural)
- **Por parágrafo semanticamente coeso** — se usar uma lib como `langchain`, o `RecursiveCharacterTextSplitter` tenta isso, mas o ideal é usar embeddings pra detectar quebras de tópico (semantic chunking de verdade: calcula similaridade de coseno entre sentenças adjacentes, corta onde a similaridade cai abaixo de um threshold)

Isso custa mais processamento no momento da indexação (é uma passada extra de embedding), mas você faz isso **uma vez só**, offline — não a cada query. É investimento, não custo recorrente.

# Por que overlap importa (e por que overlap demais é desperdício)

Overlap existe pra evitar que uma ideia fique cortada exatamente na fronteira de dois chunks. Mas cada token de overlap é **input pago em dobro** — ele aparece em dois chunks, então se ambos forem recuperados, você paga por ele duas vezes.

Regra prática: com chunking semântico bem feito (corte nas fronteiras naturais de ideia), você precisa de bem menos overlap — às vezes zero — porque as fronteiras já são "limpas" por natureza, ao contrário do corte por tamanho fixo que é arbitrário.

# O papel do positional encoding aqui 

Um detalhe que costuma passar batido: quando você concatena vários chunks recuperados no prompt final, o modelo não sabe que eles vieram de lugares diferentes do seu vault — ele só vê uma sequência contígua de tokens com positional encoding sequencial normal. Isso significa:

- Chunks meramente concatenados sem separador claro podem ser lidos pelo modelo como se fossem continuação um do outro (o self-[attention]() não tem "fronteira de documento" nativa, a menos que você marque isso explicitamente)
- **Prática recomendada**: sempre insira um separador explícito e metadata textual entre chunks recuperados, tipo:

```
[Fonte: Vault/Prometheus/arquitetura.md — seção "Strategy Pattern"]
<conteúdo do chunk>

[Fonte: Vault/Prometheus/agentes.md — seção "Roteamento"]
<conteúdo do chunk>
```

Isso custa alguns tokens extras de input, mas evita que o modelo confunda proveniência das informações — o que geraria **output ruim** (conclusões erradas por misturar contexto de fontes diferentes), te custando na reformulação depois.

# Retrieval em duas camadas (pra não gastar embedding à toa)

1. **Filtro por metadata primeiro** (tags, pasta, data de modificação) — reduz o espaço de busca _antes_ de gastar com similaridade vetorial
2. **Busca semântica só no subconjunto filtrado** — top-k (geralmente k=3 a 5) por similaridade de coseno no seu banco vetorial

Isso importa porque busca vetorial em vault grande sem filtro prévio é computacionalmente redundante — você está comparando contra vetores que nunca seriam relevantes de qualquer forma (ex: notas de outro projeto seu que não é o Prometheus).

# Resumo prático pro Prometheus OS

|Decisão|Efeito no custo|
|---|---|
|Chunking semântico vs. tamanho fixo|Reduz output desperdiçado (menos reconstrução de contexto fragmentado)|
|Overlap mínimo com chunking semântico|Reduz input duplicado|
|Filtro por metadata antes do vetorial|Reduz custo de indexação/busca (não é token de API, mas é custo computacional local)|
|Separador + metadata explícita entre chunks|Custo pequeno de input, evita erro caro de output|
