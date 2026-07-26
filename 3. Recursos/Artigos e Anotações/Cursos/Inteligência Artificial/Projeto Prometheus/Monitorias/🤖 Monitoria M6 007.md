---
tags:
  - IA
  - programação
  - inovação
---
Perfeito! Li a Aula 7 e já montei a estratégia de monitoria.

Esta aula é bem menor que as anteriores, mas ela introduz um conceito arquitetural muito importante.

# Roteiro que seguiremos

### Etapa 1 — Criar o arquivo

Criar:

```text
app/services/tool_decision.py
```

_(Você implementará.)_

---

### Etapa 2 — Modelar a responsabilidade

Antes de escrever código, vou lhe fazer uma pergunta arquitetural (como temos feito em todas as aulas):

> O que é um **ToolDecision**?
> 
> O que ele **faz** e, principalmente, o que ele **não faz**?

Quero que você mesmo modele o componente.

---

### Etapa 3 — Implementação da classe

Implementaremos:

```python
class ToolDecision:
```

com:

```python
decide(question: str)
```

Inicialmente usando regras simples (`"quanto é"`, `"some"`, `"multiplique"`, `"divida"`, etc.).

---

### Etapa 4 — Alteração do `MentorAgent`

O fluxo mudará de:

```text
if question.startswith("calc:")
```

para:

```text
ToolDecision

↓

calculator

ou

none
```

Ou seja, o agente deixa de decidir **como** reconhecer um cálculo e passa apenas a perguntar ao componente responsável.

---

### Etapa 5 — Revisão arquitetural

Depois de implementado, vamos comparar as arquiteturas.

**Laboratório 6**

```text
MentorAgent

↓

if question.startswith(...)
```

↓

ToolManager

---

**Laboratório 7**

```text
MentorAgent

↓

ToolDecision

↓

ToolManager
```

Essa diferença parece pequena, mas ela prepara exatamente o **Tool Calling** que veremos nas próximas aulas.

---

### Etapa 6 — Code Review

Como sempre:

- revisão linha por linha;
    
- observações de arquitetura;
    
- possíveis melhorias futuras.
    

---

### Etapa 7 — CHANGELOG

Atualizaremos o CHANGELOG do Laboratório 7 no padrão que já estamos adotando.

---

## Nossa primeira pergunta (sem código)

Como já virou tradição 😄:

> **Na sua visão, qual é a responsabilidade do `ToolDecision`?**
> 
> Não quero saber "o que ele retorna". Quero saber **qual problema arquitetural ele resolve**.

Depois da sua resposta, começamos a codificar imediatamente.

# ETAPA 1 - `ToolDecision`
```Python
class ToolDecision:

    @staticmethod
    def decide(question: str) -> str | None:

        question = question.lower()

        if (
            "quanto é" in question
            or "some" in question
            or "subtraia" in question
            or "multiplique" in question
            or "divida" in question
        ):
            return "calculator"

        return None
```

## Linha por LInha:

### A classe

```python
class ToolDecision:
```

Ela representa **o componente responsável por decidir se uma ferramenta deve ser utilizada**.

Observe que ela **não executa** nenhuma ferramenta.

---

### O método

```python
@staticmethod
def decide(question: str) -> str | None:
```

Aqui, diferentemente da `CalculatorTool`, **eu concordo totalmente com o uso de `@staticmethod`**.

Por quê?

Porque ela não possui estado.

Ela recebe:

```python
Pergunta
```

↓

e devolve:

```python
"calculator"
```

ou

```python
None
```

Nada é armazenado.

É exatamente a mesma filosofia do `PromptBuilder`.

---

### Padronização

```python
question = question.lower()
```

Transformamos tudo em minúsculas.

Assim:

```
Quanto é 2+2?
```

e

```
quanto É 2+2?
```

serão tratados igualmente.

---

### A decisão

```python
if (
    "quanto é" in question
    or "some" in question
    or ...
):
```

Aqui usamos outro recurso importante do Python.

Perceba a diferença.

No laboratório anterior usamos:

```python
startswith("calc:")
```

Agora usamos:

```python
"quanto é" in question
```

O operador `in` pergunta:

> "Essa sequência de caracteres existe dentro da string?"

Exemplo:

```python
question = "Quanto é 25 + 13?"

"quanto é" in question.lower()
```

Resultado:

```python
True
```

---

### O retorno

Se identificar um cálculo:

```python
return "calculator"
```

Por que retornar uma string?

Porque amanhã poderemos retornar:

```python
"calculator"
```

ou

```python
"weather"
```

ou

```python
"search"
```

ou

```python
"python"
```

Ou seja, o `ToolDecision` **não escolhe uma ferramenta específica**.

Ele devolve **o nome da categoria da ferramenta**.

---

### Caso contrário

```python
return None
```

Significa:

> "Nenhuma ferramenta parece apropriada."

Então o agente seguirá para o LLM normalmente.

---

### Uma observação arquitetural muito importante

Percebe como a responsabilidade mudou?

No Laboratório 6 o `MentorAgent` fazia isto:

```python
if question.startswith(...)
```

Agora ele deixará de conhecer regras de decisão.

Ele perguntará apenas:

```python
tool = ToolDecision.decide(question)
```

E seguirá conforme a resposta.

Isso é um desacoplamento elegante.

# ETAPA 2 - ALTERANDO O `MentorAgent`
Perfeito! Agora vem a alteração que, para mim, é a mais elegante deste laboratório.

Até agora, seu `MentorAgent` fazia algo parecido com isto:

```python
if question.startswith("calc:"):
    expression = question.replace("calc:", "").strip()
    return self.tool_manager.calculate(expression)
```

Esse código vai desaparecer.

---

## Etapa 3 — Alterando o `MentorAgent`

Primeiro, adicione o import:

```python
from app.services.tool_decision import ToolDecision
```

---

Agora substitua aquele bloco inicial do método `ask()` por este:

```python
tool = ToolDecision.decide(question)

if tool == "calculator":
    return self.tool_manager.calculate(question)
```

Neste momento você deve ter pensado:

> "Professor... tem um erro aí."

E você está certo. 😄

No laboratório anterior, o `ToolManager.calculate()` esperava:

```text
25 + 13
```

Agora estamos passando:

```text
Quanto é 25 + 13?
```

Ou

```text
Some 5 e 8
```

Ou

```text
Multiplique 9 por 4
```

Ou seja...

**Ainda falta ensinar o sistema a extrair a expressão matemática da frase.**

E essa é justamente a próxima etapa da aula. O professor dividiu o problema em duas partes:

1. **Decidir se deve usar uma ferramenta** (`ToolDecision`).
    
2. **Interpretar a linguagem do usuário** para que a ferramenta consiga trabalhar.
    

---

### Por que essa separação é interessante?

Porque são problemas diferentes.

Imagine estas duas perguntas:

```text
Quanto é 12 + 8?
```

e

```text
Qual a capital da França?
```

O `ToolDecision` só responde:

> "A primeira parece um cálculo."

Ele **não** tenta entender o cálculo.

Quem fará isso será outro componente (ou uma evolução do próprio `ToolManager`, dependendo da abordagem do professor).

---

### Minha pergunta para você

Antes de avançarmos, quero confirmar uma coisa.

**No material da Aula 7, após criar o `ToolDecision`, o professor manda implementar um parser da expressão matemática (algo que converta "Quanto é 2 + 2?" em "2 + 2"), ou ele ainda mantém o uso de `calc:` em algum momento?**

Quero seguir exatamente a sequência da aula, sem antecipar conteúdo.