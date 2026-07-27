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
VAMOOOOOOS! 🚀

Essa aula é, na minha opinião, **o verdadeiro nascimento de um agente moderno**. Até aqui, estávamos preparando o terreno. Agora muda quem "pensa". E isso é uma mudança arquitetural enorme.

Mas vamos manter nossa metodologia: **primeiro arquitetura, depois código**.

---

# O desafio da Aula 8.2

Antes de implementar qualquer coisa, quero que você responda à pergunta central da aula.

> **Quem é o cérebro do sistema agora?**

Até a Aula 7, a resposta era fácil:

```text
Usuário
    │
    ▼
MentorAgent
    │
    ▼
ToolDecision   ← cérebro
    │
    ▼
Calculator
```

Agora, o próprio material diz:

> "O cérebro mudou de lugar."

---

# Quero ouvir sua resposta

Sem olhar o texto, explique com suas palavras:

> **Por que remover o `ToolDecision` deixa a arquitetura melhor?**

Não responda apenas:

> "Porque o LLM decide."

Quero que você pense como arquiteto.

**O que essa mudança permite que antes era praticamente impossível?**

---

## Depois da sua resposta

Seguiremos exatamente estas etapas (que vêm do desafio):

1. ✅ Remover a dependência de `ToolDecision` do `MentorAgent`.
    
2. ✅ Alterar o `LLMService` para detectar um **tool call** em vez de apenas texto.
    
3. ✅ Fazer o `ToolManager` executar a ferramenta solicitada e devolver o resultado.
    
4. ✅ Desenhar o novo fluxo arquitetural em ASCII.
    
5. ✅ Revisão de código.
    
6. ✅ CHANGELOG do Laboratório 8.2.
    

Tenho quase certeza de que este será o laboratório em que você vai perceber que o `MentorAgent` está deixando de ser um "programa com IA" para se tornar um verdadeiro **orquestrador**. E isso vai influenciar toda a arquitetura do Prometheus OS daqui para frente.