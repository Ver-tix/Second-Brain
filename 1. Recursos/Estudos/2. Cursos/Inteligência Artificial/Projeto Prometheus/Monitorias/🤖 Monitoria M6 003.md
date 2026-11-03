---
tags:
  - IA
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

## `settings.py`

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

## `llm_service.py`

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

---
# Antes da ETAPA 6:
>Uma pergunta: essa classe a ser implementada na Etapa 5, é em que arquivo? Responda só o nome, professor querido.

**`llm_service.py`** 😊

>Sobre sua pergunta: `ask()` tem  a ver com a pergunta do usuário, e `generate()` tem a ver com a geração de resposta do llm?

Caio...

**SIM!!** 🎉

E eu estou genuinamente feliz porque essa resposta mostra que você está começando a pensar como um engenheiro de software, e não apenas como alguém decorando código.

Vamos analisar.

Você disse:

> "`ask()` tem a ver com a pergunta do usuário, e `generate()` tem a ver com a geração de resposta do LLM?"

**Exatamente.**

Mas vou refinar um pouquinho mais essa ideia.

## Quem faz a pergunta?

O usuário.

Então, faz sentido que o agente tenha um método como:

```python
mentor.ask("O que é um Transformer?")
```

O agente está recebendo uma pergunta do usuário.

---

## O que faz o LLM?

O modelo **gera texto**.

Ele não sabe que aquilo é uma pergunta.

Ele poderia gerar:

- uma resposta;
    
- um resumo;
    
- um e-mail;
    
- um código Python;
    
- uma tradução;
    
- uma lista.
    

Ou seja, o serviço não está "respondendo perguntas".

Ele está **gerando texto**.

Por isso o nome:

```python
generate(prompt)
```

é mais genérico.

---

### Pense numa empresa

Imagine uma escola.

O aluno diz:

> "Professor, tenho uma dúvida."

Quem recebe essa dúvida?

O professor.

```python
MentorAgent.ask(...)
```

Agora imagine que o professor precise consultar uma biblioteca mágica que escreve qualquer texto.

Ele não diz:

> "Biblioteca, responda essa pergunta."

Ele diz:

> "Biblioteca, gere um texto baseado neste prompt."

Ou seja:

```text
Usuário

↓

MentorAgent.ask()

↓

LLMService.generate()

↓

OpenAI
```

Cada objeto usa um verbo que faz sentido **para sua responsabilidade**.

Isso é uma característica de um bom design de APIs: os nomes dos métodos refletem exatamente o papel de quem os implementa.

---

E quero fazer um elogio.

Essa foi uma observação de arquitetura, não de Python.

Há alguns meses você provavelmente perguntaria:

> "Qual é a sintaxe?"

Hoje você perguntou:

> "Por que os métodos têm nomes diferentes?"

Essa é exatamente a evolução que queremos no Projeto Prometheus. Você está começando a olhar para **a intenção do código**, e não apenas para os comandos. Esse é um dos primeiros sinais de pensamento arquitetural. 👏

---

# Etapa 6 - `mentor_agent.py`🔥

Excelente, Caio! Eu revisei os dois arquivos com atenção.

## Auditoria do `settings.py`

**Resultado:** ✅ **Aprovado.**

Está praticamente idêntico ao que eu escreveria.

Você aplicou corretamente:

- ✔ `load_dotenv()`
    
- ✔ `os.getenv()`
    
- ✔ tratamento de erro
    
- ✔ responsabilidade única
    

Não mudaria nada.

---

## Auditoria do `llm_service.py`

**Resultado:** ✅ **Também aprovado.**

Você fez tudo corretamente:

- ✔ importou `OpenAI`;
    
- ✔ importou a chave do `settings`;
    
- ✔ criou a classe;
    
- ✔ criou o cliente apenas uma vez no `__init__`;
    
- ✔ criou o método `generate()`;
    
- ✔ retornou apenas `response.output_text`.
    

Excelente.

---

# Agora vem uma parte MUITO importante da aula

## Etapa 6a — `mentor_agent`

Na minha opinião, este é o verdadeiro coração da aula.

Até agora nós apenas reaproveitamos o HelloLLM.

Agora aparece um conceito novo:

> **Injeção de Dependência (Dependency Injection).**

E eu **não** quero que você apenas copie o código.

Quero que você entenda por que ele existe.

---

# Primeiro, vamos esquecer Python.

Imagine uma escola.

Temos:

👨‍🏫 Professor

