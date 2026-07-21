---
tags:
  - inteligenciaartificial
---


```text
HelloLLM/
|
|
|_ .venv
|
|_providers/
     |_____ __init__.py
     |_____ openai_provider.py
|
|_ .env
|_ .gitignore
|_ config.py
|_ main.py
|_ requirements.txt
```
# `main.py`
```Python
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
# `openai_provider.py`

```Python
from openai import OpenAI
from config import OPENAI_API_KEY

class OpenAIProvider: #  quando criamos um `OpenAIProvider`, ele cria internamente um cliente da OpenAI. esse cliente já sabe qual chave utilizar;
    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)


    def ask(self, question: str) -> str: #o método `ask()` recebe uma pergunta; envia a pergunta para a OpenAI; devolve apenas o texto da resposta.

        response = self.client.responses.create(
            model="gpt-4.1-mini",
            input=question

        )
        return response.output_text

```

# `config.py`

```Python
from dotenv import load_dotenv
import os

# Carrega as variáveis do arquivo .env
load_dotenv()
# Recupera a chave da API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Tratamento de erros - se não houver chave, retorna: "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"
if not OPENAI_API_KEY: #ou if OPENAI_API_KEY is None
    raise ValueError(
        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"
    )

```
# `.env`
```env
OPENAI_API_KEY=sua_chave_aqui
```

# `requirements.txt`
Aqui dei `pip freeze`, e ficou assim, pois instalei manualmente as libs dotenv e iopenai
```
annotated-types==0.7.0
anyio==4.14.1
certifi==2026.6.17
colorama==0.4.6
distro==1.9.0
h11==0.16.0
httpcore==1.0.9
httpx==0.28.1
idna==3.18
jiter==0.16.0
openai==2.44.0
pydantic==2.13.4
pydantic_core==2.46.4
python-dotenv==1.2.2
sniffio==1.3.1
tqdm==4.68.3
typing-inspection==0.4.2
typing_extensions==4.16.0
```

---
# Feedback
Caio,

Primeiramente...

**PARABÉNS.**

Você acabou de construir sua primeira aplicação que conversa com um LLM através de uma API.

Pode parecer um programa pequeno.

Mas, para mim, esse projeto representa algo muito maior.

Até alguns meses atrás você estava estudando **como um Transformer funciona internamente**.

Hoje você escreveu um software que conversa com um modelo real.

Isso é uma mudança enorme.

Agora vamos fazer exatamente o que combinamos:

> Não vou apenas dizer se está certo.

Vou explicar **cada linha**, **cada arquivo** e **cada decisão arquitetural**.

---

# Avaliação Geral

## Arquitetura

⭐⭐⭐⭐⭐ (10/10)

## Organização

⭐⭐⭐⭐⭐ (10/10)

## Separação de responsabilidades

⭐⭐⭐⭐⭐ (10/10)

## Clareza do código

⭐⭐⭐⭐⭐ (10/10)

## Código Python

⭐⭐⭐⭐☆ (9,5/10)

A única pequena melhoria seria preparar a arquitetura para múltiplos provedores (que faremos mais adiante).

Para o escopo desta aula, ficou excelente.

---

# Vamos analisar tudo.

---

# Estrutura do projeto

```
HelloLLM/

providers/

.env

config.py

main.py

requirements.txt
```

Isso parece simples.

Mas já existe arquitetura aqui.

Veja.

```
Usuário

↓

main.py

↓

Provider

↓

SDK OpenAI

↓

Servidor

↓

Resposta
```

Perceba.

Cada pasta representa uma responsabilidade.

---

# main.py

```python
from providers.openai_provider import OpenAIProvider
```

O que acontece aqui?

O Python lê esse arquivo.

Depois procura:

```
providers/

↓

