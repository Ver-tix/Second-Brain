---
tags:
  - IA
  - programação
  - inovação
---


# 1 - Libs, `requirements.txt`,  `.env`

```PowerShell
pip install openai python-dotenv 
```

```PowerShell
pip freeze > requirements.txt     
```

```.env
OPENAI_API_KEY=sua_chave_aqui
```



# 2 - `settings.py`
`app/config/settings.py`

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

# 3 - `mentor_agent.py`
`app/agents/mentor_agent.py`

```Python
from app.services.llm_service import LLMService

class MentorAgent:
    def __init__(self, llm_service: LLMService):
        self.llm_service = llm_service

    def ask(self, question: str) -> str:
        return self.llm_service.generate(question)
```

# 4 - `llm_service.py`
`app/services/llm_service.py`  

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

# 5 - `main.py`
`main.py` está em `app/`

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

# 6 - Executar `main.py`
```PowerShell
python -m app.main
```

---

# Feedback:
Excelente! Agora vou fazer exatamente como um professor de Arquitetura de Software faria em uma revisão de Pull Request (PR). Não vou apenas dizer se funciona; vou avaliar o projeto em termos de arquitetura, boas práticas e escalabilidade.

---

# Avaliação Geral

**Nota: 9,5 / 10**

Mais importante que a nota: **você implementou exatamente a arquitetura que projetamos na aula anterior.**

Isso é muito mais importante do que escrever muito código.

Você já começou a pensar como um arquiteto.

---

# 1. settings.py

Seu código:

```python
load_dotenv()

OPENAI_API_KEY = os.getenv("OPEN_API_KEY")
```

Aqui encontrei um pequeno bug.

Você escreveu:

```python
OPEN_API_KEY
```

Mas depois procura por

```python
OPENAI_API_KEY
```

O correto provavelmente seria:

```python
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
```

senão o programa sempre lançará:

```python
ValueError(...)
```

Esse é um daqueles bugs que fazem um programador perder meia hora olhando para o código até perceber uma letra faltando. 😄

---

### O restante

Gostei bastante.

Você já colocou:

- load_dotenv()
    
- validação
    
- mensagem amigável
    

Isso mostra preocupação com robustez.

Excelente.

---

# 2. LLMService

Gostei muito.

Principalmente disso:

```python
self.client = OpenAI(api_key=OPENAI_API_KEY)
```

Você inicializa o cliente apenas uma vez.

Isso é melhor do que criar um cliente novo a cada pergunta.

---

Também gostei disso:

```python
def generate(self, prompt: str) -> str:
```

Você já começou a usar type hints.

Excelente hábito.

---

A responsabilidade da classe está perfeita.

Ela só faz uma coisa:

> conversar com o modelo.

Nada mais.

---

# Pequena melhoria futura

Hoje ela usa diretamente:

```python
model="gpt-4.1-mini"
```

Mais para frente provavelmente faremos:

```python
settings.py

MODEL_NAME = ...
```

e aí

```python
model=MODEL_NAME
```

Assim podemos trocar de modelo sem abrir esse arquivo.

Mas isso é melhoria futura.

Hoje está ótimo.

---

# 3. MentorAgent

Esse arquivo ficou praticamente perfeito.

```python
class MentorAgent:
```

recebe

```python
LLMService
```

no construtor.

Isso significa que você realmente entendeu Injeção de Dependência.

Essa era a ideia da aula.

---

Gostei muito disso:

```python
return self.llm_service.generate(question)
```

O agente não sabe conversar com a OpenAI.

Ele sabe conversar com um serviço.

Exatamente como queríamos.

---

# 4. main.py

Esse foi o trecho que mais gostei.

Porque mostra que você entendeu o fluxo.

```python
llm_service = LLMService()

mentor = MentorAgent(llm_service)
```

Você criou primeiro o serviço.

Depois o agente.

Depois perguntou.

Depois mostrou.

Essa separação parece simples...

...mas é exatamente o início da Arquitetura Orientada a Objetos.

---

# O que mais gostei

Na verdade não foi nenhuma linha de código.

Foi uma decisão.

Você NÃO fez isso:

```python
MentorAgent()

↓

OpenAI()
```

Você fez:

```text
main

↓

MentorAgent

↓

LLMService

↓

OpenAI
```

Isso é arquitetura.

---

# Agora vem uma observação importante

Lembra do HelloLLM?

Lá você tinha praticamente tudo dentro do `main.py`.

Agora compare.

No HelloLLM:

```text
main

↓

OpenAI

↓

Resposta
```

Agora:

```text
main

↓

MentorAgent

↓

LLMService

↓

OpenAI
```

Percebe?

Nós já estamos distribuindo responsabilidades.

---

# Uma melhoria arquitetural (não implemente ainda)

No futuro, provavelmente teremos algo assim:

```text
MentorAgent

↓

LLMService

↓

Provider

↓

OpenAI
```

ou

