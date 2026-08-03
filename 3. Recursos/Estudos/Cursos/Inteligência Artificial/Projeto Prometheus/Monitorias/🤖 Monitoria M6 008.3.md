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

---

# Etapa 3 - Atualize o `ToolManager`

Perfeito. Essa etapa é pequena, mas arquiteturalmente muito importante.

Hoje, seu `ToolManager` provavelmente faz algo assim:

```python
def calculate(self, expression: str):

    parts = expression.split()

    a = float(parts[0])
    operator = parts[1]
    b = float(parts[2])

    ...
```

Perceba que ele está fazendo **parsing**.

Isso deixa de existir.

---

## Como ficará

Agora o método passa a receber diretamente os argumentos estruturados:

```python
class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()

    def calculate(
        self,
        a: float,
        b: float,
        operation: str
    ) -> float:

        return self.calculator.calculate(
            a,
            b,
            operation
        )
```

E só.

---

## O que mudou?

Antes:

```text
ToolManager

↓

Recebe string

↓

Faz split

↓

Interpreta operador

↓

Decide qual método chamar
```

Agora:

```text
ToolManager

↓

Recebe dados estruturados

↓

Delega para CalculatorTool
```

Perceba que o `ToolManager` virou realmente um **orquestrador**.

Ele não interpreta mais nada.

Ele apenas encaminha a requisição para a ferramenta correta.

Essa mudança segue exatamente o objetivo da Parte 3 do laboratório: remover o parsing de strings do `ToolManager` e deixá-lo responsável apenas por coordenar a execução das ferramentas.

Depois dessa implementação, faremos um pequeno ajuste no `MentorAgent`, porque ele ainda chama `ToolManager.calculate()` usando o formato antigo. Esse será o último passo para deixar a arquitetura consistente.

## Perfeito. Agora precisamos apenas adaptar o `MentorAgent` para a nova assinatura do `ToolManager`.

Hoje você provavelmente tem algo semelhante a isto:

```python
elif response["type"] == "tool_call":

    tool_name = response["tool"]
    tool_input = response["input"]

    if tool_name == "calculator":
        result = self.tool_manager.calculate(tool_input)

        self.memory.add_assistant_message(str(result))

        return str(result)
```

Isso usava o formato antigo:

```text
"25 + 17"
```

Agora a Aula 8.3 mudou o contrato.

O ToolManager espera:

```python
calculate(
    a,
    b,
    operation
)
```

Então o bloco passa a ficar assim:

```python
elif response["type"] == "tool_call":

    tool_name = response["tool"]

    if tool_name == "calculator":

        result = self.tool_manager.calculate(
            a=response["a"],
            b=response["b"],
            operation=response["operation"]
        )

        self.memory.add_assistant_message(str(result))

        return str(result)
```

---

## O que mudou?

Antes o `MentorAgent` passava:

```python
tool_input
```

Agora ele passa:

```python
a
b
operation
```

Isso significa que ele **não envia mais uma string**.

Ele envia dados estruturados, exatamente como o Function Schema definiu.

---

### Fluxo atualizado

Agora a arquitetura ficou assim:

```text
LLM
    │
    ▼
{
    "tool": "calculator",
    "a": 25,
    "b": 17,
    "operation": "add"
}
    │
    ▼
MentorAgent
    │
    ▼
ToolManager
    │
    ▼
CalculatorTool.calculate(
    a=25,
    b=17,
    operation="add"
)
```

Esse é exatamente o objetivo da Aula 8.3: abandonar protocolos informais (strings como `"25 + 17"`) e passar a utilizar um **contrato explícito** baseado em dados estruturados.

**Uma observação importante:** esse código ainda **não será executado de verdade**, porque o `LLMService` continua retornando apenas:

```python
{
    "type": "text",
    "content": response.output_text
}
```

Na próxima etapa do curso, quando o Tool Calling real da OpenAI for integrado, esses campos (`"a"`, `"b"` e `"operation"`) passarão a vir da resposta da API automaticamente. Hoje estamos apenas preparando a arquitetura para essa evolução.