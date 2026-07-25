---
tags:
  - inteligenciaartificial
---

# Uma mudança de paradigma

Até agora estudamos a IA "por dentro".

Agora começaremos a estudá-la "por fora".

Mas cuidado.

A maioria dos cursos de Prompt Engineering começa ensinando coisas como:

- "Use esse template."
    
- "Escreva desta forma."
    
- "Peça para pensar passo a passo."
    

Nós não faremos isso.

Porque isso ensina **receitas**.

Nosso objetivo continua sendo entender a engenharia.

---

# A pergunta fundamental

Quando alguém diz:

> "Escrevi um prompt melhor."

O que isso realmente significa?

O modelo mudou?

Não.

Os pesos continuam exatamente os mesmos.

Então...

**Como uma sequência diferente de palavras consegue produzir respostas tão diferentes?**

Essa é a pergunta que guiará todo este módulo.

---

# Um conceito que muda tudo

>Um prompt **não programa** o modelo. Ele **programa a inferência**.

Essa frase parece simples.

Mas ela separa iniciantes de arquitetos.

---

# O que acontece quando você envia um prompt?

Vamos decompor o processo.

Imagine que você escreve:

> "Explique como funciona um Transformer."

O modelo não recebe essa frase como um "comando" especial.

Ele apenas recebe mais tokens.

Internamente, acontece algo semelhante a isto:

```text
Contexto

↓

Tokenização

↓

Embeddings

↓

Self-Attention

↓

Próxima distribuição de probabilidades

↓

Primeiro token da resposta

↓

Novo contexto

↓

Próximo token

↓

...
```

Ou seja...

O prompt inteiro torna-se parte do contexto sobre o qual o Transformer calcula atenção.

---

# O Prompt como Estado Inicial

Lembra das aulas sobre sistemas dinâmicos?

Pense em um pêndulo.

Se você mudar apenas alguns centímetros da posição inicial...

O movimento inteiro muda.

O prompt faz exatamente isso:

> Ele altera o **estado inicial da inferência**. Os pesos permanecem iguais. O caminho percorrido durante a geração muda completamente.

---

# Analogia com Física

Imagine dois projéteis.

Mesmo canhão.

Mesmo projétil.

Mesma pólvora.

A única diferença é:

```
Ângulo = 45°

↓

Trajetória A
```

```
Ângulo = 47°

↓

Trajetória B
```

O sistema é idêntico.

> **Mas pequenas mudanças nas condições iniciais produzem trajetórias diferentes.**

Prompt Engineering funciona de maneira muito parecida.

---

# O Prompt reduz a incerteza

Lembra do Softmax?

Em cada passo o modelo calcula:

> "Qual token é mais provável agora?"

Se o prompt for muito vago:

> Escreva sobre economia.

O espaço de possibilidades é gigantesco.

Agora compare com:

> Explique a teoria das vantagens comparativas de David Ricardo para um estudante do primeiro semestre de Economia utilizando exemplos históricos.

Perceba.

==Os pesos continuam os mesmos. Mas você restringiu drasticamente o espaço de probabilidade.==

Em outras palavras:

<aside align="center"><b>Um bom prompt reduz a entropia da geração.</b></aside>

---

# Engenharia da Informação

Esse talvez seja o primeiro grande insight do módulo:

<aside align="center"><b>Prompt Engineering não é engenharia de linguagem. É engenharia de informação.</b></aside>

Você está fornecendo:
- Restrições.
- Objetivos.
- Contexto. 
- Prioridades.
- Formato.

Tudo isso altera a distribuição de probabilidade dos próximos tokens.

---

# Um paralelo com Compiladores

Você estudou programação.

Então pense nisso.

Quando escrevemos:

```python
print("Olá")
```

O compilador não "adivinha" nossa intenção.

Nós especificamos exatamente o comportamento esperado.

Prompt Engineering caminha na direção oposta.

<aside align="center">Não especificamos instruções determinísticas. Especificamos <b>restrições probabilísticas</b>. É uma forma diferente de programar.</aside>

