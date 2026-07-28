---
tags:
  - IA
---
## Objetivo do módulo

No Módulo 1 respondemos:

> **"Como um Transformer funciona?"**

Agora responderemos:

> **"Como nasce um ChatGPT?"**

Parece uma pergunta simples.

Não é.

Entre um Transformer recém-inicializado e um modelo como eu existe um processo gigantesco.

Esse processo envolve:

- centenas de bilhões de tokens;
- milhares de GPUs;
- meses de treinamento;
- diversas fases distintas;
- decisões de engenharia extremamente sofisticadas.

E é justamente isso que vamos estudar.

---

# 📚 Estrutura do Módulo 2

Este módulo será dividido em **oito grandes unidades**.

## Unidade I — O nascimento de um LLM

Perguntas que responderemos:

- O que significa "treinar" um modelo?
- O que é um token durante o treinamento?
- Como um Transformer aprende linguagem sem receber regras gramaticais?
- O que exatamente é uma função de perda (_loss function_)?
- O que o modelo está realmente tentando minimizar?

Ao final dessa unidade você entenderá como um modelo "aprende a ler".

---

## Unidade II — Pré-treinamento

Aqui entraremos no coração da IA moderna.

Você aprenderá:

- _next-token prediction_;
- gradiente descendente;
- backpropagation (de forma intuitiva primeiro e matemática depois);
- épocas (_epochs_);
- batches;
- otimização;
- escalabilidade.

---

## Unidade III — O surgimento da inteligência

Esta talvez seja minha unidade favorita.

Pergunta central:

> **Como um modelo que só prevê a próxima palavra aprende lógica, programação, matemática e até filosofia?**

Estudaremos:

- capacidades emergentes (_emergent abilities_);
- leis de escala (_Scaling Laws_);
- por que modelos maiores parecem "mais inteligentes";
- quando simplesmente aumentar parâmetros deixa de funcionar.

---

## Unidade IV — Alinhamento

Aqui responderemos uma pergunta que praticamente toda pessoa faz.

> **"Se o modelo aprendeu na internet inteira... por que ele responde de forma educada?"**

Entraremos em:

- _Instruction Tuning_;
- _Supervised Fine-Tuning (SFT)_;
- RLHF;
- preferências humanas;
- segurança.

Você finalmente entenderá por que um modelo base é tão diferente de um modelo conversacional.

---

## Unidade V — Engenharia de Prompt (de verdade)

Agora sim.

Entraremos naquilo que motivou nossa jornada.

Mas com um detalhe.

Você já conhecerá toda a arquitetura.

Então veremos:

- por que certos prompts funcionam;
- por que outros falham;
- prompting como programação probabilística;
- XML;
- JSON;
- decomposição;
- planejamento;
- raciocínio estruturado.

Não será um curso de "receitas".

Será engenharia.

---

## Unidade VI — Agentes

Aqui deixamos de conversar com uma IA.

Passamos a construir sistemas.

Veremos:

- Tool Use;
- MCP;
- memória;
- planejamento;
- execução;
- reflexão;
- múltiplos agentes.

---

## Unidade VII — RAG

Pergunta central:

> **Como fazer uma IA responder usando documentos sem precisar reaprender tudo?**

Você aprenderá:

- embeddings na prática;
- bancos vetoriais;
- recuperação semântica;
- contexto externo;
- arquitetura RAG.

---

## Unidade VIII — O Futuro

Terminaremos estudando:

- modelos multimodais;
- raciocínio;
- agentes autônomos;
- pesquisas atuais;
- fronteiras abertas.

---

# Uma mudança importante no curso

Até agora...

Eu fui seu professor.

A partir deste módulo...

Quero ser também seu **orientador de pesquisa**.

A diferença é sutil.

Professor ensina respostas.

Orientador ensina a formular perguntas.

Você já demonstrou maturidade suficiente para isso.

Então...

Em algumas aulas, em vez de responder imediatamente, vou lhe desafiar a construir hipóteses.

É assim que pesquisadores trabalham.

---

# 📚 Nova Bibliografia Oficial

A partir de hoje, estes passam a ser nossos livros e referências permanentes.

### Fundamentos

- Artificial Intelligence: A Modern Approach _(continuaremos usando)_
- Deep Learning
- Speech and Language Processing

### Papers obrigatórios ao longo do módulo

- [[Language Models Are Few-Shot Learners]]
- Training language models to follow instructions with human feedback
- Scaling Laws for Neural Language Models
- [[Constitutional AI - Harmlessness from AI Feedback]]Constitutional AI: Harmlessness from AI Feedback

No momento certo, estudaremos cada um deles.