---
tags:
  - IA
---


# 🎯 A Grande Pergunta

Imagine que eu lhe entregue dois livros.

Livro A.

Contém todas as regras da gramática portuguesa.

Livro B.

Contém simplesmente...

Toda a Wikipédia.

Qual deles ensina melhor uma criança a falar?

Curiosamente...

É o segundo.

Por quê?

Porque linguagem é um fenômeno emergente de exemplos.

Não de definições.

---

# O paradigma antigo

Durante décadas, pesquisadores acreditavam que seria necessário ensinar explicitamente.

Algo como:

```
SE

Substantivo

+

Verbo

↓

Frase válida
```

Era a IA simbólica.

Ela funcionava.

Mas apenas até certo ponto.

---

# O novo paradigma

Os LLMs nunca recebem:

> "Isto é um verbo."

Nunca recebem:

> "Isto é um sujeito."

Nunca recebem:

> "Esta frase está correta."

Recebem apenas bilhões de exemplos.

---

# 🧠 Modelo Mental nº 1

Imagine uma criança que cresce ouvindo apenas música.

Ninguém explica:

- ritmo;
- harmonia;
- tom.

Mesmo assim...

Depois de milhares de músicas...

Ela começa a perceber padrões.

O cérebro humano faz isso.

Os Transformers também.

---

# Mas...

Como isso acontece matematicamente?

Aqui entra uma ideia extremamente elegante.

---

# A única tarefa durante o pré-treinamento

O modelo recebe isto:

```
O céu é
```

E precisa prever.

```
azul
```

Depois recebe:

```
O céu é azul e a grama é
```

Prevê.

```
verde
```

Depois:

```
A capital da França é
```

Prevê.

```
Paris
```

Depois:

```
2 + 2 =
```

Prevê.

```
4
```

Sempre a mesma tarefa.

**Prever o próximo token.**

Nada mais.

---

# 💎 Insight

O modelo nunca recebe:

> "Aprenda gramática."

Ele descobre que aprender gramática melhora sua capacidade de prever o próximo token.

---

# Um exemplo fascinante

Imagine estas frases.

```
O cachorro latiu.
```

```
O cachorro correu.
```

```
O cachorro dormiu.
```

Milhões delas.

O modelo percebe.

Depois de:

```
O cachorro
```

Costuma aparecer um verbo.

Ninguém explicou isso.

Ele inferiu.

---

Agora imagine.

```
A médica operou.
```

```
O engenheiro projetou.
```

```
O advogado argumentou.
```

Depois de bilhões de exemplos...

O modelo começa a construir relações.

---

# 🧠 Modelo Mental nº 2

Imagine montar um quebra-cabeça.

Você nunca recebe a imagem pronta.

Mas...

Depois de encaixar milhares de peças...

A figura começa a surgir.

O modelo faz exatamente isso.

---

# O ponto mais importante da aula

Quero que guarde esta frase.

Ela será um dos pilares do Projeto Prometheus.

## 📜 Princípio XXXI

> **Os LLMs não aprendem regras para prever palavras; eles aprendem a prever palavras e, como consequência, descobrem regras.**

Essa frase resume praticamente todo o pré-treinamento.

---

# Uma provocação para você

Até hoje você estudou IA pensando:

```
Conhecimento

↓

Linguagem
```

Depois desta aula...

Quero inverter.

```
Linguagem

↓

Estrutura

↓

Regularidades

↓

Conhecimento aparente
```

Essa inversão parece pequena.

Mas muda completamente a forma de enxergar os LLMs.

---

# 📚 Leitura para a próxima aula

Quero que leia apenas a introdução do paper [[Language Models Are Few-Shot Learners]].

Não se preocupe com os experimentos ainda.

Leia apenas a motivação.

Quero que observe como o artigo descreve algo aparentemente impossível:

> Um modelo que melhora simplesmente ficando maior e vendo mais dados.

Na próxima aula, vamos responder **por que isso acontece**.

E posso adiantar: é uma das descobertas mais surpreendentes da história recente da Inteligência Artificial.