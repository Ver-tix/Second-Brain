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

---
# (Após enviar o código para o Monitor):
Excelente. Está exatamente no espírito da Aula 7.1. Fiz uma revisão arquitetural e encontrei apenas observações de estilo — **nenhuma correção obrigatória**.

## `knowledge_service.py`

```python
class KnowledgeService:
    pass
```

✅ Perfeito.

Respeita exatamente o que o professor pediu: apenas introduzir o componente, sem antecipar a API.

---

## `MentorAgent`

A alteração ficou ótima.

Você apenas adicionou:

```python
knowledge_service: KnowledgeService
```

e

```python
self.knowledge_service = knowledge_service
```

Sem utilizá-lo em lugar nenhum.

Era exatamente isso que a aula queria.

---

## `main.py`

Também ficou correto.

Fluxo:

```text
main.py
    │
    ├── LLMService
    ├── ConversationMemory
    ├── ToolManager
    └── KnowledgeService
            │
            ▼
      MentorAgent
```

Ou seja, agora o `MentorAgent` recebe quatro serviços independentes.

Isso prepara o terreno para o RAG sem alterar a lógica do agente.

---

## Um detalhe de estilo (não precisa alterar)

Este comentário:

```python
# Cria o agente, recebendo o serviço
```

agora poderia ficar um pouco mais preciso:

```python
# Cria o agente, injetando suas dependências
```

ou

```python
# Cria o agente com todos os serviços necessários
```

Isso comunica melhor o padrão arquitetural que vocês estão utilizando (injeção de dependência).

---

## Estado atual da arquitetura

Após esta aula, o Prometheus está aproximadamente assim:

```text
                 main.py
                    │
    ┌───────────────┼────────────────┐
    ▼               ▼                ▼
LLMService   ConversationMemory  ToolManager
                                       │
                                       ▼
                                 ToolRegistry
                                       │
                                       ▼
                                CalculatorTool

                    ▼
            KnowledgeService
                    │
                    ▼
               (vazio por enquanto)

                    │
                    ▼
              MentorAgent
                    │
                    ▼
             PromptBuilder
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
   System Prompt       Mentor Prompt
```

A grande novidade desta aula não é uma funcionalidade, mas um **ponto de extensão**: a arquitetura já sabe que existe uma camada de conhecimento, embora ela ainda não faça nada. Nas próximas aulas, iremos preenchê-la gradualmente com `EmbeddingService`, `VectorStore`, `Retriever` e os demais componentes do pipeline de RAG.

**Veredito:** ✅ Laboratório aprovado. Podemos seguir para a Etapa 5 (Reflexão Arquitetural).