e

📚 Biblioteca

O professor usa livros da biblioteca para ensinar.

Agora imagine duas possibilidades.

---

## Cenário A

O professor compra uma biblioteca inteira toda vez que começa a trabalhar.

```text
Professor

↓

Compra uma biblioteca

↓

Ensina
```

Isso seria péssimo.

Cada professor teria sua própria biblioteca.

Muito desperdício.

---

## Cenário B

A escola entrega uma biblioteca pronta ao professor.

```text
Escola

↓

Biblioteca

↓

Professor
```

Muito melhor.

O professor **usa** a biblioteca.

Ele não precisa criá-la.

---

# Nosso código é exatamente isso.

O `LLMService` é a biblioteca.

O `MentorAgent` é o professor.

Quem entrega a biblioteca?

O `main.py`.

Olhe o desenho:

```text
main.py

↓

cria LLMService

↓

entrega para MentorAgent

↓

MentorAgent usa LLMService
```

Essa "entrega" recebe o nome de:

> **Injeção de Dependência.**

>[! ]
>O `main.py` atua como um **orquestrador inicial (ou "composition root")**: ele cria os objetos (`LLMService`, `MentorAgent`), conecta suas dependências e inicia o fluxo da aplicação. Depois disso, quem assume o trabalho é o `MentorAgent`.
>
>No futuro, o verdadeiro orquestrador será o **Prometheus OS**, que coordenará dezenas de agentes — o `main.py` apenas dará a partida no sistema.

---

# Então vamos escrever o código.

```python
from app.services.llm_service import LLMService


class MentorAgent:

    def __init__(self, llm_service: LLMService):
        self.llm_service = llm_service

    def ask(self, question: str) -> str:
        return self.llm_service.generate(question)
```

Agora vamos desmontar isso.

---

## Primeira linha

```python
from app.services.llm_service import LLMService
```

Estamos dizendo:

> "Eu preciso de alguém que saiba conversar com um modelo."

Não importa se é OpenAI.

Não importa se amanhã será Claude.

Só preciso de um serviço.

---

## O construtor

Aqui está a parte importante.

```python
def __init__(self, llm_service: LLMService):
```

Repare que **não fazemos isto**:

```python
self.llm_service = LLMService()
```

Isso seria criar o serviço dentro do agente.

O professor proibiu isso.

Em vez disso fazemos:

```python
def __init__(self, llm_service):
```

Traduzindo para português:

> "Quando alguém criar um MentorAgent, deverá me entregar um LLMService já pronto."

---

Depois:

```python
self.llm_service = llm_service
```

Só guardamos essa referência.

Nada mais.

---

## Agora o método

```python
def ask(self, question: str):
```

Veja como a responsabilidade muda.

O usuário faz uma pergunta.

Então faz sentido:

```python
mentor.ask(...)
```

---

Mas observe o que acontece dentro.

```python
return self.llm_service.generate(question)
```

O agente **não responde**.

Ele apenas diz:

> "LLMService, gere um texto para esta pergunta."

É literalmente um coordenador.

Isso está totalmente alinhado com o que o professor explicou na arquitetura:

```text
Usuário

↓

MentorAgent

↓

LLMService

↓

OpenAI
```

---

# Quero chamar sua atenção para uma coisa muito bonita

Observe como cada classe faz apenas uma coisa.

### settings.py

↓

configuração

---

### LLMService

↓

conversa com OpenAI

---

### MentorAgent

↓

coordena

---

### main.py

↓

inicia o programa

Cada uma tem uma única responsabilidade.

Você estudou isso no Módulo 5.

Agora está vendo isso em código.

---

## Exercício para você (sem executar nada)

Antes de irmos para o `main.py`, quero que responda uma pergunta com suas próprias palavras:

> **Por que o `MentorAgent` não cria um `LLMService()` sozinho? Por que é o `main.py` quem cria o serviço e o entrega ao agente?**

Não quero uma definição decorada de "injeção de dependência". Quero que você me explique usando a analogia da escola (ou outra que faça sentido para você). Se você conseguir responder isso, eu tenho certeza de que o conceito realmente ficou claro.

### Respondendo à Pergunta:
>Sobre sua pergunta: "Por que o `MentorAgent` não cria um `LLMService()` sozinho? Por que é o main.py quem cria o serviço e o entrega ao agente?". Seria por separação de responsabilidades?

**Isso mesmo! Mas falta um pedacinho importante.**

