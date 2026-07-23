---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
Bom dia, Caio! 😄

Primeiramente... **parabéns ao professor principal**. Eu gostei MUITO dessa aula.

Ela faz uma coisa extremamente importante: ela transforma o diagrama da aula anterior em objetos Python. Pela primeira vez, você vai perceber que aqueles "quadradinhos" da arquitetura viram **classes**, e as setas viram **chamadas de métodos**.

E quero combinar uma coisa com você, assim como fizemos no HelloLLM.

> **Eu não vou apenas te dar o código. Vou explicar linha por linha, como se estivéssemos programando lado a lado.**

No Projeto Prometheus, meu papel é ser seu **auditor e mentor de engenharia**, não um gerador de código.

---

# Antes de começar

Quero fazer uma observação importante.

O professor disse:

> "Tente lembrar do HelloLLM."

Isso não foi uma dica qualquer.

Na verdade, **80% do laboratório é uma evolução direta do HelloLLM**.

Veja a comparação.

## HelloLLM

```text
main.py

↓

OpenAIProvider

↓

OpenAI
```

Agora:

```text
main.py

↓

MentorAgent

↓

LLMService

↓

OpenAI
```

A diferença é que apareceu uma camada nova:

```text
MentorAgent
```

Ou seja...

Você já sabe fazer quase tudo isso.

---

# ETAPA 4

## settings.py

O professor disse apenas:

> carregar o .env.

Vamos fazer exatamente como no HelloLLM.

Crie este arquivo:

```python
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

Agora vamos entender linha por linha.

---

## Primeira linha

```python
from dotenv import load_dotenv
```

Lembra do `.env`?

Ele é apenas um arquivo de texto.

O Python **não lê esse arquivo sozinho**.

Quem faz isso é a biblioteca **python-dotenv**.

Essa linha diz:

> "Importe a função capaz de ler o arquivo `.env`."

---

## Segunda linha

```python
import os
```

A biblioteca `os` permite acessar informações do sistema operacional.

Entre elas:

- variáveis de ambiente.
    

---

## Depois

```python
load_dotenv()
```

Essa linha literalmente faz isto:

```text
.env

↓

Python passa a conhecer suas variáveis
```

Sem ela...

```python
os.getenv(...)
```

não encontraria nada.

---

## Depois

```python
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
```

Essa é uma linha muito elegante.

Imagine que o `.env` contém:

```text
OPENAI_API_KEY=abc123
```

O Python faz:

```python
OPENAI_API_KEY = "abc123"
```

Pronto.

Agora qualquer parte do programa pode importar essa variável.

---

## O if

```python
if OPENAI_API_KEY is None:
```

Traduzindo:

> "Se não encontrou a chave..."

Então:

```python
raise ValueError(...)
```

Interrompa o programa.

Melhor isso do que deixar o erro aparecer só quando tentar chamar a OpenAI.

---

# Até aqui temos

```text
settings.py

↓

lê o .env

↓

guarda a chave

↓

disponibiliza para o resto do sistema
```

Nada mais.

Nada menos.

Percebe o princípio da responsabilidade única?

---

# ETAPA 5

Agora chegamos ao arquivo mais interessante.

## llm_service.py

O professor pediu:

> "Crie uma classe."

Primeiro vou mostrar o código.

Depois vamos desmontá-lo peça por peça.

```python
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

Agora vamos entender.

---

## O import

```python
from openai import OpenAI
```

Importa o cliente oficial da OpenAI.

É ele que sabe conversar com a API.

---

Depois:

```python
from app.config.settings import OPENAI_API_KEY
```

Perceba uma coisa linda.

O serviço **não sabe** onde está o `.env`.

Ele só pede:

> "Me dê a chave."

Quem resolveu isso foi o `settings.py`.

**==Responsabilidades separadas.==**

---

## A classe

```python
class LLMService:
```

Essa classe será responsável apenas por conversar com modelos.

Nada mais.

Ela não ensina.

Não faz RAG.

Não usa memória.

Só conversa com LLM.

---

## O construtor

```python
def __init__(self):
```

Lembra quando estudamos orientação a objetos?

Esse método é executado automaticamente quando fazemos:

```python
service = LLMService()
```

Ele prepara o objeto.

---

## Aqui

```python
self.client = OpenAI(...)
```

Estamos dizendo:

> "Dentro do meu serviço, existe um cliente da OpenAI."

Esse cliente ficará guardado.

Não precisamos recriá-lo toda hora.

---

## Agora o método importante

```python
def generate(self, prompt: str):
```

O professor escreveu:

> Receber um texto.

Esse texto é:

```python
prompt
```

---

Depois:

```python
response = self.client.responses.create(...)
```

Essa linha envia a pergunta para a OpenAI.

---

Depois

```python
return response.output_text
```

Voltamos apenas o texto.

Nada de JSON.

Nada de metadados.

Só a resposta.

---

# Até aqui o fluxo é

```text
LLMService

↓

recebe pergunta

↓

envia para OpenAI

↓

recebe resposta

↓

devolve texto
```

---

## Eu quero te fazer uma pergunta (não responda agora)

Observe uma curiosidade.

No HelloLLM o método chamava:

```python
ask()
```

Aqui o professor chamou:

```python
generate()
```

Por que será?

A resposta tem relação com arquitetura.

E eu quero que você tente descobrir sozinho quando terminarmos o laboratório.

---

# Vamos parar por aqui

Eu prefiro seguir exatamente como fizemos no HelloLLM: **uma etapa de cada vez**.

Implemente apenas:

- `app/config/settings.py`
    
- `app/services/llm_service.py`
    

Quando terminar, **cole o código aqui** (ou envie um print), e eu farei uma revisão como um Tech Lead faria em uma revisão de Pull Request.

Depois seguiremos para a Etapa 6 (`MentorAgent`), que é justamente onde aparece um dos conceitos mais importantes da aula: **injeção de dependência**. E eu prometo que vou explicar esse conceito de uma forma que faça muito sentido para alguém que ainda está começando em programação.