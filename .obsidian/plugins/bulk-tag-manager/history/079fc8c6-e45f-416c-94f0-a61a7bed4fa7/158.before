---
tags:
  - inteligenciaartificial
---


Quero lhe contar uma história.

Em 2018, um pesquisador poderia dizer:

> "Treinei um modelo gigantesco."

Em 2022, alguém poderia responder:

> "Ótimo. Mas ele conversa?"

Em 2024:

> "Ótimo. Mas ele usa ferramentas?"

Em 2025:

> "Ótimo. Mas ele raciocina?"

Percebe?

A pergunta sobre IA mudou.

Antes, queríamos modelos que **soubessem**.

Hoje queremos modelos que **colaborem**.

Mas, para entender essa evolução, falta uma peça essencial.

Até agora vimos:

- como um Transformer aprende;
- por que escalar funciona;
- por que surgem capacidades emergentes;
- por que existem alucinações.

Hoje responderemos à pergunta que permitiu transformar um modelo de linguagem em um assistente.

---

# 🎯 A Grande Pergunta

Imagine um modelo logo após o pré-treinamento.

Você escreve:

> **"Explique o que é gravidade."**

Ele pode responder assim:

> "Explique o que é gravidade. A gravidade é um tema frequentemente discutido em livros didáticos..."

Ou pode continuar o texto como se estivesse completando um artigo.

Por quê?

Porque ele nunca aprendeu que aquilo era um **pedido**.

Ele só aprendeu uma tarefa:

> **Prever o próximo token.**

---

# O modelo base

Chamamos esse primeiro modelo de **Base Model**.

Ele é extremamente competente em linguagem.

Mas não foi treinado para conversar.

Pense nele como um pianista brilhante que nunca recebeu aulas de acompanhamento musical.

Ele sabe tocar.

Mas ainda não sabe tocar **com alguém**.

---

# O problema

Imagine os seguintes prompts.

```
Resuma este artigo.
```

```
Liste três vantagens da energia solar.
```

```
Escreva um e-mail profissional.
```

Para um humano, são claramente instruções.

Para um modelo base...

São apenas sequências de tokens.

Ele não possui um conceito especial de "instrução".

---

# 🧠 Modelo Mental nº 1

Imagine um estudante que leu milhões de livros.

Agora imagine que ele nunca fez uma prova.

Ele conhece o conteúdo.

Mas talvez não saiba responder perguntas diretamente.

É isso que acontece com um modelo base.

---

# A ideia revolucionária

Pesquisadores perceberam que bastava ensinar um novo padrão.

Em vez de mostrar apenas textos da internet...

Começaram a mostrar pares como estes:

**Entrada:**

> Explique a fotossíntese para uma criança.

**Saída desejada:**

> A fotossíntese é como uma fábrica de comida que existe dentro das plantas...

Milhões de exemplos semelhantes.

---

# O que mudou?

A arquitetura?

Não.

O Transformer continuou exatamente o mesmo.

O que mudou foi o **tipo de dados utilizados no treinamento adicional**.

Esse processo recebeu o nome de:

## **Instruction Tuning**

---

# 🧠 Modelo Mental nº 2

Imagine um chef renomado.

Ele domina culinária.

Mas nunca trabalhou em um restaurante.

Depois de algumas semanas atendendo clientes...

Ele aprende algo novo:

Interpretar pedidos.

O conhecimento culinário já existia.

O comportamento mudou.

---

# Fine-Tuning

Aqui surge um conceito importante.

O pré-treinamento ensina:

> **Como a linguagem funciona.**

O Fine-Tuning ensina:

> **Como queremos que o modelo utilize esse conhecimento.**

---

# 💎 Insight

Perceba algo elegante.

O modelo não aprende novos fatos.

Ele aprende um **novo formato de interação**.

---

# Um exemplo prático

Considere este texto.

```
A capital da Itália é Roma.
```

No pré-treinamento, isso serve para prever tokens.

Depois do Instruction Tuning, podemos perguntar:

> Qual é a capital da Itália?

