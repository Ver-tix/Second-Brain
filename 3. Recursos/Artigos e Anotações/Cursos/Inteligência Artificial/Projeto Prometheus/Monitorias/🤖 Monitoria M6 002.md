---
tags:
  - inteligenciaartificial
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

me avise. Aí seguimos juntos para a construção da estrutura de pastas. Assim conseguimos resolver qualquer problema imediatamente, sem acumular erros como aconteceu no início do HelloLLM.