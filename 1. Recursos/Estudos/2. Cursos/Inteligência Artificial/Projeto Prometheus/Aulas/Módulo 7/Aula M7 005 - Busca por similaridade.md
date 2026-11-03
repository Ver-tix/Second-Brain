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
Chegamos a uma aula **central para o RAG**.

Até aqui, construímos:

```text
Documento
   │
   ▼
EmbeddingService
   │
   ▼
Embedding
   │
   ▼
VectorStore
```

O problema é que nosso `VectorStore` atualmente só sabe **guardar**.

Hoje vamos ensiná-lo a **encontrar**.

---

# 1. O problema que precisamos resolver

Imagine que nosso Vector Store tenha:

```text
"A história da inteligência artificial começou..."
"Python é uma linguagem de programação..."
"Bitcoin possui oferta máxima limitada..."
"Redes neurais utilizam parâmetros..."
```

Agora o usuário pergunta:

> "Qual é o limite de emissão do Bitcoin?"

A pergunta também pode virar um embedding:

```text
Pergunta
   │
   ▼
EmbeddingService
   │
   ▼
Query Embedding
```

Temos então dois vetores:

```text
Documento → vetor
Pergunta  → vetor
```

Precisamos descobrir:

> **Qual vetor representa o documento semanticamente mais próximo da pergunta?**

É isso que chamamos de **busca por similaridade**.

---

# 2. Uma distinção fundamental

Não estamos procurando simplesmente palavras iguais.

Por exemplo:

> "Qual é o limite de emissão do Bitcoin?"

e:

> "Bitcoin possui uma quantidade máxima de moedas."

Não possuem exatamente as mesmas palavras.

Mas seus significados são próximos.

É justamente essa propriedade que os embeddings nos dão.

---

# 3. Similaridade por Cosseno

Para esta primeira implementação, vamos utilizar **similaridade de cosseno**.

A ideia intuitiva é simples:

```text
        vetor A
          ↗
         /
        / θ
       /
───────→ vetor B
```

Quanto menor o ângulo entre os vetores, maior a similaridade.

Podemos representar conceitualmente:

```text
similaridade ≈ quão próxima é a direção dos vetores
```

**Mas atenção:** o widget acima não é necessário para implementar a aula; o conceito matemático relevante aqui é a similaridade de vetores, não momentum. Portanto, vamos manter a implementação simples e focada no laboratório.

---

# 4. O que muda na arquitetura?

Anteriormente:

```text
KnowledgeService
      │
      ├── EmbeddingService
      │
      └── VectorStore
```

Agora:

```text
KnowledgeService
      │
      ├── EmbeddingService
      │
      └── VectorStore
                │
                ▼
        busca por similaridade
```

O `VectorStore` ganhará uma nova responsabilidade:

```python
search(...)
```

Ele receberá um vetor de consulta e comparará esse vetor com os embeddings armazenados.

---

# 5. Laboratório M7.5

## [[🤖 Monitoria M7 005]] 
## [[🛠 Desafio M7 005]] 

## Objetivo

Ao final desta aula, queremos conseguir fazer:

```text
Pergunta
   │
   ▼
EmbeddingService
   │
   ▼
Query Embedding
   │
   ▼
VectorStore
   │
   ▼
Documentos mais similares
```

Ainda **não** vamos:

- construir RAG completo;
    
- montar prompt com contexto;
    
- fazer o LLM responder usando os documentos;
    
- criar ranking sofisticado;
    
- adicionar banco vetorial externo.
    

Hoje vamos construir apenas a **recuperação semântica**.

---

# Etapa 1 — Criar a função de similaridade

Crie um arquivo:

```text
app/knowledge/similarity.py
```

Nele, crie uma função:

```python
def cosine_similarity(
    vector_a: list[float],
    vector_b: list[float]
) -> float:
```

Ela deverá calcular a similaridade de cosseno entre os dois vetores.

### Importante

Não use:

