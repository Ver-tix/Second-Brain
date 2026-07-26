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
# Alterando `mentor_agent.py`

## Passo 1 - Importar o `PromptBuilder`
No início do arquivo `mentor_agent.py`, adicione:

```Python
from app.prompts.mentor_prompt import PromptBuilder
```

Agora, seus imports devem ficar parecidos com:

```Python
from app.services.llm_service import LLMService
from app.prompts.mentor_prompt import PromptBuilder
```

## Passo 2 - Alterar o método `ask()`
Hoje ele provavelmente está assim:

```Python
def ask(self, question: str) -> str:
    return self.llm_service.generate(question)
```

Vamos transformá-lo em:

```Python
def ask(self, question: str) -> str:
    prompt = PromptBuilder.build(question)
    return self.llm_service.generate(prompt)
```

## O que acabou de acontecer?

Antes:

```
Usuário
    ↓
MentorAgent
    ↓
LLMService
```

Agora:

```
Usuário
    ↓
MentorAgent
    ↓
PromptBuilder
    ↓
LLMService
```

Perceba algo muito elegante:

- O **PromptBuilder** sabe construir prompts.
- O **LLMService** sabe conversar com a OpenAI.
- O **MentorAgent** sabe apenas coordenar essas duas coisas.

Cada classe continua com **uma única responsabilidade**, exatamente como aprendemos na aula passada.

---

## 🎓 Um detalhe que o professor provavelmente cobraria

Repare que o `MentorAgent` **não monta o prompt sozinho**.

Ele faz:

```
prompt = PromptBuilder.build(question)
```

e não:

```
prompt = f"""
Você é...
"""
```

Por quê?

Porque, se amanhã você quiser mudar a personalidade do Prometheus, **você altera apenas um arquivo**: `mentor_prompt.py`.

O `MentorAgent` nem precisa saber que a constituição do agente mudou. Isso é um excelente exemplo de **baixo acoplamento** (_low coupling_), um dos pilares da boa arquitetura.

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

---

eu: `Na Anthropic, eles ensinam um loop chamado de "Loop Descrição-Discernimento", que vem do Framework dos 4 Ds (Delegação, Descrição, Discernimento e Diligência). Como ele se encaixa aqui?`

Na definição da [[AI Fluency -  Framework & Foundations#A Estrutura 4D|Anthropic]], o ciclo é:

1. **[[AI Fluency -  Framework & Foundations#Delegação|Delegação]]** → o **usuário** decide quanto de autonomia conceder à IA (augmentação, agência ou automação).
2. **[[AI Fluency -  Framework & Foundations#Descrição|Descrição]]** → o **usuário** descreve claramente o contexto, a tarefa, as restrições, o formato etc. **É aqui que entra o PromptBuilder.**
3. **[[AI Fluency -  Framework & Foundations#Discernimento|Discernimento]]** → **depois** da resposta, o **usuário** avalia criticamente: "Isso faz sentido? Está completo? Precisa de refinamento? Há inconsistências?" É um julgamento humano, não da IA.
4. **[[AI Fluency -  Framework & Foundations#Diligência|Diligência]]** → o **usuário** faz a checagem ética, factual e de risco antes de agir com base na resposta.

Então, respondendo à sua pergunta original:

> **Onde esse framework entra no Projeto Prometheus?**

Apenas o **D de Descrição** é implementado em código no `PromptBuilder`.

Os outros Ds acontecem **fora do software**, na interação entre você e o agente:

```
Usuário
│
├── Delegação
│      ↓
├── Descrição ─────────► PromptBuilder
│                           ↓
│                     Prometheus-Mentor
│                           ↓
├── Discernimento ◄──── Resposta
│
└── Diligência
```

Isso me leva a um insight interessante para o futuro.

Quando o Prometheus evoluir para um sistema de múltiplos agentes, alguns desses papéis **podem ser parcialmente automatizados**. Por exemplo, um "Agente Crítico" poderia revisar a resposta antes de entregá-la. Mas isso **não é mais o framework dos 4 Ds da Anthropic**; seria uma **decisão arquitetural do Prometheus** para auxiliar o usuário. O discernimento final continuaria sendo humano, exatamente como a Anthropic propõe.

Achei essa distinção muito importante. Ela evita confundir um **framework de uso responsável de IA** (voltado ao usuário) com uma **arquitetura de software** (voltada ao sistema). São camadas diferentes, embora possam dialogar entre si.