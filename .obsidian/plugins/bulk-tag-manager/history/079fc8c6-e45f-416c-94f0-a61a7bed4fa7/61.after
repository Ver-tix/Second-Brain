---
tags:
  - IA
  - programação
  - inovação
---
### `llm_service: LLMService`:

```Python
def __init__(self, llm_service: LLMService):
```

Aqui, o MentorAgent está dizendo:

>"Eu preciso de um LLMService, mas não sou eu quem vai criá-lo."

Quem cria é o `main.py`:

```Python
llm_service = LLMService()

mentor = MentorAgent(llm_service)
```

Ou seja:

```
main.py
    │
cria
    ▼
LLMService
    │
entrega
    ▼
MentorAgent
```

Isso é injeção de dependência.

`self.memory = ConversationMemory()`

Aqui acontece o contrário.

`self.memory = ConversationMemory()`

O próprio `MentorAgent` diz:

>"A memória faz parte de mim. Eu mesmo vou criá-la."

Ninguém entrega a memória para ele.

Então por que uma é injetada e a outra criada?

Porque pensamos na responsabilidade arquitetural.

O LLMService é um componente que pode mudar:

- OpenAI
- Claude
- Gemini
- Modelo local

Queremos poder trocar isso facilmente.

Já a memória, por enquanto, é um detalhe interno do agente. O professor ainda não pediu que ela seja compartilhada nem substituída. A aula mostra justamente essa implementação simples como um primeiro passo.