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
# Etapa 1 — Injetar `KnowledgeService`

(Já feito)

# Etapa 2 - Retrieval

Em `app/prompts/prompt_builder.py`:

```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto relevante da base de conhecimento:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"{context_section}"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

Em `app/agents/mentor_agent.py`, altere o método `ask()`:

```Python
def ask(self, question: str) -> str:

    self.memory.add_user_message(question)

    history = self.memory.get_history()

    results = self.knowledge_service.search(question)

    context = "\n".join(
        result["text"] for result in results
    )

    prompt = PromptBuilder.build(
        history,
        question,
        context,
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

        result_text = str(result)

        self.memory.add_assistant_message(result_text)

        return result_text
```

Em `app/prompts/prompt_builder.py`:
```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto relevante da base de conhecimento:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"{context_section}"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

Em `app/agents/mentor_agent.py`, altere o método `ask()`:
```Python
def ask(self, question: str) -> str:

    self.memory.add_user_message(question)

    history = self.memory.get_history()

    results = self.knowledge_service.search(question)

    context = "\n".join(
        result["text"] for result in results
    )

    prompt = PromptBuilder.build(
        history,
        question,
        context,
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

        result_text = str(result)

        self.memory.add_assistant_message(result_text)

        return result_text
```

# Etapa 3 - Contexto
Já fiz a maior parte da Etapa 3 no `mentor_agent.py`. O trecho abaixo é exatamente a transformação que o curso pede — sem criar classe nova.

O objetivo aqui não é criar um `ContextBuilder` ou similar. É entender como os dados mudam de forma ao longo do fluxo RAG.

```Python

results = self.knowledge_service.search(question)

context = "\n".join(
    result["text"] for result in results
)
```



```Python
# versão mais explícita
results = self.knowledge_service.search(question)

context_parts = []

for result in results:
    context_parts.append(result["text"])

context = "\n".join(context_parts)
```

# Etapa 4 - `PromptBuilder`

```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT


class PromptBuilder:

    @staticmethod
    def build(history: str, question: str, context: str = "") -> str:

        context_section = ""
        if context:
            context_section = (
                f"Contexto recuperado:\n"
                f"{context}\n\n"
            )

        return (
            f"{SYSTEM_PROMPT}\n\n"
            f"{MENTOR_PROMPT}\n\n"
            f"Histórico da conversa:\n"
            f"{history}\n\n"
            f"{context_section}"
            f"Nova pergunta do usuário:\n"
            f"{question}"
        )
```

# Etapa 5 Atualizar `main.py`

Boa notícia: **a cadeia de dependências já está montada corretamente** no seu `main.py`. O fluxo pedido pelo curso já existe:

```python
    # Instanciar o serviço de embedding
    embedding_service = EmbeddingService()

    # Instanciar o VectorStore
    vector_store = VectorStore()

    # Instanciar o serviço de conhecimento
    knowledge_service = KnowledgeService(
        embedding_service,
        vector_store
    )
```

```python
    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service, 
        memory,
        tool_manager,
        knowledge_service,
        )
        
```
## Código Atualizado `main.py`

```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService
from app.knowledge.embedding_service import EmbeddingService
from app.knowledge.vector_store import VectorStore


def main():

    # ===== Infraestrutura de conhecimento =====

    embedding_service = EmbeddingService()
    vector_store = VectorStore()

    knowledge_service = KnowledgeService(
        embedding_service,
        vector_store,
    )

    # ===== Ingestão de documentos =====

    knowledge_service.add_document(
        "Bitcoin possui uma oferta máxima de 21 milhões de moedas."
    )

    knowledge_service.add_document(
        "Python é uma linguagem de programação de alto nível."
    )

    knowledge_service.add_document(
        "Redes neurais artificiais são modelos computacionais."
    )

    
	# ===== Demais dependências =====
    llm_service = LLMService()
    memory = ConversationMemory()
    tool_manager = ToolManager()

    # ===== Agente =====

    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager,
        knowledge_service,
    )

    # ===== Loop de questionamento =====

    while True:
        question = input("\nPergunte alguma coisa: ")

        if question.lower() == "sair":
            print("\nAté a próxima!")
            break

        answer = mentor.ask(question)

        print("\nPrometheus-Mentor:\n")
        print(answer)


if __name__ == "__main__":
    main()
```

# Etapa 7 - Reflexão Arquitetônica

## 1 - Se o `MentorAgent` precisa apenas de conhecimento relevante, por que ele não deveria conhecer diretamente o `VectorStore`?

Porque o VectorStore é um detalhe de implementação da recuperação de conhecimento, enquanto o MentorAgent deveria se preocupar apenas com a tarefa de ensinar e responder ao usuário.

O `MentorAgent` precisa fazer algo conceitualmente simples:

```
"Preciso de conhecimento relevante para responder esta pergunta."
```

Ele não deveria precisar saber:

```
"Preciso gerar um embedding,
consultar um banco vetorial,
calcular similaridade,
ordenar resultados..."
```

Por isso introduzimos o:

```
MentorAgent
      ↓
KnowledgeService
      ↓
VectorStore
```

O `KnowledgeService` funciona como uma **camada de abstração** entre o agente e a infraestrutura de conhecimento.

Assim, o `MentorAgent` depende de uma **capacidade**:

```
knowledge_service.search(question)
```

e não de uma **implementação concreta**:

```
vector_store.search(embedding)
```

Isso produz um desacoplamento importante.

Hoje, por exemplo:

```
KnowledgeService
       ↓
VectorStore
```

pode utilizar nosso armazenamento vetorial atual.

No futuro, poderíamos substituir a implementação por outro mecanismo de busca, outro banco vetorial ou uma arquitetura mais sofisticada **sem precisar reescrever o `MentorAgent`**.

### 🔥 Princípio Prometheus

> **O agente deve conhecer capacidades, não detalhes de infraestrutura.**

Essa é uma ideia extremamente importante para o Prometheus OS: quanto mais os agentes dependerem de abstrações e serviços bem definidos, mais fácil será reutilizar os mesmos agentes em diferentes ecossistemas.

---

## 2. Qual é a diferença arquitetural entre `ConversationMemory` e `KnowledgeService`?

A diferença fundamental está em **de onde vem o conhecimento e qual é sua finalidade**.

### `ConversationMemory`

Representa o **estado contextual da conversa atual**.

Ela responde essencialmente:

> **"O que já aconteceu nesta conversa?"**

Por exemplo:

```
Usuário: Explique Transformers.

Mentor: Transformers utilizam...

Usuário: E como funciona a atenção?

Mentor: Como expliquei anteriormente...
```

A `ConversationMemory` permite que o agente mantenha continuidade entre essas interações.

Podemos visualizar:

```
Usuário
   ↓
Conversa
   ↓
ConversationMemory
   ↓
Histórico/contexto da interação
```

---

### `KnowledgeService`

Representa o **conhecimento externo que pode ser recuperado para responder à pergunta**.

Ele responde:

> **"Que conhecimento relevante eu tenho disponível para esta pergunta?"**

Por exemplo:

```
Pergunta:
"Qual é a oferta máxima do Bitcoin?"

        ↓

KnowledgeService
        ↓
VectorStore
        ↓
"Bitcoin possui oferta máxima de 21 milhões."
```

Visualmente:

```
Base de conhecimento
        ↓
VectorStore
        ↓
KnowledgeService
        ↓
Conhecimento relevante
```

---

# Feedback

## Retrieval → Context → Generation

### Resultado geral

**🟢 APROVADO — 9,2/10**

Você demonstrou que entendeu o principal objetivo da aula: o `MentorAgent` não deve implementar RAG internamente; ele deve **consumir a capacidade de conhecimento oferecida pelo `KnowledgeService`**. Isso está coerente com a arquitetura que você construiu nas aulas anteriores.

---

# 1. Etapa 1 — Injeção do `KnowledgeService`

### 🟢 Correto

A cadeia:

```text
main.py
   ↓
KnowledgeService
   ↓
MentorAgent
```

está correta.

Você também manteve:

```python
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service,
)
```

Isso é exatamente o tipo de composição que queremos.

### 💎 O que você entendeu

O `MentorAgent` recebe uma **capacidade**, não constrói sua própria infraestrutura.

Isso permite:

```text
MentorAgent
     ↓
KnowledgeService
     ↓
VectorStore A
```

e futuramente:

```text
MentorAgent
     ↓
KnowledgeService
     ↓
Qdrant
```

sem que o agente precise saber da troca.

Essa mesma ideia já aparece na evolução anterior do seu `KnowledgeService`, que encapsula `EmbeddingService` e `VectorStore`.

**Nota: 10/10**

---

# 2. Etapa 2 — Retrieval

Seu fluxo:

```python
results = self.knowledge_service.search(question)

context = "\n".join(
    result["text"] for result in results
)
```

está **correto**.

E aqui existe uma coisa que gostei bastante:

Você não colocou:

```python
self.embedding_service.generate_embedding(...)
```

dentro do `MentorAgent`.

Nem:

```python
self.vector_store.search(...)
```

O agente simplesmente diz:

```python
knowledge_service.search(question)
```

Isso demonstra que você entendeu a abstração construída na M7.005. O `KnowledgeService` recebe a pergunta, gera o embedding e delega a busca ao `VectorStore`.

### 💎 Modelo mental correto

```text
MentorAgent
   │
   │ "me dê conhecimento relevante"
   ▼
KnowledgeService
   │
   ├── EmbeddingService
   │
   └── VectorStore
          │
          ▼
       resultados
```

**Nota: 10/10**

---

# 3. Etapa 3 — Transformação em contexto

Também está correto:

```python
context_parts = []

for result in results:
    context_parts.append(result["text"])

context = "\n".join(context_parts)
```

Essa parte parece trivial, mas pedagogicamente é importante.

Você está fazendo:

```text
List[Document]
      ↓
List[str]
      ↓
str
```

Ou seja:

```text
resultado estruturado
        ↓
contexto textual
        ↓
prompt
```

E você respeitou uma decisão importante da aula: **não criou um `ContextBuilder` desnecessariamente**. O próprio desafio deixa claro que o objetivo aqui é compreender a transformação dos dados ao longo do pipeline RAG, e não criar mais uma abstração.

**Nota: 10/10**

---

# 4. Etapa 4 — `PromptBuilder`

Seu `PromptBuilder` ficou:

```python
@staticmethod
def build(history: str, question: str, context: str = "") -> str:
```

Isso está bom.

Principalmente porque você tornou `context` opcional:

```python
context: str = ""
```

e depois:

```python
if context:
```

Isso significa que o `PromptBuilder` consegue trabalhar tanto com:

```text
pergunta + histórico
```

quanto:

```text
pergunta + histórico + RAG
```

### 🟢 Boa decisão

Você não tornou o conhecimento uma dependência obrigatória do builder.

Isso mantém a interface simples.

---

### 🟡 Pequeno ponto de melhoria

Eu prefiro sua segunda versão:

```python
f"Contexto recuperado:\n"
```

em vez da primeira:

```python
f"Contexto relevante da base de conhecimento:\n"
```

Não é um erro técnico.

Mas **"Contexto recuperado"** descreve melhor o que realmente aconteceu.

O sistema não necessariamente sabe que aquele contexto é "relevante" no sentido semântico absoluto. Ele foi **recuperado pelo mecanismo de busca**.

Parece uma diferença pequena, mas é uma boa disciplina arquitetural:

> Não atribua ao componente uma garantia que ele não possui.

**Nota: 9,5/10**

---

# 5. Etapa 5 — `main.py`

Aqui também está correto.

Você criou:

```python
embedding_service = EmbeddingService()
vector_store = VectorStore()

knowledge_service = KnowledgeService(
    embedding_service,
    vector_store,
)
```

Depois:

```python
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service,
)
```

Isso produz exatamente a cadeia de composição desejada:

```text
                    main.py
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     LLMService     Memory     ToolManager
                       │
                       │
                KnowledgeService
                       │
              ┌────────┴────────┐
              ▼                 ▼
      EmbeddingService     VectorStore
```

E você ainda fez ingestão de documentos antes de iniciar o loop:

```python
knowledge_service.add_document(...)
```

Isso permite testar o RAG de verdade.

**Nota: 10/10**

---

# 6. Etapa 6 — Teste negativo

Aqui encontrei a principal **lacuna da entrega**.

No desafio, propusemos explicitamente testar:

```text
Conhecimento encontrado
VS
Conhecimento não encontrado
```

Você cadastrou:

```text
Bitcoin
Python
Redes neurais
```

mas não documentou o resultado de uma pergunta fora da base.

Por exemplo:

> "Quem inventou o telefone?"

Isso não invalida sua implementação.

Mas, como professor, eu considero essa parte **não comprovada**.

### 🟡 Falta de evidência experimental

Você mostrou que o pipeline está implementado.

Mas não mostrou que testou:

```text
query
 ↓
retrieval
 ↓
contexto vazio/irrelevante
 ↓
generation
```

**Nota: 8/10**

---

# 7. Reflexão arquitetônica — Questão 1

Sua resposta foi **excelente**.

Você escreveu:

> "O `KnowledgeService` funciona como uma camada de abstração entre o agente e a infraestrutura de conhecimento."

E depois chegou ao princípio:

> **"O agente deve conhecer capacidades, não detalhes de infraestrutura."**

### 🟢 Aprovado com distinção.

Essa é exatamente a abstração que queremos.

Você também conseguiu explicar corretamente a diferença entre:

```python
knowledge_service.search(question)
```

e:

```python
vector_store.search(embedding)
```

O primeiro expressa **intenção/capacidade**.

O segundo expressa **implementação**.

Essa distinção é fundamental para arquiteturas de agentes.

**Nota: 10/10**

---

# 8. Reflexão arquitetônica — Questão 2

Sua resposta sobre:

```text
ConversationMemory
VS
KnowledgeService
```

também está correta.

Você resumiu muito bem:

### Memory

> "O que já aconteceu nesta conversa?"

### Knowledge

> "Que conhecimento relevante eu tenho disponível para esta pergunta?"

Esse é um dos conceitos que eu quero que você carregue para os próximos módulos.

Porque futuramente teremos algo parecido com:

```text
Memory
Knowledge
Tools
Goals
State
Environment
```

e será fundamental não misturar essas coisas.

Sua resposta demonstra que você já está começando a enxergar o agente como um **sistema composto por diferentes fontes de contexto**, e não simplesmente como "um LLM com ferramentas".

**Nota: 10/10**

---

# ⚠️ Um detalhe importante que quero corrigir

Há uma questão conceitual que **não está errada na sua implementação**, mas quero deixar registrada.

Você está fazendo:

```python
self.memory.add_user_message(question)

history = self.memory.get_history()

results = self.knowledge_service.search(question)
```

Isso significa que o `history` recuperado **já contém a pergunta atual**.

Depois você passa novamente:

```python
question
```

ao `PromptBuilder`.

Então podemos acabar com:

```text
Histórico:
Usuário: Qual é a oferta máxima do Bitcoin?

Nova pergunta:
Qual é a oferta máxima do Bitcoin?
```

Isso provavelmente já existia na arquitetura anterior e **não é um erro específico da M7.006**.

Portanto:

> 🟡 **Não quero que você altere isso agora.**

Não vamos sair corrigindo arquitetura antiga no meio de uma aula nova.

Mas vou registrar como dívida técnica para uma futura refatoração do fluxo de memória/prompt.

Isso é importante no Prometheus: **não refatoramos tudo toda vez que encontramos uma imperfeição.**

---

# 🏆 Avaliação final

|Etapa|Avaliação|
|---|--:|
|Injeção do `KnowledgeService`|🟢 10/10|
|Retrieval|🟢 10/10|
|Transformação em contexto|🟢 10/10|
|PromptBuilder|🟢 9,5/10|
|`main.py`|🟢 10/10|
|Testes|🟡 8/10|
|Reflexão arquitetural|🟢 10/10|
|**Nota final**|**9,2/10**|

# 🎓 Veredito do professor

## **M7.006 — APROVADA ✅**

E mais importante que a nota:

**você entendeu a arquitetura.**

A evolução que construímos foi:

```text
M7.001
KnowledgeService
      ↓
M7.003
EmbeddingService
      ↓
M7.004
VectorStore
      ↓
M7.005
Retrieval
      ↓
M7.006
Retrieval
   +
Context
   +
Generation
      ↓
     RAG
```

A M7.005 fazia o sistema responder:

> **"Que documentos são relevantes para esta pergunta?"**

A M7.006 acrescentou:

> **"Agora entregue esses documentos ao LLM para que ele possa utilizá-los na geração."**

E essa é a virada arquitetural da aula.

Você saiu de um **sistema que sabe buscar conhecimento** para um **agente que utiliza conhecimento recuperado para responder**.

---

## 🧠 Insight Prometheus da M7.006

Guarde esta frase:

> **RAG não torna o LLM mais inteligente. RAG torna o contexto disponível ao LLM mais inteligente.**

O modelo continua sendo o mesmo.

O que mudou foi:

```text
ANTES

Pergunta
   ↓
LLM
   ↓
Resposta


DEPOIS

Pergunta
   ↓
Retrieval
   ↓
Conhecimento relevante
   ↓
Contexto aumentado
   ↓
LLM
   ↓
Resposta
```

É exatamente essa mudança que transforma nosso `MentorAgent` em uma arquitetura capaz de consultar uma **base externa de conhecimento**, mantendo essa infraestrutura separada do agente.

**M7.006 oficialmente encerrada.** 🟢

A única pendência que eu deixaria, se quisermos uma entrega 100% impecável, é você registrar o **teste negativo** e o comportamento observado. Fora isso, **não faria você reescrever o código desta aula**.