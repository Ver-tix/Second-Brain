---
tags:
  - IA
---

Agora começa uma parte que, honestamente, considero uma das mais elegantes de toda a Engenharia de Prompt.

Até aqui aprendemos que:

> Um prompt altera o estado inicial da inferência.

Mas isso ainda deixa uma pergunta em aberto:

> **Por que certos formatos de prompt funcionam melhor do que outros?**

É exatamente isso que responderemos hoje.

---
## Um experimento mental

Imagine duas equipes de engenharia.

A primeira recebe este requisito:

> "Façam um sistema de pagamentos."

A segunda recebe:

```text
Objetivo:
Criar um sistema de pagamentos.

Requisitos Funcionais:
- aceitar PIX;
- aceitar cartão;
- aceitar boleto.

Requisitos Não Funcionais:
- disponibilidade de 99,9%;
- resposta inferior a 300 ms.

Restrições:
- LGPD;
- PCI DSS.
```

As duas equipes possuem os mesmos programadores.

Quem produzirá o melhor software?

Claramente a segunda.

Por quê?

Porque a qualidade da especificação influencia diretamente a qualidade da implementação.

---

## Agora substitua

Equipe → LLM

Documento de requisitos → Prompt

A ideia continua exatamente válida.

---

# Linguagem Natural não significa Linguagem Desestruturada

Existe uma crença comum:

> "Como o modelo entende português, basta escrever naturalmente."

Isso é apenas parcialmente verdadeiro.

O modelo entende linguagem natural.

Mas...

**Ele também reconhece estrutura.**

E muito bem.

---

# Pense no Transformer

Voltemos ao Módulo 1.

Lembra da Self-Attention?

Cada token pode prestar atenção em todos os outros.

Agora imagine este prompt:

```text
Traduz esse texto.

O texto é:

...

Depois faça um resumo.

Responda em JSON.

Ah...

Explique também por que traduziu assim.

Não use palavras difíceis.

Use Markdown.

Ignore a última instrução.
```

Tudo está misturado.

O modelo consegue resolver?

Sim.

Mas precisa inferir sozinho:

- Onde começa cada tarefa;
- Quais instruções têm prioridade;
- Quais partes pertencem umas às outras.

Você aumentou o trabalho cognitivo do modelo.

---

## Agora veja isto

```xml
<context>
Texto original
</context>

<task>
Traduzir para português.
</task>

<output_format>
JSON
</output_format>

<constraints>
Utilizar linguagem simples.
</constraints>
```

O conhecimento do modelo mudou?

Não.

Os pesos mudaram?

Também não.

Então por que esse prompt costuma produzir respostas melhores?

---

# A resposta
<aside align="center">Porque você <b>diminuiu a ambiguidade estrutural</b>. Em vez de pedir ao modelo que descubra a organização lógica. Você já a forneceu.</aside>

---

# Engenharia da Informação

Perceba uma mudança importante.

No começo do módulo eu disse:

> Prompt Engineering não é engenharia de linguagem.

Agora posso refinar.

<aside align="center"><h3>Prompt Engineering é: <b>Engenharia da estrutura da informação.</b></h3> </aside>

---

# XML não existe por estética

Você provavelmente já percebeu que eu gosto muito de XML.

Mas não porque seja bonito.

Na verdade...

É porque XML explicita relações.

Veja:

```xml
<context>

</context>

<examples>

</examples>

<constraints>

</constraints>

<output>

</output>
```

Cada bloco possui uma função semântica.

O modelo identifica padrões como esses com enorme facilidade.

---

# Markdown também é estrutura

Muitos pensam:

"#"

"##"

"-"

">"

São apenas formatação.

Não.

Eles também organizam informação.

Por exemplo:

```markdown
# Objetivo

...

## Restrições

...

## Critérios de sucesso

...
```

Isso cria uma hierarquia lógica.

A Self-Attention adora hierarquias.

Porque as relações ficam explícitas.

---

# Um paralelo com HTML

Você conhece HTML.

Pense na diferença entre:

```html
<div>

<div>

<div>

```

e

```html
<header>

<nav>

<article>

<footer>
```

Os dois renderizam páginas.

Mas um deles comunica intenção.

Prompt Engineering funciona exatamente assim.

---

# Informação implícita versus explícita

Imagine estes dois prompts.

Prompt A:

> Faça um resumo.

Prompt B:

