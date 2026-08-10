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
A M7.5 terminou justamente na construção do _retrieval_. O próprio material estabelece que, na Aula 6, o próximo passo é integrar o `KnowledgeService` ao `MentorAgent`. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

Portanto, **hoje não vamos construir outra peça isolada**.

Hoje vamos fazer as peças que já construímos **começarem a trabalhar juntas**.

---

# 1. Onde estamos

Até a M7.5, construímos:

```text
                    KnowledgeService
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
      EmbeddingService              VectorStore
              │                         │
              ▼                         ▼
          embeddings              documentos
                                        │
                                        ▼
                              busca por similaridade
```

O `KnowledgeService` já consegue:

### Ingerir conhecimento

```text
documento
   ↓
embedding
   ↓
VectorStore
```

### Recuperar conhecimento

```text
pergunta
   ↓
embedding
   ↓
similaridade
   ↓
documentos relevantes
```

Isso foi exatamente a evolução da M7.4 para a M7.5. 

Mas existe um problema.

**Quem está usando esse conhecimento?**

Até agora, ninguém.

O `MentorAgent` continua essencialmente assim:

```text
Usuário
   ↓
MentorAgent
   ↓
PromptBuilder
   ↓
LLM
   ↓
Resposta
```

> O RAG existe.
> O Mentor existe.
> Mas eles ainda não estão conectados.

---

# 2. É aqui que RAG realmente começa

Vamos voltar à definição:

**RAG = Retrieval-Augmented Generation.**

Até agora construímos o:

```text
Retrieval
```

Mas ainda não fizemos o:

```text
Augmented Generation
```

Ou seja, conseguimos encontrar conhecimento, mas ainda não estamos **usando esse conhecimento para gerar a resposta**.

O fluxo completo precisa tornar-se:

```text
Usuário
   │
   ▼
MentorAgent
   │
   ▼
KnowledgeService
   │
   ▼
Contexto relevante
   │
   ▼
PromptBuilder
   │
   ▼
LLM
   │
   ▼
Resposta
```

Esse é exatamente o fluxo estabelecido para a M7.6 no planejamento do módulo. 

---

# 3. Uma mudança conceitual importante

Observe uma coisa.

Na M7.5, nós fizemos:

```python
knowledge_service.search(question)
```

e recebemos algo como:

```python
[
    {
        "text": "Bitcoin possui uma oferta máxima de 21 milhões...",
        "embedding": [...],
        "score": 0.91
    }
]
```

Isso é **conhecimento recuperado**.

Mas o LLM não sabe que esse resultado existe.

O LLM só recebe aquilo que colocarmos no seu contexto.

Portanto:

```text
VectorStore
    │
    │ encontrou conhecimento
    ▼
KnowledgeService
    │
    │ entrega contexto
    ▼
PromptBuilder
    │
    │ coloca contexto no prompt
    ▼
LLM
```

Essa é a ponte que construiremos hoje.

---

# 4. Onde o RAG NÃO deve entrar

Aqui está uma decisão arquitetural importantíssima.

Poderíamos cometer este erro:

```text
PromptBuilder
│
├── procura documentos
├── gera embeddings
├── calcula similaridade
└── monta prompt
```

**Não faremos isso.**

O `PromptBuilder` possui uma responsabilidade:

> **Construir o prompt.**

Ele não deve saber:

- onde estão os documentos;
    
- como são gerados embeddings;
    
- como funciona a similaridade;
    
- qual Vector Store estamos utilizando.
    

O próprio documento de arquitetura do Módulo 7 estabelece explicitamente essa separação. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

---

# 5. Então quem decide?

Aqui aparece uma mudança importante no `MentorAgent`.

Ele agora terá acesso ao:

```python
knowledge_service
```

E poderá tomar uma decisão:

```text
"Preciso consultar conhecimento externo?"
```

Se sim:

```text
MentorAgent
      │
      ▼
KnowledgeService
      │
      ▼
Contexto
```

Depois:

```text
Contexto
   +
Histórico
   +
Pergunta
   ↓
PromptBuilder
```

E somente então:

```text
Prompt
   ↓
LLMService
   ↓
Resposta
```

O planejamento da aula descreve justamente o Mentor como o componente que decide que precisa de conhecimento externo, enquanto o `KnowledgeService` realiza a busca. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

---

# 6. Mas atenção: o Mentor não vai aprender "RAG"

Essa frase é importante.

Quando dizemos:

> "O agente aprende quando consultar o conhecimento."

não significa que vamos treinar o modelo.

