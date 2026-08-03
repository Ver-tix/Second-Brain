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
# Etapa 1:
Criando o tool_registry

```
app/
└── tools/
    └── tool_registry.py
```

Agora vamos implementar a versão mais simples possível, exatamente como o professor descreveu.

```Python
from app.tools.calculator_tool import CalculatorTool

class ToolRegistry:
    def __init__(self):
        self.tools = {} 
        self.register(
            "calculator",
            CalculatorTool()
        )

    def register(self, name: str, tool):
        self.tools[name] = tool

    def get(self, name: str):
        return self.tools.get(name)
```

# Etapa 2 - `ToolManager` deixará de instanciar a `CalculatorTool` e passará a utilizar o `ToolRegistry`.

Hoje ele está assim (ou muito próximo disso):

```Python
class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()
```

Essa linha precisa desaparecer. Em vez disso, o `ToolManager` passa a conhecer apenas o `ToolRegistry`. A estrutura ficará assim:

```Python
from app.tools.tool_registry import ToolRegistry


class ToolManager:

    def __init__(self):
        self.registry = ToolRegistry()
```

**Por enquanto, alterarei apenas o `__init__`.**

Não mexerei ainda no método `calculate()`. Farei isso na Etapa 3 para enxergar claramente a transição de responsabilidades.

Essa separação em dois passos é didática: primeiro mudamos **quem conhece as ferramentas**; depois mudamos **como elas são executadas**.

# Etapa 3 - Refatorar o ToolManager 
Agora vamos terminar a transformação.

Hoje meu `ToolManager` possui:

```python
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

A referência `self.calculator` não existe mais. Ela deve ser substituída por uma consulta ao Registry. Implementei assim:

```python
def calculate(
    self,
    a: float,
    b: float,
    operation: str
) -> float:

    calculator = self.registry.get("calculator") #adição

	# observe que retirei o self
    return calculator.calculate(
        a,
        b,
        operation
    )
```

# Etapa 4 - Eliminar do `MentorAgent`qualquer conhecimento específico sobre `"calculator"`, tornando-o completamente agnóstico em relação às ferramentas.
Esse é o passo que fecha o principal objetivo arquitetural da aula.

Hoje meu `MentorAgent` possui:

```python
if tool_name == "calculator":
    result = self.tool_manager.calculate(
        a=response["a"],
        b=response["b"],
        operation=response["operation"]
    )

    result = str(result)

    self.memory.add_assistant_message(result)

    return result
```

Vamos transformar o `ToolManager` em um executor genérico.

Em vez de:

```python
self.tool_manager.calculate(...)
```

o Mentor fará:

```python
result = self.tool_manager.execute(
    tool_name=response["tool"],
    tool_input=response["input"]
)
```

### Vamos alterar primeiramente o `ToolManager`
Vamos acrescentar o método:
```Python
def execute(
    self,
    tool_name: str,
    tool_input: dict
):

    tool = self.registry.get(tool_name)

    if tool is None:
        raise ValueError(f"Ferramenta desconhecida: {tool_name}")

    return tool.calculate(
        a=tool_input["a"],
        b=tool_input["b"],
        operation=tool_input["operation"]
    )
```

⚠️ **Não removi o método `calculate()` ainda.**

Vou mantê-lo temporariamente até verificar que tudo funciona. Depois decidirei se ele continua ou desaparece.
# Etapa 6 — Reflexão arquitetural

## 1. Por que o Tool Registry melhora a escalabilidade do sistema?

O **Tool Registry** melhora a escalabilidade porque centraliza o cadastro de todas as ferramentas disponíveis. Antes, cada nova ferramenta exigia alterações no `ToolManager` e, em alguns casos, no `MentorAgent`. Com o Registry, adicionar uma nova ferramenta significa apenas registrá-la no catálogo, sem modificar o restante da arquitetura. Isso torna o sistema aberto para extensão e reduz o risco de introduzir erros em componentes já estáveis.

---

## 2. Qual responsabilidade pertence ao Registry e qual pertence ao ToolManager?

O **Tool Registry** é responsável apenas por **registrar, armazenar e recuperar ferramentas**. Ele funciona como um catálogo de capacidades do sistema e não executa nenhuma lógica de negócio.

Já o **ToolManager** é responsável por **orquestrar a execução das ferramentas**. Ele consulta o Registry para localizar a ferramenta solicitada e coordena sua execução, podendo futuramente concentrar responsabilidades como tratamento de erros, logs, autorização, métricas, timeouts e retries.

Em resumo:

- **Tool Registry:** conhece quais ferramentas existem.
- **ToolManager:** sabe como executá-las.

---

## 3. Por que remover conhecimento das ferramentas do MentorAgent é importante?

Porque o `MentorAgent` deve atuar apenas como um **orquestrador da conversa**, e não conhecer detalhes da implementação das ferramentas. Ao remover esse conhecimento, o agente fica desacoplado da infraestrutura e passa a depender apenas da capacidade de solicitar a execução de uma ferramenta.

Isso significa que novas ferramentas podem ser adicionadas ao sistema sem que o `MentorAgent` precise ser modificado, reduzindo o acoplamento e facilitando a manutenção da arquitetura.

---

## 4. Explique por que essa arquitetura se aproxima de um sistema baseado em plugins.

Essa arquitetura se aproxima de um sistema baseado em plugins porque novas funcionalidades são adicionadas por meio do **registro de novos componentes**, e não pela modificação do código principal. Cada ferramenta é um módulo independente que pode ser registrado no `ToolRegistry` e utilizado pela aplicação sem alterar o fluxo do `MentorAgent` ou do `ToolManager`.

Na prática, adicionar uma nova ferramenta passa a seguir um processo simples:

1. Implementar a ferramenta.
2. Registrá-la no `ToolRegistry`.
3. Disponibilizá-la para uso.

O restante da arquitetura permanece inalterado, característica típica de sistemas extensíveis baseados em plugins.