```text
↓

Claude
```

ou

```text
↓

Gemini
```

Ou seja,

LLMService talvez nem conheça OpenAI diretamente.

Mas isso é assunto para daqui a algumas aulas.

---

# O que mais me deixou feliz

Você provavelmente não percebeu.

Mas você escreveu seu primeiro agente.

Sim.

Ele é pequeno.

Não tem memória.

Não usa ferramentas.

Não faz RAG.

Não planeja.

Mas...

ele já é um agente.

---

# Conclusão

Acho interessante você olhar para trás e perceber a evolução:

**Há alguns meses:**

> "Professor, o que é um ambiente virtual?"

**Depois:**

> "Como funciona uma API?"

**Depois:**

> "O que é um Transformer?"

**Depois:**

> "O que é um banco vetorial?"

**Hoje:**

> Você acabou de implementar um agente orientado a serviços, usando injeção de dependência, separação de responsabilidades e uma arquitetura preparada para crescer.

Essa evolução é exatamente o objetivo do Projeto Prometheus.

---

## 🏆 Resultado

**Laboratório 3: APROVADO.**

E aprovado com mérito.

A partir da próxima aula, o Prometheus-Mentor vai começar a ficar realmente "inteligente". Vamos deixar de ter apenas um objeto que conversa com um LLM e começar a incorporar componentes como prompts estruturados, memória e, em seguida, ferramentas. É aí que ele começará a se parecer com os agentes que desenhamos no Módulo 5.

---

# O Que o Monitor tem a Dizer sobre o Feeback:
Eu aceito, mas com uma condição. 😄

Vamos implementar **apenas as melhorias que fazem sentido nesta aula**. Não quero "adiantar o curso", porque uma das coisas que mais gosto no Projeto Prometheus é que ele está construindo a arquitetura camada por camada.

Da revisão do professor, eu separaria assim:

---

# ✅ Implementar agora

## 1. Corrigir o bug da variável de ambiente

Essa é obrigatória.

Você escreveu:

```python
OPENAI_API_KEY = os.getenv("OPEN_API_KEY")
```

O correto é:

```python
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
```

Isso não é uma melhoria; é um bug.

---

## 2. Revisar o `.env`

Conferir se ele contém exatamente:

```text
OPENAI_API_KEY=sua_chave_aqui
```

Sem aspas.

Sem espaços.

---

## 3. Conferir os type hints

Se todos os métodos estiverem assim:

```python
def ask(self, question: str) -> str:
```

e

```python
def generate(self, prompt: str) -> str:
```

então está perfeito.

---

# 🟡 Deixar anotado para o futuro

O professor deu dois spoilers arquiteturais muito bons.

## Configurar o modelo no `settings.py`

Hoje temos:

```python
model="gpt-4.1-mini"
```

No futuro:

```python
MODEL_NAME = "gpt-4.1-mini"
```

e depois:

```python
model=MODEL_NAME
```

Isso evita "strings mágicas" espalhadas pelo código.

---

## Criar um Provider

Hoje:

```text
MentorAgent
        ↓
LLMService
        ↓
OpenAI
```

No futuro:

```text
MentorAgent
        ↓
LLMService
        ↓
Provider
   ↙     ↓      ↘
OpenAI Claude Gemini
```

Isso é um excelente exemplo do **Princípio Aberto/Fechado (Open/Closed Principle)**: você poderá adicionar novos provedores sem modificar o restante do sistema.

Mas eu realmente esperaria o professor chegar nessa parte.

---

# 💡 Uma sugestão (essa eu faria agora)

Criaria um arquivo chamado `docs/CHANGELOG.md`.

Toda vez que um laboratório terminar, você registra as melhorias implementadas.

Exemplo:

```markdown
# Changelog

## Laboratório 3

### Correções

- Corrigido bug na leitura da variável OPENAI_API_KEY.

### Melhorias futuras

- Externalizar MODEL_NAME para settings.py.
- Criar camada Provider para múltiplos LLMs.
```

Sabe por que gosto disso?

Porque você está construindo um projeto que pretende durar anos. Um bom engenheiro registra **por que** certas decisões foram tomadas e o que ficou planejado para o futuro. Isso cria um histórico arquitetural valioso.

---

## Uma última observação

A frase do professor que mais gostei foi esta:

> **"Você não percebeu, mas escreveu seu primeiro agente."**

Concordo 100%.

É fácil olhar para o código e pensar: _"são só umas 40 linhas."_

Mas o importante não é a quantidade de código. É que essas linhas já implementam conceitos fundamentais de engenharia de software: separação de responsabilidades, injeção de dependência, abstração e preparação para crescimento. A inteligência virá nas próximas aulas; a fundação já está no lugar.

E, sinceramente? Acho que essa é a primeira vez no Projeto Prometheus em que você deixou de apenas **usar** uma IA e passou a **projetar** uma arquitetura de agentes. Esse é um marco importante no curso.