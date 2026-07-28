---
tags:
  - IA
---
```xml
<question_set>
<question1>
### 1.

Para cada um dos pedidos abaixo, diga **quais ferramentas o agente utilizaria e em que ordem**.

**Pedido A**

> "Escreva um artigo para o Substack explicando a relação entre Branding e Estoicismo e gere uma imagem de capa."

---

**Pedido B**

> "Analise minha carteira de criptomoedas, calcule a distribuição percentual de cada ativo e salve o resultado em uma planilha." 

---

**Pedido C**

> "Marque uma reunião com João para terça às 14h e envie um e-mail avisando sobre o compromisso."
</question1>
<question2>
### 2.

Imagine que você tenha duas ferramentas:
Ferramenta A

Descrição:
"Calcula indicadores financeiros."

Ferramenta B

Descrição:
"Executa cálculos estatísticos em grandes conjuntos de dados."

Explique por que **a qualidade da descrição da ferramenta** influencia diretamente a capacidade do agente de tomar boas decisões.
</question2> 
</question_set>

<answer_set>
<answer1>
Pedido 1: RAG e, dependendo do LLM, uma ferramenta de geração de imagem
Pedido 2: calculadora estatística e Excel
Pedido 3: Google calendar e E-mail
</answer1>

<answer2>
Há dois motivos que quero destacar: 
1. **Carga inferencial**: uma descrição bem escrita, estruturada, hierarquizada, organizada e contextualizada reduz a carga inferencial do modelo de LLM, e o faz gerar respostas mais úteis e específicas para o contexto do usuário
2. **Shot Prompting**: o conceito de shot (few shot prompting) é de que a partir de exemplos, você consegue contextualizar melhor o modelo acerca dos resultados esperados e dos processos a serem seguido. Descrevendo com exemplos, você ajuda o LLM a entender qual ferramenta usar em determinados assuntos.
</answer2>
</answer_set>
```

---
# Feedback

Professor corrigindo. 😄

Antes da correção: **essa foi sua melhor resposta até agora no Módulo 5**.

Não porque você acertou tudo (ainda há alguns ajustes), mas porque percebi que você começou a raciocinar **arquiteturalmente**. Você deixou de responder apenas "qual ferramenta usar" e passou a justificar decisões usando conceitos como carga inferencial e few-shot prompting. Isso mostra que você está conectando módulos diferentes do curso, e esse é exatamente o tipo de evolução que eu esperava.

---

# Questão 1

## Pedido A

Você respondeu:

> RAG e, dependendo do LLM, uma ferramenta de geração de imagem.

### Correção

**Nota: 8,5/10**

Está correto, mas faltou pensar no fluxo completo.

Eu esperaria algo como:

```text
Usuário

↓

LLM interpreta

↓

RAG
(busca Branding + Estoicismo)

↓

LLM escreve o artigo

↓

Ferramenta de geração de imagem

↓

(opcional)
API do Substack

↓

Resposta
```

Observe um detalhe importante:

A ferramenta de geração de imagem **não depende do LLM**.

Ela depende do agente possuir essa ferramenta disponível.

Quem depende do LLM é a **decisão** de chamá-la.

---

## Pedido B

Você respondeu:

> Calculadora estatística e Excel.

### Correção

**Nota: 7/10**

Aqui faltou uma ferramenta importantíssima.

Você recebeu a instrução:

> "Analise minha carteira."

Como?

De onde vieram os dados?

O agente precisaria primeiro obter os ativos.

Dependendo da arquitetura:

- consultar uma API da Binance;
    
- consultar um banco de dados;
    
- ler uma planilha existente;
    
- consultar uma carteira.
    

Depois sim:

```text
Python

↓

Calcula percentuais

↓

Google Sheets

↓

Salvar
```

Outro detalhe.

Eu escolheria **Python** em vez de "calculadora estatística".

Por quê?

Porque distribuição percentual é uma operação simples.

Uma ferramenta estatística seria mais indicada para:

- regressão;
    
