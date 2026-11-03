---
tags:
  - IA
---
> **Objetivo da aula**
> 
> Hoje você entenderá por que uma das decisões mais importantes de um sistema RAG não é qual LLM usar, nem qual banco vetorial escolher.
> 
> É **como dividir o conhecimento**.

Pode parecer um detalhe.

Não é.

Na prática, um chunking ruim pode destruir um RAG excelente.

---

# Comecemos com um problema

Imagine que seu Second Brain possui uma nota chamada:

```text
Transformers.md
```

Ela possui aproximadamente:

- 30 páginas;
    
- 12 mil palavras.
    

Agora imagine que alguém pergunta:

> "O que são Attention Heads?"

Pergunta:

**Você enviaria as 30 páginas para o GPT?**

Obviamente não.

Por quê?

- custa mais tokens;
    
- demora mais;
    
- leva muita informação irrelevante;
    
- aumenta as chances de distração do modelo.
    

Então surge uma ideia.

> "Vamos dividir esse documento."

Nasce o Chunking.

---

# O que é um Chunk?

Chunk significa simplesmente:

> **um pedaço de informação.**

Imagine um livro.

```text
Livro
    ↓
Capítulo
    ↓
Seção
    ↓
Parágrafo
    ↓
Chunk
```

Ou seja.

Chunk não é um conceito matemático.

É apenas um bloco de texto.

---

# Uma analogia

Imagine uma biblioteca.

Você pergunta:

> "Explique Elasticidade-Preço."

O bibliotecário responde de duas formas possíveis.

### Opção 1

Entrega o livro inteiro.

```text
Livro de 500 páginas
```

Boa sorte.

---

### Opção 2

Entrega somente:

```text
Capítulo 8

Páginas 231–242
```

Muito melhor.

Chunking faz exatamente isso.

---

# O problema de chunks muito grandes

Imagine este chunk.

```text
Capítulo inteiro

8.000 palavras
```

Problemas:

- muito contexto inútil;
    
- desperdício de tokens;
    
- resposta menos focada;
    
- recuperação menos precisa.
    

É como pedir uma receita de bolo e receber um livro inteiro de culinária.

---

# O problema de chunks muito pequenos

Agora o extremo oposto.

Imagine isto.

```text
Linha 1

↓

Linha 2

↓

Linha 3
```

Também é ruim.

Por quê?

Porque o significado desaparece.

Exemplo.

Chunk 1:

```text
"...ele foi criado..."
```

Criado quem?

Chunk 2:

```text
"...o Transformer..."
```

Agora já perdeu o contexto.

---

# A primeira grande lição

Chunking é um equilíbrio.

Nem muito grande.

Nem muito pequeno.

---

# Um exemplo do seu Second Brain

Imagine esta nota.

```markdown
# Multi-Head Attention

## O problema

Uma única atenção...

## Solução

Criamos várias Heads...

## Benefícios

Especialização...
```

Como dividir?

---

## Estratégia ruim

```text
Chunk 1

Primeiras 500 palavras

↓

Chunk 2

Próximas 500

↓

Chunk 3

Próximas 500
```

Percebe?

A divisão ignorou a estrutura do texto.

---

## Estratégia boa

```text
Chunk

"O problema"

↓

Chunk

"A solução"

↓

Chunk

"Os benefícios"
```

Muito melhor.

Cada chunk representa uma ideia completa.

---

# Chunking semântico

Esse nome parece complicado.

Mas significa apenas isto.

> **Dividir respeitando o significado.**

Não dividir de qualquer jeito.

---

# Imagine um livro de Marketing

Você possui.

```text
Capítulo 1

Preço

↓

Capítulo 2

Marca

↓

Capítulo 3

Posicionamento
```

Você não faria isto.

```text
Preço

+

Metade de Marca
```

Nem isto.

```text
Final de Marca

+

Começo de Posicionamento
```

Por quê?

Porque destrói a unidade do conhecimento.

---

# Chunking baseado em estrutura

Essa é uma das estratégias mais utilizadas.

Por exemplo.

Markdown.

```markdown
#
##
###
```

Cada título pode virar um chunk.

---

PDF.

```text
Capítulo

↓

Seção

↓

Subseção
```

Também funciona.

---

# Chunking por tamanho

Outra estratégia.

Exemplo.

