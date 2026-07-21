---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
Perfeito! Agora começa a parte em que muitos cursos fazem o caminho inverso do nosso.

A maioria ensina:

> Python → API → Framework → Agentes

Nós faremos:

> Arquitetura → Engenharia → Python → Frameworks

Assim, quando usarmos LangChain, LangGraph, OpenAI SDK ou qualquer outra tecnologia, você saberá **por que ela existe**, e não apenas **como utilizá-la**.

---

# Projeto Prometheus

# Módulo 6 — Aula 2

# Organizando um Projeto Profissional de IA

> **Objetivo da aula**
> 
> Aprender a estruturar um projeto de IA de forma escalável, entendendo por que cada pasta existe antes de escrever qualquer código.

---

# O erro mais comum

Imagine alguém criando seu primeiro agente.

Normalmente começa assim:

```text
meu_agente.py
```

Depois cresce um pouco.

```text
meu_agente.py
funcoes.py
config.py
```

Depois:

```text
meu_agente.py
funcoes.py
config.py
teste.py
teste2.py
novo.py
```

Alguns meses depois...

```text
projeto/
    final.py
    final2.py
    final_agora_vai.py
    teste_novo.py
    main_final.py
```

😂

Parece brincadeira.

Mas acontece muito.

O problema não é Python.

É falta de arquitetura.

---

# Pense como uma empresa

Imagine que amanhã você contrata cinco desenvolvedores.

Você diria:

> "Procurem aí os arquivos..."

Claro que não.

Cada pessoa precisa saber imediatamente:

- onde criar código;
    
- onde ficam os agentes;
    
- onde ficam as ferramentas;
    
- onde ficam os testes;
    
- onde ficam as configurações.
    

Uma boa estrutura reduz a carga cognitiva da equipe.

---

# Nosso primeiro projeto

Vamos imaginar apenas o Prometheus-Mentor.

Sua estrutura inicial poderia ser:

```text
prometheus-mentor/

│
├── app/
│
├── tests/
│
├── docs/
│
├── scripts/
│
├── .env
│
├── requirements.txt
│
└── README.md
```

Repare:

Ainda não apareceu nenhum agente.

Primeiro organizamos a casa.

---

# A pasta `app/`

Tudo que faz o sistema funcionar mora aqui.

```text
app/

├── agents/
├── tools/
├── services/
├── memory/
├── prompts/
├── models/
├── config/
└── main.py
```

Essa pasta representa a **aplicação**.

---

# Agents

Aqui ficam os agentes.

```text
agents/

TutorAgent.py

EvaluatorAgent.py

CuratorAgent.py

SynthesizerAgent.py
```

Cada arquivo:

↓

uma responsabilidade.

---

# Tools

Ferramentas.

```text
tools/

rag.py

calculator.py

search.py

markdown.py
```

Perceba.

Ferramentas não pensam.

Ferramentas executam.

---

# Services

Essa pasta costuma confundir iniciantes.

Ela guarda serviços reutilizáveis.

Exemplos:

```text
services/

llm_service.py

event_bus.py

logging.py

storage.py
```

Os agentes usam esses serviços.

---

# Memory

Como vimos no Módulo 5:

existem memórias.

```text
memory/

shared_memory.py

conversation_memory.py

preferences.py
```

Elas ficam separadas.

Porque memória não é agente.

---

# Prompts

Algo muito interessante.

Muitos iniciantes fazem isto:

```python
prompt = """
...
"""
```

Dentro do código.

Nós não.

Criamos:

```text
prompts/

tutor.md

evaluator.md

curator.md
```

Os prompts tornam-se documentos.

Muito mais fáceis de editar.

---

# Models

Aqui mora a abstração dos modelos.

Não o GPT.

Nem Claude.

Nem Gemini.

A abstração.

Por exemplo:

```text
models/

base_model.py

openai_model.py

anthropic_model.py
```

Assim, trocar de provedor não exige reescrever os agentes.

---

# Config

Tudo que é configuração.

