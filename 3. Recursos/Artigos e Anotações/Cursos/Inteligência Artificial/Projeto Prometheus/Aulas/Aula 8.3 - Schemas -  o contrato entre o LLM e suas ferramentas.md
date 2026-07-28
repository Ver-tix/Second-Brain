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
Até agora fizemos isso:

```python
tools=[
    {
        "type": "function",
        "name": "calculator",
        "description": "Realiza operações matemáticas."
    }
]
```

Isso já permite que o modelo pense:

> "Existe uma calculadora."

Mas existe um enorme problema.

---

# O grande problema

Imagine que o usuário escreva:

> Quanto é 25 vezes 8?

O modelo decide:

```
calculator
```

Ótimo.

Mas...

**Como ele passa as informações?**

Será assim?

```text
25 * 8
```

Ou assim?

```text
25,8,multiply
```

Ou assim?

```json
{
   "expression":"25*8"
}
```

Ou:

```json
{
   "operation":"*",
   "left":25,
   "right":8
}
```

Ou:

```json
{
   "a":25,
   "b":8
}
```

O Python não tem como adivinhar.

O modelo também não.

Os dois precisam falar exatamente o mesmo idioma.

---

# O que é um Schema?

Um Schema é simplesmente um contrato.

Ele diz:

> "Se você quiser usar esta ferramenta, envie os dados exatamente neste formato."

É literalmente uma interface.

Se você já estudou APIs REST...

é exatamente a mesma ideia.

---

# Analogia

Imagine um formulário bancário.

O banco não escreve:

> "Mande os dados da transferência."

Ele escreve:

```
Nome:

CPF:

Valor:

Banco:

Conta:
```

Isso é um schema.

---

Com ferramentas é igual.

Você diz ao modelo:

```
Para chamar calculator você deve fornecer:

primeiro número

segundo número

operação
```

Agora não existe mais ambiguidade.

---

# O Schema da Calculator

Nossa calculadora aceita:

```
Número A

Número B

Operação
```

Logo ela pode ser descrita assim:

```python
{
    "type": "function",

    "name": "calculator",

    "description":
        "Executa operações matemáticas.",

    "parameters": {

        "type": "object",

        "properties": {

            "a": {
                "type": "number"
            },

            "b": {
                "type": "number"
            },

            "operation": {

                "type": "string",

                "enum": [
                    "add",
                    "subtract",
                    "multiply",
                    "divide"
                ]
            }

        },

        "required": [
            "a",
            "b",
            "operation"
        ]

    }

}
```

Olhe para isso com calma.

Não é código.

É documentação.

---

# Vamos interpretar linha por linha

## parameters

```
parameters
```

Significa:

> "Estes são os argumentos da função."

---

## type

```
object
```

Ou seja:

O modelo deverá devolver um JSON.

---

## properties

Aqui definimos cada campo.

```
a
```

Número.

```
b
```

Número.

```
operation
```

Texto.

---

## enum

```
enum
```

É maravilhoso.

Significa:

> "Você só pode escolher um destes valores."

Então o modelo NÃO poderá inventar:

```
power
```

Nem

```
sqrt
```

Nem

```
raizQuadrada
```

Ele será obrigado a usar:

```
add

subtract

multiply

divide
```

Isso reduz muito as chances de erro.

---

## required

```
required
```

Significa:

Todos estes campos precisam existir.

O modelo não pode esquecer.

---

# O resultado

Agora o usuário pergunta:

> Quanto é 20 vezes 8?

O modelo devolve algo parecido com:

```json
{
    "a":20,
    "b":8,
    "operation":"multiply"
}
```

Olha a diferença.

Antes:

```
20 vezes 8
```

Agora:

```json
{
    "a":20,
    "b":8,
    "operation":"multiply"
}
```

O Python adora isso.

Não existe parsing.

Não existe regex.

Não existe split.

Não existe replace.

Só existe:

```
dados["a"]

dados["b"]

dados["operation"]
```

---

# O insight arquitetural

Esta é a mesma ideia que estudamos durante todo o Módulo 5.

**Cada componente deve ter um contrato claro.**

No Módulo 5 falávamos sobre contratos entre agentes e serviços.

Agora estamos aplicando exatamente o mesmo princípio entre:

```
LLM

↓

ToolManager
```

O LLM não precisa conhecer a implementação da calculadora.

Ele só precisa conhecer o contrato.

É uma aplicação direta do princípio de baixo acoplamento que vimos anteriormente.

---

# Um detalhe importante

Repare que **não estamos ensinando matemática ao modelo**.

Estamos ensinando:

> Como conversar com outro componente do sistema.

Essa é uma diferença enorme.

Tool Calling não é sobre cálculo.

É sobre **protocolos de comunicação**.

Por isso essa aula é tão importante.

---

# Ligação com o futuro do Prometheus

Até agora existe apenas:

```
Calculator
```

Daqui a algumas aulas teremos ferramentas como:

```
SearchTool

RAGTool

GitHubTool

PerplexityTool

EmailTool

CalendarTool

FileSystemTool
```

Todas seguirão exatamente o mesmo padrão:

```
Nome

Descrição

Schema (parameters)
```

O Orquestrador não precisará conhecer os detalhes de cada ferramenta. Ele apenas disponibilizará contratos bem definidos, e o modelo escolherá qual usar e preencherá seus argumentos conforme o schema.

---

# Laboratório — Aula 8.3

Agora vamos substituir de vez a abordagem provisória por uma implementação mais próxima da usada em sistemas reais.

### Parte 1 — Defina o schema completo

Atualize o registro da ferramenta `calculator` em `LLMService` para incluir:

- `parameters`
    
- `type: object`
    
- `properties`
    
- `required`
    
- `enum` para as operações (`add`, `subtract`, `multiply`, `divide`)
    

---

### Parte 2 — Refatore o `CalculatorTool`

Em vez de depender de uma expressão como:

```text
"2 + 3"
```

crie um método que receba explicitamente:

```python
calculate(
    a: float,
    b: float,
    operation: str
)
```

e faça o roteamento interno para `add`, `subtract`, `multiply` ou `divide`.

A responsabilidade de interpretar os argumentos passa a ser da ferramenta, não mais do chamador.

---

### Parte 3 — Atualize o `ToolManager`

Faça com que ele receba diretamente os três argumentos estruturados (`a`, `b` e `operation`) e apenas delegue a execução para o `CalculatorTool`.

Observe que o `ToolManager` deixa de fazer parsing de strings e passa a atuar como um orquestrador de ferramentas.

---

### Parte 4 — Reflexão arquitetural

Responda em poucas linhas:

> **Por que um schema torna o sistema mais robusto e escalável do que enviar uma expressão textual como `"2 + 3"` para a ferramenta?**

Essa resposta é mais importante que o código, porque ela demonstra que você compreendeu a transição de um protocolo informal para um contrato explícito — uma ideia que reaparecerá continuamente quando começarmos a construir o ecossistema multiagente do Prometheus.