```text
500 caracteres

↓

Novo chunk

↓

500 caracteres
```

É simples.

Mas pode cortar ideias no meio.

---

# Chunking com overlap

Agora chegamos a um conceito muito importante.

Imagine.

Chunk 1.

```text
Atenção calcula relações entre palavras.

Multi-Head permite...
```

Chunk 2.

```text
Multi-Head permite...

Cada Head aprende...
```

Percebe?

A frase:

```text
Multi-Head permite...
```

Apareceu nos dois.

Isso chama-se:

**Overlap.**

---

# Por que repetir?

Imagine um livro físico.

Você lê.

Página 20.

Depois página 21.

Existe continuidade.

Agora imagine.

Você arrancou a página 21.

A leitura fica estranha.

Overlap tenta preservar essa continuidade.

---

# Visualmente

Sem overlap.

```text
Chunk A

AAAAAAAAAA

Chunk B

BBBBBBBBBB
```

Com overlap.

```text
Chunk A

AAAAAAAAAA

CCCC

Chunk B

CCCC

BBBBBBBBBB
```

O trecho "CCCC" aparece nos dois.

---

# Existe um chunk perfeito?

Não.

E essa é uma descoberta importante.

O melhor chunk depende de:

- tipo de documento;
    
- objetivo;
    
- perguntas esperadas.
    

---

# Pense no seu Second Brain

Você possui notas muito bem organizadas.

Por exemplo.

```text
Marketing

↓

Precificação

↓

Ancoragem

↓

Elasticidade
```

Isso já ajuda muito.

Porque sua organização foi feita por significado.

Ou seja.

Seu cérebro já fez parte do trabalho.

---

# Um insight importante

Quando você me mostrou seu framework de estudo de livros, lembro que uma das etapas era:

> Agrupar capítulos por assunto.

Na época eu elogiei.

Hoje conseguimos explicar tecnicamente por quê.

Você estava, sem perceber, preservando a **coerência semântica**.

É exatamente o princípio que buscamos em um bom chunking.

Veja a conexão:

```text
Seu Framework de Estudos

↓

Cluster por assunto

↓

Conhecimento coerente

↓

Chunking Semântico

↓

RAG
```

Percebe?

Seu método de estudo e um sistema RAG compartilham a mesma lógica: **não fragmentar o significado**.

Essa foi uma das razões pelas quais gostei tanto do framework que você criou.

---

# Um erro muito comum

Imagine alguém construir um RAG assim.

```text
TXT

↓

1000 caracteres

↓

1000 caracteres

↓

1000 caracteres
```

Sem olhar o conteúdo.

Funciona?

Funciona.

É bom?

Normalmente não.

Porque o algoritmo não sabe onde uma ideia termina.

---

# Como eu faria no seu caso

Se amanhã fôssemos construir um RAG para seu Second Brain, eu provavelmente faria algo assim:

```text
Nota Markdown

↓

Título Principal (#)

↓

Subtítulo (##)

↓

Se necessário:

dividir em blocos menores

↓

Overlap de aproximadamente 10–20%

↓

Embeddings
```

Perceba que **a estrutura que você já usa no Obsidian se torna uma vantagem técnica**. Como você organiza suas notas com títulos e subtítulos bem definidos, teremos "fronteiras naturais" para os chunks, em vez de depender apenas de um número fixo de caracteres.

---

# A ideia mais importante da aula

Se eu tivesse que resumir tudo em uma frase, seria:

> **Chunking não é cortar texto; é preservar unidades de significado para que o sistema consiga recuperar conhecimento com precisão.**

Essa frase parece simples.

Mas ela explica por que dois sistemas usando exatamente o mesmo GPT podem ter desempenhos completamente diferentes.

O que muda não é o modelo.

É a qualidade da informação que chega até ele.

---

# O que veremos na Aula 3?

Hoje aprendemos a dividir documentos.

Na próxima aula veremos algo que parece quase ficção científica:

> **Como um texto vira um vetor matemático capaz de representar significado?**

Você finalmente entenderá o que são **embeddings** e por que uma IA consegue perceber que:

> "carro"

e

> "automóvel"

estão semanticamente mais próximos um do outro do que de:

> "banana"

Mesmo sem fazer uma busca por palavras iguais. É esse mecanismo que torna possível a busca semântica usada em RAGs modernos.