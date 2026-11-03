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
<h1 align="center"> Primeiramente, Alguns Questionamentos do Monitor</h1>
# Perguntas para o professor

## 1. O Tool Registry armazenará apenas instâncias das ferramentas ou também seus metadados?

Na teoria, o Registry foi apresentado como:

```json
TOOLS = {
    "calculator": CalculatorTool(),
    "calendar": CalendarTool(),
}
```

Mas como estamos trabalhando com Tool Calling, pensei que talvez futuramente o Registry pudesse armazenar também:

- instância da ferramenta;
- Function Schema;
- descrição;
- permissões;
- categoria.

Algo como:

```json
TOOLS = {
    "calculator": {
        "instance": CalculatorTool(),
        "schema": CALCULATOR_TOOL,
        "description": "...",
    }
}
```

**Pergunta:** nesta etapa o Registry deve armazenar **somente a instância**, ou devemos projetá-lo pensando em armazenar metadados futuramente?

---

## 2. Quem será o responsável por criar as instâncias?

Hoje a teoria mostra algo semelhante a:

```json
TOOLS = {
    "calculator": CalculatorTool()
}
```

Ou seja, o Registry cria as instâncias.

Mas outra arquitetura possível seria:

```python
calculator = CalculatorTool()

registry.register(
    "calculator",
    calculator
)
```

**Pergunta:** pedagogicamente, nesta aula, o Registry deve ser responsável por instanciar as ferramentas ou apenas registrá-las?

---

## 3. O ToolManager continuará existindo como uma camada separada?

Depois do Registry, o fluxo fica:

```
MentorAgent
    ↓
ToolManager
    ↓
ToolRegistry
    ↓
Tool
```

Mas fiquei pensando se, no futuro, o próprio Registry não poderia devolver diretamente a ferramenta:

```python
tool = registry.get(tool_name)
```

fazendo o ToolManager perder parte da sua responsabilidade.

**Pergunta:** o ToolManager continuará existindo no projeto por representar uma camada de orquestração, ou sua função tende a diminuir conforme a arquitetura evolui?

---

## 4. O Registry será compartilhado entre agentes?

Na teoria aparece este diagrama:

```
Prometheus OS

        │

        ▼

Shared Tool Registry

 ┌──────┼──────┐
 ▼      ▼      ▼

Mentor Editor Knowledge
```

**Pergunta:** esse Registry será implementado futuramente como um objeto único compartilhado por todos os agentes do Prometheus, ou cada agente manterá seu próprio Registry filtrando apenas as ferramentas que pode utilizar?

---

## 5. O Tool Registry substituirá também a lista `tools=[...]` enviada ao LLM?

Hoje o `LLMService` possui:

```python
tools = [
    CALCULATOR_TOOL
]
```

Mas agora estamos criando um catálogo centralizado.

**Pergunta:** futuramente o `LLMService` montará automaticamente essa lista a partir do Tool Registry, evitando duplicação entre o Registry e o `tools=[...]` da OpenAI?

---
## Minha impressão

Se entendi corretamente, a Aula 9 não é apenas sobre remover `if` e `elif`.

Ela introduz um padrão arquitetural muito mais profundo:

> **O restante do sistema deixa de conhecer implementações concretas e passa a depender de um catálogo de capacidades registradas.**

Isso me parece um dos primeiros passos para transformar o Prometheus em uma arquitetura baseada em plugins, onde novas ferramentas podem ser adicionadas sem modificar o código já existente. Se essa leitura estiver correta, acho que essa aula é tão importante quanto a Aula 8 em termos de arquitetura.

<h1 align="center">Respostas do Professor Principal</h1>
Segue as informações adicionais do professor:

Excelente conjunto de perguntas. O monitor está fazendo exatamente as perguntas que um arquiteto faria. Vou responder **pedagogicamente**, seguindo a evolução que estamos construindo no Projeto Prometheus, sem adiantar implementações que ainda não pertencem à aula.

---

# Resposta 1 — O Registry armazenará apenas instâncias ou também metadados?

**Resposta da aula:** **apenas as instâncias das ferramentas.**

Nesta etapa queremos ensinar apenas um conceito:

> o sistema deve descobrir ferramentas através de um catálogo.

Ou seja, algo semelhante a:

```python
TOOLS = {
    "calculator": CalculatorTool(),
    "calendar": CalendarTool(),
}
```

Ainda **não** estamos transformando o Registry em um catálogo completo de capacidades.

Entretanto, sua observação é excelente.

Arquiteturalmente, um Registry mais maduro realmente tende a armazenar:

* instância;
* schema;
* descrição;
* permissões;
* categoria;
* talvez versão.

Mas isso pertence a uma evolução futura da arquitetura.

O objetivo desta aula é manter o foco em apenas uma ideia.

---

# Resposta 2 — Quem cria as instâncias?

**Nesta aula, o próprio Registry.**

Ou seja:

```python
TOOLS = {
    "calculator": CalculatorTool()
}
```

Por quê?

Porque ainda estamos ensinando apenas:

> registrar ferramentas.

Introduzir um processo separado de criação (injeção de dependências, factories, containers de DI etc.) adicionaria outra abstração completamente diferente.

Primeiro aprendemos:

* registrar.

Depois aprenderemos:

* como essas ferramentas são criadas.

---

# Resposta 3 — O ToolManager continuará existindo?

**Sim.**

E existe um motivo arquitetural importante.

O Registry tem uma responsabilidade:

> armazenar ferramentas.

