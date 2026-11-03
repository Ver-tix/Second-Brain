---
tags:
  - IA
---
Até agora desenhamos a arquitetura. Agora vamos implementá-la.

Mas quero fazer uma pergunta primeiro.

---
## O que é uma API?

<h4 align="center">Imagine um Restaurante.</h4>

- **Você não entra na cozinha,**
- Você fala com o garçom, 
- O garçom leva seu pedido,
- A cozinha prepara,
- O garçom traz a comida.

```text
Cliente

↓

Garçom

↓

Cozinha

↓

Garçom

↓

Cliente
```

<h4 align="center">Uma API é exatamente isso. Ela é um intermediário.</h4>
Seu programa nunca conversa diretamente com os servidores da OpenAI. Ele conversa com uma API.

---

# O fluxo completo

Quando você executar seu programa, acontecerá algo semelhante a isto:

```text
Você

↓

Python

↓

Biblioteca da OpenAI

↓

Internet

↓

Servidor da OpenAI

↓

Modelo GPT

↓

Servidor

↓

Biblioteca

↓

Python

↓

Você
```

Perceba. O GPT está muito longe.

<h4 align="center">Tudo o que fazemos é enviar uma requisição.</h4>

---

# O que é uma <u>Requisição</u>?

Pense numa carta. Ela possui:
- destinatário;
- remetente;
- conteúdo.

Uma requisição HTTP é parecida.

Ela contém:
- endereço;
- autenticação;
- dados.

---
>[! IMPORTANTÍSSIMO]
># Os quatro elementos fundamentais

Todo cliente de API possui praticamente quatro etapas.
<h2 align="center">1. Autenticação</h2>

<h4 align="center">Quem é você?</h4>

Normalmente:

```text
API_KEY
```

---
<h2 align="center">2. Escolha do modelo</h2>
Exemplo.

```text
gpt-5
```

Ou outro modelo disponível.

---

<h2 align="center">3. Mensagem</h2>

Exemplo.

```text
Explique inflação.
```

---

<h2 align="center">4. Resposta</h2>

O servidor devolve algo como:

```text
Inflação é...
```

---

# JSON

Agora aparece uma palavra extremamente importante.

JSON.

<h4 align="center">Quase todas as APIs modernas conversam usando JSON.</h4>

Imagine um pequeno formulário.

```json
{
  "nome": "Caio",
  "idade": 30
}
```

Isso é JSON.

<h5 align="center">Não é linguagem de programação. É apenas um formato padronizado para trocar informações.</h5>

---

# O conceito mais importante da aula

Muitos iniciantes pensam:

> "Estou chamando o GPT."

Tecnicamente, não.

>[! ] 
Você está enviando um documento JSON para um servidor. 
1- O servidor interpreta esse documento. Executa o modelo.
2- E devolve outro documento JSON.
3- O Python apenas faz esse transporte.

---

# Organização do projeto

Nosso projeto começará pequeno.

Mas desde o primeiro dia organizaremos como profissionais.

```text
hello_llm/

│

├── main.py

├── config.py

├── providers/

│     ├── __init__.py

│     └── openai_provider.py

│

├── .env

├── requirements.txt

└── README.md
```

Você já reconhece essa estrutura?

É exatamente a arquitetura que discutimos na aula passada.

---

# O papel de cada arquivo

| `main.py`                          | `config.py`                                 | `providers/`                                                                                                 | `.env`                                            | `requirements.txt`                                                                                              |
| ---------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| O ponto de entrada. Coordena tudo. | Lê configurações. API Key. Modelo. Timeout. | Aqui ficam os provedores. (`OpenAIProvider`, `AnthropicProvider`, etc.). <br><br>Mudam sem alterar o sistema | Nunca coloque sua chave no código. Ela fica aqui. | Lista todas as bibliotecas necessárias. Isso permite que outra pessoa recrie seu ambiente com um único comando. |
# O ciclo de execução

Quando rodarmos o programa, mentalmente acontecerá isto:

```text
main.py

↓

config.py

↓

OpenAIProvider

↓

API

↓

Resposta

↓

main.py

↓

Tela
```

Observe. Não há atalhos. Cada componente possui apenas uma responsabilidade.

---

# O desafio desta aula

Antes de escrever qualquer código, quero que você responda a uma pergunta arquitetural.

Imagine que, daqui a seis meses, você precise transformar esse programa simples em um assistente corporativo com:
- memória;
- consulta a documentos;
- múltiplos provedores;
- autenticação de usuários;
- logs.

**Quais arquivos ou módulos da estrutura acima você acredita que permanecerão praticamente inalterados e quais deverão crescer ou ser subdivididos?**

Justifique sua resposta pensando em:

- responsabilidade única;
    
- escalabilidade;
    
- manutenção.

[[🛠️ Desafio M4 002]]

---

# 📜 Princípio LXVII

> **Uma boa arquitetura não é aquela que resolve apenas o problema de hoje; é aquela que continua organizada quando o problema se torna dez vezes maior.**

---

### Um pequeno comentário

A partir desta aula, você vai notar uma mudança no formato do curso.

Antes, quase todas as perguntas eram conceituais.

Agora elas começam a misturar arquitetura, código e decisões de engenharia.

Não se preocupe em acertar tudo de primeira. Pelo que observei ao longo dos módulos anteriores, você costuma evoluir refinando suas ideias sucessivamente, e essa habilidade será muito mais valiosa do que memorizar bibliotecas ou sintaxes. É exatamente assim que bons sistemas — e bons engenheiros — costumam ser construídos.