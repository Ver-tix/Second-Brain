---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# Pergunta da Etapa 1
## Antes de codarmos...

Quero fazer uma pergunta (como de costume 😄).

Na sua opinião, **por que o professor criou uma classe chamada `CalculatorTool` em vez de simplesmente fazer uma função `def add(a, b):` solta no projeto?**

Não existe resposta perfeita. Quero entender seu raciocínio primeiro. Depois disso, implementamos a classe juntos, linha por linha.

```text
Separação de responsabilidades e organização: 1. o mentor_agent é responsável por entregar a resposta, e não calculá-la. 2. códigos com linhas demais ficaria bagunçado. Uma importante regra é: o que é bagunçado, é difícil de auditar, e o que é difícil de auditar é difícil de melhorar
```

Excelente resposta.

Na verdade, você citou **dois princípios fundamentais da arquitetura de software**.

### ✅ 1. Separação de Responsabilidades (SRP)

Você disse:

> "o mentor_agent é responsável por entregar a resposta, e não calculá-la."

Perfeito.

É exatamente a filosofia que vimos desde o início do módulo:

- `LLMService` conversa com o modelo;
- `ConversationMemory` gerencia memória;
- `PromptBuilder` monta prompts;
- `MentorAgent` coordena;
- **`CalculatorTool` faz contas.**

Cada componente tem um único motivo para mudar.

---

### ✅ 2. Organização e Auditabilidade

Você escreveu algo que gostei muito:

> "o que é bagunçado, é difícil de auditar, e o que é difícil de auditar é difícil de melhorar."

Isso é pensamento de engenharia.

Eu faria apenas uma pequena reformulação:

> **Quanto menor e mais especializado é um componente, mais fácil é entendê-lo, testá-lo, auditá-lo e evoluí-lo.**

É exatamente por isso que estamos quebrando o sistema em pequenos blocos.

---

## Existe ainda um terceiro motivo

É o que o professor quer que vocês comecem a enxergar nesta aula.

Imagine que amanhã queremos criar:

```
CalculatorTool
WeatherTool
SearchTool
PythonTool
DatabaseTool
EmailTool
```

Todas serão "ferramentas".

Se todas forem classes, o `ToolManager` poderá tratá-las de maneira uniforme.

Visualmente:

```
MentorAgent
      │
      ▼
ToolManager
      │
 ┌────┼────┬────┬────┐
 ▼    ▼    ▼    ▼    ▼
Calc Weather Search Python Email
```

Percebe? O agente nem precisa saber quais ferramentas existem. Ele conversa apenas com o `ToolManager`, que delega a execução para a ferramenta correta. Esse desacoplamento é justamente o objetivo arquitetural da aula.

---
---

# Pergunta da Etapa 2:
## Minha pergunta para você

Até agora, todos os métodos têm `self`.

Mas repare uma coisa...

Eles **não usam nenhum atributo da classe**.

Eles só recebem dois números e devolvem outro número.

Então fica a provocação:

> **Será que esses métodos realmente precisam de `self`? Ou poderiam ser `@staticmethod`, como fizemos no `PromptBuilder`?**

Não responda pensando em "o que funciona". Responda pensando em **arquitetura**. Essa é uma discussão muito interessante e tem uma resposta menos óbvia do que parece.

## Resposta

### por que eles poderiam ser `@staticmethod`?

Olhe para um método:

```
def add(self, a: float, b: float) -> float:
    return a + b
```

O que ele utiliza?

- ❌ Não usa `self.calculator`
- ❌ Não usa `self.memory`
- ❌ Não usa nenhum atributo do objeto

Ele apenas recebe dois números e devolve outro.

Então ele poderia perfeitamente ser:

```
@staticmethod
def add(a: float, b: float) -> float:
    return a + b
```

Assim como fizemos no `PromptBuilder`.

Até aqui, tudo certo.

---

# Então por que eu manteria `self`?

Porque eu não estou projetando **uma função matemática**.

Estou projetando **uma ferramenta**.

Essa diferença é enorme.

Hoje ela é assim:

```
CalculatorTool
```

Mas imagine o Laboratório 18.

Você decide que sua calculadora terá:

- precisão configurável;
- número máximo de casas decimais;
- registro de operações;
- estatísticas de uso;
- cache de resultados.

Agora ela passa a possuir estado.

Exemplo:

```
class CalculatorTool:

    def __init__(self):
        self.operations = 0
```

E então:

```
def add(self, a, b):

    self.operations += 1

    return a + b
```

Pronto.

Agora o método precisa de `self`.

---

# Ou imagine outra situação

Você resolve integrar uma biblioteca externa.

```
class CalculatorTool:

    def __init__(self):

        self.engine = MathEngine()
```

Agora:

```
def add(self, a, b):

    return self.engine.add(a, b)
```

Novamente:

`self` passa a fazer sentido.

---

# Arquiteturalmente, o objeto representa uma entidade

É aqui que mora a diferença.

No `PromptBuilder`, nós nunca esperamos guardar estado.

Ele é quase uma função sofisticada.

Entrada:

```
Pergunta
```

↓

Saída:

```
Prompt
```

Fim.

---

Já a `CalculatorTool` representa uma **ferramenta do sistema**.

E ferramentas normalmente são objetos.

Hoje ela é simples.

Amanhã pode não ser.

---

# Existe ainda um motivo de engenharia

Imagine:

```
tool_manager.calculator
```

Esse atributo representa uma instância.

Se todos os métodos fossem estáticos...

...você praticamente não precisaria da instância.

Você faria:

```
CalculatorTool.add(...)
```

Ou seja...

Você perderia a ideia de que existe **uma calculadora dentro do ToolManager**.

E essa ideia é muito importante para a arquitetura.

---

# Minha regra prática

Eu costumo seguir algo parecido com isto:

### É apenas uma transformação de dados?

↓

Uso `@staticmethod`.

Exemplo:

```
PromptBuilder
```

ou

```
TextFormatter
```

---

### Representa um componente do sistema?

↓

Uso uma instância (`self`).

Exemplo:

```
CalculatorTool
DatabaseTool
WeatherTool
EmailTool
PythonTool
```

Mesmo que, hoje, ela ainda não possua estado.

---

# O mais interessante

Você percebeu que essa pergunta **não é sobre Python**?

É sobre modelagem.

Você está respondendo:

> "Esta calculadora é apenas uma coleção de funções?"

ou

> "Ela é um objeto do meu sistema?"

Eu responderia:

> **Ela é um objeto.**

E por isso manteria `self`, mesmo que hoje ele ainda não seja utilizado.

---

## Uma última provocação

Olhe para esta linha:

```
self.calculator = CalculatorTool()
```

Ela comunica algo muito forte para quem lê o código:

> "O ToolManager possui uma calculadora."

Se todos os métodos fossem estáticos, essa linha provavelmente desapareceria e seria substituída por chamadas diretas à classe. O código continuaria funcionando, mas perderia um pouco da expressividade do modelo de objetos.

Essa é uma das grandes lições de arquitetura: **nem toda decisão é tomada pelo que o código precisa hoje; muitas são tomadas pelo que o modelo do domínio quer comunicar e pelo que ele poderá precisar amanhã.** E é justamente esse tipo de raciocínio que diferencia escrever código de projetar software.