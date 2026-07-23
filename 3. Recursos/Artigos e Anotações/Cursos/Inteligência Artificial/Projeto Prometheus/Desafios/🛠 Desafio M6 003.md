---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# 1 - `.env`

```.env
OPENAI_API_KEY=sua_chave_aqui
```

# 2 - `settings.py`

```Python
from dotenv import load_dotenv
import os 

# Carrega as variáveis do arquivo .env
load_dotenv()

# Recupera a chave da API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Verifica se a chave existe
if OPENAI_API_KEY is None:
    raise ValueError(
        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"
    )
```

# 3 - `llm_service.py`

```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY

class LLMService:
    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str) -> str:
        response = self.client.responses.create(
            model="gpt-4.1-mini",
            input=prompt
        )

        return response.output_text
```

# `main.py`

```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService

def main():

    # Cria o serviço responsável por conversar com a OpenAI
    llm_service = LLMService()

    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(llm_service)

    # Recebe a pergunta do usuário
    question = input("Pergunte alguma coisa: ")

    # O agente responde
    answer = mentor.ask(question)

    # Exibe a resposta
    print("\nPrometheus-Mentor:\n")
    print(answer)
    
if __name__ == "__main__":

    main()
```