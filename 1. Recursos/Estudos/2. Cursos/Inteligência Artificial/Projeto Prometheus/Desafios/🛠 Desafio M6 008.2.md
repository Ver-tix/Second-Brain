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
# `llm_service.py`

```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY, MODEL_NAME
  
class LLMService:

    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str):
        response = self.client.responses.create(
            model=MODEL_NAME,
            input=prompt,
            tools=[
                {
                    "type": "function",
                    "name": "calculator",
                    "description": "Realiza operações matemáticas básicas entre dois números."
                }
            ]
        )

        # Nesta aula ainda simulamos que veio texto.
        return {
            "type": "text",
            "content": response.output_text
        }
```

# `mentor_agent.py`

```Python
from app.services.llm_service import LLMService

from app.prompts.mentor_prompt import PromptBuilder

from app.memory.conversation_memory import ConversationMemory

from app.tools.tool_manager import ToolManager

  
  
  

class MentorAgent:

    def __init__(

            self,

            llm_service: LLMService,

            memory: ConversationMemory,

            tool_manager: ToolManager

            ):

        self.llm_service = llm_service

        self.memory = memory

        self.tool_manager = tool_manager

  

    def ask(self, question: str) -> str:

  

        self.memory.add_user_message(question)

  

        history = self.memory.get_history()

  

        prompt = PromptBuilder.build(history, question)

  

        response = self.llm_service.generate(prompt)

  

        if response["type"] == "text":

            self.memory.add_assistant_message(response["content"])

            return response["content"]

  

        elif response["type"] == "tool_call":

            tool_name = response["tool"]

            tool_input = response["input"]

  

            if tool_name == "calculator":

                result = str(self.tool_manager.calculate(tool_input))

                self.memory.add_assistant_message(result)

                return result

            else:

                raise ValueError(f"Ferramenta desconhecida: {tool_name}")
```

# Novo Fluxo

```
                USUÁRIO
                    │
                    ▼
              MentorAgent
                    │
                    ▼
              LLMService
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
 Resposta Textual          Tool Call
        │                       │
        ▼                       ▼
   MentorAgent            ToolManager
        │                       │
        │                       ▼
        │                CalculatorTool
        │                       │
        │                       ▼
        └──────────────► Resultado
                            │
                            ▼
                        Usuário
```

---

# Feedback

Perdão, professor aqui falhou. 😄 Você realmente tinha enviado o desafio anteriormente. Revendo o que você implementou, segue minha correção.

---

# Avaliação Geral

**Nota: 9,5/10**

É, sem dúvida, um dos melhores laboratórios que você fez até agora.

E digo isso por um motivo específico: **você começou a pensar em arquitetura**, e não apenas em código.

Até poucas aulas atrás você estava escrevendo funções.

Agora você está implementando fluxo de execução.

Essa é uma mudança enorme.

---

# Parte 1 — Remover o ToolDecision

✅ Objetivo alcançado.

Você removeu:

```python
ToolDecision.decide(question)
```

Isso é exatamente o que queríamos.

Agora o Mentor não decide mais.

Isso deixa de existir:

```text
Usuário
   ↓
Python decide
   ↓
Ferramenta
```

e passa a existir:

```text
Usuário
   ↓
LLM decide
   ↓
Ferramenta
```

Essa mudança parece pequena.

Na prática ela muda completamente quem possui a inteligência do sistema.

Excelente.

---

# Parte 2 — LLMService

Você já começou a registrar as ferramentas:

```python
tools = [
    {
        "type": "function",
        "name": "calculator",
        "description": "..."
    }
]
```

Excelente.

Isso prepara a API para Tool Calling.

É exatamente o estado intermediário da Aula 8.1.

---

# Parte 3 — Arquitetura

O que mais gostei foi isto.

Antes:

```text
Mentor
 │
 ├── memória
 ├── prompt
 ├── if
 ├── calculator
 ├── llm
```

Agora:

```text
Mentor

↓

LLMService

↓

OpenAI

↓

ToolManager

↓

Calculator
```

Percebe?

O Mentor ficou mais limpo.

Isso é SRP funcionando.

---

# Parte 4 — Escalabilidade

Aqui apareceu algo muito importante.

Hoje existe apenas

```
calculator
```

Mas amanhã teremos

```
calculator
rag
search
email
calendar
filesystem
github
perplexity
```

Se a arquitetura estiver correta hoje...

...ela continua funcionando quando houver 50 ferramentas.

É exatamente por isso que insistimos tanto em separar responsabilidades.

---

# O único ponto que merece atenção

Ainda existe uma pequena inconsistência.

Você mesmo comentou:

> "estamos num estado intermediário"

Perfeito.

O problema é este trecho:

```python
expression = question.replace("calc:", "").strip()
```

Agora quem escolhe a ferramenta é o LLM.

Logo...

O usuário **não deveria mais escrever**

```
calc: 2 + 3
```

Quem deve produzir a expressão é o próprio Tool Call retornado pela OpenAI.

Ou seja...

Esse código vai desaparecer daqui a pouco.

Mas como estamos exatamente na transição da Aula 8.1 → 8.2...

Está correto mantê-lo temporariamente.

---

# O que mais gostei

Foi perceber que você já está escrevendo código pensando no Prometheus.

Há alguns meses suas perguntas eram:

> "Como faço isso funcionar?"

Hoje elas são:

> "Onde essa responsabilidade deveria morar?"

Essa é exatamente a mudança que eu queria provocar neste curso.

---

# Próxima etapa (Aula 8.3)

Na próxima aula acontece uma das maiores simplificações do projeto.

Até agora fazemos algo parecido com isto:

```text
OpenAI

↓

"Quero chamar calculator"
```

Depois o Python tenta descobrir o restante.

Na Aula 8.3 acontece a virada definitiva:

```text
OpenAI

↓

calculator

↓

{
    "a": 10,
    "b": 5,
    "operation": "multiply"
}
```

Ou seja, o modelo não apenas escolhe a ferramenta: **ele já devolve os argumentos estruturados em JSON**.

Nesse momento você vai perceber por que insistimos tanto em falar de schemas, contratos e interfaces desde o Módulo 5. Tudo aquilo começa a convergir para uma implementação elegante, escalável e pronta para sustentar o ecossistema multiagente do Prometheus.