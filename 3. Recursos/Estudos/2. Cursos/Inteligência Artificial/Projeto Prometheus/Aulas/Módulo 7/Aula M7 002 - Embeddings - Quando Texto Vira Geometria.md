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
Até agora, tudo no Prometheus funciona com texto.

```text
Usuário
    ↓
Texto
    ↓
LLM
    ↓
Texto
```

Mas existe um problema enorme.

Imagine que seu Second Brain possui estas duas notas:

```
Nota A:
"Bitcoin é um ativo escasso."

Nota B:
"BTC possui oferta limitada."
```

Agora você pergunta:

> "Por que o Bitcoin é raro?"

Uma busca por palavras-chave teria dificuldade.

Ela procuraria algo como:

```
raro
```

Mas nenhuma nota possui essa palavra.

Mesmo assim, nós humanos sabemos imediatamente que as três frases falam praticamente da mesma ideia.

Como uma máquina faz isso?

---

# A grande limitação da busca textual

Imagine um banco enorme.

Buscar por texto significa fazer algo parecido com:

```
"bitcoin"
```

↓

Encontrar documentos contendo:

```
bitcoin
```

Isso funciona.

Mas agora:

```
criptomoeda
```

↓

Documento contém:

```
BTC
```

A busca já piora.

Agora:

```
ativo digital descentralizado
```

Pior ainda.

Ou seja:

**palavras diferentes podem representar exatamente o mesmo significado.**

---

# A ideia revolucionária

Em vez de representar um documento como texto...

Representamos como um ponto em um espaço matemático.

Por exemplo:

```
"gato"

↓

[0.12, -0.81, 0.44, ..., 0.91]
```

Ou:

```
"felino doméstico"

↓

[0.13, -0.79, 0.46, ..., 0.89]
```

Observe.

Os vetores são diferentes.

Mas são MUITO próximos.

Isso não é coincidência.

Foi aprendido durante o treinamento do modelo.

---

# O que é um embedding?

Um embedding é simplesmente:

> Uma representação numérica do significado de um texto.

Não da escrita.

Do significado.

---

# Uma analogia

Imagine um mapa.

Fortaleza:

```
(-3.73, -38.54)
```

Recife:

```
(-8.05, -34.90)
```

São cidades diferentes.

Mas estão relativamente próximas.

Agora:

Fortaleza

↓

Tóquio

Muito mais distante.

Embeddings fazem exatamente isso.

Só que em centenas (ou milhares) de dimensões.

---

# Um exemplo intuitivo

Imagine um universo simplificado de apenas duas dimensões.

```
                 Animal

                    ↑

 cachorro

      gato

              leão


────────────────────────────────→ Veículo

                        carro

                             ônibus

                                  caminhão
```

Não importa a palavra.

Importa onde ela está.

---

# A consequência arquitetural

Agora a busca deixa de ser:

```
Encontrar palavra.
```

E passa a ser:

```
Encontrar significado parecido.
```

Esse é o coração do RAG moderno.

---

# Onde isso entra no Prometheus?

Nossa arquitetura crescerá.

Hoje:

```text
MentorAgent

↓

KnowledgeService
```

Depois desta aula:

```text
MentorAgent

↓

KnowledgeService

↓

EmbeddingService
```

Ainda não teremos banco vetorial.

Só aprenderemos a gerar embeddings.

---

# O papel do EmbeddingService

Ele terá uma única responsabilidade:

```
Texto

↓

Embedding
```

Mais nada.

Nem salvar.

Nem buscar.

Nem consultar documentos.

Somente transformar significado em vetor.

Mais uma vez aplicando o princípio da responsabilidade única.

---

# Laboratório 7.2 — Criando o `EmbeddingService`

Hoje construiremos a primeira peça real da infraestrutura de RAG.

## [[🤖 Monitoria M7 002]] 
## [[🛠 Desafio M7 002]] 
## Objetivo

Adicionar ao Prometheus um serviço especializado em gerar embeddings.

Ainda **não** armazenaremos esses vetores. Isso ficará para a próxima aula.

---

## Etapa 1

Crie o arquivo:

```text
app/
└── knowledge/
    └── embedding_service.py
```

---

## Etapa 2

Crie a classe:

```python
class EmbeddingService:
    ...
```

Ela será responsável exclusivamente pela geração de embeddings.

---

## Etapa 3

Atualize o `KnowledgeService`.

Ele deverá receber um `EmbeddingService` por injeção de dependência.

Assim como o `MentorAgent` recebe o `KnowledgeService`, o `KnowledgeService` começará a receber seus próprios componentes internos.

Nossa arquitetura ficará em camadas:

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

## Etapa 4

Atualize o `main.py`.

Agora a construção das dependências ficará aproximadamente assim:

```text
EmbeddingService

↓

KnowledgeService

↓

MentorAgent
```

Perceba que começamos a construir uma **árvore de dependências**, em vez de uma lista plana.

---

## Etapa 5 — Reflexão

Responda:

> **Por que o `EmbeddingService` não foi colocado diretamente dentro do `MentorAgent`, mas sim atrás do `KnowledgeService`?**

Essa pergunta é o coração da aula.

Se você responder bem, terá entendido não apenas embeddings, mas também como arquiteturas escaláveis são construídas.

Quando terminar o laboratório, envie o código como fizemos durante todo o Módulo 6 e início do Módulo 7. Farei uma revisão detalhada e, como de costume, gerarei o CHANGELOG da evolução do Prometheus.