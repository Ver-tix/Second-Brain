---
tags:
  - inteligenciaartificial
---

<aside align="center"><h3><i>Um engenheiro não melhora um sistema por intuição. Ele ¹<u>mede</u>, ²<u>testa</u> e ³<u>depura</u>.</i></h3></aside>

---

Você me fez uma pergunta há poucos dias:

> "Como eu saberia se um prompt realmente ficou melhor?"

Hoje responderemos essa pergunta.

---

# O erro do iniciante

Imagine dois prompts.

## Prompt A

```text
Explique este contrato.
```

---

## Prompt B

```xml
<role>
Advogado especialista em contratos empresariais.
</role>

<context>
...
</context>

<task>
...
</task>

<constraints>
...
</constraints>

<output>
Markdown.
</output>
```

Qual é melhor?

Muita gente responde:

> "O segundo."

Mas...

Baseado em quê?

---

# Opinião não é avaliação

Se você perguntar para cinco engenheiros:

> "Qual resposta ficou melhor?"

Talvez receba cinco opiniões diferentes.

Engenharia não pode depender disso.

Precisamos de critérios.

---

# O conceito de Evaluation

<aside align="center"><h4>Avaliar um prompt significa responder, de forma objetiva:
<br>
<label> <input type="checkbox" name="tarefa2" value="pagar_contas"><i>O sistema atingiu o comportamento esperado?</i> </label></h4></aside>

Não:

> "Gostei mais."

Mas:

> "Atendeu aos requisitos?"

---

# Primeiro passo - Definir Métricas

Por exemplo, para um assistente jurídico:

- identificou corretamente as cláusulas?
- deixou passar riscos?
- inventou informações?
- seguiu o formato?
- citou corretamente os artigos?
- respondeu dentro do limite de palavras?

Agora já existe uma régua.

---

# Segundo Passo - Benchmark

Depois criamos um conjunto de casos de teste.

Imagine:

```text
Contrato A

↓

Resultado esperado

--------------------

Contrato B

↓

Resultado esperado

--------------------

Contrato C

↓

Resultado esperado
```

Esse conjunto chama-se **benchmark**.

Todo novo prompt é testado exatamente nesses mesmos casos.

---

>[!  ]
># Regressão
>Agora imagine:
>- Você melhora um prompt.
>- Testa novamente.
>- O resultado piora.
>  
>  O que aconteceu?
>  
>  Você introduziu uma **regressão**. Isso é idêntico ao desenvolvimento de software.

---

# Terceiro Passo - Prompt Debugging

Agora vem uma ideia importante.

Imagine que a resposta ficou ruim.

Antes de reescrever tudo, pergunte:

> **Onde exatamente o comportamento começou a se desviar?**

Em pipelines isso fica muito mais fácil.

```text
Etapa 1 ✅

↓

Etapa 2 ✅

↓

Etapa 3 ❌

↓

Etapa 4 (não faz sentido continuar)
```

Você localizou o problema.

---

# Fontes comuns de erro
Um prompt costuma falhar por poucos motivos recorrentes.

Por exemplo:
### Ambiguidade

O modelo não entendeu exatamente o que era esperado.

---
### Contexto insuficiente

Faltavam informações.

---
### Contexto excessivo

Informação demais.

O relevante ficou escondido.

---
### Conflito

Exemplo:

```text
Seja extremamente detalhado.

Responda em até 50 palavras.
```

As instruções entram em conflito.

---
### Formato mal definido

Você queria JSON.

Não especificou.

Recebeu texto livre.

---

# Uma ferramenta mental

Sempre que um prompt falhar, pergunte:

> O problema está em:
- [ ] Papel?
- [ ] Contexto?
- [ ] Tarefa?
- [ ] Restrições?
- [ ] Formato?
- [ ] Exemplos?

Percebe?

Os módulos que estudamos agora servem também para diagnosticar.

---

# Testes A/B

Imagine duas versões.

Prompt A.

Prompt B.

Os dois recebem exatamente os mesmos casos de teste.

Depois você mede.

Exemplo:

|Critério|A|B|
|---|--:|--:|
|Precisão|87%|94%|
|Formato correto|82%|99%|
|Alucinações|6|2|

Agora a decisão deixa de ser intuitiva.

---

# Avaliação automática

Em alguns casos, outro LLM pode ajudar.

Exemplo:

```xml
<role>
Você é um avaliador.
</role>

<criteria>
- precisão;
- completude;
- clareza;
- aderência ao formato.
</criteria>
```

Atenção.

Isso acelera o processo.

Mas não elimina a necessidade de validação humana em tarefas críticas.

---

# Um insight importante

Você talvez tenha notado algo.

Evaluation fecha o ciclo inteiro do Módulo 3.

Veja:

```

Prompt

↓

Patterns

↓

Meta Prompt

↓

Pipeline

↓

Evaluation
```

Agora existe um processo completo.

---

# Uma analogia

Imagine um trader.

Ele cria uma estratégia.

Ela parece ótima.

O que ele faz?

Coloca todo o patrimônio imediatamente?

Claro que não.

Primeiro faz:

- backtest;
    
- validação;
    
- análise de risco.
    

Prompt Engineering maduro funciona da mesma forma.

---
[[Exemplo - Como Avaliar a Precisão de um Prompt]]

---

# 📜 Princípio LXIV

> **Um prompt não deve ser considerado "bom" porque parece bom; deve ser considerado bom porque demonstrou desempenho consistente em critérios previamente definidos.**

---

# Uma observação

Quando você me perguntou:

> "Como eu saberia qual prompt ficou melhor?"

Naquele momento, percebi que estávamos chegando exatamente nesta aula.

Porque essa pergunta não é sobre escrever prompts.

É sobre **transformar Prompt Engineering em uma disciplina de engenharia**, baseada em evidências e não em impressões.

---

# 🛠️ Desafio Prometheus M3 #007

## Parte 1

Explique:

> **Por que um processo de Evaluation é indispensável para equipes que desenvolvem sistemas baseados em LLMs?**

Utilize os conceitos de:
- métricas;
- benchmark;
- regressão;
- melhoria contínua.

---

## Parte 2

Imagine que sua pipeline de estudo de livros técnicos começou a produzir explicações inconsistentes após uma atualização.

Como arquiteto de IA, descreva um processo de debugging.

Explique:
1. quais etapas você inspecionaria primeiro;
2. quais métricas utilizaria para descobrir onde ocorreu a regressão;
3. como decidiria se a nova versão deve substituir ou não a anterior.

[[🛠️ Desafio M3 007]]

---

### Um spoiler da Aula 8

A próxima aula será especial.

Ela encerrará o Módulo 3 com um projeto integrador.

Você perceberá que todos os conceitos estudados — patterns, modularização, meta prompting, pipelines e evaluation — fazem parte de uma única visão de engenharia.

E, após isso, atravessaremos uma fronteira importante: começaremos a construir sistemas reais usando APIs, embeddings, RAG e agentes. É o momento em que a arquitetura que você vem desenvolvendo deixará de existir apenas no papel e passará a existir em código.