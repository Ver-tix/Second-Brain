---
tags:
  - IA
  - programação
  - inovação
---
Bora! 😄

Este laboratório marca uma mudança importante no Prometheus.

Até agora ele era um agente que fazia apenas uma coisa:

```text
Pergunta
    ↓
LLM
    ↓
Resposta
```

Agora ele começará a fazer algo que caracteriza agentes de verdade:

```text
Pergunta
      │
      ▼
MentorAgent
      │
      ▼
ToolManager
      │
      ▼
CalculatorTool
```

Ou seja, pela primeira vez o Prometheus será capaz de **agir**, e não apenas gerar texto. Isso é um passo enorme na evolução da arquitetura.

---

# Etapa 1 — Criar os arquivos

Segundo a aula, dentro da pasta `app/tools/`, criaremos dois arquivos:

```text
app/
│
└── tools/
    ├── calculator_tool.py
    └── tool_manager.py
```

Ainda não escreveremos código no `ToolManager`.

Vamos começar pela calculadora.

# Etapa 2 — `CalculatorTool`

O arquivo `calculator_tool.py` ficará assim:

```
class CalculatorTool:

    def add(self, a: float, b: float) -> float:
        return a + b

    def subtract(self, a: float, b: float) -> float:
        return a - b

    def multiply(self, a: float, b: float) -> float:
        return a * b

    def divide(self, a: float, b: float) -> float:
        if b == 0:
            raise ValueError("Não é possível dividir por zero.")

        return a / b
```

---

# Agora vamos dissecar.

## Linha 1

```
class CalculatorTool:
```

Criamos uma **classe** chamada `CalculatorTool`.

Ela representa uma ferramenta do nosso sistema.

Hoje existe apenas uma.

Amanhã poderão existir:

- `WeatherTool`
- `SearchTool`
- `PythonTool`
- `EmailTool`

Todas seguirão a mesma ideia.

---

## Método `add`

```
def add(self, a: float, b: float) -> float:
```

Aqui temos quatro partes importantes:

```
add
```

→ nome do método.

---

```
self
```

→ significa que esse método pertence ao objeto.

Você já respondeu corretamente isso algumas aulas atrás.

---

```
a: float
```

→ espera receber um número decimal.

Exemplo:

```
10.5
```

---

```
-> float
```

→ informa que esse método devolve um número decimal.

É um **type hint**.

Não é obrigatório.

Mas melhora bastante a legibilidade.

---

## O retorno

```
return a + b
```

Simplesmente devolve a soma.

---

Os próximos dois métodos seguem exatamente a mesma lógica.

```
subtract()
```

↓

```
return a - b
```

---

```
multiply()
```

↓

```
return a * b
```

Nada de novo até aqui.

---

## O método `divide`

Aqui aparece a primeira decisão de engenharia.

```
if b == 0:
```

Estamos perguntando:

> "O divisor é zero?"

---

Se for:

```
raise ValueError(...)
```

Lançamos uma exceção.

Por quê?

Porque matematicamente essa operação não existe.

Em vez de deixar o Python produzir um erro confuso mais adiante, nós informamos claramente o problema.

Essa prática melhora muito a qualidade do software.

# Etapa 3 Criar o `ToolManager`.
# `tool_manager.py`

Primeiro o código inteiro:

```
from app.tools.calculator_tool import CalculatorTool


class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()
```

Só isso.

Sim.

É pequeno mesmo.

O professor ainda não pediu mais nada nesta etapa.

---

# Agora vamos entender linha por linha

## Linha 1

```
from app.tools.calculator_tool import CalculatorTool
```

Importamos a ferramenta que acabamos de criar.

Sem isso, o Python não sabe que ela existe.

---

## Classe

```
class ToolManager:
```

Criamos o gerenciador.

Ele será o único lugar do projeto que conhecerá todas as ferramentas.

Hoje:

```
CalculatorTool
```

Amanhã:

```
CalculatorTool
WeatherTool
SearchTool
PythonTool
...
```

---

## Construtor

```
def __init__(self):
```

Quando fizermos:

```
manager = ToolManager()
```

esse método será executado automaticamente.

---

## A parte mais importante

```
self.calculator = CalculatorTool()
```

Aqui estamos dizendo:

> "Todo ToolManager possui uma calculadora."

Visualmente:

```
ToolManager
     │
     ▼
CalculatorTool
```

Mais tarde ficará parecido com isto:

```
self.calculator = CalculatorTool()
self.search = SearchTool()
self.weather = WeatherTool()
self.python = PythonTool()
```

Percebe a ideia?

O ToolManager vai **concentrando** as ferramentas.

---

# Uma observação importante

Você deve estar pensando:

> "Professor... só isso?"

Sim.

Porque estamos construindo a arquitetura em camadas.

Nesta etapa, o `ToolManager` ainda é praticamente um **container** de ferramentas.

