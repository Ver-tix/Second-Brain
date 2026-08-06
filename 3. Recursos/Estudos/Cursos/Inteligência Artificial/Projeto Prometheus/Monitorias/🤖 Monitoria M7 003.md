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
Perfeito. Com essas respostas, temos o escopo totalmente delimitado. Vamos implementar exatamente o que a aula pede, sem antecipar abstrações.

# Etapa 1 — Atualizar `settings.py`

Adicione a constante do modelo de embeddings, ao lado da constante do modelo de chat.

```python
OPENAI_API_KEY = ...

MODEL_NAME = "gpt-5"

EMBEDDING_MODEL = "text-embedding-3-small"
```

A ideia é a mesma utilizada para `MODEL_NAME`: evitar "strings mágicas" espalhadas pelo projeto.

---

# Etapa 2 — Implementar o `EmbeddingService`
```text
app/
└── knowledge/
    ├── __init__.py
    ├── knowledge_service.py
    └── embedding_service.py   ← aqui    
```

Agora podemos transformar aquele esqueleto em um serviço funcional.

A estrutura esperada é praticamente um espelho do `LLMService`.

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
        text: str
    ) -> list[float]:

        response = self.client.embeddings.create(
            model=EMBEDDING_MODEL,
            input=text
        )

        return response.data[0].embedding
```

Observe alguns detalhes importantes:
- a classe cria seu próprio `OpenAI()` (consistência com o `LLMService`);
- usa `EMBEDDING_MODEL` centralizado no `settings.py`;
- retorna diretamente `response.data[0].embedding`;
- não trata exceções;
- retorna `list[float]`, deixando explícito o contrato do serviço.
## Por Que retorno `response.data[0].embedding`?
Excelente pergunta. Essa é uma daquelas linhas que vale a pena entender profundamente, porque ela mostra como ler a resposta da API da OpenAI.

Vamos por partes.

---

### O que enviamos para a API?

Nós fazemos:

```python
response = self.client.embeddings.create(
    model=EMBEDDING_MODEL,
    input=text
)
```

Ou seja, enviamos um texto:

```text
"Bitcoin é um ativo escasso."
```

A OpenAI responde com um objeto muito maior do que apenas o embedding.

É algo conceitualmente parecido com isto:

```python
{
    "object": "list",

    "data": [

        {
            "object": "embedding",

            "index": 0,

            "embedding": [
                0.012,
                -0.432,
                0.983,
                ...
            ]
        }

    ],

    "model": "text-embedding-3-small",

    "usage": {
        ...
    }
}
```

Perceba que existem várias informações.

Nós só queremos uma delas.

---

### Por que existe `data`?

Porque a API foi projetada para poder gerar embeddings de **vários textos de uma vez**.

Imagine:

```python
input=[
    "Bitcoin",
    "Ethereum",
    "Solana"
]
```

A resposta seria parecida com:

```python
data = [

    {
        "index": 0,
        "embedding": [...]
    },

    {
        "index": 1,
        "embedding": [...]
    },

    {
        "index": 2,
        "embedding": [...]
    }

]
```

Ou seja:

```
data
│
├── embedding do texto 0
├── embedding do texto 1
└── embedding do texto 2
```

Mesmo enviando apenas um texto, a API mantém esse formato por consistência.

---

### Por que usamos `[0]`?

Porque enviamos **apenas um** texto.

Então:

```python
response.data
```

contém uma lista com um único elemento:

```python
[
    {
        "embedding": [...]
    }
]
```

O primeiro elemento é:

```python
response.data[0]
```

---

### E por que `.embedding`?

Porque o objeto possui várias propriedades:

```python
response.data[0]

↓

{
    "object": "embedding",
    "index": 0,
    "embedding": [...]
}
```

Nós queremos somente:

```python
response.data[0].embedding
```

Que é justamente o vetor.

---

### Visualizando o caminho

```
response
    │
    ▼
data
    │
    ▼
