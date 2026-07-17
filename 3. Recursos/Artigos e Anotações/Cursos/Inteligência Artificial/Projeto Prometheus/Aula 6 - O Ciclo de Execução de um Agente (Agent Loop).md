---
tags:
  - inteligenciaartificial
  - programação
---
> **Objetivo da aula**
> 
> Entender o que realmente acontece "por dentro" de um agente enquanto ele trabalha. Até agora estudamos as peças; hoje vamos estudar o **motor** que faz essas peças funcionarem juntas.

---

# O maior mito sobre agentes

Muita gente imagina que um agente funciona assim:

```text
Pergunta

↓

Pensou

↓

Resposta
```

Na realidade...

Quase nunca.

---

Um agente moderno normalmente funciona assim:

```text
Pergunta

↓

Pensar

↓

Usar ferramenta

↓

Pensar novamente

↓

Usar outra ferramenta

↓

Pensar novamente

↓

Responder
```

Observe.

Ele pensa várias vezes.

Não apenas uma.

---

# Isso tem um nome

Chamamos isso de:

> **Agent Loop**

Ou simplesmente:

> **Loop de raciocínio do agente.**

---

# Por que existe um loop?

Imagine seu futuro Prometheus.

Você pergunta:

> "Faça uma análise completa deste empreendimento."

Será que uma única decisão resolve tudo?

Não.

Ele precisará descobrir o que fazer primeiro.

---

Vamos acompanhar o raciocínio.

---

## Iteração 1

Usuário:

> Analise este empreendimento.

O agente pensa.

```text
Ainda não tenho informações.

↓

Preciso consultar o RAG.
```

Executa.

---

## Iteração 2

Agora ele possui:

- memorial descritivo;
    
- localização;
    
- padrão do imóvel.
    

Ele pensa novamente.

```text
Agora preciso calcular
a viabilidade financeira.
```

Executa Python.

---

## Iteração 3

Recebe os cálculos.

Agora pensa.

```text
Falta consultar
os índices urbanísticos.
```

Consulta uma API.

---

## Iteração 4

Recebe tudo.

Agora pensa.

```text
Tenho informação suficiente.

Posso escrever o relatório.
```

Só agora responde.

---

Observe.

O agente mudou de ideia várias vezes.

---

# O Loop é um ciclo

Visualmente.

```text
        Pensar
           │
           ▼
 Escolher ferramenta
           │
           ▼
 Executar ferramenta
           │
           ▼
 Receber resultado
           │
           ▼
Ainda falta algo?
      │          │
     Sim        Não
      │          │
      ▼          ▼
 Pensar      Responder
```

Esse diagrama é um dos mais importantes de todo o curso.

---

# Quem controla esse ciclo?

Lembra do Orquestrador?

Agora ele ganha protagonismo.

O Orquestrador controla perguntas como:

- Ainda falta alguma informação?
    
- O objetivo foi alcançado?
    
- Existe erro?
    
- Preciso tentar outra ferramenta?
    
- Já posso finalizar?
    

Ou seja, ele é quem mantém o loop vivo.

---

# O conceito de "Estado"

Para que esse loop funcione, o agente precisa saber **em que ponto está**.

Chamamos isso de **estado (state)**.

Imagine um jogo.

Quando você salva.

O jogo guarda:

- fase;
    
- vida;
    
- inventário;
    
- posição.
    

Isso é o estado.

---

O agente também possui um estado.

Por exemplo:

```yaml
Objetivo:
Gerar análise imobiliária

Etapa Atual:
Calculando fluxo de caixa

Ferramentas Utilizadas:
- RAG
- Python

Pendências:
Consultar legislação

Status:
Em execução
```

Sem esse estado...

O agente esqueceria onde parou.

---

# Por que isso será importante?

Porque daqui a algumas aulas conheceremos uma ferramenta chamada **LangGraph**.

E sabe qual é a principal ideia do LangGraph?

Gerenciar o **estado** de um agente.

Quando você aprender LangGraph, vai lembrar imediatamente desta aula.

---

# O agente pode errar?

Sim.

E isso muda completamente o comportamento dele.

Imagine.

O agente chama:

```text
API Financeira
```

Resposta:

```text
Erro 500
```

Um chatbot comum responderia:

> "Ocorreu um erro."

Um agente pode pensar.