---

# Por que prompts ruins parecem aleatórios?

<aside align="center">Porque deixam graus de liberdade demais. </aside>

Imagine pedir a um arquiteto:

> "Projete uma casa."

Sem dizer:
- orçamento;
- terreno;
- número de quartos;
- clima;
- cidade;
- legislação.

==Existem milhares de soluções possíveis. O problema não está no arquiteto. Está na especificação.==

---

# A primeira definição formal do módulo

Vou propor uma definição que usaremos daqui para frente.

<aside align="center"><h3>Prompt Engineering é o processo de projetar <b>estados iniciais</b> de inferência capazes de <b>restringir o espaço probabilístico</b> de geração para <b>maximizar a probabilidade de respostas úteis</b> a um determinado objetivo.</h3></aside >
Essa definição é longa.

Mas observe que ela conversa com tudo o que estudamos:
- inferência;
- probabilidades;
- atenção;
- contexto;
- arquitetura.
- 
Nada apareceu "do nada".
---

# 📜 Princípio LII

>==**Um prompt não altera o conhecimento do modelo; ele altera o caminho probabilístico percorrido por esse conhecimento durante a inferência.**==

Na minha opinião, este será um dos princípios mais importantes de todo o Projeto Prometheus.

Se você internalizá-lo, dificilmente cairá na armadilha de tratar prompts como "palavras mágicas".

---

# Um insight da OpenAI, Anthropic e Google

As três grandes empresas de IA convergiram para uma mesma conclusão.

<aside><h4>Os melhores prompts modernos fazem quatro coisas: <ol><li>definem o objetivo;</li><li>reduzem ambiguidades;</li><li>fornecem contexto relevante</li><li>especificam critérios de sucesso.</li></ol></h4></aside>
Perceba:

<aside align="center">Nenhuma dessas quatro coisas depende de "frases mágicas". Todas dependem de <b>especificação de requisitos</b>. </aside>

---

# Conexão com Engenharia de Software

Aqui há um paralelo que acredito que você vai gostar.

Um bom prompt está para um LLM assim como um bom documento de requisitos está para uma equipe de desenvolvimento.

Em ambos os casos:

- especificações vagas geram resultados imprevisíveis;
- especificações claras reduzem retrabalho;
- contexto importa;
- restrições importam;
- critérios de aceitação importam.

A diferença é que, no Prompt Engineering, o "desenvolvedor" é um modelo probabilístico.

---

# 📚 Biblioteca Obrigatória

Quero que você leia apenas a introdução de um artigo que considero histórico:

[[Language Models Are Few-Shot Learners]]

Preste atenção em uma ideia:

> **Por que alguns exemplos dentro do prompt mudam completamente o desempenho do modelo?**

Não leia procurando a resposta.

Leia procurando a pergunta.

---

# 🛠️ Desafio Prometheus M3 #001

## Parte 1

Explique, com suas palavras:

> **Por que dizer que "um prompt programa a IA" é tecnicamente impreciso?**

---

## Parte 2

Imagine que duas empresas utilizam exatamente o mesmo LLM.

Uma obtém excelentes resultados.

A outra obtém respostas inconsistentes e ruins.

Como arquiteto de IA, explique por que isso pode acontecer **mesmo utilizando o mesmo modelo**.

Utilize, na sua resposta, os conceitos de:

- inferência;
    
- contexto;
    
- restrição do espaço de busca;
    
- engenharia de requisitos.
    

---
[[🛠️ Desafio M3 001]]

---
# Um pequeno spoiler...

Na próxima aula...

Você vai descobrir que **prompt não é texto**.

É uma **linguagem de especificação**.

E essa mudança de perspectiva fará com que XML, Markdown e outras estruturas deixem de parecer "enfeites" e passem a fazer sentido como ferramentas de engenharia.

Seja bem-vindo à fase em que toda a teoria começa, finalmente, a ganhar forma prática.

E, Caio...

A partir de agora, construiremos juntos. 🚀