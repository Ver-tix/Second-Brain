---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
BORA!! 😄

E antes de começarmos... quero te dizer uma coisa.

**Na minha opinião, esta é a primeira aula em que o Prometheus começa a ganhar uma "alma".**

Até agora ele era apenas um encanamento:

```text
Usuário
   ↓
MentorAgent
   ↓
LLMService
   ↓
OpenAI
```

A partir de hoje ele começa a ter **identidade**.

Isso é muito mais importante do que parece.

---

# Antes de escrever código...

Como sempre fazemos, quero que você entenda **o problema**.

Vou te fazer uma pergunta.

Hoje, se você perguntar:

```text
O que é um Transformer?
```

o `LLMService` envia exatamente isto:

```text
O que é um Transformer?
```

para a OpenAI.

Correto?

---

Mas imagine outro cenário.

Você pergunta:

```text
O que é um Transformer?
```

Só que o PromptBuilder envia isto:

```text
Você é o Prometheus-Mentor.

Sua missão é ensinar Inteligência Artificial
de forma didática.

Sempre explique primeiro a intuição.

Depois apresente a definição.

Depois um exemplo.

Depois conecte com conceitos estudados anteriormente.

Pergunta do usuário:

O que é um Transformer?
```

Percebe a diferença?

A **mesma pergunta** gera uma resposta completamente diferente.

---

# Então... por que criar um PromptBuilder?

Vou usar uma analogia.

Imagine uma empresa.

Você possui um funcionário chamado João.

Todo cliente que chega faz uma pergunta.

Seria estranho o João inventar uma forma diferente de atender cada cliente.

Em vez disso, a empresa entrega um roteiro:

> "Cumprimente o cliente."

> "Apresente-se."

> "Pergunte o problema."

> "Ofereça uma solução."

Esse roteiro é o PromptBuilder.

O funcionário continua sendo o mesmo.

Mas agora existe um padrão.

---

# O que mais gosto nesta aula

Observe como ela mantém a arquitetura limpa.

Antes:

```text
MentorAgent

↓

LLMService
```

Agora:

```text
MentorAgent

↓

PromptBuilder

↓

LLMService
```

Repare que **o LLMService continua sem saber que existe um PromptBuilder**.

E o PromptBuilder **não sabe que existe OpenAI**.

Cada classe continua tendo uma única responsabilidade.

Isso é arquitetura elegante.

---

# Como faremos?

Assim como fizemos na aula passada.

Não vamos simplesmente escrever o código.

Vamos construir cada etapa entendendo o motivo.

## Etapa 1

Criar:

```text
app/prompts/
    mentor_prompt.py
```

✅ Você faz.

---

## Etapa 2

Vamos discutir juntos a classe `PromptBuilder`.

Porque aqui existe uma oportunidade de já começar a pensar como arquiteto.

O professor deu uma dica muito interessante:

> "O método pode ser estático (`@staticmethod`)."

Quero que você descubra **por que** ele sugeriu isso.

Então vou te deixar uma pergunta:

> **O `PromptBuilder` precisa guardar alguma informação dentro dele (como um cliente OpenAI ou uma memória), ou ele apenas recebe uma pergunta e devolve um texto pronto?**

Responda essa pergunta primeiro.

Se você responder corretamente, vai entender naturalmente por que `@staticmethod` é a melhor escolha aqui, em vez de apenas decorar a sintaxe.

---

eu: `O prompt builder deveria ser a constituição de quem e como aquele agente é e deveria agir, por isso usar o @staticmethod?`

Você chegou **muito perto**. Na verdade, sua resposta mistura **o papel do PromptBuilder** com **o motivo do `@staticmethod`**. Vamos separar as duas ideias.

### ✔ O papel do PromptBuilder

Você acertou:

> O PromptBuilder é a "constituição" do agente.

Excelente analogia. Eu até refinaria um pouco:

> **O PromptBuilder é o responsável por transformar a identidade do Prometheus-Mentor em um prompt que o LLM consiga entender.**

Ele define:

- quem o agente é;
    
- como deve ensinar;
    
- qual tom usar;
    
- como estruturar as respostas.
    

Ou seja, ele é realmente a "constituição" do agente.

---

### ✔ Mas por que `@staticmethod`?

Aqui está a diferença.

Pergunte a si mesmo:

Quando chamamos o PromptBuilder, ele precisa lembrar de alguma coisa?

Por exemplo:

- Ele guarda uma API? ❌
    
- Ele guarda uma memória? ❌
    
- Ele guarda um histórico? ❌
    
- Ele guarda configurações internas? ❌
    

Não.

Ele simplesmente faz isto:

```
Pergunta
      ↓
PromptBuilder
      ↓
Prompt completo
```

É como uma calculadora.

Você faz:

```python
2 + 2
```

e ela responde:

```python
4
```

Ela não precisa "lembrar" de nada entre uma chamada e outra.

Por isso, não faz sentido criar um objeto:

```python
builder = PromptBuilder()
```

para depois chamar:

```python
builder.build(question)
```

Podemos simplesmente fazer:

```python
PromptBuilder.build(question)
```

Porque o método depende **apenas da entrada (`question`)** e produz uma saída. Ele não depende de nenhum estado armazenado dentro do objeto.

---

## Um insight de arquitetura

Percebe como o Projeto Prometheus está começando a te ensinar a fazer uma pergunta antes de escrever código?

Antes você talvez perguntasse:

> "Como usa `@staticmethod`?"

Agora você está perguntando:

> **"Essa classe realmente precisa ter estado?"**

Essa segunda pergunta é a que um arquiteto faz.

---

🎓 **Desafio rápido:** sem olhar na internet, me responda em uma frase:

> Qual é a diferença entre um método normal (`def`) e um método estático (`@staticmethod`)?

Se você responder isso, tenho certeza de que o conceito ficou sólido.

eu: `não exige instância nem recebe self implicitamente. Não entendo como isso me beneficiará. Por favor, responda de forma bem breve para não nos bagunçarmos aqui`

Perfeito. A resposta curta é:

**O benefício é simplicidade e clareza de intenção.**

Como o método **não usa `self` nem atributos da classe**, não faz sentido obrigar alguém a criar um objeto só para chamá-lo.

Em vez de:

```
builder = PromptBuilder()
prompt = builder.build(question)
```

você faz:

```
prompt = PromptBuilder.build(question)
```

Isso deixa claro que o método é apenas uma **função utilitária** que transforma uma pergunta em um prompt, sem depender de estado interno. É exatamente o caso do `PromptBuilder`.