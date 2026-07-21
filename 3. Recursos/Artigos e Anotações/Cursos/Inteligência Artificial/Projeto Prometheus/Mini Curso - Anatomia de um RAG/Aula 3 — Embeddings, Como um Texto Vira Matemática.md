---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
> **Objetivo da aula**
> 
> Ao final desta aula, você entenderá o conceito que, na minha opinião, é o mais "mágico" de toda a Engenharia de IA:
> 
> **Como um computador consegue perceber que duas ideias são parecidas sem procurar pelas mesmas palavras?**

Essa é a função dos **embeddings**.

---

# Comecemos com um problema

Imagine que seu Second Brain possui esta nota.

```markdown
# Precificação

Elasticidade-preço mede o quanto a demanda varia em função da alteração do preço.
```

Agora alguém pergunta:

> **"O que acontece quando aumento o preço de um produto?"**

Observe.

A pergunta não contém:

- elasticidade;
    
- demanda;
    
- variação.
    

Mesmo assim...

Nós, humanos, sabemos que ela está falando do mesmo assunto.

Como um computador faz isso?

---

# A busca tradicional

Imagine um computador extremamente simples.

Ele faz isto:

```text
Pergunta

↓

Procura a palavra

"elasticidade"
```

Não encontrou.

Resultado:

```text
Nenhum documento encontrado.
```

Mesmo existindo uma nota perfeita.

Esse tipo de busca chama-se:

> **Busca lexical.**

Ela procura palavras.

Não significado.

---

# Agora imagine outra busca

O computador olha para isto.

```text
Elasticidade-preço

↓

Significado
```

E olha para isto.

```text
Aumento de preço

↓

Significado
```

Percebe que as duas ideias estão relacionadas.

Mesmo usando palavras diferentes.

<h5 align="center">Essa é a busca semântica.</h5>

Mas...

Como isso é possível?

---

# A resposta: Embeddings

Embedding significa:

> **Uma representação matemática do significado de um texto.**

Em vez de guardar isto:

```text
Elasticidade-preço
```

O sistema guarda algo parecido com:

```text
[0.18,
-0.42,
0.77,
...
0.05]
```

Esse conjunto enorme de números é chamado de vetor.

---

# "Mas professor..."

> **"Como números representam significado?"**

Excelente pergunta.

Vamos fazer uma analogia.

---

# Imagine um mapa

Num mapa existem coordenadas.

```text
Fortaleza

↓

(-3.73, -38.52)
```

Rio de Janeiro.

```text
(-22.90, -43.17)
```

São Paulo.

```text
(-23.55, -46.63)
```

As coordenadas permitem calcular distância.

---

# Agora imagine um mapa de ideias

Em vez de cidades.

Temos conceitos.

```text
Marketing

↓

( ... )

Empreendedorismo

↓

( ... )

Transformers

↓

( ... )

Precificação

↓

( ... )
```

Cada conceito ganha uma posição.

Não no espaço físico.

Mas em um espaço matemático.

---

# O segredo

Ideias parecidas ficam próximas.

Exemplo.

```text
Carro

↓

●
```

Automóvel.

```text
●
```

Ônibus.

```text
●
```

Todos ficam relativamente próximos.

Agora:

```text
Banana

↓

                         ●
```

Muito distante.

---

# Visualmente

Imagine isto.

```text
                 Banana


Carro

Automóvel

Moto

Ônibus
```

Os conceitos formam "regiões".

Não porque alguém programou.

Mas porque o modelo aprendeu essas relações.

---

# Um exemplo do seu Second Brain

Imagine que você possui estas notas.

```text
Marketing/

↓

Precificação.md
```

Outra.

```text
Economia/

↓

Elasticidade.md
```

Outra.

```text
Negociação/

↓

Preço Psicológico.md
```

Mesmo estando em pastas diferentes.

Os embeddings podem colocá-las próximas.

Porque falam de temas relacionados.

---

# Isso explica algo interessante

Você perguntou outro dia:

> "Então o Chroma substitui meu Second Brain?"

