---
tags:
  - inteligenciaartificial
---

<aside align="center">
<h3>
A ordem das instruções também comunica significado.
</h3>
</aside>

---

## Uma pergunta

Imagine estes dois prompts.

### Prompt A

```text
Resuma este artigo.

O público é composto por executivos.

O texto está abaixo.

...

O resumo deve ter até 200 palavras.

Priorize riscos financeiros.
```

---

### Prompt B

```xml
<objective>
Resumir o artigo.
</objective>

<audience>
Executivos.
</audience>

<constraints>
Máximo de 200 palavras.
</constraints>

<focus>
Riscos financeiros.
</focus>

<context>

...

</context>
```

Ambos dizem exatamente a mesma coisa.

Então...

**Por que o segundo costuma gerar respostas mais consistentes?**

---

# A resposta curta

Porque o Transformer lê tudo de uma vez.

Mas...

Ele **não trata todas as partes do contexto como igualmente úteis**.

---

# Voltando ao Módulo 1

Lembra da Self-Attention?

Cada token pode prestar atenção em qualquer outro.

Isso continua sendo verdade.

Mas existe um detalhe importante.

A atenção responde à pergunta:

> **"Quais partes do contexto são mais relevantes para produzir o próximo token?"**

<aside align="center">
<h3>
Portanto, a organização do contexto influencia essas relações.
</h3>
</aside>

---

# Um erro comum

Muitas pessoas imaginam algo assim:

```text
Linha 1

↓

Linha 2

↓

Linha 3

↓

Resposta
```

Como se o modelo "lesse" em sequência.

Mas isso não acontece.

O Transformer recebe tudo praticamente ao mesmo tempo.

Então...

Por que a ordem importa?

---

# Organização Semântica

Imagine uma biblioteca.

Todos os livros estão disponíveis.

Mas existem duas possibilidades.

Biblioteca A:

```text
Economia

Física

Romances

Matemática

História

Misturados
```

Biblioteca B:

```text
Economia

↓

Macroeconomia

↓

Política Monetária

↓

Inflação
```

Os mesmos livros.

Organização completamente diferente.

Qual facilita encontrar informação?

A segunda.

---

# O Prompt também cria Hierarquias

Veja isto:

```xml
<context>

</context>

<objective>

</objective>

<constraints>

</constraints>
```

Sem perceber...

Você está dizendo ao modelo:

> Isto é contexto.

↓

>Agora vem o objetivo.

↓

>Agora vêm as restrições.

↓

>Agora vem o formato.

<h4>Você está criando uma árvore lógica.</h4>

---

# Engenharia Cognitiva

Pense no cérebro humano.

Se eu disser:

> Faça um resumo.

...

Ah...

É para um CEO.

...

Na verdade ignore aspectos jurídicos.

...

Máximo 100 palavras.

Você provavelmente terá que reorganizar mentalmente tudo.

Agora compare com:

Objetivo.

↓

Público.

↓

Restrições.

↓

Critérios.

Muito mais fácil.

---

# Carga Inferencial Temporal

Na aula passada criamos:

> Carga Inferencial.

Hoje refinaremos.

Existe também:

## Carga Inferencial Temporal

Definição:
<aside align="center">
<h3>
Quantidade de reorganização lógica que o modelo precisa realizar porque as informações relevantes foram apresentadas em uma ordem pouco natural para a tarefa.
</h3>
</aside>

---

# Um exemplo

Imagine este prompt.

```text
Faça uma tabela.

Não use Markdown.

Meu público são médicos.

A resposta deve citar estudos.

O texto está abaixo.

...

Explique também limitações.
```

Tudo funciona.

Mas o modelo precisa reorganizar mentalmente:

Quem é o público?

↓

Qual é a tarefa?

↓

Qual é o formato?

↓

Quais são as restrições?

---

Agora compare.

```xml
<context>

</context>

<objective>

</objective>

<audience>

</audience>

<output_format>

</output_format>

<constraints>

</constraints>
```

A reorganização praticamente desaparece.

---

# O Prompt como Pipeline

Talvez esta seja a melhor analogia.

Um bom prompt funciona como um pipeline de engenharia.

```text
Entrada

↓

Contexto

↓

Objetivo

↓

Restrições

↓

Critérios

↓

Formato

↓

Saída
```

