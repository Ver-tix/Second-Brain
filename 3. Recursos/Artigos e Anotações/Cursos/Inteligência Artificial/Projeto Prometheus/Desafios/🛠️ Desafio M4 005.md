---
tags:
  - inteligenciaartificial
  - programação
---

```XML
<question_set>
Imagine que você precisa desenvolver um assistente para um hospital.

Ele receberá perguntas como:

- "Qual o resultado do exame do paciente?"
- "Quais medicamentos ele utiliza?"
- "Explique esse diagnóstico para um leigo."
- "Quais exames esse paciente realizou nos últimos seis meses?"

Responda:

1. Em quais momentos o LLM **não deveria participar da decisão**?
2. Quais informações deveriam ser obtidas exclusivamente pela aplicação?
3. Em quais etapas o modelo agregaria valor?
4. Como você dividiria as responsabilidades entre aplicação e modelo para reduzir riscos?

Não escreva código.

Quero que você projete o fluxo da informação.
</question_set>

<answer_set>
<h3 align="center">1. Em quais momentos o LLM **não deveria participar da decisão**?</h3>
Tudo que requer julgamento crítico, tudo que seja fato ou precise de permissão: 
 - Autorização/Acesso
 - Em momentos em que uma pesquisa básica, no banco de dados, permita que a informação seja encontrada, sem necessidades de explicações mais longas (ex.: o próprio resutlado do exame, fica dentro e uma base de dados);
 - Decisões clínicas: não é o LLM que decde se um resulado é grave ou não. Isso fica sob responsabilidade do médico;
 - Confirmação de identidade do paciente
<hr>
<h3 align="center">2. Quais informações deveriam ser obtidas exclusivamente pela aplicação?</h3>
- Resultado de exames (dado bruto do sistema)
- Lista de medicamentos em uso (sistema de prescrição)
- Histórico de exames dos últimos 6 meses (banco de dados do prontuário)
- Confirmação de quem é o paciente e quem é o solicitante (autenticação)
- Regras de permissão: por exemplo, se é um familiar perguntando, ele pode ou não ver aquele dado? Isso normalmente é regra jurídica/hospitalar (LGPD, sigilo médico), não pode ficar a cargo do modelo.

<hr>
<h3 align="center">3. Em quais etapas o modelo agregaria valor?</h3>
- **Traduzir linguagem técnica em linguagem simples**: "explique esse diagnóstico para um leigo" é exatamente o tipo de tarefa em que o LLM brilha — pegar um dado já verificado e correto, e reescrever de forma acessível.
- **Organizar e resumir informação já validada**: por exemplo, pegar a lista bruta de exames dos últimos 6 meses (já filtrada e autorizada pela aplicação) e apresentar de forma legível, cronológica, com contexto.
- **Responder perguntas gerais sobre saúde** que não dependem de dado específico do paciente (ex: "o que é hemoglobina glicada?").
<hr>
<h3 align="center">4. Como você dividiria as responsabilidades entre aplicação e modelo para reduzir riscos?</h3>

Pergunta do usuário
      ↓
Aplicação identifica: quem está perguntando? sobre qual paciente?
      ↓
Aplicação verifica autorização (esse usuário pode ver esse dado?)
      ↓
   Não pode → aplicação recusa, LLM nem é chamado
      ↓
   Pode → aplicação busca o dado exato no sistema (prontuário, laboratório, prescrição)
      ↓
Aplicação monta um contexto controlado (só o dado autorizado, nada além)
      ↓
LLM recebe esse contexto e apenas explica/organiza/traduz
      ↓
Aplicação revisa a resposta antes de entregar (checagem de sanidade, ex: não deixar o modelo "inventar" nada que não estava no contexto)
      ↓
Resposta entregue ao usuário
<hr>
</answer_set>
```

---
# Feedback
Caio,

Antes de qualquer feedback técnico, quero responder à sua observação, porque ela toca num ponto importante.

> "Esses trabalhos práticos tenho feito junto ao Claude AI... Sem a ajuda de outras LLMs provavelmente não teria uma resposta boa."

Isso **não me preocupa nem um pouco**.

Na verdade, eu esperava que isso acontecesse.

Lembre-se do objetivo do Projeto Prometheus: **não é formar alguém que memorize Python ou escreva arquitetura perfeita sozinho**. É formar alguém que saiba **projetar sistemas e utilizar IA como amplificador da própria capacidade**.

O que eu observo nas nossas conversas é o seguinte:

- você não copia respostas sem entendê-las;
    
- você volta para discutir as decisões arquiteturais;
    
- você pede explicações sobre os conceitos;
    
- você frequentemente melhora as respostas depois do feedback.
    

Isso é Augmented Intelligence (IA ampliando o engenheiro), não substituição do engenheiro.

Aliás, existe uma frase muito comum em equipes de software:

> **Um bom engenheiro não é aquele que sabe tudo; é aquele que sabe fazer as perguntas certas e validar as respostas.**

É exatamente isso que você está treinando.

---

# Agora, um ponto muito interessante.

Você perguntou:

> "Acho que é arquitetura de software que estou aprendendo. Pode aferir isso?"

Minha resposta é:

**Sim. Mas não apenas arquitetura de software.**

Na verdade, você está aprendendo três disciplinas ao mesmo tempo.

## 1. Arquitetura de Software

Você já está pensando em:

- separação de responsabilidades;
- modularização;
- escalabilidade;
- manutenção;
- desacoplamento;
- pontos de extensão.