Nas próximas aulas, ele provavelmente ganhará métodos como:

```
execute(...)
```

ou

```
call_tool(...)
```

e passará a decidir qual ferramenta usar.

Hoje ele apenas **organiza**. Depois ele **orquestrará**.

# Etapa 4 — Alterando o `MentorAgent`

Primeiro, vamos alterar o construtor.

## 1. Novo import

Adicione:

```
from app.tools.tool_manager import ToolManager
```

---

## 2. Altere o `__init__`

Hoje ele deve estar parecido com isto:

```
def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory
):
```

Ele passará a ser:

```
def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager
):
```

E dentro do construtor:

```
self.tool_manager = tool_manager
```

Assim como fizemos com `LLMService` e `ConversationMemory`, **o ToolManager também será injetado**. Estamos mantendo a arquitetura consistente.

---

## Antes de continuar...

Quero fazer uma observação importante.

Na aula, o professor pede que o agente reconheça comandos como:

```
calc: 25 + 13
```

Mas **ele não explica como fazer esse reconhecimento**.

Em vez de eu simplesmente te entregar a solução, quero construir esse raciocínio com você.

### Pergunta

Se você recebe a variável:

```
question
```

contendo:

```
calc: 25 + 13
```

Como você acha que o Python poderia verificar se a pergunta **começa** com `"calc:"`?

Não precisa acertar o nome do método. Quero apenas sua ideia. Depois escrevemos o código juntos. É justamente essa etapa que faz o agente começar a "tomar decisões" antes de chamar o LLM.

# Primeiro: como o Python verifica se um texto começa com outro?

Existe um método das strings chamado:

```
startswith()
```

Exemplo:

```
texto = "calc: 25 + 13"

texto.startswith("calc:")
```

Resultado:

```
True
```

Agora:

```
texto = "O que é um Transformer?"

texto.startswith("calc:")
```

Resultado:

```
False
```

É exatamente esse método que usaremos.

---

# Agora vamos alterar o método `ask()`

O início dele ficará assim:

```
def ask(self, question: str) -> str:

    if question.startswith("calc:"):

        expression = question.replace("calc:", "").strip()

        return self.tool_manager.calculate(expression)
```

Vamos entender.

---

## Linha 1

```
if question.startswith("calc:"):
```

Pergunta ao Python:

> "A pergunta começa com `calc:`?"

Se sim, entramos nesse bloco.

---

## Linha 2

```
expression = question.replace("calc:", "").strip()
```

Imagine que o usuário digitou:

```
calc: 25 + 13
```

Primeiro:

```
question.replace("calc:", "")
```

vira:

```
 25 + 13
```

Ainda sobrou um espaço.

Por isso usamos:

```
.strip()
```

Resultado:

```
25 + 13
```

Agora temos apenas a expressão matemática.

---

## Linha 3

```
return self.tool_manager.calculate(expression)
```

Aqui está a grande novidade.

O agente diz:

> "ToolManager, resolva isso para mim."

Ele não faz conta.

Ele delega.

É exatamente o mesmo padrão que já vimos:

```
MentorAgent

↓

LLMService
```

Agora temos outro:

```
MentorAgent

↓

ToolManager
```

---

# Mas... tem um detalhe 😏

Você provavelmente percebeu uma coisa.

Nós escrevemos:

```
self.tool_manager.calculate(...)
```

Mas...

**esse método ainda não existe!**

No momento, o `ToolManager` só tem:

```
__init__()
```

Então o próximo passo será justamente implementar:

```
def calculate(self, expression: str):
```

É nele que aprenderemos a interpretar `"25 + 13"` e chamar:

```
CalculatorTool.add(...)
```

Essa é, na minha opinião, a parte mais divertida do laboratório.

# Continuando...

Perfeito! Agora chegamos na parte que faz o `ToolManager` realmente ser útil.

Até agora ele só guarda a calculadora:

```
class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()
```

Agora vamos ensinar o gerente a **usar** a ferramenta.

---

# Código completo

Acrescente este método à classe `ToolManager`:

```
def calculate(self, expression: str) -> float:

    parts = expression.split()

    a = float(parts[0])
    operator = parts[1]
    b = float(parts[2])

    if operator == "+":
        return self.calculator.add(a, b)

    elif operator == "-":
        return self.calculator.subtract(a, b)

    elif operator == "*":
        return self.calculator.multiply(a, b)

    elif operator == "/":
        return self.calculator.divide(a, b)

    else:
        raise ValueError("Operador inválido.")
```

---

# Agora vamos entender linha por linha

## Recebendo a expressão

```
def calculate(self, expression: str) -> float:
```

O método recebe uma string.

Exemplo:

```
25 + 13
```

Observe que o `calc:` já foi removido pelo `MentorAgent`.

---

## Separando a expressão

```
parts = expression.split()
```

Esse é um método muito útil do Python.

Imagine:

```
expression = "25 + 13"
```

