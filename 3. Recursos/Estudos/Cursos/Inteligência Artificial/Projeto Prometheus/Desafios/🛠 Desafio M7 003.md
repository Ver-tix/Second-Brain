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
# Etapa 1 — Atualizar `settings.py`:
```Python
from dotenv import load_dotenv
import os

# Carrega as variáveis do arquivo .env
load_dotenv()

# Recupera a chave da API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Toda configuração centralizada
MODEL_NAME = "gpt-4.1-mini"

# Constante do modelo de embedding
EMBEDDING_MODEL = "text-embedding-3-small"

# Verifica se a chave existe
if OPENAI_API_KEY is None:
    raise ValueError(
        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"
    )
```

# Etapa 2 — Implementar o `EmbeddingService`
```text
app/
└── knowledge/
    ├── __init__.py
    ├── knowledge_service.py
    └── embedding_service.py   ← aqui    
```

```python
from openai import OpenAI
from app.config.settings import (
    OPENAI_API_KEY,
    EMBEDDING_MODEL
) 
  
class EmbeddingService: 
    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)  

    def generate_embedding(
        self,
        text:str
    )->list[float]:

        response = self.client.embeddings.create(
            model=EMBEDDING_MODEL,
            input=text
        )
        
        return response.data[0].embedding
```

# Etapa 3 - Atualização do `KnowledgeService`

```Python
from app.knowledge.embedding_service import EmbeddingService

class KnowledgeService:
    def __init__(
            self,
            embedding_service: EmbeddingService
            ):
        self.embedding_service = embedding_service

    def generate_embedding(
            self,
            text:str
    ) ->list[float]:
        return self.embedding_service.generate_embedding(text)
```

# Etapa 4 - Teste no `main.py`

```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService
from app.knowledge.embedding_service import EmbeddingService  

def main(): 

    # Cria o serviço responsável por conversar com a OpenAI
    llm_service = LLMService()

    # Cria a memória dentro do programa
    memory = ConversationMemory()

    # Cria o gestor de ferramentas (tool manager)
    tool_manager = ToolManager()

    # Instanciar o serviço de embedding
    embedding_service = EmbeddingService()

    # Instanciar o serviço de conhecimento
    knowledge_service = KnowledgeService(
        embedding_service
    )

    # ===== Teste Temporário =====
    embedding = knowledge_service.generate_embedding(
        "Bitcoin é um ativo escasso."
    )

    print(
        f"Dimensão do embedding: {len(embedding)}"
    )
    # ==============

    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager,
        knowledge_service,
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

# Etapa 5 - Reflexão Arquitetural

> **Por que criamos um método `generate_embedding()` no `KnowledgeService`, se ele apenas encaminha a chamada para o `EmbeddingService`? Não seria mais simples chamar o `EmbeddingService` diretamente?**

Criamos esse método para **encapsular a infraestrutura interna da camada de conhecimento** e oferecer uma única interface pública para o restante do sistema.

Hoje, o `KnowledgeService` apenas delega a chamada ao `EmbeddingService`, mas isso é uma decisão arquitetural. O objetivo é impedir que componentes externos, como o `MentorAgent`, dependam diretamente dos detalhes de implementação da camada de conhecimento.

No futuro, o `KnowledgeService` deixará de conversar apenas com o `EmbeddingService` e passará a coordenar diversos componentes, como `Retriever`, `VectorStore`, `DocumentLoader` e `Chunker`. Como o restante da aplicação continuará interagindo apenas com o `KnowledgeService`, essa evolução poderá ocorrer sem modificar o `MentorAgent` ou qualquer outro componente consumidor.

Em outras palavras, o `KnowledgeService` funciona como uma **fachada (Facade)** para a infraestrutura de conhecimento: ele esconde a complexidade interna, reduz o acoplamento entre os módulos e permite que a arquitetura evolua sem exigir refatorações nas camadas superiores. Esse é o mesmo princípio que já aplicamos anteriormente com o `ToolManager` e o `ToolRegistry`: expor uma interface simples enquanto protegemos os detalhes internos da implementação.