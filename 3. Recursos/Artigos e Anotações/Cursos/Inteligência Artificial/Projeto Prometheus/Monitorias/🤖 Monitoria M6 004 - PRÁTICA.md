---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# `mentor_prompt.py`

`app/prompts/mentor_prompt.py`

```Python
class PromptBuilder:

    @staticmethod
    def build(question: str) -> str:
    
        prompt = f"""
        Você é o Prometheus-Mentor.
        
        Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara,
        didática e organizada.
        
        Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da
        definição técnica.
        
        Quando possível:
        - utilize exemplos;
        - faça analogias;
        - conecte o assunto com conceitos estudados anteriormente;
        - priorize o entendimento, e não apenas a memorização.
          
        Pergunta do usuário:
        {question}
        """
        
        return prompt
```

---

## Vamos entender cada parte

### A classe

```Python
class PromptBuilder:
```

Ela existe para centralizar toda a construção de prompts.

No futuro, talvez tenhamos:

- `MentorPromptBuilder`
- `EditorPromptBuilder`
- `CuratorPromptBuilder`

Cada agente terá sua própria "constituição".

---

### O método

```Python
@staticmethod
def build(question: str) -> str:
```

Ele recebe uma pergunta...

↓

...e devolve um prompt completo.

Nada mais.

---

### O f-string

```Python
prompt = f"""
```

O `f` permite inserir variáveis dentro do texto.

Aqui:

```Python
{question}
```

o Python substituirá pela pergunta digitada pelo usuário.

Por exemplo:

Se o usuário perguntar:

```Python
O que é um Transformer?
```

o prompt enviado para o LLM será algo como:

```Python
Você é o Prometheus-Mentor.

...

Pergunta do usuário:

O que é um Transformer?
```

---

### O retorno

```
return prompt
```

Perceba que o `PromptBuilder` **não conversa com a OpenAI**.

Ele apenas entrega uma string pronta.

---

eu: `Então, é aqui que entram os frameworks de prompts, que estudei, por exemplo, na Anthropic Academy ou na OpenAI Academy?`

**SIM!!** 🎯

Na verdade, você acabou de perceber uma conexão muito importante entre dois mundos que estudou separadamente.

Até hoje você aprendeu:

- Engenharia de Software
    
- Agentes
    
- Prompt Engineering
    

Agora eles começam a se encontrar.

---

Hoje seu prompt é simples:

```text
Você é o Prometheus-Mentor...
```

Daqui a alguns meses, ele pode seguir frameworks muito mais sofisticados, como:

```text
Role
Objective
Context
Constraints
Instructions
Output Format
Examples
User Input
```

ou

```text
Persona
Task
Knowledge
Reasoning Strategy
Guardrails
Formatting
```

Ou ainda usar técnicas como:

- XML tags (que você gostou bastante);
    
- delimitadores (`<context>`, `<task>`, `<constraints>`);
    
- few-shot prompting;
    
- chain-of-thought (quando apropriado);
    
- templates reutilizáveis.
    

---

## O mais legal?

Lembra quando você fez o curso da Anthropic e comentou:

> "Não sei onde vou usar isso."

A resposta era:

> **Aqui.**

O `PromptBuilder` é justamente o lugar onde todo esse conhecimento vai morar.

---

### Uma previsão

Conhecendo você, eu apostaria que, daqui a algumas dezenas de aulas, seu `mentor_prompt.py` não terá 15 linhas.

Ele terá talvez **300 ou 500 linhas**, muito bem organizado, com seções, comentários e templates.

E eu acho isso fantástico, porque ele deixará de ser apenas um prompt e se tornará a **Constituição do Prometheus-Mentor** — exatamente como você descreveu alguns dias atrás quando falávamos do RAG e da hierarquia entre autores. É nesse arquivo que a identidade e as regras fundamentais do agente começam a ganhar forma. 🚀