```text
A API falhou.

↓

Existe outra ferramenta?

↓

Sim.

↓

Usar API Reserva.
```

Isso é autonomia.

---

# O loop também permite autocorreção

Outro exemplo.

O agente calcula um fluxo de caixa.

Resultado:

```text
Lucro

R$ -800 milhões
```

Ele pensa.

```text
Estranho.

Vou revisar
os parâmetros.
```

Executa novamente.

Veja a diferença.

O agente não apenas executa.

Ele avalia o resultado.

---

# Aplicando ao Prometheus

Imagine este pedido:

> "Escreva um relatório semanal sobre Marketing para minha newsletter."

O loop poderia ser:

```text
Objetivo

↓

Buscar conteúdos

↓

Ler Second Brain

↓

Ainda falta?

↓

Sim

↓

Pesquisar notícias recentes

↓

Ainda falta?

↓

Sim

↓

Organizar tópicos

↓

Ainda falta?

↓

Sim

↓

Escrever artigo

↓

Ainda falta?

↓

Não

↓

Fim
```

Esse "Ainda falta?" é o coração do Agent Loop.

---

# Uma analogia

Imagine um chef preparando um jantar.

Ele não faz tudo de uma vez.

Ele prova.

↓

Corrige sal.

↓

Prova novamente.

↓

Acrescenta tempero.

↓

Prova novamente.

↓

Serve.

Esse ciclo de experimentar e ajustar é exatamente o que um agente faz.

---

# Comparando um Workflow e um Agente

Essa comparação fecha um ciclo iniciado lá na Aula 2.

|Workflow|Agente|
|---|---|
|Caminho fixo|Caminho adaptável|
|Sequência pré-definida|Decide a próxima etapa|
|Não revisa decisões|Pode revisar decisões|
|Pouca autonomia|Alta autonomia|
|Fluxo linear|Loop de raciocínio|

Agora conseguimos entender, de forma prática, por que dizíamos que um agente não é apenas um workflow sofisticado.

---

# Uma antecipação importante

Você se lembra que me perguntou:

> "Quando vamos entrar no Python?"

A resposta está cada vez mais próxima.

Porque, para implementar esse loop, precisaremos de estruturas como:

- laços (`while`);
    
- funções;
    
- objetos;
    
- estados;
    
- chamadas de ferramentas.
    

Ou seja, o Python aparecerá quando houver uma necessidade arquitetural real, e não apenas para "escrever código".

---

# Desafio da Aula 6

Imagine que você está projetando o **Prometheus-Mentor**, seu agente de estudos.

O usuário faz o seguinte pedido:

> **"Quero aprender Black-Scholes. Explique o conceito, apresente um exemplo prático, proponha um exercício e depois corrija minha resposta."**

Responda às perguntas abaixo.

### 1.

Descreva o **Agent Loop** que esse agente executaria.

Escreva as etapas na ordem em que ocorreriam.

---

### 2.

Em quais momentos o agente precisaria:

- consultar o RAG;
    
- utilizar memória;
    
- tomar uma nova decisão antes de continuar?
    

---

### 3.

Imagine que, ao buscar informações sobre Black-Scholes, o Retrieval retorne poucos resultados.

Como um agente (e não um simples workflow) poderia reagir?

---

### 4.

Explique por que esse fluxo caracteriza um **agente**, e não apenas um workflow automatizado.

---

# Projeto Prometheus — Aviso Importante

Quero encerrar esta aula com uma novidade.

**Faltam poucas aulas para iniciarmos o primeiro projeto prático.**

Mas quero fazer diferente da maioria dos cursos.

Não vamos começar escrevendo código.

Vamos começar desenhando a arquitetura completa do **Prometheus-Mentor**.

Você já criou um monitor no ZCode com esse nome. Agora vamos projetá-lo como se fôssemos arquitetos de software:

- quais módulos ele terá;
    
- onde entra o RAG;
    
- onde entra a memória;
    
- quais ferramentas possuirá;
    
- qual será seu estado;
    
- como será seu Agent Loop;
    
- e só depois... transformaremos tudo isso em Python.
    

Na minha experiência, essa abordagem produz um aprendizado muito mais sólido. Quando finalmente abrirmos o editor de código, cada classe, função e arquivo terá um propósito claro, porque nascerá de uma arquitetura que você já compreende. E tenho a impressão de que esse será um dos momentos mais marcantes de todo o Projeto Prometheus.