---
tags:
  - IA
  - programação
  - inovação
---
> **Objetivo da aula**
> 
> Entender o papel de um agente em código e implementar a primeira versão do **Prometheus-Mentor**.

---

# Antes de escrever uma linha de código...

Quero te fazer uma pergunta.

Imagine que eu te diga:

> "Construa um agente."

O que exatamente é um agente?

No Módulo 5 respondemos isso conceitualmente:

- ele planeja;
    
- utiliza ferramentas;
    
- consulta memória;
    
- toma decisões;
    
- replaneja quando necessário.
    

Mas...

**E em Python?**

É apenas uma classe.

Isso muda completamente nossa perspectiva.

---

# A ideia mais importante da aula

Um agente **não é magia**.

É um objeto.

Assim como você pode criar:

```python
carro = Carro()
```

Você pode criar:

```python
mentor = MentorAgent()
```

A inteligência vem do comportamento dessa classe.

---

# Primeira versão do nosso agente

Hoje nosso agente será extremamente simples.

Ele ainda:

- não terá memória;
    
- não utilizará ferramentas;
    
- não fará RAG;
    
- não planejará.
    

Ele apenas responderá perguntas usando um modelo.

E isso é ótimo.

Estamos construindo um prédio.

Hoje fazemos a fundação.

---

# Arquitetura da Aula

Nosso sistema ficará assim:

```text
Usuário
    │
    ▼
main.py
    │
    ▼
MentorAgent
    │
    ▼
LLMService
    │
    ▼
OpenAI API
```

Perceba uma coisa.

O agente **não conversa diretamente com a OpenAI**.

Existe um intermediário.

Isso é proposital.

---

# Por que existe um LLMService?

Imagine dois cenários.

### Cenário A

O agente chama a OpenAI diretamente.

```text
MentorAgent
      │
      ▼
OpenAI
```

Agora imagine que amanhã você queira usar Claude.

Você teria que alterar:

- TutorAgent
    
- EvaluatorAgent
    
- CuratorAgent
    
- SynthesizerAgent
    

Todos.

---

### Cenário B

Existe um serviço.

```text
MentorAgent
      │
      ▼
LLMService
      │
      ▼
OpenAI
```

Amanhã muda para Claude?

Você altera apenas:

```text
LLMService
```

Todos os agentes continuam funcionando.

Esse padrão se chama **Abstração**.

É um dos pilares da engenharia de software.

---

# Nosso primeiro fluxo

Hoje implementaremos exatamente isto:

```text
Usuário
    │
    ▼
main.py

cria

MentorAgent

MentorAgent recebe pergunta

↓

LLMService

↓

Modelo responde

↓

MentorAgent devolve resposta

↓

Usuário
```

Nada além disso.

Mas já é um agente.

---

# As responsabilidades

Veja como começamos a aplicar o princípio da responsabilidade única.

## main.py

Responsável apenas por iniciar a aplicação.

---

## MentorAgent

Responsável apenas por coordenar.

---

## LLMService

Responsável apenas por conversar com o modelo.

---

Cada componente tem um único motivo para mudar.

Você já estudou isso sem perceber.

---

# A primeira versão da árvore

Nossa pasta começará a ganhar vida.

```text
app/

agents/
    mentor_agent.py

services/
    llm_service.py

config/
    settings.py

main.py
```

Só quatro arquivos.

Mas extremamente bem definidos.

---

# O fluxo de chamadas

Visualmente:

```text
main.py

↓

MentorAgent.ask()

↓

LLMService.generate()

↓

OpenAI

↓

LLMService

↓

MentorAgent

↓

main.py
```

Repare que tudo acontece como uma conversa entre objetos.

---

# Um insight importante

Talvez o maior insight da aula seja este:

> **Os agentes não "sabem" conversar com a OpenAI.**

Eles sabem conversar com um **serviço**.

Isso parece um detalhe.

Mas é justamente o que permitirá, no futuro:

- trocar de modelo;
    
- usar vários modelos ao mesmo tempo;
    
- testar agentes sem internet;
    
- criar modelos locais.
    

Sem alterar os agentes.

---

# Ligação com o Módulo 5

Lembra quando falamos sobre separação de responsabilidades?

Hoje ela deixa de ser um diagrama e vira código.

Arquitetura não é apenas desenhar caixas.

É decidir **qual objeto faz o quê**.

---

# Laboratório 3 — Nascimento do Prometheus-Mentor

Hoje construiremos nossa primeira versão funcional. 

## [[🤖 Monitoria M6 003]]
## [[🛠 Desafio M6 003]] 

## Etapa 1 — Criar os arquivos

Dentro de `app`, crie:

```text
agents/
    mentor_agent.py

services/
    llm_service.py

config/
    settings.py
```

---

## Etapa 2 — Instalar as dependências

No ambiente virtual:

```bash
pip install openai python-dotenv
```

Depois execute:

```bash
pip freeze > requirements.txt
```

Assim seu projeto já terá as dependências registradas.

---

## Etapa 3 — Criar a chave da API

No arquivo `.env`:

```text
OPENAI_API_KEY=sua_chave_aqui
```

Não coloque aspas.

Não faça commit desse arquivo no Git.

---

## Etapa 4 — Configurar `settings.py`

Esse arquivo será responsável por:

- carregar o `.env`;
    
- ler a variável `OPENAI_API_KEY`;
    
- disponibilizá-la para o restante da aplicação.
    

**Não copie código da internet.**

Tente lembrar do projeto **HelloLLM** que fizemos. Você já implementou exatamente essa funcionalidade uma vez.

---

## Etapa 5 — Implementar `LLMService`

Crie uma classe:

```python
class LLMService:
```

Por enquanto, ela terá apenas uma responsabilidade:

Receber um texto e devolver a resposta do modelo.

---

## Etapa 6 — Implementar `MentorAgent`

Crie uma classe:

```python
class MentorAgent:
```

Ela deve:

- receber um `LLMService` no construtor (injeção de dependência);
    
- possuir um método `ask(pergunta)` que delega a geração da resposta ao serviço.
    

**Importante:** o agente **não deve criar** um `LLMService` internamente. Ele apenas o utiliza.

---

## Etapa 7 — Implementar o `main.py`

No `main.py`, faça o fluxo:

1. Criar uma instância do `LLMService`.
    
2. Criar uma instância do `MentorAgent`, passando o serviço.
    
3. Pedir uma pergunta ao usuário pelo terminal.
    
4. Exibir a resposta do agente.
    

---

# O objetivo do laboratório

No final desta aula, você deverá conseguir executar algo como:

```text
> Pergunta:
O que é um Transformer?

↓

Prometheus-Mentor:

"Um Transformer é uma arquitetura..."
```

---

## Uma missão extra (opcional)

Depois que tudo funcionar, responda para si mesmo:

> **Por que passamos o `LLMService` para o `MentorAgent`, em vez de o agente criá-lo sozinho?**

Se você conseguir responder essa pergunta com suas próprias palavras, terá compreendido um dos conceitos mais importantes da engenharia de software: **injeção de dependência**.

---

A partir da próxima aula, o Prometheus-Mentor deixará de ser apenas um "chat". Começaremos a adicionar memória, ferramentas e comportamento inteligente — sempre mantendo a arquitetura limpa que estamos construindo desde o início.