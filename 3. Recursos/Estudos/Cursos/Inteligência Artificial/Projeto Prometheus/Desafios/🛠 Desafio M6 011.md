---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
# Laboratório — Aula 11 (Fechamento)

# Parte 1 — Arquitetura atual do Prometheus-Mentor

```text
                           Usuário
                               │
                               ▼
                           main.py
                               │
                               ▼
                         MentorAgent
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
    PromptBuilder      ConversationMemory     ToolManager
          │                    │                    │
          ▼                    │                    ▼
   ┌───────────────┐            │             ToolRegistry
   ▼               ▼            │                    │
SYSTEM_PROMPT  MENTOR_PROMPT    │        ┌───────────┴───────────┐
                                │        ▼                       ▼
                                │  CalculatorTool        KnowledgeTool
                                │
                                ▼
                           Histórico
                                │
                                ▼
                          Prompt Final
                                │
                                ▼
                           LLMService
                                │
                                ▼
                           OpenAI API
```

---

# Parte 2 — Pontos de extensão

## LLMService

### O que pode mudar sem afetar o restante do sistema?

O provedor de LLM utilizado.

### Exemplo

Hoje:

```
OpenAI
```

Amanhã:

```
Anthropic
```

ou

```
Google Gemini
```

Apenas o `LLMService` precisaria ser alterado. O restante da arquitetura continuaria utilizando a mesma interface `generate()`.

---

## PromptBuilder

### O que pode mudar?

A forma como o prompt é construído.

### Exemplo

Hoje o prompt é composto por:

- System Prompt
    
- Mentor Prompt
    
- Histórico
    
- Pergunta
    

No futuro, poderemos adicionar:

- contexto do RAG;
    
- data atual;
    
- ferramentas disponíveis;
    
- perfil do usuário.
    

Essas mudanças ocorrerão apenas no `PromptBuilder`.

---

## ConversationMemory

### O que pode mudar?

A implementação da memória.

### Exemplo

Hoje a memória existe apenas durante a execução do programa.

No futuro, poderá ser armazenada em:

- SQLite;
    
- PostgreSQL;
    
- Redis;
    
- banco vetorial.
    

O restante do sistema continuará apenas chamando `add_message()` e `get_history()`.

---

## ToolRegistry

### O que pode mudar?

As ferramentas disponíveis.

### Exemplo

Adicionar:

- WebSearchTool;
    
- GitHubTool;
    
- PDFTool;
    
- RAGTool.
    

Basta registrá-las no catálogo, sem alterar o restante da arquitetura.

---

## ToolManager

### O que pode mudar?

A forma como as ferramentas são executadas.

### Exemplo

No futuro, o ToolManager poderá implementar:

- logs;
    
- tratamento de exceções;
    
- timeout;
    
- retry;
    
- métricas;
    
- controle de permissões.
    

Sem alterar o `MentorAgent`.

---

## MentorAgent

### O que pode mudar?

O fluxo de orquestração.

### Exemplo

Hoje ele coordena um único ciclo:

```
Pergunta
↓

Resposta
```

No futuro, poderá coordenar:

- planejamento;
    
- múltiplas ferramentas;
    
- múltiplos agentes;
    
- validação dos resultados.
    

Sem alterar os componentes internos responsáveis por prompts, memória ou ferramentas.

---

# Parte 3 — Planejando o próximo módulo

## 1. Quais componentes existentes poderão ser reaproveitados?

Grande parte da infraestrutura construída poderá ser reutilizada:

- `MentorAgent`
    
- `PromptBuilder`
    
- `ConversationMemory`
    
- `LLMService`
    
- `ToolRegistry`
    
- `ToolManager`
    

Esses componentes continuarão exercendo as mesmas responsabilidades, apenas passando a trabalhar em conjunto com o novo mecanismo de RAG.

---

## 2. Quais novos componentes precisarão ser criados?

Para integrar o Second Brain, serão necessários novos componentes especializados, por exemplo:

- `RAGTool`
    
- `Retriever`
    
- `EmbeddingService`
    
- `VectorDatabase`
    
- `DocumentIndexer`
    

Cada um terá uma responsabilidade específica dentro do pipeline de recuperação de conhecimento.

---

## 3. Em que ponto do fluxo você faria a consulta ao RAG?

A consulta ao RAG deve ocorrer **antes da construção do prompt final**, para que o conhecimento recuperado faça parte do contexto enviado ao LLM.

Fluxo esperado:

```text
Usuário

↓

MentorAgent

↓

RAGTool

↓

Trechos relevantes do Second Brain

↓

PromptBuilder

↓

Prompt Final

↓

LLMService

↓

Resposta
```

Assim, o modelo responderá utilizando tanto sua capacidade de raciocínio quanto o conhecimento específico armazenado no Second Brain.

---

## 4. Por que essa funcionalidade pode ser adicionada sem reescrever toda a arquitetura?

Porque a arquitetura foi construída com **baixo acoplamento** e **separação de responsabilidades**.

Cada componente possui uma função bem definida e se comunica por interfaces estáveis. Dessa forma, a introdução do RAG exige apenas a criação de novos componentes e sua integração ao fluxo existente, sem modificar profundamente os elementos já consolidados.

Essa característica demonstra que a infraestrutura desenvolvida no Módulo 6 foi projetada para ser **extensível**, permitindo incorporar novas capacidades preservando a organização e a reutilização dos componentes existentes.

---
