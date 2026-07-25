---
tags:
  - inteligenciaartificial
---

# 🎯 A Grande Pergunta

Imagine dois engenheiros.

O primeiro diz:

> "Nosso modelo não está bom. Vamos inventar uma arquitetura completamente nova."

O segundo responde:

> "E se simplesmente treinássemos um modelo muito maior?"

Durante muitos anos, a comunidade acreditava que o primeiro engenheiro estava certo.

Em 2020, um paper mudou essa visão.

---

# O contexto histórico

Antes de 2020, o pensamento dominante era:

> "Depois de certo tamanho, aumentar um modelo gera retornos cada vez menores."

Em outras palavras:

Mais parâmetros → mais custo → pouco ganho.

Parecia intuitivo.

Mas ninguém havia testado isso em larga escala.

Foi então que pesquisadores da OpenAI publicaram o paper Scaling Laws for Neural Language Models.

A pergunta era simples:

> **O que acontece se aumentarmos sistematicamente o tamanho do modelo, a quantidade de dados e o poder computacional?**

---

# O experimento

Os pesquisadores treinaram dezenas de modelos.

Mudando apenas três variáveis:

- número de parâmetros;
- quantidade de dados;
- capacidade computacional utilizada.

Nada de arquiteturas revolucionárias.

Nada de novos mecanismos.

Apenas escala.

---

# O resultado

A expectativa era encontrar uma curva como esta:

```
Qualidade
│
│        _________
│      /
│    /
│__/
└──────────────────
      Tamanho
```

Ou seja.

Após certo ponto...

Os ganhos praticamente desapareceriam.

Mas...

Foi isso que encontraram:

```
Qualidade
│
│          /
│        /
│      /
│    /
│__/
└──────────────────
      Tamanho
```

A melhoria continuava.

De forma extremamente previsível.

---

# 🧠 Modelo Mental nº 1

Imagine uma biblioteca.

Você lê:

- 10 livros;
- depois 100;
- depois 1.000;
- depois 100.000.

Em qual momento você "para" de aprender?

Provavelmente nunca.

Os ganhos diminuem.

Mas continuam existindo.

Foi exatamente isso que observaram nos modelos.

---

# O conceito de Lei de Escala

A descoberta foi que o erro do modelo seguia aproximadamente uma **lei de potência (power law)**.

Simplificando.

Quando aumentamos recursos de forma equilibrada:

- mais parâmetros;
- mais dados;
- mais computação;

o erro diminui de maneira previsível.

Isso permitiu algo inédito.

**Planejar modelos futuros antes mesmo de treiná-los.**

---

# 💎 Insight

Até então, treinar um modelo gigantesco era quase um ato de fé.

Depois das Scaling Laws...

Passou a ser engenharia.

---

# Mas atenção.

Existe uma armadilha.

Imagine dois cenários.

## Cenário A

100 bilhões de parâmetros.

Mas apenas 1 milhão de textos.

## Cenário B

100 milhões de parâmetros.

Mas 100 bilhões de tokens.

Os dois são ruins.

Por quê?

Porque existe um equilíbrio.

---

# 🧠 Modelo Mental nº 2

Imagine abrir um restaurante.

Você possui:

- 200 cozinheiros.

Mas apenas:

- 5 clientes.

Desperdício.

Agora o contrário.

5 cozinheiros.

200 mil clientes.

Caos.

O mesmo vale para LLMs.

Modelo, dados e computação precisam crescer juntos.

---

# A tríade da escala

Os pesquisadores identificaram três pilares.

```
Parâmetros

↓

Capacidade do modelo
```

```
Dados

↓

Experiência do modelo
```

```
Computação

↓

Tempo disponível para aprender
```

Nenhum substitui os outros.

---

# Uma consequência inesperada

Agora vem a parte fascinante.

Quando os modelos cresceram...

Eles começaram a fazer coisas que ninguém havia programado.

Por exemplo:

- tradução;
- raciocínio simples;
- resumo;
- programação;
- respostas em poucos exemplos (_few-shot learning_).

Essas habilidades não apareceram gradualmente.

Em muitos casos...

Pareciam surgir de repente.

---

# 🧠 Modelo Mental nº 3

Imagine aquecer água.

Você mede a temperatura.

90 °C.

95 °C.

99 °C.

Nada muito diferente.

Então...

100 °C.

A água muda completamente de estado.

Não porque você alterou a física.

Mas porque ultrapassou um limiar.

Algumas capacidades dos LLMs parecem surgir da mesma forma.

Chamamos isso de **capacidades emergentes**.

Na próxima aula estudaremos esse fenômeno em profundidade.

---

# Uma observação importante

As Scaling Laws **não dizem** que "quanto maior, melhor" de forma infinita.

Elas dizem algo mais sutil:

> **Dentro de uma determinada faixa e mantendo o equilíbrio entre recursos, aumentar a escala tende a produzir melhorias previsíveis.**

Esse detalhe é essencial.

---

# 📜 Princípio XXXII

> **O desempenho de um LLM depende menos de um único fator isolado e mais do equilíbrio entre capacidade, experiência e computação.**

---

# Por que isso mudou a indústria?

Antes de 2020, muitas empresas apostavam em arquiteturas radicalmente novas.

Depois desse paper, a estratégia mudou.

A pergunta passou a ser:

> **"E se treinarmos um modelo maior, com mais dados e mais computação?"**

Essa mudança influenciou diretamente o desenvolvimento de modelos como o GPT-3 e inspirou grande parte da corrida por modelos cada vez mais capazes.

---

# 📚 Biblioteca

### 🟢 Obrigatório

Leia:

- a Introdução do paper Scaling Laws for Neural Language Models.

Não precisa estudar as equações ainda.

Quero apenas entender a motivação e as conclusões gerais.

---

### 🔵 Complementar

Leia também a introdução do paper [[Language Models Are Few-Shot Learners]]

Observe como as ideias das Scaling Laws influenciam diretamente a construção do GPT-3.

---

# 🛠️ Desafio Prometheus M2 #002

## Versão técnica

Responda:

> **Por que simplesmente aumentar o número de parâmetros de um modelo não garante melhores resultados? Explique utilizando o conceito de equilíbrio entre parâmetros, dados e computação.**

[[À FAZER] 🛠️ Desafios Prometheus #002](https://app.notion.com/p/FAZER-Desafios-Prometheus-002-391442f8efac80b28d27e6fddf27f88e?pvs=21)

---

## Versão de engenharia

Imagine que um CEO lhe diga:

> "Compre mais GPUs que nossa IA ficará inteligente."

Explique, em até cinco frases, por que essa decisão isolada pode desperdiçar milhões de dólares.

---

## Antes de encerrarmos...

Quero deixar uma provocação.

Até hoje falamos sobre "inteligência" como se fosse uma propriedade fixa.

As Scaling Laws sugerem outra visão.

Talvez a inteligência de um modelo não seja apenas consequência da arquitetura.

Talvez ela também seja consequência da **escala**.

Essa hipótese ainda gera debates intensos na comunidade científica.

E é justamente por isso que a próxima aula será uma das mais fascinantes do módulo: vamos investigar **capacidades emergentes** — comportamentos que parecem aparecer sem terem sido explicitamente ensinados.

Tenho a impressão de que essa discussão vai conversar muito com sua curiosidade sobre sistemas complexos, economia e teoria da inovação. É um daqueles temas em que várias áreas do conhecimento começam a se encontrar.