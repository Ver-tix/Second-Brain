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

### Agora vem a última alteração do MentorAgent

O bloco antigo:

```python
elif response["type"] == "tool_call":
    tool_name = response["tool"]
    tool_input = response["input"]

    if tool_name == "calculator":
        result = self.tool_manager.calculate(
            a=response["a"],
            b=response["b"],
            operation=response["operation"]
        )

        result = str(result)

        self.memory.add_assistant_message(result)

        return result

    else:
        raise ValueError(f"Ferramenta desconhecida: {tool_name}")
```

deve desaparecer completamente.

E virar apenas:

```python
elif response["type"] == "tool_call":

    result = self.tool_manager.execute(
        tool_name=response["tool"],
        tool_input=response["input"]
    )

    result = str(result)

    self.memory.add_assistant_message(result)

    return result
```

# Etapa 5 - Provar que a Arquitetura é Realmente Extensível adicionando uma segunda ferramenta
## Por que não adicionar o RAG ainda?
A etapa 5 apenas quer mostrar que podemos agora implementar novas ferramentas úteis. Então, deve ser o mais simples possível.

Iríamos, inicialmente, criar um `DateTool`, para retornar datas. Mas, como conheço o rumo que estamos dando ao projeto, faria uma pequena adaptação.

Em vez de uma ferramenta genérica, criaria algo que fará parte do ecossistema futuramente.

Por exemplo: `KnowledgeTool`

Por enquanto, ela seria apenas um _stub_:

```Python
class KnowledgeTool:

    def execute(self, query: str):

        return f"[Knowledge] Busca simulada por: {query}"
```

Ela ainda não consulta seu Second Brain.

Ela apenas simula a existência dele.

Então você registraria:

```Python
registry.register(
    "knowledge",
    KnowledgeTool()
)
```

# Etapa 5.1 - Criar `knowledge_tool.py`
Criei um novo arquivo:

```
app/
└── tools/
    └── knowledge_tool.py
```

Conteúdo:

```Python
class KnowledgeTool:

    def search(self, query: str) -> str:
        return f"[Knowledge] Busca simulada por: '{query}'"
```

Ele **não busca nada ainda**, apenas simula o comportamento futuro.

# Etapa 5.2 — Registrar no Registry

Agora altere o `ToolRegistry`:

```Python
from app.tools.calculator_tool import CalculatorTool
from app.tools.knowledge_tool import KnowledgeTool


class ToolRegistry:

    def __init__(self):
        self.tools = {}

        self.register(
            "calculator",
            CalculatorTool()
        )

        self.register(
            "knowledge",
            KnowledgeTool()
        )
```

um detalhe MUITO interessante

Quero que você observe uma coisa que provavelmente o professor está preparando para as próximas aulas.

Hoje o seu `ToolManager.execute()` faz isso:

```python
tool = self.registry.get(tool_name)

return tool.calculate(...)
```

Isso funciona para a calculadora.

Mas acabamos de criar:

```python
KnowledgeTool
```

E ela possui:

```python
search(...)
```

Não:

```python
calculate(...)
```

Então...

Se amanhã o LLM responder:

```python
tool = knowledge
```

o `ToolManager` vai tentar fazer:

```python
tool.calculate(...)
```

e receberemos:

```python
AttributeError
```

---

## Isso é um erro?

**Não.**

Na verdade, isso é uma excelente descoberta arquitetural.

A arquitetura está nos dizendo:

> "As ferramentas não possuem uma interface comum."
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