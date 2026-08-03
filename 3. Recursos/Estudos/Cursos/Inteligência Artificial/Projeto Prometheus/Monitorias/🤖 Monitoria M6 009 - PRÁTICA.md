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