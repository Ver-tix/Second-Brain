---
tags:
  - IA
  - programação
  - inovação
---
# `calculator_tool.py`:

```Python
class CalculatorTool:
    def add(self, a: float, b: float) -> float:
        c = a + b
        return c

    def subtract(self, a: float, b: float) -> float:
        c = a - b
        return c

    def multiply(self, a: float, b: float) -> float:
        c = a * b
        return c

    def divide(self, a: float, b: float) -> float:
        if b == 0:
            raise ValueError("Não é possível dividir por zero.")

        c = a/b
        
        return c
```

# `tool_manager.py`:

```Python
from app.tools.calculator_tool import CalculatorTool

class ToolManager:
    def __init__(self):
        self.calculator = CalculatorTool()

    def calculate(self, expression: str) -> float:
        parts = expression.split()
        a = float(parts[0])
        operator = parts[1]
        b = float(parts[2])

        if operator == "+":
            return self.calculator.add(a, b)

        elif operator == "-":
            return self.calculator.subtract(a, b)

        elif operator == "*":
            return self.calculator.multiply(a, b)

        elif operator == "/":
            return self.calculator.divide(a, b)

        else:
            raise ValueError("Operador inválido.")
```

# Alterações no `mentor_agent.py`:

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
        if question.startswith("calc:"):
            expression = question.replace("calc:", "").strip()
            
            return self.tool_manager.calculate(expression)

        self.memory.add_user_message(question)
        
        history = self.memory.get_history()

        prompt = PromptBuilder.build(history, question)

        response = self.llm_service.generate(prompt)

        self.memory.add_assistant_message(response)

        return response
```

# Alterações no `main.py`:

```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager 

def main():

    # Cria o serviço responsável por conversar com a OpenAI
    llm_service = LLMService()

    # Cria a memória dentro do programa
    memory = ConversationMemory()

    # Cria o gestor de ferramentas (tool manager)
    tool_manager = ToolManager()

    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager
        )

    # Loop de Questionamento:
    while True:
        # Recebe a pergunta do usuário
        question = input("\nPergunte alguma coisa: ")

        # Condição para sair
        if question.lower() == "sair":
            print("\nAté a próxima!")
            break

        # O agente responde
        answer = mentor.ask(question)

        # Exibe a resposta
        print("\nPrometheus-Mentor:\n")
        print(answer) 

if __name__ == "__main__":
    main()
```