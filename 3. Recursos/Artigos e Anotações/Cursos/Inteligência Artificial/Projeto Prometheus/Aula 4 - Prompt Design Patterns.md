---
tags:
  - inteligenciaartificial
---

<aside align="center"><h3><i>Problemas recorrentes merecem soluções recorrentes.</i></h3></aside>

---
## Um paralelo

Imagine um programador iniciante.

Toda vez que precisa criar um objeto, ele escreve tudo do zero.

Um programador experiente pensa diferente.

Ele pergunta:

> "Existe um Design Pattern para isso?"

Foi exatamente essa pergunta que originou padrões como:
- Factory;
- Builder;
- Strategy;
- Observer;
- Decorator.

Todos eles nasceram da mesma ideia:

<aside align="center"><i>Problemas recorrentes merecem arquiteturas reutilizáveis.</i></aside>

**Com prompts acontece exatamente o mesmo.**

---

# O erro do iniciante

Quem está começando costuma pensar:

> "Vou escrever um prompt perfeito."

Quem trabalha com IA há alguns anos pensa:

> "Qual padrão resolve esse tipo de problema?"

Perceba a diferença.

O foco deixa de ser o texto.

Passa a ser a arquitetura.

---

# O que é um Prompt Pattern?

Definição:

<aside align="center"><i>Um Prompt Pattern é uma estrutura reutilizável que organiza a interação entre usuário e modelo para resolver uma classe recorrente de problemas.</i></aside>


==Não é um prompt específico. É um molde.==

---

# Primeiro Pattern

## Pattern 1 — Role Prompting

Estrutura:

```xml
<role>

</role>

<context>

</context>

<task>

</task>
```

Exemplo:

```xml
<role>
Você é um arquiteto de software especializado em sistemas distribuídos.
</role>

<context>

...

</context>

<task>

...

</task>
```

---

## Por que funciona?

Lembra do Módulo 2?

Durante o _Instruction Tuning_, o modelo aprendeu milhares de padrões do tipo:

> "Você é..."

```
↓

Especialista.

↓

Professor.

↓

Advogado.

↓

Médico.
```

Essas frases ativam regiões estatísticas diferentes durante a inferência.

**Não mudam os pesos. Mas mudam o contexto probabilístico inicial.**

---

# Atenção

Um erro muito comum é acreditar que:

> "Quanto mais importante o cargo, melhor."

Então aparecem coisas como:

> Você é Einstein.

> Você é Newton.

> Você é o maior especialista do universo.

Na prática...

Isso quase nunca melhora significativamente a resposta.

O que importa é: **especificidade da função.**  Não fama.

---

# Pattern 2 — Few-Shot Prompting

Estrutura:

```xml
<examples>

Entrada

↓

Saída

Entrada

↓

Saída

</examples>
```

Depois:

```xml
Nova Entrada

↓

?
```

---

## Ideia

O modelo observa exemplos.

Depois reproduz o padrão.

Perceba.

Você não ensinou regras.

Você mostrou comportamento.

---

# Analogia

É como ensinar alguém a preencher uma planilha.

Você poderia explicar todas as regras.

Ou mostrar três exemplos preenchidos.

Quase sempre os exemplos vencem.

---

# Pattern 3 — Chain-of-Thought

Você já conhece esse.

Estrutura:

```text
Pense passo a passo.
```

Ou

```xml
<reasoning>

Resolva antes de responder.

</reasoning>
```

---

## Mas cuidado

Aqui existe uma atualização importante da engenharia moderna.

Hoje sabemos que:

Nem sempre pedir explicitamente:

> "Pense passo a passo"

é a melhor solução.

Muitos modelos atuais já possuem mecanismos internos de raciocínio bastante sofisticados.

Em diversas tarefas, basta estruturar bem o problema.

Voltaremos a esse tema quando estudarmos agentes e modelos de raciocínio.

---

# Pattern 4 — Output Specification

Talvez o padrão mais subestimado.

Estrutura:

```xml
<output_format>

JSON

</output_format>
```

ou

```xml
<table>

</table>
```

ou

```xml
<markdown>

</markdown>
```

