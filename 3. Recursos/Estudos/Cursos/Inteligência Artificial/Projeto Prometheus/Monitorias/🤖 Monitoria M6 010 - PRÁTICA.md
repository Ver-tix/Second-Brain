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
# ==Etapa 1 — Criar `system_prompt.py`
## Objetivo
Criar um componente que contenha **apenas as regras globais do Prometheus**.==

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
# ==Etapa 2 - Refatorar o `mentor_prompt.p
O objetivo será deixá-lo contendo **somente a identidade do Prometheus Mentor**, removendo qualquer regra global que agora pertence ao `system_prompt.py`.==

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

Perceba que **desaparecem** estas partes:

```Python
Histórico da conversa:

{history}

Nova pergunta do usuário:

{question}
```

Porque elas não pertencem ao Mentor.

Elas pertencem ao **PromptBuilder**.

---
## Então... para onde vai a classe `PromptBuilder`?

Ela sai deste arquivo completamente.

Você criará um novo arquivo (caso ainda não exista):
```
app/
└── prompts/
    ├── system_prompt.py
    ├── mentor_prompt.py
    └── prompt_builder.py   ← aqui ficará a classe
```
E a classe ficará parecida com isto:
```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT

class PromptBuilder:
    @staticmethod
    def build(history: str, question: str) -> str:

        return f"""
{SYSTEM_PROMPT}

{MENTOR_PROMPT}

Histórico da conversa:

{history}

Nova pergunta do usuário:

{question}
"""
```
Observe como agora cada componente tem uma única responsabilidade:

- `system_prompt.py` → regras globais.
- `mentor_prompt.py` → identidade do Mentor.
- `prompt_builder.py` → monta o prompt final.

---

💡 **Uma observação importante:** se atualmente sua classe `PromptBuilder` já está em um arquivo chamado `mentor_prompt.py`, este é um ótimo momento para reorganizar a estrutura do projeto:

```
app/
└── prompts/
    ├── system_prompt.py
    ├── mentor_prompt.py      # apenas MENTOR_PROMPT
    └── prompt_builder.py     # classe PromptBuilder
```

Essa organização é muito mais coerente e deixa os nomes dos arquivos alinhados com suas responsabilidades. É exatamente o tipo de refatoração arquitetural que esta aula pretende ensinar.

---
# ==Etapa 3 - Verificar se o mentorAgent Continuar Importando o PromptBuilder do Novo Arquivo `prompt_builder.py` em vez do Antigo `mentor_prompt.py`

Antes:

```python
from app.prompts.mentor_prompt import PromptBuilder
```

Depois:

```python
from app.prompts.prompt_builder import PromptBuilder
```

Todo o restante do `MentorAgent` continua exatamente igual.

Ou seja:

```python
prompt = PromptBuilder.build(
    history,
    question
)
```

não muda.

## Por quê?

Porque a Aula 10 não quis mudar **a interface** do `PromptBuilder`.

Ela quis mudar apenas **a implementação interna**.

Essa é uma diferença arquitetural enorme.

O `MentorAgent` continua dizendo:

> "Construa um prompt."

Ele **não sabe**:

- de onde veio o `SYSTEM_PROMPT`;
- de onde veio o `MENTOR_PROMPT`;
- como o histórico foi concatenado;
- como a pergunta foi adicionada.

Ele apenas usa a interface:

```python
PromptBuilder.build(...)
```

Essa é uma consequência direta do desacoplamento.

## Um teste mental

Imagine que daqui a duas aulas o professor resolva adicionar:

```
SYSTEM_PROMPT
+
AGENT_PROMPT
+
RAG Context
+
Memory
+
Available Tools
+
Current Date
+
User Question
```

Quem precisará mudar?

Resposta:

Somente:

```
PromptBuilder
```

Quem **não** precisará mudar?

- ✅ MentorAgent
- ✅ LLMService
- ✅ ConversationMemory
- ✅ ToolManager

Isso mostra que a interface ficou estável.

---

## Minha avaliação

Até agora, a Aula 10 está praticamente concluída.

Você criou:

- ✅ `system_prompt.py`
- ✅ `mentor_prompt.py`
- ✅ `prompt_builder.py`
- ✅ Atualizou o import do `MentorAgent`

E o mais importante: **não precisou alterar uma única linha da lógica do `MentorAgent`**.

Esse é um ótimo sinal arquitetural. Quando uma refatoração muda a organização interna de um componente sem obrigar os consumidores a mudarem, significa que a interface pública foi bem projetada. Foi exatamente isso que aconteceu aqui.