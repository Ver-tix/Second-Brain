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

| Aula   | Pergunta                                 |
| ------ | ---------------------------------------- |
| Aula 3 | Como conversar com um LLM?               |
| Aula 4 | Onde deve ficar a comunicação com a API? |
| Aula 5 | Como dar memória ao agente?              |
| Aula 6 | Como dar ferramentas ao agente?          |
| Aula 7 | Quem decide quando usar uma ferramenta?  |
| Aula 8 | Como o Tool Calling realmente funciona?  |

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


A ideia é que cada etapa tenha um objetivo arquitetural claro.

---

Refatorar a arquitetura de ferramentas do **Prometheus-Mentor**.

Hoje temos:

```text
MentorAgent
      |
      ↓
ToolManager
      |
      ↓
CalculatorTool
```

O problema:

O `ToolManager` conhece diretamente todas as ferramentas.

A nova arquitetura será:

```text
MentorAgent
      |
      ↓
ToolManager
      |
      ↓
ToolRegistry
      |
      ↓
CalculatorTool
```

O agente deixa de conhecer implementações concretas.

---

# Etapa 1 — Criar o Tool Registry

## Objetivo

Criar uma camada responsável apenas por **registrar e armazenar ferramentas disponíveis**.

Criar:

```
app/
 └── tools/
      ├── calculator_tool.py
      ├── tool_manager.py
      └── tool_registry.py   ← novo
```

O Registry inicialmente deve possuir:

- um catálogo de ferramentas;
    
- uma forma de registrar ferramentas;
    
- uma forma de recuperar ferramentas pelo nome.
    

Exemplo conceitual:

```python
registry.get("calculator")
```

deve retornar:

```python
CalculatorTool()
```

---

## Conceito aprendido

Separação de responsabilidade:

Antes:

> ToolManager conhece ferramentas.

Depois:

> ToolManager conhece o Registry.

---

# Etapa 2 — Registrar a CalculatorTool no Registry

## Objetivo

Mover a responsabilidade de criação/conhecimento da ferramenta para o catálogo.

Antes:

```python
class ToolManager:

    def __init__(self):
        self.calculator = CalculatorTool()
```

Depois:

```python
ToolRegistry:

    "calculator": CalculatorTool()
```

---

## Resultado esperado

O Registry deve saber:

```
calculator
      ↓
CalculatorTool
```

Mas o restante da aplicação não deve precisar saber disso.

---

# Etapa 3 — Refatorar o ToolManager

## Objetivo

Fazer o ToolManager deixar de conhecer ferramentas específicas.

Antes:

```python
if tool_name == "calculator":
    return self.calculator.calculate(...)
```

Depois:

O fluxo deve ser:

```text
ToolManager recebe:

"calculator"

        ↓

consulta Registry

        ↓

recebe CalculatorTool

        ↓

executa ferramenta
```

---

## Resultado esperado

Adicionar uma nova ferramenta futuramente não deve exigir alterar o ToolManager.

---

# Etapa 4 — Remover conhecimento de ferramentas do MentorAgent

## Objetivo

Garantir que o agente não saiba quais ferramentas existem.

O MentorAgent não deve possuir:

```python
if tool_name == "calculator"
```

nem:

```python
CalculatorTool()
```

nem:

```python
SearchTool()
```

Ele deve apenas dizer:

> "Preciso executar esta ferramenta."

E delegar.

---

Novo fluxo:

```text
MentorAgent

"preciso usar calculator"

        ↓

ToolManager

        ↓

ToolRegistry

        ↓

CalculatorTool
```

---

# Etapa 5 — Testar extensibilidade

## Objetivo

Comprovar que a arquitetura realmente ficou mais escalável.

Criar uma segunda ferramenta simples.

Exemplo:

```
DateTool
```

ou

```
TextTool
```

Registrar:

```python
registry.register(
    "date",
    DateTool()
)
```

E verificar:

O ToolManager precisou ser alterado?

O MentorAgent precisou ser alterado?

A resposta esperada:

**Não.**

---

# Etapa 6 — Reflexão arquitetural

Responder:

## 1.

Por que o Tool Registry melhora a escalabilidade do sistema?

---

## 2.

Qual responsabilidade pertence ao Registry e qual pertence ao ToolManager?

---

## 3.

Por que remover conhecimento das ferramentas do MentorAgent é importante?

---

## 4.

Explique por que essa arquitetura se aproxima de um sistema baseado em plugins.

---

# Estado final esperado

Ao final da aula, teremos:

```text
                MentorAgent
                     |
                     ↓
                ToolManager
                     |
                     ↓
               ToolRegistry
                     |
        ┌────────────┼────────────┐
        ↓            ↓            ↓

 Calculator      Search        RAG

```

O MentorAgent não sabe:

- quantas ferramentas existem;
    
- como elas funcionam;
    
- onde estão implementadas.
    

Ele apenas solicita capacidades.

---

## Por que essa aula é importante no Prometheus?

Esta é a primeira aula em que começamos a construir uma **infraestrutura de ecossistema**.

Até agora estávamos construindo um agente.

Agora começamos a construir uma plataforma onde múltiplos agentes poderão existir.

O Tool Registry será uma das primeiras peças reutilizáveis do futuro:

- Prometheus-Mentor;
    
- Prometheus-Editor;
    
- Prometheus-Knowledge.
    

Ele será a base para o crescimento do sistema.