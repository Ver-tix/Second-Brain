---
tags:
  - IA
  - programação
  - inovação
---
Até agora, o fluxo era este:

```text
Usuário
    ↓
MentorAgent
    ↓
if question.startswith("calc:")
    ↓
ToolManager
```

Ou seja...

**Quem decide usar uma ferramenta é o Python.**

Mas isso tem um problema.

---

## O problema

Imagine estas perguntas:

> "Quanto é 25 × 17?"

> "Qual o resultado de 45 dividido por 9?"

> "Some 200 com 350."

Todas representam um cálculo.

Mas nenhuma começa com:

```text
calc:
```

Então nosso agente não consegue responder usando a calculadora.

A regra ficou rígida demais.

---

# Mudança de paradigma

Agora mudamos a pergunta.

Antes perguntávamos:

> "Como o Python sabe que deve chamar uma ferramenta?"

Agora perguntamos:

> "Como o modelo sabe que precisa de uma ferramenta?"

Essa é uma mudança enorme.

O LLM deixa de ser apenas um gerador de texto.

Ele passa a ser um **tomador de decisões**.

---

# O novo fluxo

Em vez de:

```text
Pergunta

↓

Python decide

↓

Ferramenta
```

Teremos:

```text
Pergunta

↓

LLM analisa

↓

"Preciso da calculadora"

↓

ToolManager

↓

Calculator

↓

Resultado

↓

LLM monta a resposta
```

Perceba como isso coincide exatamente com o raciocínio que você descreveu na conversa anterior.

---

# Como a OpenAI faz isso?

Internamente, o modelo recebe algo parecido com:

```text
Ferramenta disponível:

calculator

Descrição:

"Realiza operações matemáticas."
```

Então ele pensa:

> "O usuário pediu um cálculo."

Logo:

> "Vou usar a ferramenta calculator."

Depois devolve uma estrutura dizendo:

```text
Use calculator

Argumentos:

25 * 17
```

Quem executa continua sendo o Python.

---

# O que é Tool Calling?

Tool Calling é justamente esse mecanismo.

O modelo não executa a ferramenta.

Ele apenas responde algo como:

```text
Quero usar:

calculator

com estes parâmetros.
```

Depois disso:

```text
Python executa

↓

devolve resultado

↓

LLM responde ao usuário
```

---

# Uma analogia

Imagine um médico.

O paciente diz:

> "Estou com dor."

O médico não faz o exame.

Ele diz:

> "Peça um raio-X."

O laboratório faz o exame.

O resultado volta.

Só então o médico produz o diagnóstico.

O LLM faz exatamente isso.

---

# Mas hoje...

Hoje ainda não vamos usar o Tool Calling da API.

Vamos simulá-lo.

Por quê?

Porque quero que você enxergue a arquitetura antes da biblioteca.

---

# Laboratório 7

Vamos modificar o Prometheus-Mentor.

Em vez de:

```python
if question.startswith("calc:")
```

Vamos criar um pequeno "planejador".

Uma função parecida com:

```python
decide_tool(question)
```

Inicialmente ela poderá retornar algo como:

```text
calculator
```

ou

```text
none
```

Por enquanto ela ainda pode usar regras simples (palavras como "some", "multiplique", "divida", "quanto é"...).

O importante não é a inteligência.

É separar a decisão da execução.

---

# Nova arquitetura

Ao final da aula teremos:

```text
                main.py
                    │
                    ▼
             MentorAgent
                    │
                    ▼
           ToolDecision
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      ToolManager         LLMService
          │
          ▼
    CalculatorTool
```

Observe o nascimento de um novo componente.

---

# O objetivo oculto desta aula

Talvez você ache que estamos apenas reorganizando código.

Na verdade, estamos preparando o terreno para a próxima grande evolução.

Mais tarde, `ToolDecision` praticamente desaparecerá.

Quem assumirá seu papel será o próprio LLM.

Mas, como já aprendemos desde o início do Projeto Prometheus, primeiro entendemos **a arquitetura**, depois automatizamos.

---

# Laboratório

## Etapa 1

Criar:

```text
app/services/tool_decision.py
```

---

## Etapa 2

Criar uma classe:

```python
class ToolDecision:
```

Com um método:

```python
decide(question: str)
```

Que retorna:

- `"calculator"`
    
- `"none"`
    

---

## Etapa 3

Usar regras simples para reconhecer perguntas de matemática.

Exemplos:

- "quanto é"
    
- "some"
    
- "multiplique"
    
- "divida"
    
- "calcule"
    

---

## Etapa 4

Modificar o `MentorAgent`.

Agora ele deverá:

1. Perguntar ao `ToolDecision` se alguma ferramenta é necessária.
    
2. Se for `"calculator"`, encaminhar ao `ToolManager`.
    
3. Caso contrário, seguir para o fluxo normal do LLM.
    

---

# Desafio da aula

Ao terminar, responda:

> **Por que criamos um componente `ToolDecision` separado, em vez de colocar essa lógica diretamente dentro do `ToolManager`?**

Essa pergunta parece simples, mas ela prepara exatamente o conceito que veremos quando substituirmos esse componente pelo Tool Calling nativo dos modelos de LLM.