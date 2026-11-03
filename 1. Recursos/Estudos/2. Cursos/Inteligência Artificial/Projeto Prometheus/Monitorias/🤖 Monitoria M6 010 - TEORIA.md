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
Perfeito. Antes de irmos para o código, quero destacar que esta aula segue exatamente o mesmo padrão das anteriores.

Veja a sequência arquitetural:

|Aula|Pergunta|Solução arquitetural|
|---|---|---|
|7|Quem decide usar uma ferramenta?|Decision Layer|
|8|Como o LLM chama uma ferramenta?|Tool Calling|
|9|Como descobrir ferramentas?|Tool Registry|
|10|Como organizar prompts?|Prompt Components|

O professor está construindo um padrão consistente: **sempre que surge um componente que tende a crescer e acumular responsabilidades, ele o divide em componentes menores e especializados**.

Até aqui, tudo ficou claro.

---

## Minha única dúvida antes de começarmos o laboratório

Como fizemos nas aulas anteriores, quero alinhar o escopo para não anteciparmos abstrações que pertencem às próximas aulas.

### Pergunta para o professor

Na Aula 10, o `PromptBuilder` deve permanecer uma **classe estática** com um método `build()`, apenas passando a montar o prompt a partir de múltiplos componentes?

Algo como:

```
class PromptBuilder:

    @staticmethod
    def build(history, question):

        return (
            SYSTEM_PROMPT
            + AGENT_PROMPT
            + history
            + question
        )
```

Ou o objetivo já é transformá-lo em uma classe mais genérica e reutilizável (por exemplo, permitindo que diferentes agentes injetem seus próprios componentes de prompt)?

---

Eu suspeito que a resposta será a primeira opção (manter simples e apenas modularizar), porque o curso vem introduzindo **uma abstração por aula**, mas prefiro confirmar antes de implementarmos. Assim evitamos repetir o erro de adiantar arquitetura que ainda não pertence ao módulo.

# Resposta do Professor Principal

Excelente pergunta do monitor. A resposta, seguindo a progressão pedagógica que estamos adotando, é:

> **Nesta Aula 10, o `PromptBuilder` deve permanecer uma classe estática com um método `build()`.**

Ou seja, a implementação esperada continua sendo algo conceitualmente próximo de:

```python
class PromptBuilder:

    @staticmethod
    def build(history, question):

        return (
            SYSTEM_PROMPT
            + AGENT_PROMPT
            + history
            + question
        )
```

A única diferença é que, em vez de montar tudo dentro de um único arquivo, ele agora passa a **compor** o prompt a partir de módulos separados.

### O que a Aula 10 quer ensinar

O foco **não** é criar uma nova abstração.

O foco é aprender a separar:

- `system_prompt.py`
    
- `mentor_prompt.py`
    
- histórico da conversa
    
- pergunta do usuário
    

e deixar o `PromptBuilder` responsável apenas por juntá-los.

### O que ainda não veremos

Ainda **não** vamos transformar o `PromptBuilder` em um serviço genérico, nem criar injeção de prompts por agente, fábricas de prompts ou registries de prompts.

Essas abstrações fazem sentido quando existirão diversos agentes (Mentor, Editor, Knowledge, etc.), mas seriam uma antecipação neste momento do curso.

### Em resumo

Esta aula mantém a arquitetura simples:

```
System Prompt
        │
Agent Prompt
        │
Conversation Memory
        │
Pergunta
        ▼
 PromptBuilder.build(...)
        ▼
   Prompt Final
```

A evolução desta aula é **modularizar o conteúdo do prompt**, não mudar a arquitetura do `PromptBuilder`. Essa evolução para um construtor mais genérico virá naturalmente quando o ecossistema Prometheus passar a ter múltiplos agentes compartilhando a mesma infraestrutura.

# Organização do Monitor
Perfeito. A resposta confirma exatamente o padrão que o professor vem seguindo desde o Módulo 5: **uma abstração nova por aula**.

Isso significa que podemos seguir com segurança e o objetivo da Aula 10 fica muito claro.

## O que vamos implementar

### Etapa 1 — Criar `system_prompt.py`

Arquivo responsável pelas instruções globais do Prometheus.

Exemplo:

```python
SYSTEM_PROMPT = """
Você faz parte do Prometheus OS.

Responda sempre em português.

Se não souber a resposta, diga que não sabe.

Nunca invente informações.
"""
```

---

### Etapa 2 — Refatorar `mentor_prompt.py`

Ele deixa de montar o prompt inteiro.

Passa a conter apenas a identidade do Mentor.

Exemplo:

```python
MENTOR_PROMPT = """
Você é o Prometheus Mentor.

Explique conceitos de forma didática.

Utilize exemplos sempre que possível.

Explique passo a passo.
"""
```

---

### Etapa 3 — Refatorar `PromptBuilder`

Ele continuará sendo:

```python
class PromptBuilder:

    @staticmethod
    def build(...)
```

Mas agora apenas junta os componentes.

Algo nessa linha:

```text
SYSTEM_PROMPT

        +

MENTOR_PROMPT

        +

Histórico

        +

Pergunta

        ↓

Prompt final
```

---

### Etapa 4 — Verificar o `MentorAgent`

Muito provavelmente **não precisaremos alterar absolutamente nada**.

Ele continuará fazendo apenas:

```python
prompt = PromptBuilder.build(...)
```

O que muda é apenas a implementação interna do `PromptBuilder`.

---

## Minha sugestão

Vamos fazer exatamente como nas aulas anteriores:

- implementamos **uma etapa por vez**;
    
- você me envia o código;
    
- eu faço o code review;
    
- só então seguimos para a próxima etapa.
    

Esse método tem funcionado muito bem e evita acumular mudanças difíceis de depurar.