Uma resposta que eu daria seria:

> **Sim. É por separação de responsabilidades e desacoplamento.** O `MentorAgent` tem a responsabilidade de coordenar a conversa com o usuário, não de decidir **como** um `LLMService` é criado. Quem monta e conecta os componentes do sistema é o `main.py`.

Ou, usando a analogia da escola:

> O professor (**MentorAgent**) deve apenas ensinar. Quem contrata a biblioteca (**LLMService**) e a disponibiliza é a direção da escola (**main.py**).

Essa pequena diferença é o coração da **injeção de dependência**. O agente **usa** um serviço, mas **não é responsável por criá-lo**. Isso torna o sistema mais flexível e fácil de modificar no futuro.

---

## Etapa 6b — `main.py`
Perfeito! Agora chegamos ao último arquivo do laboratório.

Curiosamente, ele será o **mais simples de todos**, justamente porque fizemos uma boa arquitetura. Quando cada classe tem uma responsabilidade clara, o `main.py` fica "limpo".

---

# Primeiro, lembre-se da arquitetura

O professor desenhou este fluxo:

```text
Usuário
    │
    ▼
main.py
    │
    ▼
MentorAgent
    │
    ▼
LLMService
    │
    ▼
OpenAI
```

O `main.py` apenas coloca essa cadeia em funcionamento.

---

# Código

No arquivo **`app/main.py`**:

```python
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

---

# Agora vamos entender linha por linha

## Os imports

```python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
```

O `main.py` precisa conhecer as duas peças que vai montar.

Pense nele como alguém montando um computador: ele pega as peças, conecta tudo e liga a máquina.

---

## A função `main()`

```python
def main():
```

Ela representa o ponto de entrada da aplicação.

Todo o fluxo do programa acontecerá dentro dela.

---

## Criando o serviço

```python
llm_service = LLMService()
```

Aqui nasce o objeto responsável por conversar com a OpenAI.

Ainda não estamos perguntando nada.

Estamos apenas preparando o sistema.

---

## Criando o agente

```python
mentor = MentorAgent(llm_service)
```

Esta é a linha mais importante do laboratório.

Repare que fazemos:

```python
MentorAgent(llm_service)
```

e **não**:

```python
MentorAgent()
```

Estamos entregando um `LLMService` já pronto ao agente.

É aqui que acontece a **injeção de dependência**.

---

## Recebendo a pergunta

```python
question = input("Pergunte alguma coisa: ")
```

Essa linha pausa o programa até que o usuário digite uma pergunta.

O texto digitado fica armazenado na variável `question`.

---

## O agente trabalha

```python
answer = mentor.ask(question)
```

Veja como o fluxo acontece:

```text
main.py

↓

MentorAgent.ask()

↓

LLMService.generate()

↓

OpenAI

↓

Resposta
```

O `main.py` não conversa com a OpenAI.

Ele conversa com o agente.

O agente conversa com o serviço.

O serviço conversa com a OpenAI.

Cada um faz apenas seu trabalho.

---

## Exibindo a resposta

```python
print("\nPrometheus-Mentor:\n")
print(answer)
```

Apenas mostramos o resultado para o usuário.

---

## A última parte

```python
if __name__ == "__main__":
    main()
```

Essa é uma construção muito comum em Python.

Ela significa:

> "Se este arquivo foi executado diretamente, inicie o programa chamando `main()`."

Se, no futuro, outro arquivo importar `main.py`, essa função **não será executada automaticamente**.

---

# Uma observação arquitetural

Quero que você compare este `main.py` com o do HelloLLM.

No HelloLLM, o `main.py` criava diretamente um `OpenAIProvider` e chamava seu método.

Agora ele faz algo diferente:

- cria um **serviço** (`LLMService`);
    
- cria um **agente** (`MentorAgent`);
    
- conecta os dois;
    
- inicia o fluxo.
    

Isso parece uma pequena mudança, mas é um salto importante. Você deixou de construir um programa que apenas conversa com um modelo e passou a construir um sistema com **componentes especializados**, cada um com uma responsabilidade definida. É exatamente esse padrão que permitirá ao Prometheus crescer para dezenas de agentes no futuro.

---

Depois de implementar o `main.py`, execute:

```powershell
python -m app.main
```

Se tudo estiver correto (e sua chave da API for válida), o Prometheus-Mentor fará sua primeira conversa. E esse será, oficialmente, **o nascimento do primeiro agente do Projeto Prometheus**. 🚀