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
Professor corrigindo. Excelente trabalho. Esta foi uma aula simples em código, mas muito importante arquiteturalmente. Vou avaliar como se fosse uma revisão de Pull Request.

---

# Avaliação geral

**Nota: 10/10**

Você seguiu exatamente o nível de abstração da aula.

O que mais gostei foi que você **não antecipou** nenhuma implementação de RAG. Isso mostra maturidade arquitetural.

---

# Etapa 1 — Estrutura

```text
app/
└── knowledge/
    ├── __init__.py
    └── knowledge_service.py
```

Perfeito.

Nada a alterar.

---

# Etapa 2 — KnowledgeService

```python
class KnowledgeService:
    pass
```

Exatamente o que eu esperava.

Muita gente teria criado:

- search()
    
- retrieve()
    
- get_context()
    

Todas essas abstrações seriam prematuras.

Você resistiu à tentação.

Excelente.

---

# Etapa 3 — MentorAgent

Você fez exatamente a mudança necessária:

```python
knowledge_service: KnowledgeService
```

e

```python
self.knowledge_service = knowledge_service
```

Sem utilizar o serviço.

Perfeito.

É assim que uma arquitetura cresce.

---

# Observação arquitetural importante

Quero destacar uma evolução que talvez você ainda não tenha percebido.

Observe o construtor do Mentor hoje:

```python
MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service
)
```

Ele já está ficando parecido com um verdadeiro agente.

Cada dependência representa uma capacidade distinta.

Hoje ele possui:

- inteligência
    
- memória
    
- ferramentas
    
- conhecimento
    

Nos próximos módulos isso ficará ainda mais evidente.

---

# Etapa 4 — main.py

Perfeito.

Gostei especialmente porque você apenas fez:

```python
knowledge_service = KnowledgeService()
```

e injetou.

Sem chamadas.

Sem testes artificiais.

Exatamente como deveria ser.

---

# Etapa 5 — Reflexão

Essa foi a melhor parte da entrega.

Você escreveu:

> "...quando adicionarmos componentes como Retriever, EmbeddingService e VectorStore, eles serão incorporados ao KnowledgeService sem exigir mudanças na estrutura do agente."

Essa frase mostra que você entendeu o verdadeiro objetivo da aula.

A aula nunca foi sobre criar uma classe vazia.

Foi sobre evitar isto no futuro:

```text
MentorAgent

↓

(reescrever metade do projeto)

↓

RAG
```

Em vez disso, fizemos:

```text
MentorAgent

↓

KnowledgeService

↓

(em breve)

Retriever

↓

Vector Store

↓

Embeddings
```

Toda a evolução acontecerá "atrás" do `KnowledgeService`, mantendo a interface do agente estável.

Isso é um exemplo clássico do princípio **Open/Closed**: o `MentorAgent` permanecerá fechado para modificações, enquanto o `KnowledgeService` será expandido internamente.

---

# Uma observação interessante

Se você olhar para trás, verá que o Prometheus evoluiu de forma muito consistente.

No início do Módulo 6, o `MentorAgent` conhecia praticamente tudo.

Hoje ele apenas coordena serviços especializados:

```text
MentorAgent
│
├── LLMService
├── ConversationMemory
├── ToolManager
├── PromptBuilder
└── KnowledgeService
```

Isso é exatamente o que queremos de uma arquitetura limpa: o agente atua como um **orquestrador**, enquanto cada componente tem uma responsabilidade bem definida.

---

# Conclusão

A Aula 1 do Módulo 7 está concluída com sucesso.

E uma observação final: a partir da próxima aula, começaremos a trabalhar com **embeddings**. Na minha experiência, esse é o conceito que mais muda a forma como alguém enxerga sistemas de IA depois de entender Transformers. É o ponto em que o "texto" deixa de ser apenas texto e passa a ser representado como geometria em um espaço vetorial. É uma das ideias mais elegantes de toda a IA moderna.

Como combinado no Projeto Prometheus, segue o **CHANGELOG** desta evolução:

```markdown
## M7-001 — Fundação da Camada de Conhecimento

### Adicionado
- Nova pasta `app/knowledge/`.
- Classe `KnowledgeService` criada como ponto de entrada da futura infraestrutura de RAG.

### Alterado
- `MentorAgent` passou a receber `KnowledgeService` por injeção de dependência.
- `main.py` atualizado para instanciar e injetar o novo serviço.

### Arquitetura
- Introduzida a camada de conhecimento sem alterar o fluxo atual do agente.
- Preparada a arquitetura para integração futura de `Retriever`, `EmbeddingService`, `VectorStore` e pipeline de RAG.
```

Parabéns. O Prometheus agora possui um espaço reservado para a sua futura "memória de longo prazo". Esse pequeno passo será a base de praticamente todo o restante do Módulo 7.