- ANOVA;
    
- testes estatísticos;
    
- clustering;
    
- PCA.
    

---

## Pedido C

Você respondeu:

> Google Calendar e E-mail.

### Correção

**Nota: 10/10**

Perfeito.

O fluxo seria exatamente esse.

Talvez:

```text
LLM

↓

Google Calendar

↓

Gmail

↓

LLM responde
```

Excelente.

---

# Questão 2

Essa foi a parte que mais gostei.

Você respondeu:

> Carga inferencial

e

> Few-shot Prompting

---

## Sobre carga inferencial

Isso me deixou feliz porque percebi que você está começando a reutilizar conceitos que estudamos lá atrás.

Você escreveu:

> Uma descrição bem escrita reduz a carga inferencial do modelo.

Concordo.

Só faria um pequeno refinamento.

A ferramenta não precisa necessariamente ser "hierarquizada".

O mais importante é:

- ser específica;
    
- descrever claramente quando deve ser usada;
    
- descrever claramente o que retorna.
    

Por exemplo.

Ruim:

```text
Ferramenta Financeira
```

Boa:

```text
Calcula:

• VPL

• TIR

• Fluxo de Caixa

• Payback

Não realiza projeções estatísticas.
```

Perceba que ela também diz **o que não faz**.

Isso ajuda muito o LLM.

---

## Sobre Few-shot

Aqui aconteceu uma pequena confusão.

Você escreveu:

> Descrevendo com exemplos...

Na verdade...

O few-shot normalmente serve para ensinar um comportamento.

Exemplo.

```text
Entrada

↓

Saída

Entrada

↓

Saída

Agora faça igual.
```

Mas, no contexto das ferramentas, **o principal fator não são exemplos**.

É a descrição semântica.

Ou seja.

```text
Nome

Descrição

Parâmetros

Quando utilizar

O que retorna
```

Isso costuma ser suficiente.

---

Então eu reescreveria sua resposta assim:

> Uma boa descrição reduz a ambiguidade. O LLM consegue compreender melhor o propósito de cada ferramenta e inferir qual delas resolve determinado problema. Em alguns casos, exemplos de uso (few-shot) também podem ajudar, mas a descrição clara e específica continua sendo o fator mais importante.

---

# Nota Final

|Questão|Nota|
|---|--:|
|Pedido A|8,5|
|Pedido B|7,0|
|Pedido C|10,0|
|Descrição das ferramentas|9,0|

## **Nota Final: 8,9/10**

Excelente desempenho.

---

# Um detalhe que você percebeu sem perceber

Quero mostrar algo interessante.

Há algumas semanas você me perguntou:

> "Então a API simplifica o HTTP e o SDK simplifica a API?"

Na época isso parecia uma dúvida isolada.

Hoje você respondeu naturalmente:

> Google Calendar

> Gmail

> Gerador de Imagem

Você já não pensa mais:

> "Vou fazer HTTP."

Você pensa:

> "Vou usar a ferramenta."

É exatamente essa abstração que SDKs e frameworks proporcionam. Você não deixou de usar HTTP; apenas passou a trabalhar em um nível mais alto da arquitetura.

---

# Professor para aluno

Vou te contar uma curiosidade sobre o que vem pela frente.

Estamos muito próximos de um ponto em que as aulas deixarão de ser apenas conceituais.

Até agora, estudamos:

- o que é um agente;
    
- memória;
    
- RAG;
    
- orquestração;
    
- ferramentas;
    
- seleção de ferramentas.
    

A próxima grande pergunta é:

> **"Como tudo isso conversa dentro do código?"**

E é aí que começaremos a entrar em um território que considero um dos mais divertidos da Engenharia de IA: transformar esses blocos arquiteturais em componentes de software. Você vai perceber que o código não surge do nada; ele é apenas a implementação das ideias que você já está aprendendo a organizar mentalmente.

Tenho a impressão de que esse será o momento em que muitas peças do quebra-cabeça finalmente vão se encaixar.