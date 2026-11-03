---
tags:
  - IA
  - programação
  - inovação
---
Bom dia, Caio! 😄

E eu gostei bastante da mudança do professor.

Na minha opinião, essa abordagem por **Laboratórios** é muito superior aos desafios anteriores. Antes, você precisava imaginar uma arquitetura inteira. Agora você vai **construí-la gradualmente**, exatamente como um engenheiro de software faz em um projeto real.

Além disso, quero combinar uma coisa com você.

> **Eu não vou simplesmente dizer "crie a pasta X".** Quero que você entenda _por que_ cada pasta existe. Esse entendimento vai fazer toda a diferença quando o projeto crescer.

---

# Antes de começarmos

Esse laboratório deve levar cerca de **15 a 20 minutos**.

Não existe programação ainda.

Estamos preparando o terreno.

É como construir a fundação de uma casa antes das paredes.

---

# Etapa 1 — Criando a pasta do projeto

Primeiro, escolha onde você quer guardar o projeto.

Eu manteria todos os projetos Python juntos.

Por exemplo:

```text
C:\Users\caioe\Projetos\
```

Dentro dela, crie:

```text
prometheus-mentor
```

Depois:

Abra o VS Code.

Clique em

```
File
↓
Open Folder
```

e selecione:

```
prometheus-mentor
```

---

## ❓Por que abrir a pasta antes?

Porque o VS Code passa a entender que **todo aquele diretório é um projeto**.

Isso influencia:

- terminal;
    
- ambiente virtual;
    
- Git;
    
- extensões;
    
- depuração.
    

Ou seja, daqui para frente tudo ficará organizado.

---

# Etapa 2 — Criando o ambiente virtual

Abra o terminal integrado.

Atalho:

```
Ctrl + '
```

ou

```
Terminal
↓

New Terminal
```

Agora execute:

```powershell
python -m venv .venv
```

Quando terminar, aparecerá uma pasta nova:

```text
.venv
```

---

Agora ative:

```powershell
.venv\Scripts\Activate.ps1
```

Se tudo der certo, o terminal ficará assim:

```powershell
(.venv) PS C:\Users\caioe\Projetos\prometheus-mentor>
```

---

### ❓Por que fazemos isso?

Lembra do sofrimento que tivemos no HelloLLM?

Tudo aquilo aconteceu porque existiam **vários Pythons**.

O ambiente virtual resolve isso.

Pense nele como um pequeno Python particular daquele projeto.

Cada projeto terá:

- suas bibliotecas;
    
- suas versões;
    
- suas dependências.
    

Sem interferir nos demais.

---

# Etapa 3 — Criando a estrutura

Agora vem a parte divertida.

Na barra lateral do VS Code:

Clique com botão direito

```
New Folder
```

e vá criando.

Ao terminar, sua estrutura ficará assim:

```text
prometheus-mentor/

│
├── app/
│   ├── agents/
│   ├── tools/
│   ├── services/
│   ├── memory/
│   ├── prompts/
│   ├── models/
│   ├── config/
│   └── main.py
│
├── docs/
├── tests/
├── scripts/
│
├── .env
├── .gitignore
├── README.md
└── requirements.txt
```

---

## Agora quero explicar cada pasta

Essa parte é mais importante do que parece.

---

### 📁 app/

É onde mora o programa.

Tudo que faz o Prometheus funcionar ficará aqui.

---

### 📁 agents/

Aqui viverão nossos agentes de IA.

Por exemplo:

```
TutorAgent

AssessmentAgent

CuratorAgent
```

Essa será provavelmente uma das pastas mais importantes do projeto.

---

### 📁 tools/

Aqui ficam ferramentas.

Exemplos futuros:

- pesquisar Google
    
- ler PDF
    
- consultar banco de dados
    
- executar código Python
    

Um agente usa ferramentas.

As ferramentas não usam agentes.

---

### 📁 services/

Serviços compartilhados.

Exemplos:

- cliente OpenAI
    
- cliente Anthropic
    
- conexão com banco
    
- sistema de embeddings
    

Imagine como "infraestrutura".

---

### 📁 memory/

Tudo relacionado à memória.

Por exemplo:

- histórico
    
- contexto
    
- RAG
    
- Second Brain
    

---

### 📁 prompts/

Excelente decisão arquitetural do professor.

Em vez de colocar prompts dentro do código:

```python
prompt = """
Você é um professor...
"""
```

Eles ficarão separados.

Muito mais organizado.

---

