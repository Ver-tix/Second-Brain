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
Perfeito. As respostas do professor deixam a implementação muito bem delimitada. Antes de escrever qualquer código, vale destacar os princípios arquiteturais que esta aula está ensinando, porque eles vão se repetir durante todo o módulo de RAG.

## O que aprendemos com as respostas

### 1. O VectorStore é um repositório, não um processador

Sua única responsabilidade será armazenar e fornecer acesso aos documentos.

Ele **não**:

- gera embeddings;
    
- faz buscas;
    
- calcula similaridade;
    
- conhece a OpenAI.
    

Ele apenas guarda informações.

---

### 2. O EmbeddingService continua especializado

Sua única responsabilidade permanece:

```text
Texto
   ↓
Embedding
```

Nada mais.

---

### 3. O KnowledgeService cresce como um mini-orquestrador

O professor até destacou esse padrão explicitamente.

Assim como temos:

```text
MentorAgent
        │
        ├── LLMService
        ├── ToolManager
        ├── ConversationMemory
        └── KnowledgeService
```

Agora teremos:

```text
KnowledgeService
        │
        ├── EmbeddingService
        └── VectorStore
```

Ou seja, estamos repetindo o mesmo padrão arquitetural em outro nível da aplicação.

---

### 4. O restante do sistema continua isolado

Nem o `MentorAgent`, nem o `main.py` saberão:

- como embeddings são gerados;
    
- onde são armazenados;
    
- qual estrutura o VectorStore utiliza.
    

Eles conversarão apenas com:

```python
knowledge_service.add_document(...)
```

Essa é uma excelente demonstração de encapsulamento.

---

# Plano para implementar o laboratório

Seguiremos exatamente esta ordem:

### Etapa 1

Criar:

```
app/
└── knowledge/
    └── vector_store.py
```

---

### Etapa 2

Implementar:

```python
class VectorStore:
```

com

```python
self.documents = []
```

---

### Etapa 3

Adicionar

```python
add_document(
    text,
    embedding
)
```

---

### Etapa 4

Adicionar

```python
count_documents()

list_documents()
```

---

### Etapa 5

Modificar o `KnowledgeService`.

Agora ele passará a receber:

```python
EmbeddingService
VectorStore
```

e implementará

```python
add_document(text)
```

fazendo:

```text
texto
      ↓
EmbeddingService
      ↓
embedding
      ↓
VectorStore
```

Também criaremos as fachadas:

```python
count_documents()

list_documents()
```

---

### Etapa 6

Atualizar o `main.py`.

---

### Etapa 7

Responder à reflexão arquitetural.

---

Como nas últimas aulas, recomendo fazermos uma etapa por vez. Comecemos pela **Etapa 1 e 2**, criando o arquivo `vector_store.py` e implementando a classe com sua estrutura interna. Depois você me envia o código para revisão e seguimos para a próxima etapa.