Perceba.

Você está projetando um fluxo lógico.

---

# Por que eu quase sempre começo por `<context>`?

Você provavelmente percebeu isso no Projeto Prometheus.

Quase todos os seus prompts seguem algo parecido com:

```xml
<context>

</context>

<task>

</task>

<answer>

</answer>
```

Não é coincidência.

É intencional.

1. Primeiro eu forneço o universo.
2. Depois o problema.
3. Depois a resposta.

Essa ordem reduz reorganizações.

---

# Ordem não significa rigidez

Aqui existe um detalhe importante.

Não existe uma sequência universal.

O melhor fluxo depende da tarefa.

Por exemplo.

Um agente pode precisar receber primeiro:

```xml
<tools>

</tools>

<constraints>

</constraints>

<objective>

</objective>
```

Enquanto um tradutor talvez precise:

```xml
<context>

</context>

<source_language>

</source_language>

<target_language>

</target_language>
```

A arquitetura muda.

O princípio permanece.

---

# Um paralelo com Programação

Imagine isto.

```python
resultado = calcular(
    dados,
    parametros,
    restricoes
)
```

Agora compare com:

```python
resultado = calcular(
    restricoes,
    dados,
    parametros
)
```

Os mesmos argumentos.

Dependendo da API...

Uma ordem faz muito mais sentido que outra.

Prompt Engineering é parecido.

---

# O erro do "Prompt Frankenstein"

Você já deve ter visto prompts assim.

```text
Faça isso.

Ignore aquilo.

Ah...

Antes disso...

Também...

Depois...

Na verdade...

```

Eles costumam surgir após dezenas de edições.

O resultado?

Alta carga inferencial temporal.

---

# Um conceito importante

Vou introduzir uma palavra que usaremos muito.

## Coesão Estrutural

Definição:
<aside align="center">
<h3>
Grau em que informações relacionadas permanecem agrupadas, reduzindo reorganizações internas durante a interpretação do prompt.
</h3>
</aside>

---

# 📜 Princípio LVI

> **A eficácia de um prompt depende não apenas das informações presentes, mas também da arquitetura com que essas informações são organizadas.**

Observe.

Não falamos de "palavras mágicas".

Estamos falando de arquitetura.

---

# Conexão com Engenharia de Software

Lembra do SOLID?

Uma classe bem organizada é mais fácil de entender.

Mesmo contendo exatamente os mesmos métodos.

Aqui acontece algo semelhante.

Dois prompts podem conter exatamente as mesmas instruções.

Mas um possui melhor arquitetura.

---

# 📚 Leitura recomendada

Quero que leia um texto curto sobre **context engineering** (quando tiver tempo). Você perceberá que a comunidade está migrando gradualmente do termo "prompt engineering" para "context engineering", justamente porque o foco deixa de ser apenas escrever boas instruções e passa a ser **projetar todo o contexto da inferência**.

Guarde esse termo. Voltaremos a ele mais adiante.

---

# 🛠️ Desafio Prometheus M3 #003

## Parte 1

Explique:

> **Por que dois prompts contendo exatamente as mesmas informações ainda podem produzir resultados diferentes?**

Utilize os conceitos de:

- arquitetura da sequência;
    
- carga inferencial temporal;
    
- coesão estrutural.
    

---

## Parte 2

Imagine que você é responsável pelo assistente de IA de um grande escritório de advocacia.

Você percebe que, ao longo dos meses, vários advogados foram acrescentando novas instruções ao prompt principal.

Hoje ele possui quase 500 linhas, com observações espalhadas em posições aleatórias.

Como arquiteto de IA, explique:

1. quais problemas estruturais você espera encontrar;
    
2. como reorganizaria esse prompt;
    
3. por que essa reorganização pode melhorar a previsibilidade do sistema **sem alterar uma única capacidade do modelo**.

---
[[🛠️ Desafio M3 003]]

---
### Um pequeno spoiler da Aula 4

Na próxima aula, faremos algo que eu estava esperando desde o início do Projeto Prometheus.

Você vai descobrir que **prompts também possuem padrões de projeto**, assim como software possui _design patterns_.

E, a partir daí, começaremos a construir uma biblioteca de padrões reutilizáveis — algo que poderá acompanhar você por muitos anos como engenheiro de IA.