Não estamos fazendo fine-tuning.

Não estamos alterando os pesos do GPT.

Estamos ensinando **o sistema agente** a incorporar uma nova etapa no seu processo de decisão.

Antes:

```text
pergunta
   ↓
LLM
```

Agora:

```text
pergunta
   ↓
MentorAgent
   ↓
[precisa de conhecimento?]
   ↓
KnowledgeService
   ↓
contexto
   ↓
LLM
```

É uma mudança **arquitetural**, não de treinamento.

---

# 7. O papel do `KnowledgeService`

Até agora ele era:

```text
KnowledgeService
│
├── EmbeddingService
└── VectorStore
```

Ele continua exatamente assim.

Não vamos colocar o `LLMService` dentro dele.

Não vamos colocar o `PromptBuilder` dentro dele.

Não vamos transformar o `KnowledgeService` em um agente.

Sua responsabilidade continua sendo:

> **gerenciar a recuperação e o armazenamento do conhecimento.**

Portanto:

```text
MentorAgent
     │
     │ "me dê conhecimento relevante"
     ▼
KnowledgeService
     │
     ├── EmbeddingService
     └── VectorStore
     │
     ▼
contexto
```

---

# 8. O que é "contexto" nesse momento?

Imagine que o usuário pergunte:

> "Qual é a oferta máxima do Bitcoin?"

O `KnowledgeService` pode retornar:

```text
Documento:
"Bitcoin possui uma oferta máxima de 21 milhões de moedas."

Score:
0.91
```

Esse documento passa a ser **contexto externo**.

O LLM receberá algo conceitualmente parecido com:

```text
CONTEXTO DE CONHECIMENTO:

Bitcoin possui uma oferta máxima de 21 milhões de moedas.

PERGUNTA:

Qual é a oferta máxima do Bitcoin?
```

Então temos:

```text
             Conhecimento recuperado
                       │
                       ▼
Pergunta ───────► PromptBuilder
                       │
                       ▼
                      LLM
```

É isso que transforma retrieval em **retrieval-augmented generation**.

---

# 9. O `PromptBuilder` entra novamente em cena

Na M6, construímos o `PromptBuilder` para separar:

```text
construção do prompt
```

do restante do sistema.

Agora ele ganha uma nova informação:

```text
contexto
```

Portanto, conceitualmente, podemos chegar a algo como:

```python
PromptBuilder.build(
    history,
    question,
    context
)
```

O importante aqui não é decorar a assinatura.

O importante é entender **quem produz cada coisa**:

|Informação|Responsável|
|---|---|
|Pergunta|Usuário|
|Histórico|`ConversationMemory`|
|Contexto|`KnowledgeService`|
|Construção do prompt|`PromptBuilder`|
|Geração|`LLMService`|

Essa tabela é praticamente o coração arquitetural da aula.

---

# 10. A arquitetura completa

Depois desta aula, nosso Mentor começará a se parecer com:

```text
                         Usuário
                            │
                            ▼
                     ┌─────────────┐
                     │ MentorAgent │
                     └──────┬──────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
       ConversationMemory       KnowledgeService
                                       │
                              ┌────────┴────────┐
                              ▼                 ▼
                     EmbeddingService      VectorStore
                                                │
                                                ▼
                                         conhecimento
                                                │
                                                ▼
                                           contexto
                 └──────────┬──────────────────┘
                            ▼
                     PromptBuilder
                            │
                            ▼
                       LLMService
                            │
                            ▼
                         resposta
```

Agora temos, pela primeira vez:

```text
Agent
 +
Tools
 +
Memory
 +
Knowledge
```

Essa é justamente a evolução arquitetural central do Módulo 7. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

---

# 11. Uma distinção importantíssima: memória ≠ conhecimento

Isso merece ser reforçado.

Temos agora duas coisas diferentes:

### ConversationMemory

Responde:

> "O que aconteceu durante nossa interação?"

```text
Usuário: ...
Mentor: ...
Usuário: ...
Mentor: ...
```

### KnowledgeService

Responde:

> "O que existe no meu acervo de conhecimento relevante para esta pergunta?"

```text
Livro
Nota
Documento
Artigo
...
```

Portanto:

```text
                 MentorAgent
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
ConversationMemory       KnowledgeService
     │                         │
     ▼                         ▼
 contexto conversacional   conhecimento externo
```

O documento do Módulo 7 faz explicitamente essa distinção entre memória de conversa e _knowledge memory_. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

Isso será extremamente importante quando o Prometheus crescer.

