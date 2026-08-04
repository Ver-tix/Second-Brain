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
## Etapa 1 — Criar a infraestrutura

CrieI a estrutura:

```
app/
└── knowledge/
    ├── __init__.py
    └── knowledge_service.py
```

---

## Etapa 2 — Criar o `KnowledgeService`

```python
class KnowledgeService:
    pass
```

Não adicionei MAIS nada, conforme orientação.

---
## Etapa 3 — Injetar no `MentorAgent`

**antes**:
```python
from app.services.llm_service import LLMService
from app.prompts.prompt_builder import PromptBuilder
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager

...

def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager
):
	self.llm_service = llm_service
    self.memory = memory
    self.tool_manager = tool_manager
```

**Agora**:
```python
from app.services.llm_service import LLMService
from app.prompts.prompt_builder import PromptBuilder
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService

...

def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager,
    knowledge_service: KnowledgeService
):
	self.llm_service = llm_service
    self.memory = memory
    self.tool_manager = tool_manager
    self.knowledge_service = knowledge_service

```

**Ficando:**
```Python
from app.services.llm_service import LLMService
from app.prompts.prompt_builder import PromptBuilder
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService

class MentorAgent:
    def __init__(
            self,
            llm_service: LLMService,
            memory: ConversationMemory,
            tool_manager: ToolManager,
            knowledge_service: KnowledgeService
            ):
        self.llm_service = llm_service
        self.memory = memory
        self.tool_manager = tool_manager
        self.knowledge_service = knowledge_service

    def ask(self, question: str) -> str:

        self.memory.add_user_message(question)
        history = self.memory.get_history()
        prompt = PromptBuilder.build(
             history,
             question
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

                result_text=str(result)

                self.memory.add_assistant_message(result_text)

                return result_text
```

---

## Etapa 4 — Atualizar o `main.py`

Instanciei:

```python
knowledge_service = KnowledgeService()
```

E passei para o agente:

```python
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service
)
```

E se tornou:
```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService

def main():

    # Cria o serviço responsável por conversar com a OpenAI
    llm_service = LLMService()

    # Cria a memória dentro do programa
    memory = ConversationMemory()

    # Cria o gestor de ferramentas (tool manager)
    tool_manager = ToolManager()

    # Instanciar o serviço de conhecimento
    knowledge_service = KnowledgeService()

    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager,
        knowledge_service
        )

    # Loop de Questionamento:
    while True:

        # Recebe a pergunta do usuário
        question = input("\nPergunte alguma coisa: ")

        # Condição para sair
        if question.lower() == "sair":
            print("\nAté a próxima!")
            break

        # O agente responde
        answer = mentor.ask(question)

        # Exibe a resposta
        print("\nPrometheus-Mentor:\n")
        print(answer)

if __name__ == "__main__":
    main()
```

---

# Etapa 5 - Reflexão
> **Por que decidimos introduzir o `KnowledgeService` agora, mesmo sem implementar nenhuma busca de documentos?**

## Minha resposta:
Introduzimos o `KnowledgeService` antes de implementar a busca de documentos para preparar a arquitetura para sua futura evolução. Ao aplicar a injeção de dependência desde o início, o `MentorAgent` já passa a conhecer apenas a existência de um serviço de conhecimento, sem depender de sua implementação. Assim, quando adicionarmos componentes como `Retriever`, `EmbeddingService` e `VectorStore`, eles serão incorporados ao `KnowledgeService` sem exigir mudanças na estrutura do agente. Essa abordagem reduz o acoplamento, evita grandes refatorações futuras e permite que o RAG seja integrado de forma incremental, preservando a organização e a escalabilidade da arquitetura do Prometheus.

---

# Feedback
