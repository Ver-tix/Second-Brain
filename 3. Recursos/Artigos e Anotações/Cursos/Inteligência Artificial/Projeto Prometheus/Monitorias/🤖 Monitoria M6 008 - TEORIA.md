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
==minha resposta:== 
`Tool decision decide qual ferramenta é necessária para o trabalho; tool calling chama a ferramenta efetivamente`

==Professor: ==
Sua resposta faz sentido, mas ela responde **o que acontece** e não **onde a decisão está**. E essa é justamente a pergunta do professor.

Vou conduzir você até a resposta.

---

## Aula 7

Quem decide?

```
Usuário
    │
    ▼
ToolDecision
    │
    ▼
"calculator"
```

Quem tomou a decisão?

**O código Python.**

Mais especificamente:

```
if "quanto é" in question:
    return "calculator"
```

A inteligência da decisão está escrita por você.

---

## Aula 8

Agora imagine a mesma pergunta:

> "Qual é a raiz quadrada de 144?"

Quem decide?

Não será mais:

```
if ...
```

Será o próprio modelo.

Algo conceitualmente assim:

```
LLM

↓

"Para responder isso, preciso chamar CalculatorTool."
```

Depois ele devolve uma chamada estruturada:

```
{
  "tool": "calculator",
  "arguments": {
    ...
  }
}
```

---

# Então, qual é a diferença arquitetural?

A resposta que eu esperava de você seria algo próximo disto:

> **No ToolDecision, a lógica de decisão está codificada pelo desenvolvedor, através de regras em Python. No Tool Calling, a decisão deixa de estar no código e passa a ser tomada dinamicamente pelo próprio LLM, que escolhe quando e qual ferramenta utilizar.**

Essa é a grande mudança.

Não é simplesmente:

> "Antes decidia e agora chama."

Porque quem chama a ferramenta continua sendo o sistema.

O que muda é **quem tomou a decisão**.

---

## Resumindo em uma tabela

|ToolDecision|Tool Calling|
|---|---|
|A decisão está no código Python.|A decisão está no LLM.|
|O desenvolvedor escreve as regras.|O modelo raciocina sobre qual ferramenta usar.|
|Baseado em `if`, `startswith`, palavras-chave.|Baseado na compreensão da linguagem natural.|
|Pouco flexível.|Muito mais flexível e escalável.|

---

## Nota

Eu daria **8,5/10** para sua resposta.

Você identificou corretamente que o Tool Calling está relacionado ao uso efetivo das ferramentas, mas faltou enxergar o aspecto mais profundo da arquitetura: **a migração da camada de decisão do código para o modelo**.

E essa ideia é extremamente importante. Na verdade, eu diria que ela é um dos conceitos centrais do Módulo 6.

---

# Agora sim, vamos ao laboratório 🚀

A primeira etapa da Aula 8 é modificar o `LLMService` para que ele deixe de fazer apenas:

```
response = client.responses.create(...)
```

e passe a informar ao modelo:

> "Estas são as ferramentas que você pode usar."

Essa é a primeira vez que o Prometheus vai **anunciar suas ferramentas** ao LLM.

Essa mudança é pequena em código, mas enorme em arquitetura.

---
# EATPA 1 - ENTENDER O OBJETIVO DA AULA

Antes de escrever uma linha de código, quero que você fixe uma ideia.

Até agora, nosso `LLMService` fazia algo assim:

```text
LLM

↓

Pergunta

↓

Resposta
```

Agora ele fará:

```text
LLM

↓

Pergunta
+
Lista de ferramentas disponíveis

↓

O modelo decide se quer usar alguma delas

↓

Resposta ou Tool Call
```

Perceba que **não vamos alterar o `ToolManager` nem a `CalculatorTool`**.

<h4 align="center">Vamos alterar apenas <b>a forma como conversamos com o modelo</b>.</h4>

---

# ETAPA 2 — ABRIR O ARQUIVO

Abra:

```text
app/services/llm_service.py
```

Não altere nada ainda.

---

## Quero ver o código atual

Cole aqui o conteúdo completo do seu `llm_service.py`.

Vamos modificá-lo juntos, linha por linha, para que você entenda exatamente **por que cada alteração existe**, em vez de apenas copiá-la. Isso será importante porque esta aula introduz um conceito que você verá em praticamente todos os frameworks modernos de agentes (OpenAI Agents SDK, LangGraph, AutoGen, Semantic Kernel, etc.).