```text
config/

settings.py

constants.py
```

Nunca espalhe constantes pelo projeto.

---

# Tests

Uma pasta extremamente importante.

```text
tests/

test_tutor.py

test_tools.py

test_memory.py
```

Código profissional sempre nasce acompanhado de testes.

---

# Docs

Aqui mora a documentação.

Não apenas README.

Mas diagramas.

Arquitetura.

ADRs.

Decisões.

Fluxos.

Você já percebeu que gostamos bastante disso. 😄

---

# Scripts

Pequenas tarefas auxiliares.

Exemplo:

```text
scripts/

index_second_brain.py

reset_database.py

import_notes.py
```

Não fazem parte da aplicação.

São ferramentas para desenvolvedores.

---

# README

O README deve responder:

> O que é este projeto?

> Como instalar?

> Como executar?

> Como contribuir?

Nunca subestime um bom README.

---

# Requirements

Lista das dependências.

Exemplo:

```text
openai

python-dotenv

langchain

chromadb
```

Sem isso ninguém consegue reproduzir seu projeto.

---

# O arquivo `.env`

Nunca coloque:

```python
API_KEY="abc123"
```

Dentro do código.

Use:

```text
.env
```

E carregue em tempo de execução.

Isso protege credenciais e facilita trocar ambientes.

---

# Uma observação importante

Você deve ter notado algo.

Até agora...

Não apareceu LangChain.

Nem LangGraph.

Nem OpenAI.

Nem Chroma.

Porque a estrutura pertence **ao projeto**.

As bibliotecas apenas ocupam os espaços dela.

Essa diferença é enorme.

---

# Nossa filosofia

Primeiro desenhamos:

```text
Projeto
```

Depois:

```text
Arquitetura
```

Depois:

```text
Pastas
```

Depois:

```text
Arquivos
```

Depois:

```text
Classes
```

Depois:

```text
Código
```

Nunca o contrário.

---

# Ligando com o HelloLLM

Lembra do primeiro projeto que fizemos?

Ele tinha algo como:

```text
config.py

main.py

providers/

requirements.txt

.env
```

Na época parecia apenas organização.

Hoje você consegue enxergar que aquilo já era uma **pequena arquitetura**.

Agora estamos apenas expandindo essa ideia para um sistema muito maior.

---

# Um insight importante

A qualidade de um projeto raramente é determinada pelo arquivo `main.py`.

Ela é determinada por **como o projeto continua organizado depois de 100 arquivos**.

Projetos pequenos funcionam quase de qualquer jeito.

Projetos grandes exigem arquitetura.

---

# Desafio — Aula 2

## Parte 1 — Revisão da Estrutura

Observe a estrutura apresentada nesta aula.

Escolha **três pastas** e explique:

- qual problema ela resolve;
    
- por que ela merece existir separadamente;
    
- o que aconteceria se seu conteúdo fosse espalhado pelo projeto.
    

---

## Parte 2 — Evoluindo o Prometheus-Mentor

Pensando apenas no módulo **Prometheus-Mentor**, proponha uma estrutura um pouco mais detalhada.

Você pode adicionar novas pastas, remover algumas ou reorganizá-las.

O importante é justificar cada decisão arquitetural.

---

## Parte 3 — Pensando como Arquiteto

Suponha que, daqui a dois anos, o Prometheus tenha:

- 40 agentes;
    
- 15 desenvolvedores;
    
- milhares de arquivos.
    

Na sua opinião:

- quais princípios desta aula continuarão válidos?
    
- quais partes da estrutura talvez precisem evoluir?
    
- como evitar que o projeto se torne um "final_final_v2.py" gigante?
    

---

### Observação do professor

Esta aula pode parecer "simples", porque fala de pastas e arquivos.

Mas ela é uma das mais importantes do módulo.

Já vi projetos excelentes fracassarem não por causa da IA, mas porque ninguém conseguia entender onde as coisas estavam.

Uma boa arquitetura não acelera apenas a máquina.

Ela acelera as pessoas.