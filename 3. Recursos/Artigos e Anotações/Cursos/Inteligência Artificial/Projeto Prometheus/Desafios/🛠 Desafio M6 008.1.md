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
# Atualizar `settings.py`
```Python
from dotenv import load_dotenv
import os

# Carrega as variáveis do arquivo .env
load_dotenv()

# Recupera a chave da API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Toda configuração centralizada
MODEL_NAME = "gpt-4.1-mini"

# Verifica se a chave existe
if OPENAI_API_KEY is None:

    raise ValueError(

        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"

    )
```

# Preparando para o futuro
## `llm_catalog.py`
```Python
LLM_CATALOG = {
    "gpt_fast": {
        "provider": "openai",
        "model": "gpt-4.1-mini"
    },
    
    "gpt_reasoning": {
        "provider": "openai",
        "model": "o4-mini"
    },


    "gpt_deep_reasoning": {
        "provider": "openai",
        "model": "o3"
    },


    "claude_fast": {
        "provider": "anthropic",
        "model": "claude-3.5-haiku"
    },

    "claude_reasoning": {
        "provider": "anthropic",
        "model": "claude-sonnet-4"
    },
  
    "gemini_fast": {
        "provider": "google",
        "model": "gemini-2.5-flash"
    },

    "gemini_pro": {
        "provider": "google",
        "model": "gemini-2.5-pro"
    },

    "perplexity_search": {
        "provider": "perplexity",
        "model": "sonar-pro"
    },

    "grok_realtime": {
        "provider": "xai",
        "model": "grok"
    }

}
```

## `agent_profiles.py`

```Python
AGENT_PROFILES = {
    "mentor": {
        "llm": "gpt_reasoning"
    },
    
    "editor": {
        "llm": "claude_reasoning"
    },

    "researcher": {
        "llm": "perplexity_search"
    },

    "social_media": {
        "llm": "grok_realtime"
    }

}
```

# Atualizar `llm_servies.py`
```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY, MODEL_NAME

class LLMService:
    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str) -> str:
        response = self.client.responses.create(
            model=MODEL_NAME,
            input=prompt,
            tools = [
                {
                    "type": "function",
                    "name": "calculator",
                    "description": "Realiza operações matemáticas básicas entre dois números."
                }
            ]
        )
        return response.output_text
```

---
# Feedback