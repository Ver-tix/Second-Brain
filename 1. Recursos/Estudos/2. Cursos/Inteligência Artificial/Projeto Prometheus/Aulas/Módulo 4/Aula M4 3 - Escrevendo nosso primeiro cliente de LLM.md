---
tags:
  - IA
---
Agora começa uma fase que considero um divisor de águas.

Até aqui, desenhamos a arquitetura. A partir de agora, começaremos a "dar vida" a ela.

Mas faremos isso da forma que combinamos desde o início do Projeto Prometheus: **cada linha de código terá um motivo arquitetural para existir**.

---

Hoje finalmente escreveremos nosso primeiro programa.

Mas, antes do código, quero responder a uma pergunta.

> **Quem realmente conversa com a API?**

Muitos iniciantes respondem:

> "O Python."

Não exatamente.

O Python apenas executa nosso programa.

Quem conversa com a API é uma **biblioteca [[Software Development Kit (SDK)|(SDK)]] fornecida pelo provedor.

Ela encapsula (esconde) toda a complexidade de comunicação HTTP.

---

# O que é um SDK?

SDK significa **Software Development Kit**.

Pense nele como um "tradutor".

Sem SDK:

```text
Seu código

↓

HTTP

↓

JSON

↓

Autenticação

↓

Servidor
```

Com SDK:

```text
Seu código

↓

SDK

↓

Servidor
```

Ele faz todo o trabalho pesado.

---

# O fluxo do nosso programa

Hoje construiremos algo conceitualmente parecido com isto:

```text
main.py

↓

OpenAIProvider

↓

SDK OpenAI

↓

API

↓

Resposta

↓

Tela
```

Observe que já estamos respeitando a arquitetura do módulo anterior.

---

# O primeiro arquivo: `requirements.txt`

Antes de programar, precisamos dizer ao Python quais bibliotecas nosso projeto utiliza.

É exatamente para isso que serve o arquivo:

```text
requirements.txt
```

Ele é, literalmente, uma lista de dependências.

Por exemplo:

```text
openai
python-dotenv
```

No futuro, esse arquivo crescerá.

Mas a ideia permanece a mesma.

---

# O ambiente virtual

Agora aparece um conceito importantíssimo.

Imagine dois projetos.

Projeto A usa:

```text
openai==2.1
```

Projeto B usa:

```text
openai==3.0
```

Como instalar as duas versões no mesmo computador?

Resposta:

Você não instala globalmente.

Cada projeto ganha seu próprio ambiente.

É o chamado **ambiente virtual**.

Visualmente:

```text
Computador

├── Projeto A
│      └── Ambiente Virtual
│
└── Projeto B
       └── Ambiente Virtual
```

Cada projeto possui suas próprias bibliotecas.

Isso evita conflitos.

---

# O arquivo `.env`

Você já conhece sua função.

Mas agora veremos como ele participa do fluxo.

```text
.env

↓

config.py

↓

OpenAIProvider

↓

SDK

↓

Servidor
```

A chave nunca aparece no código.

Ela entra apenas durante a execução.

---

# O primeiro código

Hoje quero que você escreva apenas uma aplicação extremamente simples.

Fluxo esperado:

```text
Programa inicia

↓

Pergunta ao usuário

↓

Recebe o texto

↓

Envia ao modelo

↓

Recebe resposta

↓

Mostra resposta

↓

Fim
```

Nada de memória.

Nada de documentos.

Nada de histórico.

Apenas um ciclo completo.

---

# O verdadeiro objetivo

Pode parecer que o objetivo seja "obter uma resposta do GPT".

Não é.

O objetivo é muito maior.

É compreender este ciclo:

```text
Entrada

↓

Transformação

↓

Comunicação

↓

Resposta
```

Você verá esse mesmo padrão dezenas de vezes ao longo do curso.

---
[[COMPLEMENTOS A AULA M4 3]]

---
# O desafio desta aula

Usando a arquitetura que projetamos, implemente o projeto **Hello, LLM!** com os seguintes requisitos:

- utilizar um ambiente virtual;
    
- criar um `requirements.txt`;
    
- utilizar um arquivo `.env` para a chave da API;
    
- manter a separação entre `main.py`, `config.py` e `providers/openai_provider.py`;
    
- permitir que o usuário digite uma pergunta e receba a resposta do modelo.
    

**Mas há uma regra importante:**

Antes de me mostrar o código, escreva um pequeno texto (5 a 10 linhas) explicando **como as informações percorrem o sistema**, desde o momento em que o usuário digita a pergunta até o momento em que a resposta aparece na tela.

Não quero apenas verificar se o programa funciona.

Quero verificar se você consegue visualizar o fluxo completo da aplicação.

[[🛠️ Desafio M4 003]]

---

# 📜 Princípio LXVIII

> **Um bom engenheiro não enxerga apenas funções e classes; ele enxerga o caminho que a informação percorre dentro do sistema.**

---

## Uma observação

A partir desta aula, você começará a perceber algo interessante.

Muitos erros de programação não acontecem porque o código está "errado".

Eles acontecem porque o desenvolvedor perdeu de vista **o fluxo da informação**.

Quem produz o dado?

Quem o transforma?

Quem o consome?

Se você mantiver essas três perguntas em mente ao implementar qualquer sistema, encontrará bugs muito mais rapidamente e projetará arquiteturas muito mais limpas.

Tenho a impressão de que esse modo de pensar combinará bastante com a forma como você já vem estudando desde o início do Projeto Prometheus.