[0]
    │
    ▼
embedding
    │
    ▼
[0.012, -0.432, 0.983, ...]
```

---

### Por que retornamos apenas isso?

Porque o **`EmbeddingService` possui uma única responsabilidade**.

Ele deve transformar:

```
Texto

↓

Embedding
```

Nada mais.

Se retornássemos o objeto inteiro:

```python
return response
```

quem chamasse o serviço teria que saber navegar por:

```python
response.data[0].embedding
```

Isso faria com que detalhes da API da OpenAI "vazassem" para o restante do sistema.

Ao retornar apenas:

```python
return response.data[0].embedding
```

o contrato do `EmbeddingService` fica muito simples:

```
Entrada:
texto

↓

Saída:
list[float]
```

E isso é uma excelente prática de arquitetura: **esconder detalhes de implementação atrás de uma interface simples**.

Essa mesma ideia já apareceu no `LLMService`. O restante do Prometheus não sabe como a OpenAI organiza sua resposta; ele apenas recebe um dicionário padronizado com `"type"` e `"content"`. O `EmbeddingService` faz a mesma coisa: esconde a estrutura da resposta da API e entrega exatamente o que os demais componentes precisam.

---

# Etapa 3 - Atualização do `KnowledgeService`
Atualizar o `KnowledgeService` para expor o método `generate_embedding()` como uma fachada sobre o `EmbeddingService`.

## O que queremos fazer?

Hoje o restante do sistema conhece apenas:

```text
MentorAgent
        │
        ▼
KnowledgeService
```

Ele **não deve conhecer** o `EmbeddingService`.

Portanto, o `KnowledgeService` passa a oferecer uma interface pública para geração de embeddings.

A arquitetura fica:

```text
MentorAgent
        │
        ▼
KnowledgeService
        │
        ▼
EmbeddingService
```

---

## Alteração em `knowledge_service.py`

Seu arquivo hoje está assim:

```python
from app.knowledge.embedding_service import EmbeddingService


class KnowledgeService:

    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service
```

Agora basta acrescentar um método:

```python
def generate_embedding(
    self,
    text: str
) -> list[float]:

    return self.embedding_service.generate_embedding(text)
```

O arquivo completo ficará:

```python
from app.knowledge.embedding_service import EmbeddingService

class KnowledgeService:
    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service

    def generate_embedding(
        self,
        text: str
    ) -> list[float]:
        return self.embedding_service.generate_embedding(text)
```

---

### Por que fazer isso?

À primeira vista parece desnecessário:

```python
KnowledgeService

↓

EmbeddingService

↓

Embedding
```

"Por que não chamar logo o `EmbeddingService`?"

Porque queremos que o restante da aplicação conheça apenas **uma porta de entrada** para a camada de conhecimento.

Hoje:

```text
KnowledgeService
        │
        ▼
EmbeddingService
```

Daqui a algumas aulas será algo como:

```text
KnowledgeService
        │
 ┌──────┼───────────────┐
 ▼      ▼               ▼

Retriever
EmbeddingService
VectorStore
```

E o `MentorAgent` continuará fazendo apenas:

```python
knowledge_service.generate_embedding(...)
```

Ele nunca precisará saber quantos componentes existem dentro da camada de conhecimento.

---

Essa é uma aplicação clássica do princípio do **Encapsulamento**: esconder a complexidade interna atrás de uma interface simples.

Implemente esse método e me envie o `knowledge_service.py`. Depois partiremos para a **Etapa 4**, onde faremos o teste temporário no `main.py`.

# Etapa 4 - Teste temporário no `main.py`
Ótimo! A **Etapa 4** é bem simples. Ela existe apenas para validar que a infraestrutura construída até agora realmente consegue conversar com a API de embeddings.

## Objetivo

Testar o fluxo completo:

```text
main.py
     │
     ▼
KnowledgeService.generate_embedding(...)
     │
     ▼
EmbeddingService.generate_embedding(...)
     │
     ▼