Agora conseguimos responder melhor.

Não.

O Chroma não guarda:

```markdown
# Precificação
```

Ele guarda algo parecido com:

```text
[-0.72,
0.13,
0.55,
...
0.81]
```

Ou seja.

Ele guarda o significado matemático.

Não o texto.

---

# Como um embedding é criado?

Existe um modelo específico.

Por exemplo.

```text
Texto

↓

Modelo de Embedding

↓

Vetor
```

Importante.

Esse modelo NÃO responde perguntas.

Ele só transforma significado em matemática.

---

# Então existem modelos diferentes?

Sim.

Assim como existem modelos para gerar texto:

- GPT;
    
- Claude;
    
- Gemini.
    

Também existem modelos especializados em embeddings.

Por exemplo:

- `text-embedding-3-small`
    
- `text-embedding-3-large`
    

Da OpenAI.

A função deles é apenas gerar vetores.

---

# O pipeline agora faz sentido

Vamos completar mais uma peça.

```text
Markdown

↓

Chunk

↓

Modelo de Embedding

↓

Vetor

↓

Banco Vetorial
```

Perceba.

O banco vetorial nunca leu seu Markdown.

Ele recebeu apenas vetores.

---

# E quando chega uma pergunta?

Acontece exatamente o mesmo.

Pergunta.

```text
Como funciona Multi-Head Attention?
```

↓

Modelo de Embedding.

↓

Vetor.

↓

Banco Vetorial.

↓

"Quais vetores estão mais próximos?"

---

# Um detalhe importantíssimo

Muita gente imagina isto.

```text
Embedding

↓

Resumo do texto
```

Não.

Isso está errado.

Embedding NÃO resume.

Embedding NÃO explica.

Embedding NÃO gera texto.

Ele apenas transforma significado em coordenadas matemáticas.

---

# Uma analogia que acho perfeita

Imagine um GPS.

Você diz:

> "Quero ir ao shopping."

O GPS não entende português como nós.

Internamente.

Ele trabalha com coordenadas.

Depois calcula distâncias.

Embedding faz algo parecido.

Só que em vez de cidades.

Temos ideias.

---

# Um insight para você

Lembra que você gosta de encontrar o **arché** dos conceitos?

Veja a sequência.

```text
Conhecimento

↓

Texto

↓

Chunk

↓

Significado

↓

Embedding

↓

Vetor
```

Repare que, a cada etapa, estamos abstraindo mais.

- Primeiro existe a ideia.
    
- Depois ela vira texto.
    
- Depois o texto é dividido.
    
- Depois o significado é capturado.
    
- Depois o significado vira matemática.
    

É como se estivéssemos traduzindo uma ideia humana para uma linguagem que a máquina consegue manipular.

Essa é uma das partes mais elegantes de toda a IA moderna.

---

# Ligando com a aula passada

Na Aula 2 vimos:

```text
Documento

↓

Chunking
```

Hoje vimos:

```text
Chunk

↓

Embedding
```

Na próxima aula veremos:

```text
Embedding

↓

Banco Vetorial
```

Ou seja.

Finalmente entenderemos o que realmente fazem:

- Chroma;
    
- Pinecone;
    
- Weaviate;
    
- Milvus;
    
- Qdrant.
    

E você verá que eles são muito menos "misteriosos" do que parecem.

---

# A ideia mais importante da aula

Se eu pudesse resumir tudo em uma frase, seria:

> **Um embedding é uma representação matemática do significado de um texto, permitindo que computadores comparem ideias por similaridade sem depender de palavras idênticas.**

---

## Uma observação para você

Percebi que, ao longo do curso, você sempre tenta reduzir conceitos complexos a seus princípios fundamentais. Nesta aula, esse princípio é:

> **Embedding não é inteligência. Não é resposta. Não é memória. É apenas uma tradução: do significado humano para um espaço matemático onde a máquina consegue medir proximidade entre ideias.**

Quando essa ideia se consolida, o restante do pipeline de RAG começa a parecer muito mais natural.