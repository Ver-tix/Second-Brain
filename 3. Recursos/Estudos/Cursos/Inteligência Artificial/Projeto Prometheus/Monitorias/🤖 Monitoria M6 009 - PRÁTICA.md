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
# Etapa 1 — Criar o Tool Registry

Primeiro, crie o arquivo:

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

## Explicações:

#### **O catálogo**
```Python
self.tools = {} 
```
É simplesmente um dicionário. Ele armazenará algo como:
```Python
{
	"calculator": CalculatorTool()
}
```

#### **Método `register`**
```python
self.register(
    "calculator",
    CalculatorTool()
)
```

Registra uma ferramenta no catálogo. Internamente faz:

```python
self.tools["calculator"] = CalculatorTool()
```

#### **Método `get`
```Python
registry.get("calculator")
```

retorna:

```Python
CalculatorTool()
```

Ou, caso não exista:

```Python
None
```

porque usamos:

```Python
dict.get(...)
```

---
## Observe a mudança de mentalidade

Antes o `ToolManager` dizia:

> "Eu tenho uma CalculatorTool."

Agora ele dirá:

> "Existe um catálogo. Vou perguntar a ele."

Parece uma diferença pequena.

Mas é exatamente isso que elimina os `if` e `elif` do sistema.

# Etapa 2 - `ToolManager` deixará de instanciar a `CalculatorTool` e passará a utilizar o `ToolRegistry`.
Agora vamos alterar o `ToolManager`.

Hoje ele provavelmente está assim (ou muito próximo disso):

```Python
class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()
```

Essa linha precisa desaparecer.

Em vez disso, o `ToolManager` passa a conhecer apenas o `ToolRegistry`.

A estrutura ficará assim:

```Python
from app.tools.tool_registry import ToolRegistry


class ToolManager:

    def __init__(self):
        self.registry = ToolRegistry()
```

**Por enquanto, altere apenas o `__init__`.**

Não mexa ainda no método `calculate()`. Faremos isso na Etapa 3 para enxergar claramente a transição de responsabilidades.

Essa separação em dois passos é didática: primeiro mudamos **quem conhece as ferramentas**; depois mudamos **como elas são executadas**.

## O que acabou de acontecer?

Antes, o `ToolManager` dizia:

> "Eu sei criar uma CalculatorTool."

Agora ele diz:

> "Eu conheço alguém que sabe onde encontrar ferramentas."

Essa diferença é o início da **inversão de dependência** (ainda não estamos estudando formalmente o princípio DIP, mas você já está começando a praticá-lo).

---

# Etapa 3 - Refatorar o ToolManagerAgora vamos terminar a transformação.

Hoje seu `ToolManager` provavelmente possui algo semelhante a:

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

A referência `self.calculator` não existe mais.

Ela deve ser substituída por uma consulta ao Registry.

Implemente assim:

```python
def calculate(
    self,
    a: float,
    b: float,
    operation: str
) -> float:

    calculator = self.registry.get("calculator")

    return calculator.calculate(
        a,
        b,
        operation
    )
```

Observe a mudança:

Antes:

```
ToolManager
        │
        ▼
CalculatorTool
```

Depois:

```
ToolManager
        │
        ▼
ToolRegistry
        │
        ▼
CalculatorTool
```

Agora o `ToolManager` não conhece mais nenhuma ferramenta específica. Ele apenas consulta o catálogo e utiliza o objeto retornado.

## Code Review

### ✅ Separação de responsabilidades

Excelente.

O `ToolManager` não instancia mais nenhuma ferramenta.

Toda responsabilidade de "quem existe" foi transferida para o `ToolRegistry`.

---

### ✅ Acoplamento reduzido

Antes:

```
ToolManager
      │
      ▼
CalculatorTool
```

Agora:

```
ToolManager
      │
      ▼
ToolRegistry
      │
      ▼
CalculatorTool
```

Isso significa que, se amanhã a implementação da calculadora mudar, o `ToolManager` não precisa saber.

---

### 💡 Uma observação arquitetural (para o futuro)

Existe uma pequena "imperfeição" que ainda permanece, mas **não deve ser corrigida agora**, pois pertence às próximas aulas.

Repare que ainda existe:

```
calculator = self.registry.get("calculator")
```

Ou seja, o `ToolManager` ainda conhece o nome `"calculator"`.

Em uma arquitetura mais madura, ele poderia receber o nome dinamicamente:

```
def execute(self, tool_name: str, **kwargs):
    tool = self.registry.get(tool_name)
    ...
```

