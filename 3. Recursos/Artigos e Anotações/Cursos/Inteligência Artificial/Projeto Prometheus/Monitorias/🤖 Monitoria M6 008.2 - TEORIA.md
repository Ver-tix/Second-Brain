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

---

# Agora vem a parte mais importante da Aula 8.2

O professor escreve:

> **"Faça com que o `LLMService` detecte quando a resposta da API contém um tool call em vez de uma resposta textual."**

Perceba a mudança.

Até hoje nosso `LLMService` sempre assumiu:

```Python
return response.output_text
```

Ou seja, ele acreditava que toda resposta da API era texto.

A partir desta aula, isso deixa de ser verdade.

O modelo poderá responder algo conceitualmente assim:

```
Não vou responder ainda.

↓

Quero usar:

calculator
```

O `LLMService` passa a ter uma nova responsabilidade:

```
Receber resposta da API

↓

Ela é texto?

↓

SIM → devolve texto

NÃO

↓

É um Tool Call?

↓

SIM → informa isso ao restante do sistema
```

Essa é a primeira vez que o `LLMService` deixa de ser apenas um "cliente HTTP" e começa a interpretar a estrutura da resposta da API.

---

## Próxima etapa

Vamos trabalhar no `LLMService`.

Mas, como sempre, não quero entregar o código pronto.

Primeiro quero que você pense comigo:

> **Se o `LLMService.generate()` pode retornar duas coisas diferentes (texto ou um Tool Call), faz sentido ele continuar retornando apenas `str`?**

Essa pergunta é o coração da próxima implementação. Ela vai definir a assinatura do método e influenciar toda a arquitetura daqui para frente.

# Antes de escrever código...

Quero que você olhe para a assinatura atual do método:

```Python
def generate(self, prompt: str) -> str:
```

Ela diz uma coisa muito específica:

> **"Eu sempre devolvo uma string."**

Até a Aula 8.1 isso era verdade.

Mas o próprio professor acabou de nos dizer que isso deixou de ser verdade.

Agora a OpenAI pode responder duas coisas:

```
Resposta textual
```

ou

```
Tool Call
```

---

# Pense como o Python

Imagine que você chama:

```
response = self.client.responses.create(...)
```

A API devolve um objeto.

Esse objeto pode conter:

```
Texto
```

ou

```
Pedido para usar uma ferramenta
```

Então pergunto:

## Faz sentido o método continuar prometendo isto?

```
-> str
```

Na minha opinião...

**Não.**

---

# Aqui entra uma decisão arquitetural

Temos algumas possibilidades.

## Opção A

Retornar qualquer coisa.

```Python
def generate(...):
```

Sem tipagem.

❌ Eu não gosto.

---

## Opção B

Retornar um dicionário.

```Python
{
    "type": "text",
    "content": "Olá"
}
```

ou

```Python
{
    "type": "tool_call",
    ...
}
```

Muito comum.

---

## Opção C (minha favorita)

Retornar o objeto inteiro da OpenAI.

```Python
return response
```

E deixar quem chamou decidir.

---

# Qual eu escolheria neste laboratório?

**A Opção C.**

E sabe por quê?

Porque ela preserva a informação.

Hoje você só precisa de texto.

Amanhã vai precisar de:

- Tool Calls
- Finish Reason
- Usage
- Token Count
- IDs
- Raciocínio (quando disponível)
- Chamadas múltiplas

Se hoje você faz:

```Python
return response.output_text
```

Você joga tudo isso fora.

---

# Então eu faria uma pequena mudança

Hoje:

```Python
response = ...

return response.output_text
```

↓

Primeiro passo:

```Python
response = ...

return response
```

Pronto.

Nada mais.

---

# "Mas aí o MentorAgent quebra!"

Sim!

E isso é esperado.

Lembra de uma frase do professor?

> "Estamos num estado intermediário."

Essa é exatamente uma dessas etapas.

Primeiro preservamos a resposta inteira.

Depois ensinaremos o `MentorAgent` a interpretá-la.

---

# Minha pergunta para você

Antes de codarmos:

**Por que eu prefiro retornar o objeto inteiro da OpenAI em vez de extrair apenas `output_text`?**

Não quero a resposta decorada.

Quero que você pense como arquiteto.

Essa decisão vai aparecer muitas vezes durante a construção do Prometheus