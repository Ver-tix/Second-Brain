---
tags:
  - IA
  - tecnologia
  - programação
---
# Single Responsibility Principle (SRP)

Cada arquivo deve possuir **uma única responsabilidade**.

Não significa apenas "ter pouco código".

Significa:

> **Ter apenas um motivo para mudar.**

Vamos analisar.

---

## `main.py`

Responsabilidade:

> Coordenar a execução da aplicação.

Ele deve fazer coisas como:

- iniciar o programa;
- pedir uma pergunta ao usuário;
- chamar o provider;
- mostrar a resposta.

Ele **não** deve:

- conhecer API Keys;
- montar requisições HTTP;
- saber como conversar com a OpenAI.

---

## `config.py`

Responsabilidade:

> Centralizar todas as configurações.

Ele sabe:

- onde está o `.env`;
- qual é a API Key;
- eventualmente qual modelo usar.

Ele **não** conversa com a OpenAI.

Ele apenas fornece configurações.

---

## `.env`

Responsabilidade:

Guardar informações sensíveis.

Por exemplo:

```
OPENAI_API_KEY=...
```

Ele não contém código.

---

## `openai_provider.py`

Responsabilidade:

Conversar com a OpenAI.

Ele sabe:

- criar o cliente;
- enviar mensagens;
- receber respostas.

Ele não sabe:

- quem é o usuário;
- como imprimir na tela;
- como pedir uma pergunta.

---

## `requirements.txt`

Responsabilidade:

Descrever as dependências do projeto.

Ele não participa da execução.

Serve para que outra pessoa consiga reconstruir o ambiente.

---

## `.gitignore`

Responsabilidade:

Dizer ao Git quais arquivos devem ser ignorados.

Também não participa da execução.

---

# Li a aula inteira.

Primeiro, quero dizer algo como auditor do Projeto Prometheus.

**Esta é uma excelente aula de arquitetura.**

Mas, do ponto de vista pedagógico para um leigo em programação, ela possui uma lacuna importante.

Ela diz **o que deve ser feito**, mas não explica **como fazer**.

Para alguém que já programa há alguns anos, isso é suficiente.

Para quem está começando do zero, ainda ficam várias perguntas:

- Onde eu crio esses arquivos?
    
- Como faço um ambiente virtual?
    
- O que é um terminal?
    
- O que é instalar uma biblioteca?
    
- Como o Python encontra o arquivo `.env`?
    
- Como o `requirements.txt` é usado?
    

Ou seja, falta um "modo oficina".

E é exatamente esse papel que vou assumir.

---

# Primeiro: esqueça programação por alguns minutos.

Vamos imaginar que estamos montando uma oficina.

Queremos construir um robô.

Antes de construir o robô precisamos preparar a oficina.

É exatamente isso que faremos.

---

# PASSO 1 — Criar uma pasta do projeto

Imagine que todo projeto é uma empresa.

Toda empresa possui um prédio.

Nosso prédio será uma pasta.

Exemplo:

```
HelloLLM/
```

Tudo ficará dentro dela.

Até aqui temos:

```
HelloLLM/
```

Nada mais.

---

# PASSO 2 — Abrir essa pasta no VS Code

Provavelmente você está usando o VS Code.

Abra essa pasta.

Ela aparecerá assim:

```
HelloLLM
```

Vazia.

---

# PASSO 3 — O Terminal

Aqui começa uma das maiores dificuldades dos iniciantes.

A aula diz:

> "Crie um ambiente virtual."

Mas...

Como?

Primeiro precisamos abrir o Terminal.

No VS Code:

```
Terminal

↓

Novo Terminal
```

Aparecerá algo parecido com isto:

```
C:\Users\Caio\HelloLLM>
```

Esse terminal é simplesmente uma forma de conversar com o computador usando texto.

Pense nele como um chat.

Você escreve.

O computador responde.

---

# PASSO 4 — Criar o ambiente virtual

Agora escrevemos:

```
python -m venv .venv
```

Mas...

O que isso significa?

Vamos quebrar.

```
python
```

Estamos dizendo:

> Execute o Python.

---

Depois:

```
-m
```

Significa:

> Execute um módulo interno do Python.

---

Depois:

```
venv
```

É justamente o módulo que sabe criar ambientes virtuais.

---

Depois:

```
.venv
```

É o nome da pasta onde ficará nosso ambiente.

Então esse comando inteiro significa:

> Python, por favor crie um ambiente virtual chamado ".venv".

---

Depois disso sua pasta ficará assim:

```
HelloLLM/

│

├── .venv
```

Essa pasta pode ter centenas de arquivos.

É normal.

---

# PASSO 5 — Ativar o ambiente virtual

Agora vem outra dúvida.