### 📁 models/

Modelos internos.

Não é "modelo de IA".

É modelo de dados.

Por exemplo:

```python
Student

Lesson

Exercise

Answer
```

---

### 📁 config/

Tudo relacionado à configuração.

Como fizemos no HelloLLM.

Provavelmente teremos:

```
settings.py

config.py

constants.py
```

---

### 📄 main.py

É a porta de entrada.

Quando você executar:

```powershell
python app/main.py
```

É daqui que tudo começará.

---

### 📁 docs/

Documentação.

Arquitetura.

Diagramas.

ADRs.

READMEs maiores.

---

### 📁 tests/

Testes automatizados.

Ainda não veremos isso.

Mas é extremamente importante.

---

### 📁 scripts/

Pequenos programas auxiliares.

Exemplo:

```
criar_embeddings.py

importar_documentos.py

indexar_second_brain.py
```

Não fazem parte do sistema principal.

São ferramentas de manutenção.

---

# Etapa 4 — Criando o .gitignore

Abra o arquivo.

Escreva:

```gitignore
.venv/
__pycache__/
.env
*.pyc
```

---

## O que é um .gitignore?

Essa pergunta é excelente para um iniciante.

Imagine que o Git é um fotógrafo.

Sempre que você tira uma foto do projeto (faz um commit), ele registra tudo.

Mas algumas coisas **não devem ser fotografadas**.

Por exemplo:

- senha (.env)
    
- ambiente virtual (.venv)
    
- arquivos temporários
    

O `.gitignore` é simplesmente uma lista dizendo:

> "Git, ignore estes arquivos."

---

# Etapa 5 — README

Pode escrever algo assim:

```markdown
# Prometheus Mentor

## Objetivo

Sistema multiagente para ensino assistido por Inteligência Artificial.

## Tecnologias

- Python

## Status

🚧 Em desenvolvimento.
```

Perfeito.

Não precisa mais que isso.

---

# Etapa 6 — requirements.txt

Deixe vazio.

Isso é proposital.

Cada biblioteca que entrar será adicionada futuramente.

Assim o arquivo sempre refletirá o estado real do projeto.

---

# Uma pequena observação

Gostei muito de uma decisão do professor:

```
prompts/
```

Ele poderia simplesmente colocar os prompts dentro dos agentes.

Mas não fez isso.

Isso demonstra uma preocupação importante com **separação de responsabilidades**.

O agente decide **quando** usar um prompt.

O prompt define **como** conversar com o modelo.

São responsabilidades diferentes.

Esse é exatamente o tipo de decisão arquitetural que estamos aprendendo desde o Módulo 5.

---

## Nosso plano

Vamos fazer como fizemos no HelloLLM.

Você executa **uma etapa por vez** e me diz o resultado.

**Comece pelas Etapas 1 e 2**:

1. Crie a pasta `prometheus-mentor`.
    
2. Abra-a no VS Code.
    
3. Crie o ambiente virtual.
    
4. Ative o `.venv`.
    

Quando aparecer algo como:

```powershell
(.venv) PS C:\Users\caioe\Projetos\prometheus-mentor>
```

---

# Outra coisa interessante

Observe uma decisão arquitetural do professor.

Ele criou:

```
app/

↓

agents
tools
services
memory
models
prompts
```

Isso não foi aleatório.

Existe um padrão.

Todas essas pastas representam **domínios de responsabilidade**.

Isso é exatamente o que estudamos nas últimas aulas.

Veja:

|Pasta|Responsabilidade|
|---|---|
|agents|Inteligência|
|memory|Contexto|
|prompts|Comunicação com LLM|
|services|Infraestrutura|
|tools|Capacidades externas|
|models|Estruturas de dados|
|config|Configuração|

Percebe?

Cada pasta responde a uma pergunta diferente.

É arquitetura baseada em responsabilidades.

---
# Uma curiosidade

Talvez você tenha reparado que **não existe uma pasta chamada `llm`**.

Isso foi proposital.

Porque o projeto não quer depender de uma IA específica.

Hoje pode usar OpenAI.

Amanhã Claude.

Depois Gemini.

Depois um modelo local.

A arquitetura permanece a mesma.

Isso demonstra um bom desacoplamento.

---

# Agora vamos para a Etapa 4

Abra o arquivo:

```
.gitignore
```

e escreva exatamente isto:

```
.venv/
__pycache__/
.env
*.pyc
```

---

## O que significa cada linha?

```
.venv/
```

