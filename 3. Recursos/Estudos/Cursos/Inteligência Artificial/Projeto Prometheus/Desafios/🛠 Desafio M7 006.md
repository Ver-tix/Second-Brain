---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
  - RAG
tags:
  - IA
  - programação
  - inovação
---
# Etapa 1 — Injetar `KnowledgeService`

(Já feito)

# Etapa 2 - Retrieval

Em `app/prompts/prompt_builder.py`:

```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto relevante da base de conhecimento:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"{context_section}"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

Em `app/agents/mentor_agent.py`, altere o método `ask()`:

```Python
def ask(self, question: str) -> str:

    self.memory.add_user_message(question)

    history = self.memory.get_history()

    results = self.knowledge_service.search(question)

    context = "\n".join(
        result["text"] for result in results
    )

    prompt = PromptBuilder.build(
        history,
        question,
        context,
    )

    response = self.llm_service.generate(prompt)

    if response["type"] == "text":
        self.memory.add_assistant_message(response["content"])
        return response["content"]

    elif response["type"] == "tool_call":
        result = self.tool_manager.execute(
            tool_name=response["tool"],
            tool_input=response["input"]
        )

        result_text = str(result)

        self.memory.add_assistant_message(result_text)

        return result_text
```

Em `app/prompts/prompt_builder.py`:
```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto relevante da base de conhecimento:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"{context_section}"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

Em `app/agents/mentor_agent.py`, altere o método `ask()`:
```Python
def ask(self, question: str) -> str:

    self.memory.add_user_message(question)

    history = self.memory.get_history()

    results = self.knowledge_service.search(question)

    context = "\n".join(
        result["text"] for result in results
    )

    prompt = PromptBuilder.build(
        history,
        question,
        context,
    )

    response = self.llm_service.generate(prompt)

    if response["type"] == "text":
        self.memory.add_assistant_message(response["content"])
        return response["content"]

    elif response["type"] == "tool_call":
        result = self.tool_manager.execute(
            tool_name=response["tool"],
            tool_input=response["input"]
        )

        result_text = str(result)

        self.memory.add_assistant_message(result_text)

        return result_text
```

# Etapa 3 - Contexto
Já fiz a maior parte da Etapa 3 no `mentor_agent.py`. O trecho abaixo é exatamente a transformação que o curso pede — sem criar classe nova.

O objetivo aqui não é criar um `ContextBuilder` ou similar. É entender como os dados mudam de forma ao longo do fluxo RAG.

```Python

results = self.knowledge_service.search(question)

context = "\n".join(
    result["text"] for result in results
)
```



```Python
# versão mais explícita
results = self.knowledge_service.search(question)

context_parts = []

for result in results:
    context_parts.append(result["text"])

context = "\n".join(context_parts)
```

# Etapa 4 - `PromptBuilder`

```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto recuperado:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"{context_section}"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

# Etapa 5 Atualizar `main.py`

Boa notícia: **a cadeia de dependências já está montada corretamente** no seu `main.py`. O fluxo pedido pelo curso já existe:

```python
    # Instanciar o serviço de embedding
    embedding_service = EmbeddingService()

    # Instanciar o VectorStore
    vector_store = VectorStore()

    # Instanciar o serviço de conhecimento
    knowledge_service = KnowledgeService(
        embedding_service,
        vector_store
    )
```

```python
    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service, 
        memory,
        tool_manager,
        knowledge_service,
        )
        
```
## Código Atualizado `main.py`

```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService
from app.knowledge.embedding_service import EmbeddingService
from app.knowledge.vector_store import VectorStore