Criar não basta.

Precisamos entrar nele.

No Windows:

```
.venv\Scripts\activate
```

Quando der certo aparecerá algo parecido com:

```
(.venv)

C:\HelloLLM>
```

Esse

```
(.venv)
```

é como uma luz verde dizendo:

> Agora tudo que você instalar ficará apenas neste projeto.

---

# PASSO 6 — Criar o requirements.txt

Agora sim.

Criamos um arquivo chamado:

```
requirements.txt
```

Dentro dele escrevemos:

```
openai

python-dotenv
```

## Na Prática: por que NÃO criar o `requirements.txt` na mão?
Muita gente faz:

```
Novo arquivorequirements.txt
```

Mas, em projetos reais, quase nunca é assim.

Nós deixamos o ambiente ser construído primeiro e depois perguntamos ao `pip`:

> "Quais bibliotecas realmente estão instaladas aqui?"

Ele responde gerando o arquivo automaticamente.

Execute:

```
pip freeze > requirements.txt
```

Depois abra o arquivo.

Você verá algo parecido com:

```
annotated-types==0.7.0anyio==4.14.1httpx==0.28.1openai==2.44.0python-dotenv==1.2.2...
```

Esse arquivo é uma **fotografia exata do ambiente**.


---

# PASSO 7 — Instalar as bibliotecas

Agora o Python lê esse arquivo.

Escrevemos:

```
pip install -r requirements.txt
```

Vamos traduzir.

```
pip
```

É o instalador de bibliotecas.

---

```
install
```

Instale.

---

```
-r
```

Leia um arquivo.

---

```
requirements.txt
```

Leia esta lista.

Então estamos dizendo:

> Instale tudo que estiver nesta lista.

---

# PASSO 8 — Criar o .env

Agora criamos outro arquivo.

```
.env
```

Dentro:

```
OPENAI_API_KEY=sua_chave_aqui
```

Não coloque aspas.

---

# PASSO 9 — Criar os arquivos

Agora começamos a montar nossa arquitetura.

```
HelloLLM/

│

├── main.py

├── config.py

├── .env

├── requirements.txt

│

└── providers

      └── openai_provider.py
```

Perceba que ainda não escrevemos código.

Só montamos a oficina.

---

# O que existe em cada arquivo?

Esta parte é importantíssima.

## main.py

É o gerente.

Ele coordena tudo.

---

## config.py

É o cofre.

Ele busca informações importantes.

Exemplo:

A chave da API.

---

## openai_provider.py

É o funcionário especializado.

Ele sabe conversar com a OpenAI.

---

## .env

É o cofre secreto.

Guarda senhas.

---

## requirements.txt

É a lista de ferramentas necessárias para montar a oficina.

---

# O fluxo completo

Agora quero mostrar algo que, na minha opinião, deveria estar na aula.

```
Você digita:

↓

main.py

↓

config.py

↓

Lê a chave da API

↓

openai_provider.py

↓

SDK da OpenAI

↓

Internet

↓

Servidor da OpenAI

↓

Resposta

↓

SDK

↓

openai_provider.py

↓

main.py

↓

Tela
```

Esse desenho vale mais do que centenas de linhas de código.

---

# Minha crítica pedagógica à aula

Como auditor do Projeto Prometheus, registraria mais um gap.

## 🚨 Gap #006 — Da Arquitetura para a Implementação

A aula explica **por que** cada elemento existe, mas pressupõe que o aluno já saiba executar tarefas básicas do ecossistema Python.

Para um iniciante absoluto, seria muito útil inserir uma **mini-oficina prática** antes do desafio, abordando:

1. Como abrir o terminal no VS Code.
    
2. O que é um comando de terminal.
    
3. Como criar um ambiente virtual (`python -m venv .venv`).
    
4. Como ativá-lo.
    
5. Como criar arquivos e pastas do projeto.
    
6. Como usar o `pip install -r requirements.txt`.
    
7. Como verificar se tudo foi instalado corretamente.
    

Essa oficina não precisaria ensinar programação, apenas **preparar o ambiente**. Ela serviria como uma ponte entre a excelente explicação arquitetural da aula e a primeira experiência prática do aluno.

## Minha proposta

Em vez de simplesmente escrever o código do **Hello, LLM!**, gostaria de atuar como seu professor de laboratório.

Nós construiremos esse projeto **juntos**, linha por linha. A cada comando que digitarmos, vou explicar:

- o que ele faz;
    
- por que ele existe;
    
- o que aconteceria se ele não existisse;
    
- como ele se encaixa na arquitetura do Projeto Prometheus.
    

