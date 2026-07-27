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
## Etapa 1 — Remover o `ToolDecision`

O desafio pede explicitamente:

> **"Remova a dependência de `ToolDecision` do `MentorAgent`."**

Então a primeira missão é bem objetiva.

### O que deve desaparecer do `MentorAgent`?

Você deverá remover:

1. O import:

```Python
from app.services.tool_decision import ToolDecision
```

2. A linha:

```Python
tool = ToolDecision.decide(question)
```

3. Todo o bloco:

```Python
if tool == "calculator":
    ...
```

Ao final dessa etapa, o `MentorAgent` **não decidirá mais absolutamente nada** sobre ferramentas.

Ele apenas enviará a requisição ao `LLMService`.

> **Ainda não implemente a nova lógica. Apenas remova a antiga.**

Quando terminar essa etapa, me envie o novo `MentorAgent`. A partir daí construiremos juntos a nova responsabilidade do `LLMService`, que será o verdadeiro protagonista da Aula 8.2.

# Então, qual é o primeiro passo?

Eu faria exatamente isto:

De:

```Python
def generate(self, prompt: str) -> str:
```

Para:

```Python
def generate(self, prompt: str):
```

(removemos temporariamente o `-> str`)

E, no final:

De:

```Python
return response.output_text
```

Para:

```Python
return response
```

Perceba que ainda **não estamos implementando Tool Calling**.

Estamos apenas parando de jogar informação fora.

É um pequeno refactor que prepara o restante da Aula 8.2.