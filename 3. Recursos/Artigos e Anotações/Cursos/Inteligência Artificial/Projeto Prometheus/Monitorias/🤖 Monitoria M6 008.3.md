---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
# Etapa 1 - Atualizar o schema da ferramenta em `llm_service`

Até agora sua ferramenta está registrada assim:

```Python
tools=[
    {
        "type": "function",
        "name": "calculator",
        "description": "Realiza operações matemáticas básicas entre dois números."
    }
]
```

Agora ela passa a ter um **contrato explícito**.

Substitua por:

```Python
tools=[
    {
        "type": "function",
        "name": "calculator",
        "description": "Realiza operações matemáticas básicas.",

        "parameters": {
            "type": "object",

            "properties": {

                "a": {
                    "type": "number",
                    "description": "Primeiro número."
                },

                "b": {
                    "type": "number",
                    "description": "Segundo número."
                },

                "operation": {
                    "type": "string",
                    "enum": [
                        "add",
                        "subtract",
                        "multiply",
                        "divide"
                    ],
                    "description": "Operação matemática."
                }
            },

            "required": [
                "a",
                "b",
                "operation"
            ]
        }
    }
]
```

---

## O que acabou de acontecer?

Até a Aula 8.2, o LLM sabia apenas:

```
Existe uma ferramenta chamada calculator.
```

Agora ele sabe também:

- quais argumentos ela recebe;
- quais tipos esses argumentos possuem;
- quais são obrigatórios;
- quais operações são válidas.

**É exatamente isso que o professor chama de Function Schema.**

---

### Pare aqui.

Implemente **somente essa etapa** no `LLMService` e me envie o arquivo (ou diga "feito"). Depois seguimos para a refatoração do `CalculatorTool`, que será bem mais elegante do que a implementação atual.

---

# Etapa 2 - Refatorar o `CalculatorTool`

Essa é, para mim, a parte mais bonita da aula.

Hoje sua calculadora possui quatro métodos independentes:

```Python
add()
subtract()
multiply()
divide()
```

Eles permanecem.

**O que vamos adicionar é um quinto método**, chamado `calculate()`.

Ele será a **porta de entrada única** da ferramenta.

Ficará assim:

```Python
class CalculatorTool:

    def calculate(
        self,
        a: float,
        b: float,
        operation: str
    ) -> float:

        if operation == "add":
            return self.add(a, b)

        elif operation == "subtract":
            return self.subtract(a, b)

        elif operation == "multiply":
            return self.multiply(a, b)

        elif operation == "divide":
            return self.divide(a, b)

        else:
            raise ValueError("Operação inválida.")
```

Depois dele, permanecem exatamente como já estão:

```python
def add(...)
def subtract(...)
def multiply(...)
def divide(...)
```

---

## O que mudou arquiteturalmente?

Antes:

```
ToolManager

↓

if +

↓

add()

↓

if -

↓

subtract()
```

Agora:

```
ToolManager

↓

CalculatorTool.calculate()

↓

add()
subtract()
multiply()
divide()
```

Percebe a diferença?

A **própria ferramenta** passa a conhecer suas operações.

O `ToolManager` deixa de conhecer detalhes internos da calculadora.

Essa mudança é exatamente o que o professor descreveu:

> _"A responsabilidade de interpretar os argumentos passa a ser da ferramenta, não mais do chamador."_

---

Implemente **apenas esse novo método `calculate()`**, sem alterar os outros quatro métodos. Depois me envie o `CalculatorTool` e seguimos para a refatoração do `ToolManager`.