- NumPy;
    
- bibliotecas de machine learning;
    
- bibliotecas de banco vetorial.
    

Queremos implementar a matemática **manualmente** nesta primeira versão.

Isso é proposital.

Quero que você veja que, por baixo de uma ferramenta como FAISS ou Chroma, existe uma operação matemática relativamente simples.

---

# Etapa 2 — Adicionar busca ao `VectorStore`

Agora alteraremos:

```text
app/knowledge/vector_store.py
```

Crie:

```python
search(
    query_embedding: list[float],
    top_k: int = 3
)
```

O método deverá:

1. percorrer os documentos armazenados;
    
2. calcular a similaridade entre o `query_embedding` e cada embedding;
    
3. associar o score ao documento;
    
4. ordenar os resultados pelo score;
    
5. retornar os `top_k` mais similares.
    

Conceitualmente:

```text
Query
 │
 ├── comparação → Documento A → 0.21
 ├── comparação → Documento B → 0.87
 ├── comparação → Documento C → 0.42
 └── comparação → Documento D → 0.91
                         │
                         ▼
                    ordenar
                         │
                         ▼
                  top_k resultados
```

---

# Etapa 3 — Não esconda o score

Os resultados deverão trazer também a similaridade.

Algo conceitualmente semelhante a:

```python
{
    "text": "...",
    "embedding": [...],
    "score": 0.91
}
```

Por quê?

Porque o sistema precisa saber **quão relevante** foi aquele resultado.

Esse score será extremamente importante mais adiante.

---

# Etapa 4 — Atualizar o `KnowledgeService`

Agora crie uma operação de recuperação.

Sugestão:

```python
search(query: str, top_k: int = 3)
```

O fluxo será:

```text
query
 │
 ▼
EmbeddingService
 │
 ▼
query_embedding
 │
 ▼
VectorStore.search()
 │
 ▼
resultados
```

Perceba a importância arquitetural:

O `MentorAgent` **não precisa saber**:

- o que é cosine similarity;
    
- como embeddings funcionam;
    
- como o Vector Store ordena documentos.
    

Ele simplesmente poderá futuramente pedir:

```python
knowledge_service.search(...)
```

---

# Etapa 5 — Teste

Adicione alguns documentos:

```python
knowledge_service.add_document(
    "Bitcoin possui uma oferta máxima de 21 milhões de moedas."
)

knowledge_service.add_document(
    "Python é uma linguagem de programação de alto nível."
)

knowledge_service.add_document(
    "Redes neurais artificiais são modelos computacionais."
)
```

Depois faça uma consulta semanticamente relacionada ao primeiro documento.

Por exemplo:

```text
Qual é a quantidade máxima de Bitcoins?
```

O primeiro documento deverá aparecer entre os resultados mais relevantes.

---

# Etapa 6 — Reflexão

Responda:

> **Por que precisamos transformar a pergunta do usuário em um embedding antes de consultar o Vector Store?**

E uma segunda:

> **Por que o `MentorAgent` não deveria calcular a similaridade diretamente?**

Essas duas respostas são mais importantes que a implementação matemática.

---

# 🧠 O insight arquitetural da M7.5

Observe o que está acontecendo.

Antes:

```text
KnowledgeService
        │
        ▼
    armazenar
```

Agora:

```text
KnowledgeService
        │
        ├── ingestão
        │      │
        │      ▼
        │   armazenamento
        │
        └── recuperação
               │
               ▼
          similaridade
               │
               ▼
          documentos
```

Estamos começando a separar duas operações fundamentais de qualquer sistema RAG:

### Ingestion

```text
documento
   ↓
chunk
   ↓
embedding
   ↓
storage
```

### Retrieval

```text
query
   ↓
embedding
   ↓
similaridade
   ↓
documentos relevantes
```

**Essa separação será importantíssima nas próximas aulas.**

E ainda não chegamos ao RAG completo.

Estamos construindo suas peças, uma por uma.
