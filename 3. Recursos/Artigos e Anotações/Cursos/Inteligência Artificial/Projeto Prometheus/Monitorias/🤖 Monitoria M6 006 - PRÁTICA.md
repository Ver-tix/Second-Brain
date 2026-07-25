---
tags:
  - inteligenciaartificial
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