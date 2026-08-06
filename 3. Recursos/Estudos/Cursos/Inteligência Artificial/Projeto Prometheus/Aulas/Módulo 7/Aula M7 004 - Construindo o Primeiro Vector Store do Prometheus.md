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
Hoje chegamos a um dos momentos mais importantes de todo o Projeto Prometheus.

Até agora conseguimos fazer isto:

```text
Texto
      │
      ▼
EmbeddingService
      │
      ▼
Embedding (vetor)
```

Mas existe um enorme problema.

Assim que o método termina, o vetor desaparece.

Ou seja:

```text
Texto

↓

Embedding

↓

(lixo)
```

Isso torna impossível responder perguntas usando conhecimento previamente armazenado.

Hoje resolveremos esse problema.

---

# O problema

Imagine que você possua 10.000 documentos.

Você gera embeddings para todos eles.

Depois fecha o programa.

Tudo foi perdido.

Na próxima execução você precisaria gerar tudo novamente.

Isso seria extremamente caro.

Precisamos de um lugar para armazenar esses vetores.

Esse lugar chama-se **Vector Store**.

---

# O que é um Vector Store?

Pense nele como um banco de dados.

Só que, em vez de guardar apenas textos:

```text
ID → Texto
```

ele guarda:

```text
ID
Texto
Embedding
```

Ou seja:

```text
Documento

↓

Embedding

↓

Armazenamento
```

Mais tarde faremos buscas nesse armazenamento.

Mas ainda não.

Hoje aprenderemos apenas a armazenar.

---

# Uma decisão pedagógica importante

No mercado existem diversas soluções:

- ChromaDB
- FAISS
- Pinecone
- Weaviate
- Milvus
- Qdrant

Todas são excelentes.

Mas **não começaremos por elas**.

Quero que você primeiro compreenda a arquitetura.

Por isso, construiremos um **Vector Store em memória**, usando apenas Python.

Depois será extremamente fácil substituir essa implementação por qualquer banco vetorial profissional.

---

# Arquitetura da Aula

Hoje a camada de conhecimento evoluirá.

Antes:

```text
KnowledgeService
        │
        ▼
EmbeddingService
```

Depois:

```text
KnowledgeService
│
├── EmbeddingService
│
└── VectorStore
```

Perceba que o `KnowledgeService` passa a orquestrar dois componentes especializados.

---

# O papel do Vector Store

Nesta aula ele terá apenas três responsabilidades:

```text
armazenar()

listar()

contar()
```

Nada mais.

Ainda não existe busca por similaridade.

Essa responsabilidade pertence à próxima aula.

---

# Laboratório M7.4 — Primeiro Vector Store

## Objetivo

Criar uma infraestrutura capaz de armazenar documentos juntamente com seus embeddings.

---

## Etapa 1 — Criar `vector_store.py`

Dentro de:

```text
app/
└── knowledge/
```

crie:

```text
vector_store.py
```

Implemente uma classe:

```python
class VectorStore:
```

---

## Etapa 2 — Estrutura interna

Utilize apenas uma lista em memória.

Por exemplo:

```python
self.documents = []
```

Cada elemento deverá possuir:

- texto original;
    
- embedding.
    

Você pode representar cada item como um dicionário.

---

## Etapa 3 — Adicionar documentos

Implemente:

```python
add_document(
    text: str,
    embedding: list[float]
)
```

Esse método apenas adiciona o documento à lista.

Nenhuma lógica de busca.

Nenhuma ordenação.

---

## Etapa 4 — Métodos auxiliares

Implemente também:

```python
count_documents()
```

retornando a quantidade armazenada.

E:

```python
list_documents()
```

retornando a lista completa.

Ainda não nos preocuparemos com encapsulamento fino.

---

## Etapa 5 — Atualizar o `KnowledgeService`

Agora ele receberá duas dependências:

```text
EmbeddingService

VectorStore
```

No construtor.

Além disso, implemente:

```python
add_document(text: str)
```

Fluxo esperado:

```text
Texto

↓

EmbeddingService

↓

Embedding

↓

VectorStore

↓

Fim
```

Ou seja:

1. gerar embedding;
    
2. armazenar documento;
    
3. terminar.
    

O `KnowledgeService` continua sendo o único ponto de entrada.

---

## Etapa 6 — Atualizar o `main.py`

Instancie:

```python
vector_store = VectorStore()
```

e injete-o no `KnowledgeService`.

Depois faça um teste simples:

```python
knowledge_service.add_document(
    "Bitcoin é um ativo escasso."
)

knowledge_service.add_document(
    "Ethereum permite contratos inteligentes."
)

print(
    knowledge_service.count_documents()
)
```

Para esta aula, esperamos ver:

```text
2
```

---

## Etapa 7 — Reflexão Arquitetural

Responda:

> **Por que criamos um `VectorStore` separado, em vez de simplesmente armazenar os embeddings dentro do `EmbeddingService`?**

Pense em termos de **responsabilidade única**, **coesão** e **substituição de implementações**.

---

# O que NÃO faremos nesta aula

Ainda **não** implementaremos:

- busca vetorial;
    
- distância de cosseno;
    
- similaridade;
    
- ranking;
    
- recuperação de contexto.
    

Hoje estamos apenas construindo a infraestrutura onde esses recursos irão operar.

---

## Uma prévia da próxima aula

Na **M7.5**, o Prometheus dará seu primeiro passo em direção ao RAG de verdade.

Até agora ele consegue armazenar documentos.

Na próxima aula, aprenderá a responder à pergunta:

> **"Qual dos documentos armazenados é semanticamente mais parecido com esta consulta?"**

Será a primeira vez que o sistema utilizará embeddings não apenas para representar conhecimento, mas para **encontrá-lo**. 🚀