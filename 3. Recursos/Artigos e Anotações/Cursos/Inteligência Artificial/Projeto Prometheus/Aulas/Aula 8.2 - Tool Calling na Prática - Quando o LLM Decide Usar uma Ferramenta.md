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
Até agora construímos esta evolução:

```
Aula 6
Usuário
    ↓
MentorAgent
    ↓
LLM
```

↓

```
Aula 7
Usuário
    ↓
ToolDecision
    ↓
Calculator
```

↓

```
Aula 8.1
LLM conhece que existe uma ferramenta chamada Calculator
```

Mas existe um problema enorme.

---

# O problema

Até agora nosso código faz isso:

```python
tool = ToolDecision.decide(question)

if tool == "calculator":
    ...
```

Ou seja:

> **o Python decide qual ferramenta usar.**

Mas essa **não é a filosofia do Tool Calling**.

Queremos isto:

```
Usuário
    ↓
LLM
    ↓
"Preciso usar Calculator."
    ↓
Python
    ↓
Calculator
    ↓
Resultado
    ↓
LLM
    ↓
Resposta
```

Perceba a mudança.

Antes:

```
Python pensava.
```

Agora:

```
O LLM pensa.
```

---

# Quem é o cérebro?

Essa é a pergunta arquitetural da aula.

Até agora:

```
Usuário

↓

MentorAgent

↓

ToolDecision

↓

Calculator
```

O cérebro estava aqui:

```
ToolDecision
```

Depois desta aula:

```
Usuário

↓

MentorAgent

↓

LLM
      │
      ├── Responder
      │
      └── Pedir ferramenta
```

O cérebro mudou de lugar.

---

# O novo fluxo

Quando enviamos isto:

> Quanto é 15 + 27?

O LLM pode responder algo equivalente a:

```
Não vou responder ainda.

Antes preciso chamar:

calculator()
```

Ele ainda não sabe o resultado.

Ele apenas sabe:

> "Existe uma ferramenta melhor para isso."

---

# O papel do Python muda

Antes:

```
Python analisava.

Python decidia.

Python executava.
```

Agora:

```
Python recebe uma decisão.

Executa.

Devolve o resultado.
```

O Python deixou de ser o "pensador".

Virou o executor.

---

# Uma analogia

Imagine um hospital.

Antes:

```
Recepcionista

↓

Decide sozinho:

"Você precisa do cardiologista."
```

Depois:

```
Recepcionista

↓

Médico

↓

"Leve este paciente ao cardiologista."

↓

Recepcionista executa.
```

A recepcionista continua trabalhando.

Mas quem decide é o médico.

---

# O MentorAgent fica mais simples

Repare que toda esta lógica:

```python
if "quanto é" in question:

...

elif "multiplique":

...

elif ...
```

desaparece.

O MentorAgent passa a fazer algo muito mais elegante:

```
Receber pergunta

↓

Enviar ao LLM

↓

Se veio Tool Call

↓

Executar ferramenta

↓

Mandar resultado de volta

↓

Receber resposta final

↓

Mostrar ao usuário
```

---

# Uma mudança importante

Na Aula 7 você escreveu dezenas de linhas para reconhecer frases como:

```
quanto é

some

multiplique

divida
```

Depois desta aula...

Tudo isso desaparece.

Porque agora o próprio modelo entende linguagem natural.

Por exemplo:

```
Você poderia calcular
quanto custa
a soma de
cento e vinte
com quarenta?
```

Ou

```
Quanto dá quinze mais vinte e oito?
```

Ou

```
Me ajuda fazendo essa conta?
```

Não precisamos ensinar cada frase.

O modelo já sabe interpretar.

---

# O verdadeiro ganho

Muitos alunos acham que Tool Calling serve para acessar APIs.

Na verdade, o maior ganho é outro:

> **Parar de escrever regras em Python para reconhecer intenções humanas.**

Quem entende linguagem natural é o LLM.

O Python deve apenas executar.

Essa é uma das ideias mais importantes de toda a arquitetura moderna de agentes.

---

# O laboratório de hoje

Hoje vamos fazer uma mudança pequena, mas histórica.

## Objetivo

Remover completamente o uso de:

```python
ToolDecision
```

e deixar que **o LLM seja o responsável por decidir quando chamar a calculadora**.

⚠️ Ainda **não** vamos trabalhar com `parameters`, `properties` e `required`. Isso fica para a Aula 8.3. Nesta aula, queremos apenas consolidar a mudança arquitetural: **a decisão sai do Python e passa para o modelo**.

---

# Desafio Prático — Laboratório 8.2

## [[🤖 Monitoria M6 008.2 - TEORIA]] 

## [[🤖 Monitoria M6 008.2 - PRÁTICA]] 
## [[🛠 Desafio M6 008.2]] 

Sem fornecer o código completo, implemente os seguintes passos:

### Parte 1

Remova a dependência de `ToolDecision` do `MentorAgent`.

### Parte 2

Faça com que o `LLMService` detecte quando a resposta da API contém um **tool call** em vez de uma resposta textual.

### Parte 3

Quando houver um tool call:

- identifique qual ferramenta foi solicitada;
    
- execute a `CalculatorTool` usando o `ToolManager`;
    
- capture o resultado.
    

> Nesta aula, você pode assumir um cenário simplificado para a execução. O foco não é o schema dos argumentos, mas entender o novo fluxo de controle.

### Parte 4

Desenhe (em texto ou ASCII) o novo fluxo da aplicação mostrando quem toma a decisão e quem apenas executa.

---

### Objetivo da aula

Ao final deste laboratório, você deverá perceber que ocorreu uma mudança profunda:

- Antes: **Python decidia e o LLM respondia.**
    
- Agora: **O LLM decide e o Python executa.**
    

Esse é o primeiro passo para construir agentes capazes de usar dezenas de ferramentas sem depender de centenas de `if/elif` espalhados pelo código.