OpenAI Embeddings API
     │
     ▼
list[float]
```

Observe que **não chamamos o `EmbeddingService` diretamente**. Isso é proposital: queremos testar a API pública da camada de conhecimento.

---

## Onde inserir o teste?

Logo após criar o `KnowledgeService` no `main.py`, antes da criação do `MentorAgent`.

Ficará aproximadamente assim:

```python
# Instanciar o serviço de embedding
embedding_service = EmbeddingService()

# Instanciar o serviço de conhecimento
knowledge_service = KnowledgeService(
    embedding_service
)

# ===== Teste temporário =====
embedding = knowledge_service.generate_embedding(
    "Bitcoin é um ativo escasso."
)

print(
    f"Dimensão do embedding: {len(embedding)}"
)
# ===========================

# Cria o agente
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service,
)
```

---

## O que deve acontecer?

### Se você possuir uma API Key válida

O programa imprimirá algo semelhante a:

```text
Dimensão do embedding: 1536
```

O número **1536** é a dimensionalidade do vetor gerado pelo modelo `text-embedding-3-small`.

---

### No seu caso

Como você ainda **não configurou uma chave válida da OpenAI** (por decisão consciente para evitar custos), é esperado que ocorra um erro ao tentar chamar a API.

Isso **não significa que sua implementação esteja errada**.

O professor deixou claro que este teste serve para mostrar **onde a chamada ocorrerá** quando a chave estiver disponível.

---

## Por que imprimir apenas `len(embedding)`?

Porque um embedding é uma lista enorme de números de ponto flutuante.

Algo parecido com:

```python
[
    0.0184,
   -0.0932,
    0.7421,
   -0.1167,
    ...
]
```

Imprimir tudo só poluiria o terminal.

Ao imprimir:

```python
len(embedding)
```

você confirma duas coisas:

1. A API respondeu corretamente.
    
2. O vetor possui a dimensionalidade esperada.
    

---

### Observação arquitetural

Esse teste é **temporário**. Na próxima aula, quando introduzirmos o banco vetorial, ele será removido. O fluxo passará a ser:

```text
Texto
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

Ou seja, o embedding deixará de ser apenas gerado e passará a ser armazenado, preparando o terreno para as buscas semânticas do RAG.

Faça essa alteração no `main.py` e me envie o código. Em seguida, partiremos para a **Etapa 5**, que é a reflexão arquitetural mais importante desta aula.

# Etapa 5 - Reflexão Arquitetural

> **Por que criamos um método `generate_embedding()` no `KnowledgeService`, se ele apenas encaminha a chamada para o `EmbeddingService`? Não seria mais simples chamar o `EmbeddingService` diretamente?**

Criamos esse método para **encapsular a infraestrutura interna da camada de conhecimento** e oferecer uma única interface pública para o restante do sistema.

Hoje, o `KnowledgeService` apenas delega a chamada ao `EmbeddingService`, mas isso é uma decisão arquitetural. O objetivo é impedir que componentes externos, como o `MentorAgent`, dependam diretamente dos detalhes de implementação da camada de conhecimento.

No futuro, o `KnowledgeService` deixará de conversar apenas com o `EmbeddingService` e passará a coordenar diversos componentes, como `Retriever`, `VectorStore`, `DocumentLoader` e `Chunker`. Como o restante da aplicação continuará interagindo apenas com o `KnowledgeService`, essa evolução poderá ocorrer sem modificar o `MentorAgent` ou qualquer outro componente consumidor.

Em outras palavras, o `KnowledgeService` funciona como uma **fachada (Facade)** para a infraestrutura de conhecimento: ele esconde a complexidade interna, reduz o acoplamento entre os módulos e permite que a arquitetura evolua sem exigir refatorações nas camadas superiores. Esse é o mesmo princípio que já aplicamos anteriormente com o `ToolManager` e o `ToolRegistry`: expor uma interface simples enquanto protegemos os detalhes internos da implementação.