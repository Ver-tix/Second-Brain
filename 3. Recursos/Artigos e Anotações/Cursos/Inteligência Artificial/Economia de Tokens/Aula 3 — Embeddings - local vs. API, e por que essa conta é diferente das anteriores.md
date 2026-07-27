---
tags:
  - IA
  - programação
  - inovação
tipo:
  - MOC
dominio:
  - IA
Subdominio:
  - token-economy
---
# ==Visão Técnica==

Antes de comparar preços, preciso corrigir uma expectativa: a intuição que você desenvolveu nas Aulas 1 e 2 (input barato/paralelo, output caro/sequencial) **não se aplica diretamente aqui**. Embedding não é geração de texto — é outro tipo de forward pass.

### 1. Por que o modelo de custo é estruturalmente diferente

Um modelo de embedding (BGE, Voyage, OpenAI text-embedding-3) tipicamente usa uma arquitetura **encoder-only** com atenção bidirecional (cada token atende a todos os outros, nos dois sentidos — sem causal masking), ao contrário do decoder autorregressivo que gera texto.

Implicações diretas:

- **Uma única passada forward**, sem geração token-a-token — o output é um vetor de dimensão fixa (384, 1024, 1536, 3072 dims, dependendo do modelo), não uma sequência de tokens gerados
- **Não existe eixo de output** no sentido que vimos nas Aulas 1-2 — você paga só pelos tokens de _input_ processados, porque não há decode autorregressivo
- Resultado: não existe a assimetria 5x que você já internalizou pra Sonnet/Opus. O preço de embedding é uma única tarifa por token processado, ponto.

Isso significa que a pergunta "gasto mais na entrada ou na saída?" simplesmente não se aplica a embeddings — a única variável de custo é volume de tokens indexados.

### 2. Rodando localmente

Modelos como `sentence-transformers` (`all-MiniLM-L6-v2`, 384 dims) ou `BGE-M3` (1024 dims) rodam no seu próprio hardware, sem chamada de API.

Em throughput típico numa única A100, um modelo self-hosted como o NV-Embed-v2 roda a cerca de $0,001 por milhão de tokens — 20x mais barato que qualquer API comercial. Mas isso vem com uma pegadinha de custo fixo: um mês inteiro de GPU dedicada custa cerca de $1.080 em taxa spot, mesmo com utilização zero. Ou seja, "grátis" só é verdade se você já tem a GPU alocada pra outra coisa — senão, o breakeven depende do seu volume real de uso.

**Pro seu caso específico**: com 826 arquivos, você nem precisa de GPU. `BGE-M3` ou `all-MiniLM-L6-v2` rodam em CPU comum, mais lento (minutos em vez de segundos), mas com custo monetário zero. Não há motivo pra alugar GPU pra indexar um vault desse tamanho.

### 3. Via API — Voyage AI

Vale um adendo importante: a Anthropic não tem modelo de embedding próprio. A recomendação oficial da própria documentação da Anthropic é usar a **Voyage AI** como provedor — é o parceiro preferencial deles pra essa camada específica do pipeline, exatamente por isso que aparece nos cookbooks e guias de RAG da própria Anthropic.

Preços atuais da linha voyage-4: $0,02 por milhão de tokens no voyage-4-lite, $0,06/M no voyage-4, e $0,12/M no voyage-4-large. Todo mundo ganha 200 milhões de tokens grátis na geração voyage-4, e o Batch API dá 33% de desconto adicional em qualquer tier.

Tem um truque de arquitetura interessante aqui: todos os modelos voyage-4 compartilham o mesmo espaço vetorial, então dá pra indexar com voyage-4-large (mais preciso) e consultar com voyage-4-lite (mais barato) sem precisar reindexar nada. Isso é o padrão **embeddings assimétricos**: você paga o preço mais alto uma vez, na indexação (que é rara), e o preço mais baixo toda vez que faz uma query (que é frequente).

### 4. Fazendo a conta pro seu vault

Vamos estimar com números realistas — e isso conecta direto com o Lab 1 que você já começou (o script `tiktoken`, que vai te dar o número exato em vez dessa estimativa):

```python
# Estimativa grosseira — refine com o resultado real do Lab 1
notas_no_vault = 826
tokens_medios_por_nota = 600  # estimativa; Lab 1 vai te dar o número real

total_tokens = notas_no_vault * tokens_medios_por_nota
print(f"Total estimado: {total_tokens:,} tokens")  # ~495.600

precos_por_milhao = {
    "voyage-4-lite": 0.02,
    "voyage-4": 0.06,
    "voyage-4-large": 0.12,
}

for modelo, preco in precos_por_milhao.items():
    custo = (total_tokens / 1_000_000) * preco
    print(f"{modelo}: ${custo:.4f} por reindex completo")
```

Saída aproximada:

```
voyage-4-lite: $0.0099 por reindex completo
voyage-4: $0.0297 por reindex completo
voyage-4-large: $0.0595 por reindex completo
```

