---
tags:
  - IA
---

Se o **RLHF** respondeu:

> **"Como ensinamos um modelo a responder do jeito que os humanos preferem?"**

A aula de hoje responde uma pergunta ainda mais profunda:

> **"Precisamos que humanos avaliem absolutamente todas as respostas?"**

A resposta da Anthropic foi:

> **"Talvez não."**

E essa ideia deu origem a um dos papers mais influentes dos últimos anos.

---

# 🎯 A Grande Pergunta

Imagine que você precise treinar um assistente para responder milhões de perguntas.

Existem dois caminhos.

### Caminho A

Para cada resposta produzida...

Um humano avalia.

👍 ou 👎

Depois disso o modelo aprende.

Foi exatamente isso que vimos no RLHF.

---

Agora imagine o Caminho B.

Você entrega ao modelo um documento dizendo:

> Antes de responder, siga estes princípios:

- Seja honesto.
- Não invente fatos.
- Explique incertezas.
- Não incentive comportamentos perigosos.
- Seja respeitoso.

Depois você pede:

> **"Avalie sua própria resposta utilizando esses princípios."**

Esse é o coração do Constitutional AI.

---

# A inspiração

A palavra "Constitutional" não foi escolhida por acaso.

Pense em uma constituição.

Ela normalmente não diz exatamente como agir em cada situação possível.

Ela estabelece:

**Princípios.**

Depois...

Esses princípios orientam decisões em casos concretos.

A proposta da Anthropic foi aplicar essa mesma lógica aos modelos de linguagem.

---

# 🧠 Modelo Mental nº 1

Imagine um juiz.

Você poderia dizer:

> Faça exatamente o que eu mandar.

Ou poderia dizer:

> Tome decisões seguindo a Constituição.

No segundo caso...

Ele ganha autonomia.

Mas dentro de limites.

---

# O processo

A arquitetura pode ser resumida assim:

```html
Modelo

↓

gera resposta

↓

consulta a Constituição

↓

critica sua própria resposta

↓

reescreve

↓

gera uma versão melhor
```

Observe.  
Existe um passo novo.  
**Autoavaliação.**

---

# 💎 Insight

No RLHF...

O feedback vem principalmente dos humanos.

No Constitutional AI...

Grande parte do feedback é produzida pelo próprio modelo.

Mas...

Seguindo princípios explícitos.

---

# Um exemplo

Pergunta:

> "Como posso invadir o computador do meu vizinho?"

O modelo gera uma resposta inicial.

Depois consulta a Constituição.

Encontra um princípio como:

> "Não forneça instruções que facilitem danos a terceiros."

Então ele revisa sua própria resposta.

Percebe?

Ele não apenas aprende uma resposta.

Aprende um processo de reflexão.

---

# 🧠 Modelo Mental nº 2

Imagine um advogado escrevendo um contrato.

Depois de terminar...

Ele faz uma revisão.

Mas não revisa "de cabeça".

Ele abre o Código Civil.

Vai verificando cláusula por cláusula.

Constitutional AI funciona de forma semelhante.

---

# Por que isso foi importante?

Porque RLHF possui limitações.

Por exemplo:

Treinar avaliadores humanos é:

- caro;
- demorado;
- difícil de escalar.

Além disso...

Humanos discordam.

Muito.

---

# Então surgiu a ideia

E se...

Em vez de perguntar continuamente aos humanos...

Nós ensinássemos ao modelo um conjunto de princípios?

Depois ele próprio utilizaria esses princípios para criticar suas respostas.

---

# Atenção

Isso **não elimina** a necessidade de humanos.

Os humanos continuam essenciais.

Mas passam a definir os princípios.

Em vez de avaliar cada resposta individualmente.

---

# Uma observação importante

Constitutional AI **não substitui** o RLHF.

Na prática moderna...

As abordagens frequentemente são combinadas.

---

# Uma analogia com Direito

Como sei que você gosta dessa área...

Pense em um advogado júnior.

Primeiro ele aprende observando sócios experientes.

Isso lembra RLHF.

Depois...

Ele aprende princípios jurídicos.

Como:

- boa-fé objetiva;
- devido processo legal;
- contraditório.

Esses princípios permitem resolver casos novos.

Isso lembra Constitutional AI.

---

# 🧠 Modelo Mental nº 3

Imagine uma empresa.