Assim, você não apenas terá um programa funcionando, mas entenderá profundamente cada etapa do processo. Acho que essa abordagem está totalmente alinhada com o espírito do Projeto Prometheus: compreender os fundamentos antes de avançar para implementações mais complexas.


---

# Como as informações percorrem o sistema

Quando o usuário executa o programa, o arquivo `main.py` é o ponto de entrada da aplicação. Ele solicita que o usuário digite uma pergunta e recebe esse texto como entrada. Em seguida, o `main.py` utiliza a classe responsável por conversar com o modelo de linguagem, localizada em `providers/openai_provider.py`.

O `OpenAIProvider`, antes de enviar a pergunta, obtém a chave da API por meio do `config.py`, que lê essa informação do arquivo `.env`. Dessa forma, a chave permanece separada do código-fonte, aumentando a segurança da aplicação.

Com a chave disponível, o `OpenAIProvider` utiliza o SDK da OpenAI para enviar a pergunta ao servidor do modelo através da API. O servidor processa a solicitação, gera uma resposta e a devolve ao SDK. Por fim, essa resposta retorna ao `main.py`, que a exibe na tela para o usuário.

---

# Agora vamos implementar o projeto

Primeiro, a estrutura de pastas deve ficar assim:

```text
HelloLLM/
│
├── .venv/
├── .env
├── requirements.txt
├── config.py
├── main.py
│
└── providers/
    ├── __init__.py
    └── openai_provider.py
```

---

## 1. requirements.txt

```text
openai
python-dotenv
```

Este arquivo informa quais bibliotecas precisam ser instaladas no projeto.

Depois execute no terminal:

```bash
pip install -r requirements.txt
```

---

## 2. Arquivo .env

```text
OPENAI_API_KEY=sua_chave_aqui
```

Substitua `sua_chave_aqui` pela chave obtida na OpenAI.

---

## 3. config.py

Este arquivo será responsável apenas por carregar a variável de ambiente.

```python
from dotenv import load_dotenv
import os

load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
```

Observe como este arquivo tem apenas uma responsabilidade: disponibilizar a chave para o restante do sistema.

---

## 4. providers/openai_provider.py

```python
from openai import OpenAI
from config import OPENAI_API_KEY


class OpenAIProvider:

    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def ask(self, question: str) -> str:

        response = self.client.responses.create(
            model="gpt-4.1-mini",
            input=question
        )

        return response.output_text
```

Vamos interpretar essa classe:

- quando criamos um `OpenAIProvider`, ele cria internamente um cliente da OpenAI;
    
- esse cliente já sabe qual chave utilizar;
    
- o método `ask()` recebe uma pergunta;
    
- envia a pergunta para a OpenAI;
    
- devolve apenas o texto da resposta.
    

---

## 5. main.py

```python
from providers.openai_provider import OpenAIProvider


def main():

    provider = OpenAIProvider()

    question = input("Pergunte alguma coisa: ")

    answer = provider.ask(question)

    print("\nResposta:\n")
    print(answer)


if __name__ == "__main__":
    main()
```

---

# O fluxo acontecendo de verdade

Agora quero mostrar exatamente o que acontece quando você executa o programa.

Você digita:

```text
Qual a capital da França?
```

↓

O `main.py` recebe essa pergunta.

↓

Ele chama:

```python
provider.ask(question) #em openai_provider.py
```

↓

O `OpenAIProvider` pega essa pergunta.

↓

Ele utiliza o SDK da OpenAI.

↓

O SDK envia uma requisição HTTP para a API da OpenAI.

↓

O servidor da OpenAI processa a pergunta.

↓

O modelo gera:

```text
A capital da França é Paris.
```

↓

A resposta retorna pelo SDK.

↓

O método `ask()` devolve apenas o texto.

↓

O `main.py` imprime:

```text
Resposta:

A capital da França é Paris.
```

---

# Auditoria Prometheus

Como auditor do curso, encontrei um pequeno detalhe que eu melhoraria na implementação.

Na aula, usamos um arquivo chamado `config.py`, mas sua responsabilidade ainda é apenas carregar uma variável de ambiente. Isso funciona e é perfeitamente aceitável para um projeto pequeno.

No entanto, eu acrescentaria uma pequena validação para evitar um erro comum de iniciantes:

```python
from dotenv import load_dotenv
import os

load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

if OPENAI_API_KEY is None:
    raise ValueError(
        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"
    )
```

Esse pequeno trecho melhora bastante a experiência do aluno. Sem ele, caso a chave esteja ausente ou escrita incorretamente, o erro só aparecerá quando a aplicação tentar acessar a API, tornando o diagnóstico mais difícil. Com essa validação, o problema é identificado imediatamente e com uma mensagem clara, reforçando um princípio importante de engenharia de software: **falhar cedo e com mensagens compreensíveis**.