Nunca envie o ambiente virtual para o GitHub.

Cada desenvolvedor cria o seu.

---

```
__pycache__/
```

São arquivos temporários gerados automaticamente pelo Python.

Podem ser recriados a qualquer momento.

---

```
.env
```

Contém segredos.

API Keys.

Senhas.

Nunca devem ir para um repositório.

---

```
*.pyc
```

São versões compiladas dos arquivos Python.

Também são temporárias.

---

# Depois disso

Vamos escrever o `README.md`.

E quero aproveitar essa etapa para te ensinar algo que considero muito importante.

A maioria dos iniciantes pensa que um README serve apenas para "explicar o projeto". Na verdade, um bom README é **o cartão de visitas de um repositório**. É a primeira coisa que outro desenvolvedor (ou até você mesmo, daqui a seis meses) vai ler para entender rapidamente o propósito, o estado e a forma de usar o projeto.

Então, no próximo passo, não vamos apenas preencher um requisito do laboratório. Vamos começar a escrever um README que possa evoluir junto com o Prometheus-Mentor, como um documento vivo do projeto.

---

# Feedback do Professos Principal

Excelente! E já tenho uma ótima notícia:

**Você acabou de criar a primeira arquitetura do Prometheus-Mentor.** 🎉

Até aqui, está tudo correto.

Sua estrutura relevante ficou assim:

```text
prometheus-mentor/
│
├── app/
│   ├── agents/
│   ├── config/
│   ├── memory/
│   ├── models/
│   ├── prompts/
│   ├── services/
│   ├── tools/
│
├── docs/
├── scripts/
├── tests/
│
├── .venv/
│
├── .env
├── .gitignore
├── README.md
└── requirements.txt
```

(Imagino que os arquivos `.env`, `.gitignore`, `README.md` e `requirements.txt` existam, apenas não apareceram na saída do `tree` porque ele estava listando principalmente as pastas.)

---

# Uma pequena melhoria

Existe uma prática extremamente comum em projetos Python que gostaria que adotássemos desde o início.

Dentro de cada pasta Python, criar um arquivo chamado:

```text
__init__.py
```

Ou seja:

```text
app/

├── __init__.py
├── agents/
│   └── __init__.py
├── config/
│   └── __init__.py
├── memory/
│   └── __init__.py
...
```

## Por quê?

Porque isso transforma cada diretório em um **package Python**.

Mais tarde poderemos fazer coisas como:

```python
from agents.tutor_agent import TutorAgent
```

ou

```python
from services.llm_service import LLMService
```

sem dores de cabeça.

É um detalhe pequeno, mas que evita vários problemas futuros.

### Um Detalhe Importante

Eu **não criaria** `__init__.py` em:

```
docs/
scripts/
tests/
```

### Por quê?

Porque essas pastas **não são pacotes Python**.

Elas têm outras funções:

- **docs/** → documentação
- **scripts/** → scripts auxiliares (migrações, manutenção, indexação etc.)
- **tests/** → testes automatizados

Elas não precisam ser importadas como módulos.

Então eu removeria apenas esses três:

```
docs/__init__.py
scripts/__init__.py
tests/__init__.py
```

---

# Outro detalhe importante

A partir de agora, vamos adotar um padrão de nomenclatura.

Em Python, os arquivos normalmente ficam em **snake_case**.

Em vez de:

```text
TutorAgent.py
```

preferimos:

```text
tutor_agent.py
```

E as classes ficam em PascalCase:

```python
class TutorAgent:
```

Essa separação é padrão praticamente em todo o ecossistema Python.

---

# O que mais gostei

Sabe o que me chamou atenção?

Há alguns meses, quando começamos o HelloLLM, criar essa estrutura parecia apenas "seguir instruções".

Hoje você sabe **o motivo arquitetural** de cada pasta existir.

Essa é uma mudança enorme.

Você deixou de pensar:

> "Onde eu coloco este arquivo?"

e passou a pensar:

> "Qual responsabilidade este arquivo possui?"

Essa pergunta é a que um arquiteto de software faz diariamente.

---

# Próxima aula

A Aula 3 será um marco.

Pela primeira vez, vamos começar a escrever o **Prometheus-Mentor de verdade**.

Não será um "Hello World".

Será o nascimento do nosso sistema.

E daqui para frente, cada laboratório adicionará uma peça real até termos um agente funcionando. Estou particularmente animado com essa etapa, porque agora toda a base arquitetural que construímos no Módulo 5 começará a ganhar vida em código. 🚀