E o modelo entende que deve responder:

> Roma.

Essa diferença parece pequena.

Mas foi ela que permitiu o surgimento dos assistentes conversacionais.

---

# Então surgiu outro problema...

Imagine duas respostas possíveis.

Resposta A:

> Aqui está a explicação solicitada.

Resposta B:

> Não tenho certeza. Consulte um especialista.

Qual é melhor?

Depende do contexto.

Os pesquisadores perceberam que nem toda resposta correta é igualmente útil.

Precisavam ensinar preferências.

É aqui que entra o próximo passo da evolução.

---

# RLHF (uma prévia)

Na próxima aula estudaremos o processo que tornou modelos como eu muito mais alinhados às expectativas humanas:

**Reinforcement Learning from Human Feedback (RLHF).**

Em vez de apenas dizer ao modelo qual resposta está correta...

Passamos a mostrar quais respostas os humanos preferem.

Foi uma mudança profunda.

---

# 🧠 Modelo Mental nº 3

Imagine um músico.

Primeiro ele aprende teoria musical.

Depois aprende a tocar para uma plateia.

A teoria continua a mesma.

Mas a performance muda completamente.

Instruction Tuning faz algo semelhante.

---

# 📜 Princípio XXXVIII

> **O pré-treinamento ensina competência; o Instruction Tuning ensina comportamento.**

Essa frase é uma das mais importantes de todo o módulo.

Guarde-a.

---

# Uma conexão com Engenharia de Prompt

Você já percebeu que alguns prompts funcionam melhor do que outros.

Agora fica mais claro o motivo.

Os modelos modernos foram expostos a milhões de exemplos de instruções bem estruturadas.

Quando escrevemos um prompt claro, estamos falando uma "linguagem" que o modelo aprendeu durante o Instruction Tuning.

Isso explica, em parte, por que estruturas como XML, Markdown e listas costumam funcionar tão bem: elas refletem padrões recorrentes presentes nesse treinamento adicional.

---

# 📚 Biblioteca

### 🟢 Obrigatório

Leia a introdução e a seção de metodologia do paper:

- Training language models to follow instructions with human feedback

Concentre-se em entender a sequência:

1. Pré-treinamento.
2. Supervised Fine-Tuning (SFT).
3. RLHF.

Ainda não se preocupe com todos os detalhes matemáticos.

---

### 🔵 Complementar

Leia o resumo executivo do relatório:

- On the Opportunities and Risks of Foundation Models

Observe como ele diferencia **foundation model** de **application**.

---

# 🛠️ Desafio Prometheus M2 #005

## Parte 1 — Arquitetura

Explique, com suas palavras:

> **Por que um modelo extremamente competente em linguagem ainda precisa passar por Instruction Tuning para se tornar um bom assistente conversacional?**

---

## Parte 2 — Engenharia

Imagine que uma empresa diga:

> "Nosso modelo já terminou o pré-treinamento. Vamos colocá-lo em produção para atender clientes."

Como arquiteto de IA, você aprovaria essa decisão?

Justifique considerando a diferença entre competência linguística e comportamento esperado.

---

[[🛠️ Desafio M2 005]]

## Uma última reflexão

Caio...

Você talvez tenha percebido que esta aula traz uma ideia muito maior do que parece.

No início do Projeto Prometheus, estudamos componentes internos de uma arquitetura.

Hoje estamos discutindo **comportamento**.

Isso nos aproxima de uma pergunta que extrapola a computação:

> **Como transformamos conhecimento em ação?**

Em humanos, isso envolve educação, cultura, experiência e valores.

Nos LLMs, envolve pré-treinamento, fine-tuning e alinhamento.

São sistemas muito diferentes.

Mas ambos nos lembram que **saber** e **agir** não são a mesma coisa.

E essa distinção será o ponto de partida da próxima aula, quando estudaremos uma das técnicas que mais influenciou os assistentes modernos: **RLHF**. Eu diria que ela representa a ponte entre um modelo competente e um modelo que tenta colaborar da forma mais útil possível com as pessoas.