Depois do `.split()`:

```
parts
```

vira:

```
["25", "+", "13"]
```

Ou seja, ele quebra a frase pelos espaços.

---

## Primeiro número

```
a = float(parts[0])
```

`parts[0]` é:

```
25
```

Transformamos em número decimal.

---

## Operador

```
operator = parts[1]
```

Agora:

```
+
```

---

## Segundo número

```
b = float(parts[2])
```

Agora temos:

```
13
```

---

## Escolhendo a operação

Depois fazemos:

```
if operator == "+":
```

Se for soma:

```
return self.calculator.add(a, b)
```

Se for:

```
-
```

Chamamos:

```
subtract()
```

E assim sucessivamente.

---

# Percebe a arquitetura?

Quem faz a conta?

❌ ToolManager

Quem faz a conta é:

```
CalculatorTool
```

O ToolManager apenas interpreta:

> "Ah... o operador é '+'. Então vou pedir para a calculadora somar."

Ele continua sendo um **gerente**, não um especialista.

---

## Uma observação de arquiteto

Você talvez esteja pensando:

> "Professor... isso parece um mini compilador."

E... você está certo. 😄

Veja o que ele faz:

Entrada:

```
25 + 13
```

↓

Quebra em tokens:

```
25

+

13
```

↓

Interpreta

↓

Executa

↓

Retorna

É exatamente a lógica de um interpretador muito simples. Mais para frente, quando estudarmos agentes mais sofisticados, você verá que muitos deles seguem essa mesma ideia: **receber texto, interpretar intenções e despachar ações**.

---

Implemente esse método e me avise. Depois faremos a **última alteração**, integrando tudo ao `main.py` e executando o laboratório (mesmo que a chamada à OpenAI continue falhando por falta da API key, conseguiremos validar todo o fluxo da calculadora).

# Finalizando

Até agora construímos as peças:

- ✅ `CalculatorTool`
- ✅ `ToolManager`
- ✅ Parte do `MentorAgent`

Agora precisamos conectar essas peças.

---

# Passo 1 — Atualizar o construtor do `MentorAgent`

Ele deve receber o `ToolManager`.

Ficará assim:

```
def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager
):
    self.llm_service = llm_service
    self.memory = memory
    self.tool_manager = tool_manager
```

Se você já fez isso, ótimo.

---

# Passo 2 — Atualizar o `main.py`

Agora o `main.py` precisa criar o `ToolManager`, assim como já cria o `LLMService` e a `ConversationMemory`.

Acrescente o import:

```
from app.tools.tool_manager import ToolManager
```

Depois, dentro do `main()`:

```
llm_service = LLMService()

memory = ConversationMemory()

tool_manager = ToolManager()

mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager
)
```

Observe como a arquitetura continua consistente:

```
main.py
    │
    ├── cria LLMService
    ├── cria ConversationMemory
    ├── cria ToolManager
    ▼
MentorAgent
```

O `main.py` continua sendo o "compositor" da aplicação: ele cria os componentes e os conecta.

---

# Passo 3 — Revisar o método `ask()`

No início do método, ele deve ter algo parecido com:

```
if question.startswith("calc:"):

    expression = question.replace("calc:", "").strip()

    return self.tool_manager.calculate(expression)
```

Depois desse bloco, permanece todo o fluxo normal:

```
self.memory.add_user_message(question)

history = self.memory.get_history()

prompt = PromptBuilder.build(history, question)

response = self.llm_service.generate(prompt)

self.memory.add_assistant_message(response)

return response
```

Assim, existem **dois caminhos**:

```
Pergunta começa com "calc:"
            │
           Sim
            ▼
      ToolManager
            ▼
     CalculatorTool
```

ou

```
Pergunta comum
        │
        ▼
PromptBuilder
        ▼
LLMService
        ▼
OpenAI
```

---

## Uma observação arquitetural importante

Quero destacar algo que considero um marco no seu aprendizado.

Lá no início do Projeto Prometheus, você perguntava:

> "Onde eu coloco esse código?"

Hoje, naturalmente, você está fazendo perguntas como:

> "Por que existe um ToolManager se o MentorAgent poderia chamar a CalculatorTool?"

Essa mudança é enorme.

Você deixou de pensar apenas em **escrever código** e começou a pensar em **como organizar sistemas**. Essa é justamente a transição de um programador iniciante para alguém que começa a desenvolver visão de arquitetura.

---

### Sua missão agora

Atualize o `main.py` e confira se o `MentorAgent` possui os três ajustes:

- recebe `tool_manager` no construtor;
- salva `self.tool_manager`;
- trata perguntas iniciadas por `calc:` antes de seguir o fluxo do LLM.

Depois me envie os códigos do `main.py` e do `MentorAgent`. Vou fazer uma revisão completa, como um code review de Pull Request, e então geraremos o CHANGELOG do Laboratório 6.