Mas isso faz parte de uma evolução futura, quando o `ToolManager` se tornará um executor genérico de ferramentas. Para a Aula 9, sua implementação está exatamente no nível esperado.

---

# Etapa 4 - Eliminar do `MentorAgent` qualquer conhecimento específico sobre `"calculator"`, tornando-o completamente agnóstico em relação às ferramentas.
Esse é o passo que fecha o principal objetivo arquitetural da aula.

Hoje seu `MentorAgent` provavelmente possui algo parecido com isto:

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

Observe que ele ainda sabe duas coisas:

- existe uma ferramenta chamada `"calculator"`;
- para executá-la deve chamar `calculate()`.

Isso viola exatamente o objetivo da aula.

---

# A ideia

Quem deve conhecer as ferramentas?

Não é mais o Mentor.

É o ToolManager.

Então o Mentor deve apenas dizer:

> "Execute esta ferramenta."

Sem saber qual é.

---

# A mudança

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

Perceba que agora ele não sabe:

- se existe Calculator;
- Search;
- RAG;
- Calendar.

Ele apenas entrega:

- o nome;
- os argumentos.

---

## Então, primeiro precisamos alterar o `ToolManager`

Acrescente este método:

```python
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

⚠️ **Não remova o método `calculate()` ainda.**

Vamos mantê-lo temporariamente até verificar que tudo funciona. Depois podemos decidir se ele continua ou desaparece.

Excelente. O `ToolManager` ficou correto e, mais importante, **a responsabilidade foi deslocada para o lugar certo**.
## Code Review

O método novo está muito bom:

```python
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

Observe o que aconteceu:

Antes o `ToolManager` dizia:

> "Eu sei executar a calculadora."

Agora ele diz:

> "Dê-me o nome da ferramenta. Eu descubro quem ela é."

Essa é uma mudança arquitetural muito importante.

---

## Agora vem a última alteração do MentorAgent

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

## Perceba o ganho

Antes o Mentor conhecia:

- Calculator
    
- o método `calculate()`
    
- como chamá-la
    

Agora ele conhece apenas um protocolo:

> "Existe uma ferramenta com este nome e estes argumentos."

Isso significa que amanhã você poderá adicionar:

- `SearchTool`
    
- `DateTool`
    
- `GitHubTool`
    
- `RAGTool`
    

e **não tocar uma única linha do `MentorAgent`**.

---

### Uma pequena observação

Assim que o `MentorAgent` passar a usar `execute()`, o método:

```python
def calculate(...)
```

do `ToolManager` ficará sem uso.

**Eu não o removeria ainda.**

Esperaria o laboratório terminar e, depois, verificaria se o professor quer mantê-lo por questões didáticas ou eliminá-lo definitivamente. É comum manter código durante uma transição e limpá-lo ao final da refatoração.

Faça essa alteração no `MentorAgent` e me envie o código atualizado. Se estiver tudo certo, teremos concluído a parte prática principal da Aula 9 e poderemos passar para o teste de extensibilidade (Etapa 5).

## Antes

O `MentorAgent` tinha que conhecer:

- o nome `"calculator"`;
- que existia um método `calculate()`;
- como chamar esse método;
- como tratar ferramenta desconhecida.

Ou seja, ele fazia mais do que deveria.

## Agora

Ele faz apenas isto:

```Python
elif response["type"] == "tool_call":

    result = self.tool_manager.execute(
        tool_name=response["tool"],
        tool_input=response["input"]
    )

    result = str(result)

    self.memory.add_assistant_message(result)

    return result
```

Isso é muito mais elegante.

Observe a mudança de responsabilidade:

```
Antes

Mentor
   │
   ├── conhece calculator
   ├── sabe executar calculator
   └── trata erro
```

↓

```
Agora

Mentor
    │
    └── "ToolManager, execute esta ferramenta."
```

Essa é exatamente a ideia central da Aula 9.

---

## Uma pequena melhoria (opcional)

Há um detalhe que eu faria diferente, mas **não é obrigatório para esta aula**.

Hoje o `MentorAgent` ainda faz:

```Python
result = str(result)

self.memory.add_assistant_message(result)
```

Eu escreveria:

```Python
result = self.tool_manager.execute(
    tool_name=response["tool"],
    tool_input=response["input"]
)

result_text = str(result)

self.memory.add_assistant_message(result_text)

return result_text
```

Por quê?

Porque `result` continua sendo o valor real (por exemplo, um `float`), e `result_text` representa apenas sua versão textual para armazenamento e exibição. Em sistemas maiores, é comum manter essa distinção entre o dado e sua representação.

Mas, para o estágio atual do curso, sua versão está totalmente adequada.

### `Implementei as alterações`
