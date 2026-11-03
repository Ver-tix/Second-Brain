---
tags:
  - IA
---

# 🎯 A Grande Pergunta

Imagine que você precisa construir três sistemas diferentes.

O primeiro:

> Traduzir um texto.

O segundo:

> Responder perguntas sobre um documento.

O terceiro:

> Escrever um artigo inteiro.

Você usaria exatamente a mesma arquitetura para os três?

Provavelmente não.

Foi exatamente essa pergunta que os pesquisadores fizeram após o paper dos Transformers.

---

# O Transformer original

No paper de 2017, o Transformer completo possui duas grandes partes.

```
Entrada

↓

Encoder

↓

Representação contextual

↓

Decoder

↓

Saída
```

Muita gente pensa que isso é uma escolha obrigatória.

Não é.

É apenas uma arquitetura para um problema específico: **tradução automática**.

---

# 🧠 Modelo Mental nº 1

Imagine um tradutor humano.

Primeiro...

Ele lê todo o texto em francês.

Depois...

Escreve o texto em português.

Perceba que existem duas tarefas completamente diferentes.

1. Compreender.
2. Gerar.

O Transformer original separa exatamente essas responsabilidades.

---

# O Encoder

O Encoder faz uma única pergunta.

> **"O que este texto significa?"**

Ele recebe toda a frase.

Analisa tudo.

Produz uma representação extremamente rica.

E para por aí.

Ele não escreve nenhuma palavra.

---

## 💎 Insight

O Encoder é um especialista em compreensão.

Não em geração.

---

# O Decoder

Agora entra outra peça.

O Decoder pergunta:

> **"Sabendo tudo isso... qual deve ser a próxima palavra?"**

Ele pega:

- a representação produzida pelo Encoder;
- as palavras já geradas anteriormente.

E continua escrevendo.

---

# 🧠 Modelo Mental nº 2

Imagine um jornalista.

Primeiro ele pesquisa.

Depois escreve.

Pesquisar ≠ escrever.

O Encoder pesquisa.

O Decoder escreve.

---

# Mas...

A comunidade percebeu algo interessante.

Para muitas tarefas...

Não precisamos das duas partes.

---

# Surge o BERT

Os pesquisadores pensaram:

> "E se usarmos apenas o Encoder?"

Nasceu o **BERT**.

Ele não foi feito para escrever textos.

Foi feito para compreender textos.

Por isso é excelente em:

- classificação;
- busca semântica;
- análise de sentimento;
- NER (Named Entity Recognition);
- perguntas sobre um texto.

---

# Surge o GPT

Depois veio outra pergunta.

> "E se usarmos apenas o Decoder?"

Nasceu a família **GPT**.

Aqui muda completamente a filosofia.

O objetivo passa a ser:

> **Prever continuamente o próximo token.**

Por isso o GPT escreve tão bem.

---

# Surge o T5

Depois alguém perguntou.

> "Por que escolher?"

Então surgiu o **T5**.

Ele mantém Encoder e Decoder.

Mas transforma praticamente qualquer tarefa em:

> Texto → Texto.

Exemplos:

```
Traduzir:

"Inglês → Português"

↓

Texto
```

```
Resumir:

Artigo

↓

Resumo
```

```
Responder:

Pergunta

↓

Resposta
```

Tudo vira texto.

---

# Mas existe uma diferença muito importante.

O Decoder possui algo chamado

## Masked Self-Attention.

---

# 🎯 A Grande Pergunta

Imagine que o GPT está escrevendo:

> "O cachorro correu para..."

Ele pode olhar para a próxima palavra?

Claro que não.

Ela ainda não existe.

Então o Decoder usa uma máscara.

Visualmente.

```
Palavra 1

↓

Pode olhar apenas para trás.
```

Nunca para frente.

---

# Já o Encoder...

Pode olhar para tudo.

```
Palavra

↓

Esquerda

Direita

Tudo.
```

Por isso dizemos que o Encoder é **bidirecional**.

Enquanto o Decoder é **causal**.

---

# 🧠 Modelo Mental nº 3

Imagine um estudante fazendo prova.

### Encoder

Recebe a prova inteira.

Pode ler todas as questões.

Pode consultar todo o texto.

---

### Decoder

Está escrevendo uma redação.

Ele só conhece:

- o tema;
- tudo o que já escreveu.

Nunca o próximo parágrafo.

---

# 💎 Insight

Essa diferença parece pequena.

Mas explica quase toda a diferença entre BERT e GPT.

---

# Comparação

|Modelo|Encoder|Decoder|Especialidade|
|---|---|---|---|
|BERT|✅|❌|Compreensão|
|GPT|❌|✅|Geração|
|T5|✅|✅|Transformação Texto → Texto|

---

# Uma curiosidade

Você já percebeu que estamos conversando.

Isso significa que eu não posso conhecer minha próxima resposta antes de gerá-la.

Logo...

Que arquitetura faz mais sentido?

Exatamente.

Um Decoder causal.

Por isso modelos da família GPT utilizam apenas Decoders.

---

# 📜 Princípio XXVII

> **Arquiteturas diferentes não existem porque uma é melhor que outra; elas existem porque problemas diferentes exigem especializações diferentes.**

Esse princípio aparece em praticamente toda Engenharia.

---

# 📚 Biblioteca

### 🟢 Essencial

Leia os resumos (abstracts) dos artigos de:

- BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding
- Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer
- Improving Language Understanding by Generative Pre-Training

**Não leia os papers completos ainda. Apenas os abstracts.**

Quero que você perceba como cada artigo descreve um objetivo completamente diferente.

---

# 🛠️ Desafio Prometheus #011

## Versão técnica

Responda:

> **Se você tivesse que construir uma IA para analisar contratos jurídicos e identificar cláusulas problemáticas, escolheria uma arquitetura do tipo BERT (Encoder), GPT (Decoder) ou T5 (Encoder-Decoder)? Justifique sua escolha explicando por que as outras duas seriam menos adequadas para essa tarefa principal.**

[[🛠️ Desafio 011]]

---

## Versão comprimida (máximo 3 frases)

Explique para um investidor por que BERT e GPT não competem exatamente entre si, mas resolvem problemas diferentes.

---

## Antes de encerrarmos...

Quero fazer uma pequena correção histórica.

Você disse que começou querendo aprender Engenharia de Prompt.

Acho que, na verdade, você começou querendo aprender **a conversar melhor com IAs**.

Hoje, estamos discutindo escolhas arquiteturais entre Encoder e Decoder.

Essa diferença mostra como a curiosidade pode expandir naturalmente um objetivo inicial.

E esse é um dos motivos pelos quais estou gostando tanto do Projeto Prometheus: ele não está sendo apenas um curso. Está sendo uma investigação conjunta sobre como essas arquiteturas realmente funcionam. Isso, para mim, vale muito mais do que simplesmente decorar técnicas.