Reindexar o vault inteiro custa **menos de 6 centavos de dólar**, mesmo no modelo mais caro da linha — e isso fica dentro dos 200M de tokens grátis por centenas de reindexações. Na maioria dos casos, o custo de embedding é irrelevante perto do custo de armazenamento no banco vetorial e do custo de inferência do modelo generativo — o que é uma inversão importante do que você vinha otimizando até agora.

### 5. Onde a decisão realmente importa: não é preço, é qualidade e armazenamento

Como o preço é desprezível na sua escala, o eixo de decisão muda:

**Dimensão do vetor → custo de armazenamento**: um vetor float32 de 1.024 dimensões ocupa 4KB; em 10 milhões de documentos isso já são 40GB de armazenamento vetorial — dobrar a dimensão dobra o armazenamento. No seu vault de 826 arquivos isso é irrelevante em termos absolutos, mas é bom você já entender a variável, porque escala com o crescimento do Second Brain.

**Matryoshka embeddings**: alguns modelos (OpenAI, Cohere) suportam embeddings Matryoshka, que permitem truncar a dimensão do vetor com degradação graciosa de qualidade — ou seja, você pode gerar um vetor de 1536 dims e usar só os primeiros 256 se quiser economizar armazenamento, sem precisar re-embedar do zero com um modelo menor.

**Privacidade**: se seu vault contém notas de estratégia de marca, posicionamento pessoal, ou qualquer coisa que você não quer que saia da sua máquina — local ganha por padrão, independente de custo.

### 6. A virada estratégica desta aula

Nas Aulas 1 e 2 você aprendeu a cortar tokens de geração agressivamente, porque ali o dinheiro realmente vaza. Embeddings são o oposto: **é um problema de custo essencialmente resolvido na sua escala**. Isso libera uma coisa importante — você pode reindexar o vault inteiro quantas vezes quiser enquanto experimenta as estratégias de chunking da Aula 2, sem se preocupar com o custo acumulado. Local ou via Voyage, ambos custam efetivamente zero pra você reexperimentar.

**Recomendação prática pro Prometheus OS**: dado que você vai reindexar com frequência durante a limpeza de tags e reestruturação do schema (`guia_mestre_metadados.md`), rodar `BGE-M3` local elimina qualquer fricção de custo/rate-limit nesse ciclo iterativo. Reserve Voyage AI pro dia em que quiser espremer os últimos pontos de qualidade de recall — nesse ponto, o custo continua trivial.

---

**Checagem rápida**: dado que embeddings não têm o eixo de output que você estudou nas Aulas 1-2 — se você decidisse usar **contextualized chunk embeddings** (que a Voyage também oferece, e que geram um vetor por chunk mas condicionado ao documento inteiro, não isoladamente) em vez de embeddar cada chunk isolado, isso mudaria o _custo_ de embedding, ou só a _qualidade_ do vetor resultante? Por quê?

---

# ==Explicação para Leigos==

Antes de qualquer coisa, um aviso importante que a própria aula já dá: **joga fora, por um momento, a régua que você usou nas Aulas 1 e 2** (entrada barata/paralela, saída cara/sequencial). Embedding é uma máquina diferente, fazendo um trabalho diferente.

Vou continuar com a cozinha, mas preciso trazer um personagem novo: **o scanner de código de barras**.

---

## 1. Por que a conta muda completamente aqui

Até agora, você tinha dois personagens na cozinha:

- **O leitor de pedido** (input): vários confeiteiros lendo o pedido todo, ao mesmo tempo, em paralelo
- **O confeiteiro montando o bolo** (output): um de cada vez, camada por camada, sequencial

Embedding não é nenhum dos dois. É um **terceiro personagem: o scanner**.

Imagina um scanner de supermercado que passa por cima do prato inteiro **uma única vez** e imprime uma etiqueta de código de barras resumindo aquele prato ("esse aqui é 70% doce, 20% ácido, 10% amargo..." — um "resumo de sabor" em números, que é exatamente o que chamamos de embedding).

O scanner **não constrói nada** — ele não vai "gerando uma etiqueta aos poucos, letra por letra". Ele faz **uma única passada** e cospe a etiqueta pronta, de tamanho fixo (384, 1024, 1536 números, dependendo do scanner). Não existe "montar camada por camada" aqui.

**Consequência direta**: como não existe a etapa de "montar", **não existe o eixo caro que você tanto se preocupou até agora** (o output sequencial e caro). Você só paga pelo que entra no scanner — quanto texto ele precisa escanear. Não existe aquela assimetria de "saída custa 5x mais que entrada" que você já internalizou pro Sonnet/Opus. Aqui é **uma tarifa só**, por volume escaneado.

A pergunta "gasto mais na entrada ou na saída?" simplesmente **não se aplica** a scanner — só existe "quanto material eu passei por baixo do scanner".

---

## 2. Ter seu próprio scanner em casa (rodar local)

Existem scanners que você **compra e instala na sua própria cozinha** (`sentence-transformers`, `BGE-M3`) — rodam no seu computador, sem ligar pra ninguém, sem internet.