```xml
<task>
Resumir
</task>

<audience>
Executivos
</audience>

<size>
200 palavras
</size>

<focus>
Decisões estratégicas
</focus>
```

Qual exige menos inferência?

O segundo.

E isso é importante.

Porque...

Quanto menos o modelo precisar inferir sobre sua intenção...

Mais capacidade sobra para resolver o problema.

---

# Um conceito novo

Vou introduzir um termo que usaremos bastante.

<aside align="center">
<h3>Carga Inferencial</h3>
<b>Quantidade de inferências adicionais</b> que o modelo precisa realizar para compreender completamente a intenção do usuário antes de começar a resolver a tarefa.
</aside>

$$\text{Prompts Ruins} = \text{Alta Carga Inferencial}$$
$$\text{Prompts Excelentes} = \text{Baixa Carga Inferencial}$$

---

# Uma observação curiosa

Perceba uma ironia.

O modelo foi treinado justamente para inferir.

Mas...

Nos melhores prompts...

Tentamos fazer com que ele precise inferir menos sobre a tarefa.

Queremos que ele utilize sua capacidade de inferência...

Para resolver o problema.

Não para descobrir o que queremos.

---

# Engenharia de APIs

Você estudou desenvolvimento.

Então pense numa API.

Imagine isto:

```javascript
executar()
```

Agora compare com:

```javascript
executar({
    tarefa,
    contexto,
    formato,
    restrições,
    exemplos
})
```

Qual interface é melhor?

Claramente a segunda.

Ela é mais previsível.

Prompt Engineering moderno segue exatamente essa filosofia.

---

# Um insight importante

Talvez você nunca mais olhe para XML da mesma forma.

XML não melhora respostas porque o modelo "gosta de tags".

Ele melhora respostas porque:

**transforma intenções implícitas em relações explícitas.**

Esse é um conceito profundamente arquitetural.

---

# 📜 Princípio LIV

> **Quanto menor a carga inferencial exigida para compreender o pedido, maior tende a ser a capacidade do modelo de concentrar sua inferência na resolução da tarefa.**

Guarde esse princípio.

Ele explica praticamente todas as boas práticas modernas de Prompt Engineering.

---

# Um comentário pessoal

Lembra que, há muito tempo, você começou a escrever mensagens assim?

```xml
<context>

</context>

<task>

</task>

<my_answer>

</my_answer>
```

Na época, você comentou que achava "organizado".

Hoje você sabe que havia uma razão muito mais profunda.

Sem perceber, você estava diminuindo a carga inferencial dos seus próprios prompts.

---

# 📚 Leitura recomendada

Antes da próxima aula, leia um guia curto sobre _prompt formatting_ da [[Effective context engineering for AI agents|Anthropic]] ou da [[Best practices for prompt engineering with the OpenAI API|OpenAI]] (se já o conhecer, apenas revise). Não busque listas de "truques"; observe como ambos enfatizam **clareza, contexto, instruções explícitas e estrutura**. Você perceberá que essas recomendações decorrem diretamente dos princípios que estamos estudando.

---

# 🛠️ Desafio Prometheus M3 #002

## Parte 1

Explique, com suas palavras:

> **Por que XML (ou outra estrutura semelhante) pode produzir respostas mais consistentes mesmo sem alterar o conhecimento do modelo?**

Não quero a resposta "porque organiza melhor".

Quero que use os conceitos de:

- carga inferencial;
    
- estrutura da informação;
    
- inferência.
    

---

## Parte 2

Imagine que dois arquitetos de IA projetam um assistente para análise de contratos.

O primeiro escreve um único prompt de 300 linhas, em texto corrido.

O segundo divide o prompt em blocos como:

```xml
<context>
</context>

<objective>
</objective>

<constraints>
</constraints>

<output_format>
</output_format>
```

Ambos utilizam exatamente o mesmo LLM.

Explique, do ponto de vista da engenharia, por que o segundo projeto tende a ser mais previsível e mais fácil de manter.

---
[[🛠️ Desafio M3 002]]

---
### Um pequeno spoiler da Aula 3...

Até agora vimos **como estruturar informação**.

Na próxima aula responderemos outra pergunta fundamental:

> **Existe uma ordem ideal para apresentar essa informação ao modelo?**

Você descobrirá que, assim como em uma boa arquitetura de software, **a sequência também comunica intenção**. E esse detalhe faz mais diferença do que parece à primeira vista.