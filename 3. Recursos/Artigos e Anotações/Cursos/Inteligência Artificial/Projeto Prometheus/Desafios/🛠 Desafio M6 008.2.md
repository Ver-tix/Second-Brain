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