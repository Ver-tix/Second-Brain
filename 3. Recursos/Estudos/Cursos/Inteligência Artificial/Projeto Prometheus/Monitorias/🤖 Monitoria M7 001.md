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
Perfeito. As respostas do professor deixam a direção da aula muito clara: **esta é uma aula de preparação arquitetural, não de implementação de RAG**.

Isso significa que devemos ser extremamente conservadores. Vamos apenas "abrir espaço" na arquitetura para que as próximas aulas preencham esse espaço.

## Etapa 1 — Criar a infraestrutura

Crie a estrutura:

```
app/
└── knowledge/
    ├── __init__.py
    └── knowledge_service.py
```

---

## Etapa 2 — Criar o `KnowledgeService`

Conforme orientação do professor, ele deve ser apenas um esqueleto.

```python
class KnowledgeService:
    pass
```

Não adicione métodos.

Não adicione comentários de TODO.

Não adicione `search()`.

Não adicione `retrieve()`.

A ideia é apenas introduzir um novo componente na arquitetura.

---

## Etapa 3 — Injetar no `MentorAgent`

Agora altere apenas o construtor.

Hoje ele deve estar parecido com:

```python
def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager
):
```

Ele ficará:

```python
def __init__(
    self,
    llm_service: LLMService,
    memory: ConversationMemory,
    tool_manager: ToolManager,
    knowledge_service: KnowledgeService
):
```

E dentro do `__init__`:

```python
self.knowledge_service = knowledge_service
```

Não utilize esse atributo em nenhum outro lugar.

---

## Etapa 4 — Atualizar o `main.py`

Instancie:

```python
knowledge_service = KnowledgeService()
```

E passe para o agente:

```python
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service
)
```

Mais nenhuma alteração.

---

Depois dessas quatro etapas, teremos concluído toda a parte prática da Aula 7.1. A única coisa restante será responder a reflexão arquitetural (Etapa 5), que faremos ao final, como de costume.