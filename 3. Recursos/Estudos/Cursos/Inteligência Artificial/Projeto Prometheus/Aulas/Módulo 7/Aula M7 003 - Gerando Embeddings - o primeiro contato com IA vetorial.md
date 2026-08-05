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
Hoje é um daqueles dias em que o Prometheus deixa de ser apenas um chatbot bem organizado e começa a adquirir uma nova capacidade.

Até agora, ele entendia texto.

A partir de hoje, ele começará a transformar significado em vetores.

---

# O problema

Até agora criamos isto:

```text
MentorAgent
        │
        ▼
KnowledgeService
        │
        ▼
EmbeddingService
```

Mas o `EmbeddingService` é uma casca vazia.

Ele ainda não faz absolutamente nada.

Hoje ele ganhará sua primeira responsabilidade.

---

# O que é gerar um embedding?

Até agora usamos um modelo de linguagem.

```text
Texto

↓

LLM

↓

Texto
```

Agora utilizaremos outro tipo de modelo.

```text
Texto

↓

Embedding Model

↓

Vetor Numérico
```

Observe.

Ele **não responde perguntas**.

Ele não escreve.

Ele não conversa.

Sua única função é converter significado em números.

---

# A arquitetura

Até hoje nosso fluxo termina aqui:

```text
Texto

↓

EmbeddingService

↓

???
```

Depois desta aula:

```text
Texto

↓

EmbeddingService

↓

Embedding
```

Ainda não armazenaremos esse embedding.

Isso será papel do banco vetorial na próxima aula.

---

# Onde entra a OpenAI?

Você já conhece:

```python
client.responses.create(...)
```

Hoje conhecerá outro endpoint.

```python
client.embeddings.create(...)
```

Perceba algo importante.

Continuamos usando o mesmo cliente.

Só mudamos o serviço utilizado.

Isso reforça uma ideia importante:

> A OpenAI oferece vários modelos especializados, não apenas modelos de conversa.

---

# O modelo de embeddings

Utilizaremos:

```text
text-embedding-3-small
```

Não porque seja o único.

Mas porque ele oferece excelente qualidade, baixo custo e é amplamente utilizado em aplicações de RAG.

==Mais tarde, no Prometheus OS, será possível trocar esse modelo sem alterar o restante da arquitetura.==

---

# Laboratório M7.3 — O primeiro embedding do Prometheus

Hoje construiremos a primeira funcionalidade real da camada de conhecimento.

## [[🤖 Monitoria M7 003]] 
## [[🛠 Desafio M7003]] 

## Objetivo

Fazer o `EmbeddingService` transformar um texto em um vetor numérico utilizando a API da OpenAI.

---

## Etapa 1 — Atualizar `settings.py`

Centralize também o modelo de embeddings.

Adicione uma nova constante:

```python
EMBEDDING_MODEL = "text-embedding-3-small"
```

Assim como fizemos com `MODEL_NAME`, evitamos espalhar strings pelo projeto.

---

## Etapa 2 — Implementar o `EmbeddingService`

Atualize `embedding_service.py`.

Agora ele deverá:

- criar um cliente da OpenAI;
    
- utilizar a `OPENAI_API_KEY`;
    
- utilizar `EMBEDDING_MODEL`;
    
- possuir um método:
    

```python
generate_embedding(text: str)
```

Esse método deverá chamar:

```python
client.embeddings.create(...)
```

e retornar **apenas o vetor** (`embedding`), sem lógica adicional.

A responsabilidade continua sendo única:

```text
Texto

↓

Embedding

↓

return
```

---

## Etapa 3 — Atualizar o `KnowledgeService`

Nesta aula, ele ainda **não fará buscas**.

Mas vamos adicionar um pequeno método de fachada:

```python
generate_embedding(text: str)
```

que simplesmente delega para:

```python
self.embedding_service.generate_embedding(text)
```

Por quê?

Porque queremos que, no futuro, o restante do sistema converse apenas com o `KnowledgeService`, e não diretamente com seus componentes internos.

---

## Etapa 4 — Teste temporário no `main.py`

Apenas para validar que tudo funciona, antes da criação do banco vetorial.

Depois de criar o `KnowledgeService`, faça um teste simples:

```python
embedding = knowledge_service.generate_embedding(
    "Bitcoin é um ativo escasso."
)

print(len(embedding))
```

Não imprima o vetor inteiro.

Apenas seu tamanho.

Se tudo estiver correto, você verá um número inteiro (a dimensionalidade do embedding).

Na próxima aula esse teste será removido.

---

## Etapa 5 — Reflexão Arquitetural

Responda:

> **Por que criamos um método `generate_embedding()` no `KnowledgeService`, se ele apenas encaminha a chamada para o `EmbeddingService`? Não seria mais simples chamar o `EmbeddingService` diretamente?**

Essa é uma pergunta sobre encapsulamento e desenho de APIs, não sobre Python.

---

### Observação importante

Como você ainda **não gerou sua chave da OpenAI** para evitar custos, é esperado que você **não consiga executar** essa integração localmente agora. Isso **não é um erro da implementação**.

O objetivo desta aula é construir corretamente a arquitetura e a integração. Quando você adicionar a chave no futuro, essa parte passará a funcionar sem necessidade de refatoração.

Quando concluir o laboratório, envie os arquivos como sempre. Farei a revisão detalhada e gerarei o CHANGELOG da **M7.3**. 🚀