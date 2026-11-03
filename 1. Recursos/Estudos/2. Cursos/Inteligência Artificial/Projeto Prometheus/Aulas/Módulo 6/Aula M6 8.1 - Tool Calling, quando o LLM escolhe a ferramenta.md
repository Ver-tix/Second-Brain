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
Perfeito, Caio. Vamos avançar. 🚀

Chegamos a uma das aulas mais importantes do Módulo 6.

Até agora, construímos um agente que:

- possui memória;
    
- possui contexto;
    
- possui ferramentas;
    
- possui uma camada de decisão.
    

Mas existe uma limitação.

Observe:

```python
if "quanto é" in question:
    return "calculator"
```

Nós ainda estamos **programando a inteligência**.

Ou seja:

```text
Usuário
   ↓
Regra fixa
   ↓
Ferramenta
```

Isso ainda é um sistema tradicional com um pouco de IA.

A próxima evolução é:

```text
Usuário
   ↓
LLM
   ↓
Decisão
   ↓
Ferramenta
   ↓
Resultado
   ↓
LLM
   ↓
Resposta
```

Agora o modelo participa da decisão.

---
# O problema que estamos resolvendo

Imagine nosso Prometheus-Mentor.

Usuário:

> "Quanto é 35 vezes 42?"

O agente precisa perceber:
- isso não é uma pergunta de conhecimento;
- não precisa consultar memória;
- não precisa responder com raciocínio próprio;
- precisa calcular.

A pergunta é:

**Quem decide isso?**

Na arquitetura anterior:

```text
MentorAgent
```

Agora:

```text
LLM
```

---

# A grande mudança de paradigma

Antes:

```python
if question.startswith("calc"):
    calculator()
```

A aplicação dizia:

> "Se acontecer isso, faça aquilo."

---

Agora:

O aplicativo diz ao modelo:

> "Estas são as ferramentas disponíveis. Escolha se alguma delas ajuda."

O modelo responde:

> "Eu quero chamar a calculadora."

---

# O LLM vira um planejador

Essa é uma mudança conceitual gigantesca.

Antes:

```text
LLM = gerador de texto
```

Agora:

```text
LLM = planejador + gerador de texto
```

Ele passa a responder duas perguntas:

## 1. Preciso de uma ferramenta?

Exemplo:

> "Qual a capital da França?"

Resposta:

```
Não.
```

---

## 2. Qual ferramenta?

Exemplo:

> "Quanto é 200 / 8?"

Resposta:

```
calculator
{
 operation: divide,
 a: 200,
 b: 8
}
```

---

# Como isso funciona internamente?

A OpenAI (e outros modelos) recebem uma lista de ferramentas.

Algo conceitualmente assim:

```json
{
  "name": "calculator",
  "description": "Calcula operações matemáticas",
  "parameters": {
      "a": "number",
      "b": "number",
      "operation": "string"
  }
}
```

O modelo lê isso junto com a pergunta.

---

# O fluxo completo

Agora temos:

```
Usuário
   |
   v
MentorAgent
   |
   v
LLM
   |
   |
   | "Preciso da calculadora"
   |
   v
ToolManager
   |
   v
CalculatorTool
   |
   v
Resultado
   |
   v
LLM
   |
   v
Resposta final
```

---

# Perceba algo importante

A ferramenta não mudou.

Nossa:

```python
CalculatorTool
```

continua igual.

O:

```python
ToolManager
```

continua igual.

A mudança está somente na camada de decisão.

Isso é arquitetura boa.

---

# Por que isso é revolucionário?

Porque agora podemos adicionar:

```text
Ferramentas:

- Calculadora
- Pesquisa Web
- Banco Vetorial
- Agenda
- Email
- GitHub
- Banco SQL
- Sistema financeiro
```

Sem criar:

```python
if pergunta_tem_palavra_X:
```

---

# O nascimento do agente moderno

Vamos comparar:

## Workflow

```text
Passo 1
Faça busca

Passo 2
Resuma

Passo 3
Escreva
```

Tudo definido antes.

---

## Agente

```text
Objetivo:

"Produza uma newsletter"

↓

LLM decide:

"Preciso pesquisar"

↓

Busca

↓

"Preciso consultar memória"

↓

RAG

↓

"Preciso escrever"

↓

Editor
```

---

# Ligação com o Prometheus OS

Agora começa a ficar muito próximo da arquitetura que desenhamos.

Imagine:

Usuário:

> "Prepare minha newsletter semanal de IA e Marketing."

O Orquestrador poderia disponibilizar:

```text
Ferramentas:

search_news()
retrieve_second_brain()
generate_image()
save_markdown()
publish_newsletter()
```

E o agente decidir:

```text
1. Buscar notícias
2. Consultar Second Brain
3. Criar estrutura
4. Gerar texto
5. Pedir aprovação
```

Isso é exatamente o Prometheus-Editor.

---

# Laboratório da Aula 8

## [[🤖 Monitoria M6 008.1 - TEORIA]] 
## [[🤖 Monitoria M6 008.1 - PRÁTICA]]
## [[🛠 Desafio M6 008.1]] 

Vamos evoluir nosso projeto.

## Objetivo

Remover:

```python
ToolDecision
```

e deixar o LLM decidir.

---

Nova arquitetura:

```
MentorAgent

↓

LLMService

↓

ToolManager

↓

Tools
```

O fluxo será:

1. Usuário envia pergunta.
    
2. Mentor envia pergunta + ferramentas disponíveis ao LLM.
    
3. LLM decide se quer uma ferramenta.
    
4. Python executa.
    
5. Resultado volta ao LLM.
    
6. LLM responde.
    

---

# Primeira implementação

Vamos criar uma nova pasta:

```
app/tools/
```

Agora cada ferramenta terá uma descrição.

Exemplo:

```python
calculator_tool.py
```

ganhará algo como:

```python
description = """
Executa operações matemáticas básicas.
"""
```

Porque agora o modelo precisa saber:

> "Para que serve essa ferramenta?"

---

# Desafio da aula

Antes de codificar, responda:

> **Qual é a principal diferença arquitetural entre o ToolDecision que criamos na aula anterior e o Tool Calling?**

Dica:

Não responda:

"um usa código e outro usa IA".

A resposta esperada é mais profunda.

Pense:

**Onde está localizada a decisão?**

---

Caio, essa aula é um divisor de águas.

Depois dela, o Prometheus-Mentor deixa de ser apenas um programa que contém um LLM e começa a se comportar como um **sistema que utiliza um LLM como componente de raciocínio**.

É exatamente aqui que começamos a cruzar a fronteira entre "aplicação com IA" e "agente de IA". 🚀