def main():

    # ===== Infraestrutura de conhecimento =====

    embedding_service = EmbeddingService()
    vector_store = VectorStore()

    knowledge_service = KnowledgeService(
        embedding_service,
        vector_store,
    )

    # ===== Ingestão de documentos =====

    knowledge_service.add_document(
        "Bitcoin possui uma oferta máxima de 21 milhões de moedas."
    )

    knowledge_service.add_document(
        "Python é uma linguagem de programação de alto nível."
    )

    knowledge_service.add_document(
        "Redes neurais artificiais são modelos computacionais."
    )

    
	# ===== Demais dependências =====
    llm_service = LLMService()
    memory = ConversationMemory()
    tool_manager = ToolManager()

    # ===== Agente =====

    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager,
        knowledge_service,
    )

    # ===== Loop de questionamento =====

    while True:
        question = input("\nPergunte alguma coisa: ")

        if question.lower() == "sair":
            print("\nAté a próxima!")
            break

        answer = mentor.ask(question)

        print("\nPrometheus-Mentor:\n")
        print(answer)


if __name__ == "__main__":
    main()
```

# Etapa 7 - Reflexão Arquitetônica

## 1 - Se o `MentorAgent` precisa apenas de conhecimento relevante, por que ele não deveria conhecer diretamente o `VectorStore`?

Porque o VectorStore é um detalhe de implementação da recuperação de conhecimento, enquanto o MentorAgent deveria se preocupar apenas com a tarefa de ensinar e responder ao usuário.

O `MentorAgent` precisa fazer algo conceitualmente simples:

```
"Preciso de conhecimento relevante para responder esta pergunta."
```

Ele não deveria precisar saber:

```
"Preciso gerar um embedding,
consultar um banco vetorial,
calcular similaridade,
ordenar resultados..."
```

Por isso introduzimos o:

```
MentorAgent
      ↓
KnowledgeService
      ↓
VectorStore
```

O `KnowledgeService` funciona como uma **camada de abstração** entre o agente e a infraestrutura de conhecimento.

Assim, o `MentorAgent` depende de uma **capacidade**:

```
knowledge_service.search(question)
```

e não de uma **implementação concreta**:

```
vector_store.search(embedding)
```

Isso produz um desacoplamento importante.

Hoje, por exemplo:

```
KnowledgeService
       ↓
VectorStore
```

pode utilizar nosso armazenamento vetorial atual.

No futuro, poderíamos substituir a implementação por outro mecanismo de busca, outro banco vetorial ou uma arquitetura mais sofisticada **sem precisar reescrever o `MentorAgent`**.

### 🔥 Princípio Prometheus

> **O agente deve conhecer capacidades, não detalhes de infraestrutura.**

Essa é uma ideia extremamente importante para o Prometheus OS: quanto mais os agentes dependerem de abstrações e serviços bem definidos, mais fácil será reutilizar os mesmos agentes em diferentes ecossistemas.

---

## 2. Qual é a diferença arquitetural entre `ConversationMemory` e `KnowledgeService`?

A diferença fundamental está em **de onde vem o conhecimento e qual é sua finalidade**.

### `ConversationMemory`

Representa o **estado contextual da conversa atual**.

Ela responde essencialmente:

> **"O que já aconteceu nesta conversa?"**

Por exemplo:

```
Usuário: Explique Transformers.

Mentor: Transformers utilizam...

Usuário: E como funciona a atenção?

Mentor: Como expliquei anteriormente...
```

A `ConversationMemory` permite que o agente mantenha continuidade entre essas interações.

Podemos visualizar:

```
Usuário
   ↓
Conversa
   ↓
ConversationMemory
   ↓
Histórico/contexto da interação
```

---

### `KnowledgeService`

Representa o **conhecimento externo que pode ser recuperado para responder à pergunta**.

Ele responde:

> **"Que conhecimento relevante eu tenho disponível para esta pergunta?"**

Por exemplo:

```
Pergunta:
"Qual é a oferta máxima do Bitcoin?"

        ↓

KnowledgeService
        ↓
VectorStore
        ↓
"Bitcoin possui oferta máxima de 21 milhões."
```

Visualmente:

```
Base de conhecimento
        ↓
VectorStore
        ↓
KnowledgeService
        ↓
Conhecimento relevante
```