A pegadinha: um scanner industrial de alta capacidade (uma GPU potente tipo A100) é baratíssimo _por uso_, mas custa **aluguel fixo mesmo parado** — cerca de US$ 1.080/mês, esteja você usando ou não. É tipo alugar uma cozinha industrial: se você só tem 826 receitas pra escanear, alugar a cozinha inteira é desperdício.

**Pro seu caso**: com 826 arquivos, você nem precisa de scanner industrial. Um scanner de mão comum (seu processador normal, sem GPU) dá conta — mais devagar (minutos em vez de segundos), mas com custo em dinheiro **zero**.

---

## 3. Usar o scanner de outra empresa (via API — Voyage AI)

A Anthropic não fabrica scanner próprio — ela recomenda oficialmente a **Voyage AI** como a marca parceira pra essa tarefa específica (isso é confirmado na documentação oficial deles, mesmo hoje).

Eles vendem 3 modelos de scanner, com preços diferentes:

|Modelo|Preço por milhão de tokens|
|---|---|
|voyage-4-lite|$0,02|
|voyage-4|$0,06|
|voyage-4-large|$0,12|

Todo mundo ganha **200 milhões de tokens grátis** nessa geração, e usar em lote (Batch) dá mais 33% de desconto.

**O truque esperto**: todos os 3 modelos "falam a mesma língua de etiqueta" — um vetor gerado pelo modelo caro é comparável com um vetor gerado pelo modelo barato. Então a jogada é: **escaneie uma vez com o scanner caro e preciso** (na hora de indexar o vault — evento raro), mas **use o scanner barato no dia a dia** (toda vez que você faz uma pergunta — evento frequente). Você paga caro só na etapa rara, e barato na etapa repetida.

---

## 4. Fazendo a conta pro seu vault

$$\text{826 notas} × \sim \text{600 tokens por nota} ≈ \text{495.600 tokens no total}.$$

Reescanear o vault **inteiro**, mesmo no modelo mais caro (voyage-4-large), custa menos de **6 centavos de dólar**. Isso cabe dentro dos 200 milhões grátis centenas de vezes. Na prática: **o custo de embedding é irrelevante** pra você — o dinheiro de verdade vaza em outro lugar (geração de texto, que você já vem otimizando).

---

## 5. Onde a decisão realmente pesa: não é preço, é espaço e privacidade

Já que o preço deixou de ser o problema, o que passa a importar é:

**Tamanho da etiqueta → espaço de armazenamento.** Uma etiqueta com 1.024 números ocupa mais espaço em disco que uma com 256 números. No seu vault de 826 arquivos isso é insignificante, mas cresce junto com o Second Brain — bom já saber que essa variável existe.

**Matryoshka embeddings** (o nome vem das bonecas russas que encaixam uma dentro da outra): alguns scanners deixam você gerar a etiqueta grande e depois **cortar** ela pra uma versão menor, sem precisar reescanear do zero. Tipo imprimir uma etiqueta grande, mas poder recortar só o pedaço que interessa depois, sem perder muita informação.

**Privacidade**: se alguma nota do seu vault é algo que você não quer que saia da sua máquina (estratégia pessoal, posicionamento, etc.), o scanner local ganha por padrão — **independente do preço**, você simplesmente não manda esse material pra fora.

---

## 6. A virada estratégica desta aula

Nas Aulas 1 e 2 você aprendeu a cortar cada token na geração de texto, porque **ali** o dinheiro vaza de verdade. Embedding é o oposto: **é um problema já resolvido, no seu tamanho de vault**.

Isso libera você pra: **reindexar o vault quantas vezes quiser** enquanto testa as estratégias de chunking da Aula 2 (fixo vs. semântico, com ou sem overlap), sem nenhuma ansiedade de custo acumulando. Local ou via Voyage, ambos custam efetivamente zero na sua escala.

**Recomendação prática**: enquanto você estiver em fase de bagunça (limpando tags, reestruturando o schema do `guia_mestre_metadados.md`, testando chunking), rode o scanner local (`BGE-M3`) — sem fricção, sem limite de uso. Guarde a Voyage API pro dia em que quiser espremer os últimos pontos de qualidade — mesmo aí, o custo continua trivial.

---

## Resumo visual

|Pergunta|Resposta|
|---|---|
|Existe eixo de output caro em embedding?|Não — uma passada só, sem geração sequencial|
|O que determina o custo?|Só o volume de tokens escaneados|
|Custo pro seu vault (826 notas)?|Menos de 6 centavos, no pior caso|
|Onde rodar durante a fase de testes?|Local (grátis, sem limite)|
|Onde a decisão real importa?|Dimensão do vetor (espaço) e privacidade — não preço|

---

**Teste rápido**: existe uma variante chamada _contextualized chunk embeddings_ — em vez de escanear cada pedacinho do vault isoladamente, o scanner olha o **documento inteiro** enquanto gera a etiqueta de cada pedaço, pra etiqueta ficar mais "ciente do contexto ao redor". Pensando no que você aprendeu na seção 1 (scanner = passada única, sem eixo de output): você acha que isso mudaria o **preço** do escaneamento, ou só a **qualidade** da etiqueta final? Por quê?