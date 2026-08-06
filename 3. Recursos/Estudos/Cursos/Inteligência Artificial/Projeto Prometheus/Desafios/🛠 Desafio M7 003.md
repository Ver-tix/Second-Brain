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

---
# Feedback
Professor corrigindo. E posso dizer que esta foi uma das melhores entregas do Projeto Prometheus até agora. Não pelo volume de código, mas porque você começou a conectar **conceitos de arquitetura** com a implementação. Todo o desafio está documentado no arquivo enviado.

# Avaliação Geral

**Nota: 10/10**

Você implementou exatamente o escopo da aula, sem antecipar o banco vetorial, o retriever ou qualquer lógica de RAG.

---

# Etapa 1 — `settings.py`

Perfeito.

Você centralizou:

```python
MODEL_NAME
EMBEDDING_MODEL
OPENAI_API_KEY
```

Isso mantém toda configuração de modelos em um único ponto.

Uma observação para o futuro: quando chegarmos ao Prometheus OS, esse arquivo provavelmente deixará de armazenar apenas constantes e passará a carregar configurações por ambiente (desenvolvimento, produção, testes etc.). Mas, para esta etapa, está exatamente como eu esperava.

---

# Etapa 2 — `EmbeddingService`

Excelente implementação.

Você manteve exatamente a responsabilidade que definimos:

```text
Texto
      │
      ▼
EmbeddingService
      │
      ▼
Embedding
```

E nada além disso.

Gostei especialmente da assinatura:

```python
def generate_embedding(...) -> list[float]
```

Ela comunica claramente o contrato do serviço.

Também acertou ao retornar diretamente:

```python
response.data[0].embedding
```

Sem criar abstrações desnecessárias.

---

# Etapa 3 — `KnowledgeService`

Perfeito.

Você implementou uma verdadeira fachada.

Hoje ela apenas encaminha:

```text
KnowledgeService
      │
      ▼
EmbeddingService
```

Aparentemente inútil.

Mas arquiteturalmente essencial.

Esse é exatamente o tipo de camada que cresce naturalmente sem afetar quem está acima.

---

# Etapa 4 — Teste temporário

Excelente.

Gostei de um detalhe que muitos esquecem.

Você imprimiu:

```python
len(embedding)
```

e não o vetor inteiro.

Isso demonstra que entendeu o objetivo do teste:

não queremos inspecionar milhares de números.

Queremos apenas verificar se o pipeline inteiro funciona.

---

# Reflexão Arquitetural

Aqui, para mim, está o ponto alto da entrega.

Você escreveu:

> "O KnowledgeService funciona como uma fachada (Facade)..."

Excelente.

Inclusive fez uma conexão muito boa com aulas anteriores.

Você percebeu que:

```text
ToolManager
```

já era uma fachada.

Depois veio:

```text
ToolRegistry
```

Agora temos:

```text
KnowledgeService
```

E todos seguem exatamente a mesma filosofia:

> esconder a complexidade interna e expor uma interface simples.

Esse tipo de associação mostra que você não está apenas decorando o conteúdo; está reconhecendo padrões arquiteturais que se repetem ao longo do sistema.

---

# Uma observação arquitetural importante

Quero aproveitar esta aula para destacar algo que será muito importante daqui para frente.

Hoje temos:

```text
MentorAgent
        │
        ▼
KnowledgeService
        │
        ▼
EmbeddingService
```

Na próxima aula surgirá mais um componente:

```text
MentorAgent
        │
        ▼
KnowledgeService
        │
        ├──────────────┐
        ▼              ▼
EmbeddingService   VectorStore
```

Pouco depois:

```text
KnowledgeService
│
├── EmbeddingService
├── VectorStore
├── Retriever
├── Chunker
└── DocumentLoader
```

Observe algo curioso.

O `MentorAgent` continuará enxergando exatamente o mesmo objeto:

```python
knowledge_service
```

Mesmo quando a infraestrutura interna quadruplicar de tamanho.

Isso é encapsulamento arquitetural em ação.

---

# Conclusão

A **M7.3** está concluída.

Você acabou de construir a primeira integração real da camada de RAG.

Ainda não recuperamos documentos.

Ainda não existe banco vetorial.

Mas o Prometheus já consegue transformar significado em vetores — exatamente a primeira peça necessária para todo o restante.

---

## CHANGELOG — M7.3

```markdown
## M7-003 — Geração de Embeddings

### Adicionado
- Constante `EMBEDDING_MODEL` em `settings.py`.
- Implementação do `EmbeddingService` utilizando a API de Embeddings da OpenAI.
- Método `generate_embedding(text)` retornando `list[float]`.

### Alterado
- `KnowledgeService` passou a expor `generate_embedding()`, delegando ao `EmbeddingService`.
- `main.py` recebeu um teste temporário para validar a geração de embeddings.

### Arquitetura
- Primeira integração funcional da camada de conhecimento com modelos de embeddings.
- Consolidação do `KnowledgeService` como fachada da infraestrutura de RAG.
- Preparação da arquitetura para a introdução do banco vetorial na próxima aula.
```

---

## O que vem agora?

Na **M7.4**, acontece uma das maiores mudanças do módulo.

Até agora o Prometheus sabe gerar vetores... **mas os descarta imediatamente**.

Na próxima aula, construiremos o primeiro **Vector Store**. Pela primeira vez, o Prometheus conseguirá **armazenar representações semânticas** e preparar o terreno para responder perguntas utilizando conhecimento externo — o verdadeiro começo do RAG.