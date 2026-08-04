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
# Aula 10 — Prompts como Componentes da Arquitetura

Até agora você já construiu praticamente toda a infraestrutura de um agente:

- ✅ `LLMService`
    
- ✅ `ConversationMemory`
    
- ✅ `Tool Calling`
    
- ✅ `Tool Registry`
    
- ✅ `Tool Manager`
    
- ✅ `MentorAgent`
    

Repare numa coisa interessante: existe um componente que ainda está muito simples...

```python
PromptBuilder
```

Até agora ele apenas concatena texto.

Mas, em um sistema profissional, o prompt deixa de ser uma string e passa a ser uma **camada arquitetural**.

---

# O problema

Imagine que amanhã o Prometheus tenha estes agentes:

- Mentor
    
- Editor
    
- Knowledge
    
- Researcher
    

Todos usam LLM.

Todos precisam de prompts.

A primeira ideia seria:

```
mentor_prompt.py
editor_prompt.py
knowledge_prompt.py
researcher_prompt.py
```

Funciona.

Durante algum tempo.

Depois de alguns meses aparecem problemas.

---

## Problema 1

Os prompts começam a repetir informações.

Exemplo:

Todos dizem:

> "Você é um assistente útil."

Todos dizem:

> "Responda em português."

Todos dizem:

> "Nunca invente informações."

Você copiou tudo.

---

## Problema 2

Agora você decide alterar uma regra.

Antes:

```
Nunca use markdown.
```

Agora:

```
Sempre use markdown.
```

Você precisa editar:

- mentor_prompt
    
- editor_prompt
    
- knowledge_prompt
    
- researcher_prompt
    

Já apareceu duplicação.

---

## Problema 3

Cada agente começa a possuir:

- personalidade
    
- objetivo
    
- ferramentas
    
- contexto
    
- memória
    
- instruções permanentes
    
- instruções temporárias
    

O prompt vira um monstro.

---

# A ideia importante

Assim como fizemos com Tool Registry...

...também podemos separar responsabilidades nos prompts.

Em vez de um prompt gigante:

```
Prompt Final
```

Passamos a montar o prompt por componentes.

---

## Exemplo

Em vez disso:

```
Você é o Prometheus Mentor.

Responda em português.

Explique passo a passo.

Nunca invente.

Use exemplos.

Histórico:
...

Pergunta:
...
```

Temos:

```
System Prompt
      +

Agent Prompt
      +

Memory
      +

User Message
```

Cada parte possui uma responsabilidade.

---

# Isso lembra alguma coisa?

É exatamente igual ao que fizemos anteriormente.

Antes:

```
if
elif
elif
elif
```

Agora:

```
Tool Registry
```

Antes:

```
prompt gigante
```

Agora:

```
Prompt Builder
```

A arquitetura continua evoluindo na mesma direção:

**separação de responsabilidades**.

---

# Os quatro blocos do Prompt

## 1) System Prompt

Quem é o sistema?

Exemplo:

```
Você faz parte do Prometheus OS.

Nunca invente informações.

Responda em português.

Se não souber, diga que não sabe.
```

É praticamente constante.

---

## 2) Agent Prompt

Quem é aquele agente específico?

Mentor:

```
Explique como um professor.
```

Editor:

```
Escreva como um redator.
```

Knowledge:

```
Organize conhecimento.
```

Cada agente possui o seu.

---

## 3) Contexto

Aqui entram:
- memória
- RAG
- documentos
- ferramentas disponíveis
- histórico

Tudo variável.

---

## 4) Pergunta

A mensagem atual.

```
Como funciona Attention?
```

---

# O Prompt Builder passa a montar

```
+--------------------+
| System Prompt      |
+--------------------+

+--------------------+
| Agent Prompt       |
+--------------------+

+--------------------+
| Contexto           |
+--------------------+

+--------------------+
| Pergunta           |
+--------------------+

          ↓

Prompt Final
```

---

# Vantagens

Agora cada parte pode evoluir sozinha.

Você altera apenas o System Prompt.

Todos os agentes recebem a mudança.

Você altera apenas o Agent Prompt do Editor.

O Mentor continua igual.

Isso é o mesmo ganho arquitetural que tivemos com o Tool Registry.

---

# Relação com o Prometheus

Mais para frente teremos algo próximo disso:

```
  Prometheus OS
        │
		|
        ▼
  Prompt Service
          │
 ┌─-────-─┼──────────┐
 ▼        ▼          ▼
Mentor Editor Knowledge
```

Todos compartilham a mesma infraestrutura.

Mas cada um monta um prompt diferente.

Assim como compartilham o Tool Registry.

---

# O conceito central da aula

A principal ideia não é "escrever prompts melhores".

É perceber que **prompts também são software**.

E, como qualquer software, eles devem ser:
- modulares;
- reutilizáveis;
- desacoplados;
- fáceis de manter.

Quando o sistema cresce para dezenas de agentes, tratar prompts como componentes arquiteturais faz tanta diferença quanto tratar ferramentas como componentes reutilizáveis.

---

# Laboratório — Aula 10

Nesta aula vamos dar mais um passo na arquitetura do Prometheus-Mentor.

**Objetivo:** transformar o `PromptBuilder` em um construtor modular de prompts.

Ao final do laboratório você terá:

1. Um `system_prompt.py`, contendo apenas as instruções globais do Prometheus.
    
2. Um `mentor_prompt.py`, contendo apenas as instruções específicas do Prometheus-Mentor.
    
3. Um `PromptBuilder` responsável por montar o prompt final a partir desses componentes e do histórico da conversa.
    
4. Um `MentorAgent` que não conhece o conteúdo dos prompts, apenas utiliza o `PromptBuilder`.
    

Perceba o padrão: assim como criamos um **Tool Registry** para desacoplar ferramentas, agora criaremos uma infraestrutura para desacoplar prompts. Essa será a base para que, no futuro, cada agente do ecossistema (Mentor, Editor e Knowledge) tenha sua identidade própria sem duplicar instruções nem espalhar regras pelo código.