---

# 12. O fluxo completo de uma pergunta

Vamos acompanhar uma pergunta do início ao fim.

Usuário:

> "Qual é a estratégia de posicionamento que aparece nas minhas notas?"

### Passo 1

`MentorAgent` recebe a pergunta.

### Passo 2

Ele decide que a resposta depende de conhecimento externo.

### Passo 3

Solicita ao:

```text
KnowledgeService
```

### Passo 4

O `KnowledgeService` transforma a pergunta em embedding.

### Passo 5

O `VectorStore` procura os documentos semanticamente mais próximos.

### Passo 6

O `KnowledgeService` devolve os resultados relevantes.

### Passo 7

O `MentorAgent` entrega esse contexto ao `PromptBuilder`.

### Passo 8

O `PromptBuilder` constrói o prompt.

### Passo 9

O `LLMService` envia o prompt ao modelo.

### Passo 10

O modelo produz a resposta.

---

# 13. O que vamos modificar

Agora que entendemos o fluxo, o laboratório será relativamente pequeno.

Vamos mexer principalmente em **três pontos**:

### `MentorAgent`

Passará a conhecer:

```python
KnowledgeService
```

e utilizará o conhecimento recuperado.

### `PromptBuilder`

Passará a receber o contexto recuperado e incorporá-lo ao prompt.

### `main.py`

Passará a construir a árvore de dependências:

```text
EmbeddingService
       ↓
VectorStore
       ↓
KnowledgeService
       ↓
MentorAgent
```

O restante da arquitetura permanece intacto.

---

# 14. Uma decisão deliberada da aula

Não vamos transformar isso ainda em um sistema sofisticado de decisão.

Não vamos criar:

- um agente separado de RAG;
    
- um classificador complexo;
    
- um roteador LLM;
    
- thresholds sofisticados;
    
- reranking;
    
- memória semântica;
    
- banco vetorial externo.
    

Essas coisas pertencem a etapas posteriores da arquitetura.

Nesta aula queremos provar uma coisa muito mais fundamental:

> **O conhecimento recuperado consegue entrar no ciclo de geração do Mentor?**

Se a resposta for sim, teremos construído nosso primeiro **RAG end-to-end conceitual**.

---

# 15. O laboratório

Agora vamos transformar tudo isso em código.

## Etapa 1 — Injetar `KnowledgeService`

Atualize o `MentorAgent` para receber:

```python
knowledge_service: KnowledgeService
```

e armazená-lo.

---

## Etapa 2 — Recuperar contexto

Dentro do fluxo de `ask()`, o Mentor deverá utilizar:

```python
self.knowledge_service.search(...)
```

para recuperar conhecimento relevante.

O resultado da busca da M7.5 será a fonte do contexto.

---

## Etapa 3 — Preparar o contexto

Transformaremos os resultados recuperados em um texto que possa ser enviado ao `PromptBuilder`.

Conceitualmente:

```text
Resultado 1
Resultado 2
Resultado 3
```

↓

```text
Contexto
```

Não vamos criar ainda uma abstração separada para isso.

---

## Etapa 4 — Atualizar o `PromptBuilder`

O `PromptBuilder` deverá receber o contexto.

Sua responsabilidade continuará sendo exclusivamente:

```text
dados
 ↓
prompt
```

Não:

```text
dados
 ↓
buscar documentos
```

---

## Etapa 5 — Fechar o ciclo

O fluxo final deverá ser:

```text
Pergunta
   │
   ▼
MentorAgent
   │
   ▼
KnowledgeService
   │
   ▼
Retrieval
   │
   ▼
Contexto
   │
   ▼
PromptBuilder
   │
   ▼
LLMService
   │
   ▼
Resposta
```

---

# 🧪 Teste que queremos conseguir fazer

Imagine que você tenha colocado no `VectorStore`:

```text
"Bitcoin possui uma oferta máxima de 21 milhões de moedas."
```

E pergunte ao Mentor:

> **"Qual é a quantidade máxima de Bitcoins?"**

O sistema deverá:

```text
Pergunta
   ↓
Embedding
   ↓
Busca
   ↓
Documento sobre Bitcoin
   ↓
Contexto
   ↓
Prompt
   ↓
LLM
```

E agora há uma diferença fundamental: 

**o modelo não está respondendo apenas com aquilo que já sabia.**

Ele está recebendo conhecimento recuperado pelo nosso sistema.

---

# 🧠 Reflexão arquitetural

Antes de implementar, quero que você tenha clareza sobre duas perguntas.

