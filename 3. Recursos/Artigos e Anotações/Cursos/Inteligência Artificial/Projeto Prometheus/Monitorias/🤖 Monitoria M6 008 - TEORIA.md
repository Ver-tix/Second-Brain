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
Essa é, na minha opinião, **a aula mais importante do Módulo 6**. Ela marca a transição entre um sistema "com IA" e um sistema "agentic".

Mas, como sempre fazemos, **não vamos escrever código ainda**.

---

# Desafio da Aula 8

O professor pergunta:

> **Qual é a principal diferença arquitetural entre o ToolDecision que criamos na aula anterior e o Tool Calling?**
> 
> Não responda "um usa código e outro usa IA". Pense:
> 
> **Onde está localizada a decisão?**

Quero ouvir primeiro a sua resposta.

---

## Uma dica (sem entregar)

Na Aula 7 tínhamos:

```text
Usuário
    │
    ▼
ToolDecision
    │
    ▼
ToolManager
```

Na Aula 8 teremos:

```text
Usuário
    │
    ▼
LLM
    │
    ▼
ToolManager
```

A ferramenta continua a mesma.

O `ToolManager` continua o mesmo.

A calculadora continua a mesma.

**Só uma peça muda.** E é justamente essa peça que faz toda a diferença arquitetural.

---

Depois da sua resposta, faremos o laboratório exatamente como nas outras aulas:

1. ✅ Discussão arquitetural.
    
2. ✅ Etapa 1 (criação/alteração dos arquivos).
    
3. ✅ Implementação passo a passo.
    
4. ✅ Revisão de código.
    
5. ✅ Testes.
    
6. ✅ Feedback.
    
7. ✅ CHANGELOG do Laboratório 8.

---
minha repsosta: `Tool decision decide qual ferramenta é necessária para o trabalho; tool calling chama a ferramenta efetivamente`

Profesor: 