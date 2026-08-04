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
# Etapa 1 — Criar `system_prompt.py`

## Objetivo

Criar um componente que contenha **apenas as regras globais do Prometheus**.

Esse arquivo **não pertence ao Mentor**.

Ele pertence ao **Prometheus OS**.

É justamente isso que o professor quis mostrar na teoria:

```text
Prompt Final

↓

System Prompt
+
Agent Prompt
+
Contexto
+
Pergunta
```

---

## Onde criar?

Dentro da pasta de prompts:

```text
app/
└── prompts/
    ├── system_prompt.py   ← novo
    ├── mentor_prompt.py
    └── prompt_builder.py
```

---

## Conteúdo sugerido

Como estamos na Aula 10 e o professor pediu simplicidade, eu faria algo assim:

```python
SYSTEM_PROMPT = """
Você faz parte do Prometheus OS.

Responda sempre em português.

Se não souber a resposta, diga que não sabe.

Nunca invente informações.
"""
```

Perceba que **nenhuma linha fala sobre o Mentor**.

Isso é importante.

Essas regras deverão servir para qualquer agente do ecossistema no futuro.

---

## O que NÃO colocar

Não coloque coisas como:

```text
Explique como professor.

Use exemplos.

Explique passo a passo.
```

Essas instruções pertencem ao **Mentor**, não ao sistema.

---

Quando você criar o arquivo, me envie o conteúdo. Em seguida faremos a **Etapa 2**, que será enxugar o `mentor_prompt.py` para conter apenas a identidade do agente.

---
# Etapa 2 - Refatorar o `mentor_prompt.py`
O objetivo será deixá-lo contendo **somente a identidade do Prometheus Mentor**, removendo qualquer regra global que agora pertence ao `system_prompt.py`.

Agora temos
```Python
class PromptBuilder:
  
    @staticmethod
    def build(history: str,question: str) -> str:

        prompt = f"""
Você é o Prometheus-Mentor. 

Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara, didática e organizada. 

Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da definição técnica.

Histórico da conversa:

{history}

Nova pergunta do usuário: 

{question}
"""

        return prompt
```

## Objetivo

O arquivo deve responder apenas uma pergunta:

> **"Quem é o Prometheus Mentor?"**

Nada mais.

As regras gerais agora pertencem ao `SYSTEM_PROMPT`.

---

## Estrutura esperada

Eu faria algo assim:

```python
MENTOR_PROMPT = """
Você é o Prometheus Mentor.

Seu objetivo é ensinar conceitos de Inteligência Artificial, Engenharia de Software e Arquitetura de Agentes.

Explique de forma didática.

Sempre que possível, utilize exemplos.

Explique passo a passo.

Adapte a profundidade da resposta ao contexto da conversa.
"""
```

---

## O que deve sair deste arquivo

Se hoje houver algo parecido com:

```text
Responda em português.

Nunca invente informações.

Se não souber, diga que não sabe.
```

Essas linhas devem ser removidas.

Agora elas pertencem ao:

```python
SYSTEM_PROMPT
```

---

## Separação de responsabilidades

Ao final desta etapa teremos:

### `system_prompt.py`

```text
Regras do Prometheus OS
```

↓

### `mentor_prompt.py`

```text
Identidade do Mentor
```

Essa separação é exatamente análoga ao que fizemos na Aula 9:

```text
Tool Registry
        +
ToolManager
```

Agora temos:

```text
System Prompt
        +
Mentor Prompt
```

---

Implemente essa refatoração no seu `mentor_prompt.py` e me envie o código. Em seguida passaremos à **Etapa 3**, onde o `PromptBuilder` deixará de construir um prompt "na mão" e passará a compor esses módulos.