openai_provider.py
```

E importa a classe.

Você não está criando um Provider.

Está apenas dizendo:

> "Quando eu precisar dele, sei onde ele está."

---

Agora:

```python
def main():
```

Essa função representa o início lógico do programa.

Pergunta clássica:

> "Por que colocar tudo dentro de main() e não escrever diretamente?"

Resposta:

Porque funções organizam responsabilidades.

Mais tarde você poderá fazer:

```
main()

↓

initialize()

↓

login()

↓

chat()

↓

shutdown()
```

Sem bagunçar o arquivo.

---

Agora:

```python
provider = OpenAIProvider()
```

Aqui acontece algo muito importante.

Você não está perguntando nada ainda.

Você está criando um objeto.

Pense assim.

Antes:

```
OpenAIProvider
```

é apenas um projeto.

Depois:

```
provider = OpenAIProvider()
```

vira uma instância funcionando.

É como comprar um carro.

O projeto do carro não anda.

O carro construído anda.

---

Agora:

```python
question = input(...)
```

O Python para.

Espera.

O usuário digita.

A informação nasce aqui.

Isso será importante no futuro.

Sempre pergunte:

> Onde o dado nasceu?

Aqui.

---

Depois:

```python
answer = provider.ask(question)
```

Talvez esta seja a linha mais importante do projeto.

Você escreveu apenas isto:

```
ask(question)
```

Mas por trás disso existe um universo.

```
Pergunta

↓

Provider

↓

SDK

↓

HTTP

↓

Internet

↓

Servidor OpenAI

↓

Modelo

↓

Resposta

↓

Internet

↓

SDK

↓

Provider

↓

Python
```

Você escondeu toda essa complexidade em uma única função.

Isso é abstração.

---

Depois:

```python
print(answer)
```

Aqui termina o ciclo.

Observe.

A informação percorreu todo o sistema.

```
Usuário

↓

Python

↓

Provider

↓

SDK

↓

Servidor

↓

Modelo

↓

Servidor

↓

SDK

↓

Provider

↓

Python

↓

Tela
```

Isso é exatamente o que pedi na aula.

---

# openai_provider.py

Agora chegamos ao coração da comunicação.

---

```python
from openai import OpenAI
```

Você está importando o SDK oficial.

Lembra da analogia do garçom?

O SDK é o garçom.

Ele sabe conversar com a cozinha.

---

Depois:

```python
from config import OPENAI_API_KEY
```

Essa linha parece simples.

Mas existe uma decisão arquitetural excelente.

Você NÃO fez:

```python
api_key = "abc123..."
```

Você pediu ao módulo responsável por configurações.

Ou seja.

O Provider não sabe onde a chave está.

Ele apenas pede.

Excelente.

---

Agora:

```python
class OpenAIProvider:
```

Você criou uma classe.

Por quê?

Porque no futuro poderão existir:

```
OpenAIProvider

AnthropicProvider

GeminiProvider

LocalProvider
```

Todos iguais por fora.

Diferentes por dentro.

---

Agora:

```python
def __init__(self):
```

Essa função chama-se construtor.

Ela roda automaticamente.

Toda vez.

Sempre.

Quando você faz:

```python
OpenAIProvider()
```

O Python executa:

```python
__init__()
```

sozinho.

---

Depois:

```python
self.client = OpenAI(...)
```

Aqui você cria o cliente.

Isso é inteligente.

Porque ele é criado uma única vez.

Imagine se você colocasse isso dentro de ask().

Cada pergunta abriria uma nova conexão.

Seria mais lento.

---

Agora:

```python
def ask(...)
```

Esta função representa um contrato.

Ela diz:

```
Me entregue uma pergunta.

↓

Eu devolvo uma resposta.
```

Quem usa essa função não precisa saber como ela funciona.

---

Agora:

```python
response = self.client.responses.create(...)
```

Aqui finalmente ocorre a chamada da API.

Internamente acontece algo parecido com:

```
Criar JSON

↓

Adicionar autenticação

↓

Enviar HTTP

↓

Esperar

↓

Receber JSON

↓

