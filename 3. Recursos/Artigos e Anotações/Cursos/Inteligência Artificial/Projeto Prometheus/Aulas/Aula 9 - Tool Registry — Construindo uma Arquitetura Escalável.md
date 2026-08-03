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
Até agora, cada aula respondeu a uma pergunta arquitetural.

|Aula|Pergunta|
|---|---|
|Aula 3|Como conversar com um LLM?|
|Aula 4|Onde deve ficar a comunicação com a API?|
|Aula 5|Como dar memória ao agente?|
|Aula 6|Como dar ferramentas ao agente?|
|Aula 7|Quem decide quando usar uma ferramenta?|
|Aula 8|Como o Tool Calling realmente funciona?|

Agora surge um problema completamente novo.

---

# O problema

Imagine que daqui a alguns meses o Prometheus tenha estas ferramentas:

```
CalculatorTool

SearchTool

PerplexityTool

GitHubTool

MarkdownTool

SVGTool

RAGTool

CalendarTool

EmailTool

NewsTool

PDFTool

ExcelTool
```

Se continuarmos como estamos hoje, o `ToolManager` ficará parecido com isto:

```python
class ToolManager:

    def __init__(self):

        self.calculator = CalculatorTool()
        self.search = SearchTool()
        self.github = GithubTool()
        self.calendar = CalendarTool()
        self.email = EmailTool()
        self.rag = RAGTool()
        ...
```

Depois:

```python
if tool_name == "calculator":
    ...

elif tool_name == "calendar":
    ...

elif tool_name == "github":
    ...

elif tool_name == "email":
    ...
```

Depois:

```python
elif ...

elif ...

elif ...

elif ...
```

E assim para sempre.

---

## O problema não é o tamanho.

O problema é este princípio:

> **Toda vez que nasce uma ferramenta nova, precisamos modificar um código antigo.**

Isso é um cheiro arquitetural muito conhecido.

---

# Princípio Open/Closed (OCP)

Existe um princípio clássico da Engenharia de Software chamado:

> **Open for Extension**
> 
> **Closed for Modification**

Ou seja:

Um sistema deveria ser:

- aberto para ganhar novas funcionalidades;
    

mas

- fechado para alterações em código que já funciona.
    

Hoje o nosso ToolManager viola exatamente isso.

Toda ferramenta nova exige editar um arquivo antigo.

---

# Fazendo uma analogia

Imagine um shopping.

Lá existem centenas de lojas.

Quando uma loja nova inaugura...

...o shopping precisa demolir sua administração?

Claro que não.

A administração apenas registra:

```
Loja

Nome

Localização

Tipo
```

E pronto.

O shopping continua funcionando.

---

Nosso ToolManager deveria agir exatamente assim.

Ele não deveria conhecer ferramentas.

Ele deveria conhecer apenas um cadastro.

---

# O nascimento do Tool Registry

Em vez de fazer isso:

```
ToolManager

↓

CalculatorTool

SearchTool

GitHubTool

CalendarTool
```

Passaremos a fazer isto:

```
ToolManager

↓

Tool Registry

↓

calculator

↓

CalculatorTool
```

Outro exemplo:

```
ToolManager

↓

Tool Registry

↓

calendar

↓

CalendarTool
```

Perceba a mudança.

O ToolManager deixa de conhecer ferramentas específicas.

Ele conhece apenas um catálogo.

---

# O Registry

Imagine um enorme dicionário.

```
TOOLS = {

"calculator": CalculatorTool(),

"calendar": CalendarTool(),

"github": GithubTool(),

"search": SearchTool(),

"rag": RAGTool()

}
```

Agora o código deixa de ser:

```python
if tool == "calculator":
    ...

elif tool == "calendar":
    ...
```

E passa a ser algo conceitualmente parecido com:

```python
tool = registry["calculator"]
```

ou

```python
tool = registry[tool_name]
```

Observe a diferença.

O código não pergunta mais:

> "Qual ferramenta é?"

Ele pergunta:

> "Existe uma ferramenta registrada com esse nome?"

É uma mudança pequena.

Mas muda completamente a escalabilidade.

---

# O verdadeiro ganho

Agora adicionar uma ferramenta significa apenas registrar:

```
TOOLS["email"] = EmailTool()
```

Nada mais.

Nenhum `if`.

Nenhum `elif`.

Nenhuma alteração no agente.

Nenhuma alteração na arquitetura.

---

# O que acabamos de construir?

Sem perceber, estamos recriando um padrão extremamente comum em sistemas grandes.

Frameworks modernos fazem isso o tempo inteiro:

- Django registra URLs.
    
- FastAPI registra rotas.
    
- Flask registra endpoints.
    
- VSCode registra extensões.
    
- Navegadores registram plugins.
    

Todos eles seguem a mesma ideia:

> **Registrar capacidades, não codificar decisões.**

---

# Ligando isso ao Prometheus

Lembra quando desenhamos o ecossistema?

```
Prometheus OS

├── Mentor

├── Editor

├── Knowledge
```

Agora imagine isto:

```
Prometheus OS

            │

            ▼

     Shared Tool Registry

            │

 ┌──────────┼───────────────┐

 ▼          ▼               ▼

Mentor    Editor      Knowledge
```

Todos os módulos utilizam o mesmo catálogo.

O Mentor pode usar:

- Calculator
    
- RAG
    
- Search
    

O Editor pode usar:

- Markdown
    
- SVG
    
- Perplexity
    

O Knowledge pode usar:

- GitHub
    
- Chroma
    
- Obsidian
    

Cada agente usa apenas o que precisa, mas a infraestrutura é compartilhada.

---

# A ideia mais importante da aula

Até hoje, nosso código respondia:

> **"Como executar uma ferramenta?"**

A partir de agora, ele passa a responder:

> **"Como descobrir qual ferramenta executar?"**

Essa é exatamente a mudança de mentalidade que separa um programa que funciona de uma arquitetura que continua funcionando quando cresce.

---

## Laboratório Prático — Aula 9

## [[🤖 Monitoria M6 009 - TEORIA]]  
## [[🤖 Monitoria M6 009 - PRÁTICA]]
## [[🛠 Desafio M6 009]] 

Nesta aula, vamos dar o próximo passo no Prometheus-Mentor.

**Objetivo:** substituir um `ToolManager` "engessado" por um sistema baseado em um **Tool Registry**.

Ao final do laboratório, teremos:

- Um arquivo dedicado ao registro de ferramentas (`tool_registry.py`).
    
- Um `ToolManager` que busca ferramentas dinamicamente pelo nome.
    
- Um `MentorAgent` que não precisa conhecer nenhuma ferramenta específica.
    
- A possibilidade de adicionar novas ferramentas apenas registrando-as no catálogo, sem alterar o restante da arquitetura.
    

Essa será a primeira infraestrutura verdadeiramente reutilizável do ecossistema Prometheus e servirá de base para as futuras ferramentas de RAG, busca na web, Perplexity e integração com o Second Brain.