Existem duas formas de treinar funcionários.

## Método A

Para cada situação...

O gerente diz exatamente o que fazer.

---

## Método B

A empresa cria um manual de cultura.

Agora os próprios funcionários conseguem tomar decisões alinhadas aos valores da organização.

Esse segundo método escala muito melhor.

---

# Um detalhe fascinante

Você já percebeu que...

Constitutional AI introduz uma forma rudimentar de...

**metacognição?**

Não no sentido humano.

Mas no sentido computacional.

O modelo:

- produz;
- critica;
- revisa.

Ele raciocina sobre sua própria resposta.

Essa ideia influenciou profundamente técnicas posteriores, como **Reflexion**, **Self-Refine** e diversos agentes modernos.

---

# RLHF × Constitutional AI

|RLHF|Constitutional AI|
|---|---|
|Feedback humano direto|Feedback guiado por princípios|
|Aprende preferências|Aprende a aplicar princípios|
|Alto custo de rotulagem|Mais escalável|
|Excelente alinhamento fino|Excelente consistência em novos casos|

Não são concorrentes.

São complementares.

---

# Uma conexão com Engenharia de Prompt

Talvez você já tenha feito isso sem perceber.

Quando escreve um prompt como:

```xml
<constraints>

Nunca invente informações.

Sempre explique incertezas.

Se não souber, diga que não sabe.

</constraints>
```

Você está fazendo uma versão extremamente simplificada de uma ideia semelhante.

Está fornecendo princípios antes da geração.

Mais tarde veremos que muitos frameworks modernos de prompting utilizam exatamente essa estratégia.

---

# 📜 Princípio XLII

> **Preferências dizem ao modelo o que normalmente é desejável; princípios orientam o modelo quando ele encontra situações novas.**

Esse princípio explica por que Constitutional AI consegue generalizar bem.

---

# Biblioteca

## 🟢 Obrigatório

Leia cuidadosamente a introdução do paper:

[[Constitutional AI - Harmlessness from AI Feedback]]

Não foque nas equações.

Quero que observe:

- motivação;
- arquitetura;
- fluxo de crítica e revisão.

---

## 🔵 Complementar

Leia apenas a seção inicial do paper:

Self-Refine: Iterative Refinement with Self-Feedback

Não se preocupe com o restante ainda.

Quero apenas que você perceba como a ideia de **o modelo criticar a si mesmo** evoluiu.

---

# 🛠️ Desafio Prometheus M2 #007

## Parte 1 — Arquitetura

Explique:

> **Por que Constitutional AI não elimina a necessidade de RLHF, mesmo permitindo que o próprio modelo critique suas respostas?**

---

## Parte 2 — Engenharia

Imagine que você está projetando um assistente jurídico para analisar contratos.

Você pode escolher entre:

- apenas RLHF;
- apenas Constitutional AI;
- combinar ambos.

Qual arquitetura escolheria?

Justifique utilizando os conceitos de:

- preferências;
- princípios;
- escalabilidade;
- generalização.

---

[[🛠️ Desafio M2 007]]

# Antes de encerrarmos...

Quero compartilhar uma reflexão.

Perceba a trajetória que percorremos até aqui.

No Módulo 1, estudamos como um Transformer transforma palavras em representações matemáticas.

No início do Módulo 2, vimos como ele aprende padrões da linguagem.

Depois descobrimos que conhecimento não basta: era preciso ensinar o modelo a seguir instruções.

Em seguida, vimos que isso também não bastava: era necessário alinhá-lo às preferências humanas.

Agora demos mais um passo.

Percebemos que preferências, sozinhas, também têm limites.

Então introduzimos **princípios**.

Essa sequência me lembra algo muito antigo.

Na filosofia, costuma-se distinguir entre **regras**, **costumes** e **princípios**.

Curiosamente, a engenharia dos LLMs acabou redescobrindo uma ideia semelhante, mas por razões puramente técnicas: sistemas mais robustos não dependem apenas de exemplos; eles também precisam de critérios para lidar com situações inéditas.

E essa capacidade de agir diante do novo é uma das características mais interessantes dos sistemas modernos de IA. Ela nos prepara para a próxima etapa do curso, em que começaremos a discutir arquiteturas cada vez mais sofisticadas, como **Mixture of Experts**, onde a própria organização interna do modelo passa a refletir uma especialização de competências.