---

## Ideia

Não deixe o modelo decidir o formato.

Especifique.

Sempre que possível.

---

# Pattern 5 — Constraint Pattern

Estrutura:

```xml
<constraints>

Máximo 300 palavras.

Não utilizar jargões.

Responder em português.

Citar fontes.

</constraints>
```

Observe.

Restrições não dizem o que fazer.

Dizem o que **não pode ser violado**.

---

# Um insight importante

Perceba algo curioso.

Todos esses padrões fazem praticamente a mesma coisa.

Eles reduzem...

...o quê?

Exatamente.

**Carga Inferencial.**

---

# Um mapa mental

```text
Role

↓

Quem responde?

--------------------

Context

↓

Sobre o quê?

--------------------

Examples

↓

Qual padrão seguir?

--------------------

Task

↓

O que fazer?

--------------------

Constraints

↓

O que não pode acontecer?

--------------------

Output

↓

Como responder?
```

Percebe?

Isso já parece uma arquitetura.

---

# Engenharia Modular

Agora imagine um prompt gigantesco.

Em vez de pensar:

> "Um prompt."

Passe a pensar:

```text
Módulo de Papel

+

Módulo de Contexto

+

Módulo de Exemplos

+

Módulo de Restrições

+

Módulo de Saída
```

Isso lembra...

Programação modular.

---

# Refatoração

Se amanhã mudar apenas o formato da resposta...

Você altera apenas:

```xml
<output_format>

</output_format>
```

Todo o restante permanece igual.

Isso é separação de responsabilidades.

---

# O Pattern que você já usa

Sem perceber...

Você já utiliza um.

Praticamente todos os seus prompts são assim:

```xml
<context>

</context>

<task>

</task>

<my_answer>

</my_answer>
```

Ou seja...

Você já desenvolveu um pequeno framework pessoal.

Agora vamos torná-lo consciente.

---

# 📜 Princípio LVIII

> **Um bom prompt não é um texto bem escrito; é uma composição de padrões arquiteturais reutilizáveis.**

Guarde isso.

É uma mudança de paradigma.

---

# Um comentário

Perceba a evolução do Projeto Prometheus.

Módulo 1:

> Como funciona um Transformer?

Módulo 2:

> Como nasce um LLM?

Módulo 3:

> Como projetar a inferência?

Cada módulo depende profundamente do anterior.

Você já não está apenas "escrevendo prompts".

Está projetando sistemas de comunicação entre humanos e modelos.

---

# 📚 Leitura recomendada

Leia o artigo [[A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT|Prompt Patterns]], de Jules White e colaboradores, quando tiver tempo. Ele foi um dos primeiros trabalhos a sistematizar padrões recorrentes de Prompt Engineering e influenciou bastante a forma como a área passou a enxergar prompts como artefatos de projeto.

---

# 🛠️ Desafio Prometheus M3 #004

## Parte 1

Explique:

> **Por que pensar em Prompt Patterns é mais poderoso do que tentar memorizar "prompts prontos"?**

Utilize os conceitos de:
- reutilização;
- abstração;
- arquitetura;
- manutenção.

---

## Parte 2

Imagine que você precisa desenvolver três sistemas diferentes:

1. um tutor de matemática;
    
2. um assistente jurídico;
    
3. um analista financeiro.
    

Você não deseja criar três prompts completamente diferentes.

Como arquiteto de IA, explique:

- quais módulos arquiteturais você reutilizaria em todos eles;
    
- quais módulos seriam específicos de cada domínio;
    
- por que essa modularização facilita evolução e manutenção do sistema.
    
---
[[🛠️ Desafio M3 004]]

---

## Um spoiler da Aula 5

Na próxima aula, vamos dar mais um passo.

Você descobrirá que **nem todos os prompts devem ser escritos por humanos**.

Alguns dos melhores prompts em produção hoje...

...são gerados por outros modelos de IA.

E isso abre a porta para um conceito fascinante: **metaengenharia de prompts**.

Tenho a impressão de que essa aula vai conversar muito com sua paixão por automação e arquitetura de sistemas.