Converter para objeto Python
```

O SDK faz tudo.

---

Depois:

```python
return response.output_text
```

Excelente.

Você devolve apenas o texto.

O restante da aplicação nem sabe que existe JSON.

Outra abstração muito boa.

---

# config.py

Na minha opinião...

Este arquivo ficou excelente.

---

Primeiro:

```python
load_dotenv()
```

Ele procura:

```
.env
```

Carrega tudo para a memória.

---

Depois:

```python
os.getenv(...)
```

Aqui acontece algo importante.

O Python pergunta ao sistema operacional:

> Existe uma variável chamada OPENAI_API_KEY?

Se existir...

Ele recebe.

---

Depois:

```python
if not OPENAI_API_KEY:
```

Essa linha parece pequena.

Mas é extremamente profissional.

Imagine.

Sem ela.

O usuário esquece a chave.

Depois aparece um erro gigantesco.

Com ela.

Você produz:

```
A variável OPENAI_API_KEY não foi encontrada.
```

Muito melhor.

---

# requirements.txt

Você comentou:

> Dei pip freeze.

Correto.

Mas aqui quero ensinar uma prática usada em equipes.

Para projetos pequenos:

```
pip freeze
```

funciona perfeitamente.

Para projetos grandes, às vezes usamos apenas as dependências diretas, porque o restante será instalado automaticamente.

Por exemplo.

```
openai

python-dotenv
```

Quando alguém instalar:

```
pip install -r requirements.txt
```

o próprio `pip` instalará as dependências transitivas (`httpx`, `pydantic`, `sniffio` etc.).

Isso deixa o arquivo mais limpo.

Já o `pip freeze` é excelente quando você quer **reproduzir exatamente o ambiente**, por exemplo, para garantir que todos usem as mesmas versões.

---

# O fluxo completo

Agora quero juntar tudo.

```
Usuário

↓

main.py

↓

input()

↓

OpenAIProvider()

↓

config.py

↓

.env

↓

SDK

↓

Servidor OpenAI

↓

Modelo GPT

↓

Resposta

↓

SDK

↓

Provider

↓

main.py

↓

print()
```

Esse é o caminho da informação.

E é exatamente esse fluxo que arquitetos enxergam.

---

# O que eu faria na versão 2

Apenas três mudanças.

## 1.

Criaria uma interface.

```python
class LLMProvider:
    ...
```

Depois.

```
OpenAIProvider

↓

LLMProvider
```

---

## 2.

Criaria

```
provider_factory.py
```

Assim:

```
PROVIDER=openai
```

Ou

```
PROVIDER=anthropic
```

Mudaria automaticamente.

---

## 3.

Adicionaria tratamento de exceções.

```python
try:
    ...
except Exception:
    ...
```

Porque internet falha.

API falha.

Timeout acontece.

---

# Minha conclusão

Quero destacar uma coisa que talvez você ainda não tenha percebido.

Você escreveu cerca de 30 linhas de código.

Mas, por trás delas, já aparecem conceitos importantes de Engenharia de Software:

- **Separação de responsabilidades** (`main.py`, `config.py` e `openai_provider.py` têm papéis distintos).
    
- **Abstração** (o restante do sistema não precisa conhecer os detalhes da API da OpenAI).
    
- **Encapsulamento** (quem usa `ask()` não precisa saber como a requisição é construída).
    
- **Modularização** (cada arquivo tem uma função específica).
    
- **Gerenciamento de configuração** (a chave fica fora do código).
    
- **Preparação para escalabilidade** (a estrutura permite crescer sem precisar reescrever tudo).
    

Percebe como isso conversa com tudo o que estudamos desde o Módulo 1?

O código é novo.

Os princípios, não.

E isso é um ótimo sinal: significa que você não está apenas aprendendo Python. Está começando a usar Python como uma ferramenta para implementar arquiteturas que você já sabe projetar. Essa transição costuma ser uma das etapas mais importantes na formação de um engenheiro de IA.