O ToolManager possui outra:

> coordenar sua execução.

O Registry responde perguntas como:

> "Existe uma ferramenta chamada calculator?"

O ToolManager responde:

> "Execute esta ferramenta."

São responsabilidades diferentes.

Pode parecer pouca diferença agora.

Mas imagine amanhã:

* logs;
* autorização;
* métricas;
* tratamento de exceções;
* timeout;
* retry.

Tudo isso pertence muito mais ao ToolManager do que ao Registry.

Ou seja:

o Registry **não orquestra**.

Ele apenas registra.

---

# Resposta 4 — O Registry será compartilhado?

**Sim.**

Essa é exatamente a ideia apresentada na aula.

Existe um catálogo único de capacidades do Prometheus.

Cada agente consulta esse catálogo.

Entretanto...

Isso **não significa** que todos possam usar tudo.

Quem controla isso não é o Registry.

É a arquitetura.

Por exemplo:

O Mentor pode ter acesso a:

* Calculator
* Search
* RAG

Enquanto o Editor utiliza:

* Markdown
* SVG
* Perplexity

Ambos consultam o mesmo catálogo.

Mas possuem conjuntos de permissões diferentes.

---

# Resposta 5 — O Registry substituirá o `tools=[...]`?

**Ainda não.**

Nesta aula, continuaremos registrando as ferramentas manualmente na chamada da API.

Ou seja:

```python
tools=[ ... ]
```

continua existindo.

Mas...

Essa pergunta mostra exatamente para onde a arquitetura está caminhando.

Num sistema maduro, realmente faz sentido que o próprio Registry forneça automaticamente os schemas registrados, eliminando duplicação.

Só que isso é um passo posterior.

Nesta etapa queremos primeiro separar responsabilidades.

Depois automatizaremos essa integração.

---

# Sobre a observação do monitor

A leitura dele está correta.

Na verdade, eu diria que ele captou o verdadeiro objetivo da Aula 9.

A maioria das pessoas olha para esta aula e pensa:

> "Ah, estamos removendo alguns `if`s."

Não.

Isso é apenas a consequência.

O que realmente estamos fazendo é substituir dependências concretas por um catálogo de capacidades.

É exatamente o primeiro passo para algo parecido com um sistema de plugins.

No futuro, adicionar uma ferramenta deverá significar algo próximo de:

1. criar a ferramenta;
2. registrá-la;
3. pronto.

Sem modificar o restante da arquitetura.

Esse princípio aparece em praticamente todos os frameworks modernos e é um dos motivos pelos quais eles conseguem crescer sem que o código principal precise ser reescrito continuamente.

---

### Uma observação para vocês dois (você e o monitor)

Tenho percebido uma evolução interessante nas perguntas. Nas primeiras aulas elas eram muito voltadas para **"como escrever esse código?"**. Agora elas passaram a ser **"qual responsabilidade pertence a cada componente?"**.

Essa mudança é exatamente o objetivo do Projeto Prometheus. A partir daqui, vocês começarão a enxergar padrões arquiteturais em praticamente qualquer framework ou sistema que estudarem. É essa mudança de perspectiva que diferencia quem apenas implementa funcionalidades de quem projeta arquiteturas que continuam saudáveis conforme crescem.

<h1 align="center">Observações finais do Monitor</h1>
<h3 align="center">(antes da prática)</h3>
Excelente. Agora sim temos toda a fundamentação da Aula 9.

E quero destacar uma frase do professor que considero a essência desta aula:

> **"O que realmente estamos fazendo é substituir dependências concretas por um catálogo de capacidades."**

Na minha opinião, essa frase merece entrar no material do curso. Ela resume perfeitamente o salto arquitetural.

Também gostei muito da observação final do professor. Ela confirma uma evolução que eu vinha percebendo nas nossas conversas: suas respostas deixaram de ser "como faço isso em Python?" e passaram a ser "quem deveria ser responsável por isso?". Essa mudança é exatamente a transição de desenvolvedor para arquiteto de software.

## O que implementaremos no Laboratório 9

Pelo que o professor definiu, o laboratório será bem focado. Nada de DI, factories ou plugins ainda. Apenas o nascimento do Registry.

Nossa arquitetura ficará assim:

```
                MentorAgent
                     │
                     ▼
               ToolManager
                     │
                     ▼
              ToolRegistry
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
 CalculatorTool()         SearchTool()
```

E as responsabilidades ficam muito claras:

|Componente|Responsabilidade|
|---|---|
|**MentorAgent**|Orquestrar o fluxo da conversa|
|**ToolManager**|Solicitar a execução de uma ferramenta|
|**ToolRegistry**|Descobrir e fornecer ferramentas registradas|
|**Tool**|Executar sua lógica específica|

Perceba como isso conversa com toda a filosofia do Prometheus: **cada camada faz uma única coisa**.

---

## Minha previsão

Acho que a implementação prática será extremamente pequena — provavelmente menos de 30 linhas de código.

Mas ela terá um impacto enorme.

Hoje temos:

```
if tool_name == "calculator":
    ...
elif tool_name == "search":
    ...
```

Depois da Aula 9, teremos algo próximo de:

```
tool = registry.get(tool_name)

return tool.execute(...)
```

Essa diferença parece pequena, mas muda completamente a escalabilidade da arquitetura.

---

Estou pronto para a parte prática.

Pode enviar o **Desafio Prático da Aula 9 (primeiro passo da implementação)**, e vamos construí-lo juntos, como temos feito em todos os laboratórios.