### 1.

> **Por que o `KnowledgeService` deve recuperar o contexto antes de o `PromptBuilder` montar o prompt?**

### 2.

> **Por que não devemos colocar a lógica de busca vetorial dentro do `PromptBuilder`?**

Se você compreender essas duas respostas, compreendeu a arquitetura da M7.6.

---

# 🎯 O verdadeiro salto da M7.6

Na M7.5 tínhamos:

```text
Pergunta
   ↓
Retrieval
   ↓
Documento relevante
```

Hoje chegamos a:

```text
Pergunta
   ↓
Retrieval
   ↓
Documento relevante
   ↓
Contexto
   ↓
Prompt
   ↓
LLM
   ↓
Resposta
```

Isso é o momento em que as peças que construímos separadamente finalmente formam **um sistema RAG funcional em arquitetura**.

O planejamento do Módulo 7 define justamente esta aula como a integração do `KnowledgeService` ao `MentorAgent`, com o contexto passando antes pelo `PromptBuilder` e chegando então ao LLM. ([GitHub](https://github.com/Ver-tix/Second-Brain/blob/d7412138d98e9f3ad827ad5fa6645ac5a2a625b4/3.%20Recursos/Estudos/Cursos/Intelig%C3%AAncia%20Artificial/Projeto%20Prometheus/Aulas/M%C3%B3dulo%207/Introdu%C3%A7%C3%A3o%20007.md "Second-Brain/3. Recursos/Estudos/Cursos/Inteligência Artificial/Projeto Prometheus/Aulas/Módulo 7/Introdução 007.md at d7412138d98e9f3ad827ad5fa6645ac5a2a625b4 · Ver-tix/Second-Brain · GitHub"))

---

# 🛠️ Desafio M7.006 — Integrando RAG ao MentorAgent

**Agora, sim, vem o desafio.**

## [[🛠 Desafio M7 006]]
## [[🤖 Monitoria M7 006]]

### Parte 1 — Dependência

Atualize o `MentorAgent` para receber e armazenar uma instância de:

```python
KnowledgeService
```

---

### Parte 2 — Retrieval

Faça o `MentorAgent` consultar o `KnowledgeService` durante o processamento de uma pergunta.

O resultado deverá ser utilizado como contexto.

---

### Parte 3 — Contexto

Transforme os resultados recuperados pelo `KnowledgeService` em um contexto textual.

**Não crie uma nova classe para isso.**

Nesta etapa, queremos apenas compreender o fluxo.

---

### Parte 4 — PromptBuilder

Atualize o `PromptBuilder` para receber o contexto recuperado.

O prompt deverá passar a conter:

```text
instruções
+
histórico
+
contexto recuperado
+
pergunta
```

---

### Parte 5 — Integração

Atualize o `main.py` para montar corretamente as dependências:

```text
EmbeddingService
       ↓
VectorStore
       ↓
KnowledgeService
       ↓
MentorAgent
```

---

### Parte 6 — Teste

Cadastre pelo menos **três documentos** no conhecimento.

Depois faça uma pergunta relacionada semanticamente a um deles.

Verifique se:

1. o `KnowledgeService` recupera o documento relevante;
    
2. o contexto chega ao `PromptBuilder`;
    
3. o contexto aparece no prompt enviado ao LLM.
    

---

### Parte 7 — Reflexão

Responda:

> **Se o `MentorAgent` precisa apenas de conhecimento relevante, por que ele não deveria conhecer diretamente o `VectorStore`?**

E:

> **Qual é a diferença arquitetural entre `ConversationMemory` e `KnowledgeService`?**

---

## Regra desta aula

**Não implemente nada além do que o desafio pede.**

Especialmente, não crie ainda:

- `Retriever` separado;
    
- `RAGService`;
    
- `ContextBuilder`;
    
- reranker;
    
- thresholds;
    
- banco vetorial externo;
    
- agentes especializados.
    

Queremos terminar a M7.6 com uma arquitetura simples, porém fundamental:

```text
                    MentorAgent
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
 ConversationMemory   KnowledgeService   ToolManager
                         │
                  ┌──────┴──────┐
                  ▼             ▼
          EmbeddingService  VectorStore
                         │
                         ▼
                      Contexto
                         │
                         ▼
                   PromptBuilder
                         │
                         ▼
                    LLMService
```

**Agora é laboratório.** Você implementa o M7.006 e me manda os códigos. Eu faço a revisão como professor — e, depois de aprovado, registramos a evolução no CHANGELOG.

---

