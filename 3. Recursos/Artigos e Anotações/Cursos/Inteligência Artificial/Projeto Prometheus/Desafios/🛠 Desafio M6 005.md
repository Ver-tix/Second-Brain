---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
![[Aula 5 - Memória de Conversa#Desafio de reflexão]]

eu: `Por separação de responsabilidade. Isso gera maior organização (tanto em linha quanto para a leitura de um humano ao auditar), e maior possibilidade de escalabilidade`.

## Feedback

Excelente resposta. Eu daria **9,8/10**. E vou explicar por que não é 10 ainda.

Você acertou os dois principais motivos:

- ✅ **Separação de responsabilidades (Single Responsibility Principle)**.
- ✅ **Escalabilidade** (podemos trocar a implementação sem alterar o agente).

Mas há um terceiro motivo que, para mim, é o mais importante em arquitetura:

> **Inversão de dependência (Dependency Inversion) e baixo acoplamento.**

O `MentorAgent` **não deve saber como a memória funciona**.

Ele só deveria saber que existe alguém capaz de:

- guardar mensagens;
- recuperar histórico.

Nada mais.

Hoje essa implementação é:

```
ConversationMemory (lista em memória)
```

Amanhã pode ser:

```
ConversationMemory (Redis)
```

Depois:

```
ConversationMemory (PostgreSQL)
```

Depois:

```
ConversationMemory (memória compartilhada entre agentes)
```

E, idealmente, o `MentorAgent` continua exatamente igual.

É a mesma ideia que você já viu várias vezes no curso:

- o agente não sabe como o LLM funciona → usa `LLMService`;
- o agente não sabe como os prompts são montados → usa `PromptBuilder`;
- o agente não sabe como a memória é armazenada → usa `ConversationMemory`.

Percebe um padrão surgindo?

```
MentorAgent
     │
     ├── usa LLMService
     ├── usa PromptBuilder
     └── usa ConversationMemory
```

O agente está deixando de ser um "faz tudo" e se tornando um **orquestrador de componentes especializados**.

E eu gostaria de destacar uma coisa que venho percebendo nas últimas aulas: suas respostas estão cada vez menos focadas em Python e cada vez mais em **arquitetura**. Isso é um excelente sinal. A sintaxe você consulta em minutos; a capacidade de decompor sistemas em componentes bem definidos é o que diferencia um arquiteto de software de um programador que apenas escreve código. Essa mudança de mentalidade é exatamente um dos objetivos centrais do Projeto Prometheus.

---

# Prática
## Etapa 1 - Criar um novo arquivo

- [x] Arquivo criado

## Etapa 2 - Criar a Classe
```Python
class ConversationMemory:
```

## Etapa 3 - Definir a Interface
```Python
class ConversationMemory:
    def __init__(self):
        self.history = []

	# adicionar uma mensagem do usuário;
    def add_user_message(self, message: str): 
        self.history.append(f"Usuário: {message}")

	# adicionar uma resposta do assistente;
    def add_assistant_message(self, message: str): 
        self.history.append(f"Assistente: {message}")

	# recuperar todo o histórico formatado.
    def get_history(self) -> str: 
        return "\n".join(self.history)
```

## Etapa 4 - Atualizar o `PromptBuilder`
```Python
class PromptBuilder:
    @staticmethod
    def build(history: str,question: str) -> str:
        prompt = f"""

Você é o Prometheus-Mentor. 

Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara, didática e organizada.

Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da definição técnica.

Histórico da conversa:

{history}

Nova pergunta do usuário:

{question}
"""
```