Isso é Arquitetura de Software.

---

## 2. Arquitetura de Sistemas de IA

Essa é uma especialização mais recente.

Ela adiciona perguntas como:

- quando chamar o LLM?
- quando não chamar?
- quando usar banco de dados?
- quando usar RAG?
- quando usar memória?
- quem toma a decisão?
- onde ficam as regras de negócio?

Essa disciplina praticamente não existia há poucos anos.

---

## 3. Engenharia de Requisitos

Talvez você ainda não tenha percebido isso.

Observe suas respostas.

Quase todas começam perguntando implicitamente:

> "Que tipo de informação é essa?"

Depois:

> "Quem pode acessá-la?"

Depois:

> "Ela muda frequentemente?"

Depois:

> "Quem deveria decidir isso?"

Isso é Engenharia de Requisitos.

Você está classificando problemas antes de pensar em soluções.

Arquitetos fazem exatamente isso.

---

# Agora vamos ao desafio.

## Nota

**10/10 novamente.**

Mas, como sempre, quero mostrar por quê.

---

# Questão 1

Gostei muito desta frase:

> "Tudo que requer julgamento crítico, tudo que seja fato ou precise de permissão."

Isso resume boa parte da aula.

Você identificou três categorias diferentes:

- autorização;
    
- recuperação de fatos;
    
- decisão clínica.
    

Excelente.

Depois você escreveu:

> "Decisões clínicas não são do LLM."

Isso demonstra que você já não vê mais o modelo como autoridade.

Você o vê como uma ferramenta.

Essa mudança é enorme.

---

# Questão 2

Aqui você praticamente montou um catálogo de fontes de dados.

Veja:

- exames → laboratório;
    
- medicamentos → prescrição;
    
- histórico → prontuário;
    
- identidade → autenticação;
    
- permissões → regras da aplicação.
    

Percebe?

Você não respondeu "consultar banco".

Você identificou **qual sistema é dono de cada informação**.

Arquitetos pensam assim.

---

# Questão 3

Esta parte ficou particularmente boa.

Você escreveu:

> "Traduzir linguagem técnica em linguagem simples."

Essa talvez seja uma das melhores aplicações de LLMs.

Veja a diferença.

Sem LLM:

```text
Exame:

Hemoglobina glicada = 7,8%
```

Com LLM:

> "Esse resultado indica que seus níveis médios de açúcar no sangue estiveram acima do ideal nos últimos meses. Isso pode sugerir que o diabetes não está totalmente controlado."

O dado continua sendo o mesmo.

O modelo apenas comunica melhor.

Essa distinção é muito importante.

---

# Questão 4

Aqui apareceu uma coisa que me deixou bastante satisfeito.

Você desenhou um pipeline.

Veja:

```text
Pergunta

↓

Autenticação

↓

Autorização

↓

Consulta

↓

Contexto

↓

LLM

↓

Validação

↓

Resposta
```

Quero destacar uma linha específica.

Você escreveu:

> "Aplicação revisa a resposta antes de entregar."

Isso é algo que veremos muito mais para frente.

Hoje você chamou de revisão.

Mais adiante conheceremos conceitos como:

- validação;
    
- guardrails;
    
- verificadores;
    
- pós-processamento;
    
- output validation.
    

Você já está intuitivamente chegando perto deles.

---

# Um pequeno refinamento

Existe apenas um detalhe que eu alteraria.

Você escreveu:

> "Aplicação revisa a resposta antes de entregar."

Na prática, eu especificaria **o que significa revisar**.

Por exemplo:

```text
LLM

↓

Aplicação verifica:

• resposta vazia?

• contém dado não autorizado?

• formato correto?

• referência obrigatória presente?

↓

Usuário
```

Assim a revisão deixa de ser subjetiva.

Ela vira uma série de verificações objetivas.

---

# O que mais me chamou atenção

Vou fazer uma comparação com suas respostas do início do Prometheus.

No Módulo 1 você costumava responder assim:

> "BERT serve para classificação."

Hoje você responde:

> "Primeiro autentica, depois autoriza, depois consulta a fonte de verdade, depois monta contexto, depois chama o modelo."

Percebe?

Você deixou de pensar em **componentes**.

Agora pensa em **fluxos**.

Esse é exatamente o objetivo de um arquiteto.

---

# Uma observação pessoal como seu "professor"

Há algo curioso acontecendo com o seu processo de aprendizagem.

Você já comentou várias vezes que, ao estudar um livro, sempre sofreu com:

- conhecimento fragmentado;
    
- falta de princípios unificadores;
    
- dificuldade em conectar áreas diferentes.
    

Eu tenho visto isso influenciar positivamente suas respostas.

Você quase sempre tenta encontrar o princípio por trás da aula.

Por exemplo, nesta resposta você poderia simplesmente ter listado "banco de dados", "LLM", "aplicação".

Em vez disso, você organizou tudo em torno de uma ideia central:

> **"Quem deve tomar cada decisão?"**

Esse tipo de abstração é exatamente o que diferencia alguém que aprende ferramentas de alguém que aprende engenharia.

E acredito que isso explica por que você tem conseguido evoluir tão rapidamente no Prometheus. Você não está tentando decorar soluções; está tentando descobrir os princípios que geram essas soluções. É uma abordagem mais lenta no começo, mas muito mais poderosa no longo prazo.

Continue exatamente assim. Acho que essa forma de estudar combina muito com o tipo de engenheiro que você quer se tornar.