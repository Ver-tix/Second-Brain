---
tags:
  - IA
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
[[🛠 Desafio M5 006]]


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

---

# Epílogo
```text
Imagine este pedido:

> "Escreva um relatório semanal sobre Marketing para minha newsletter."

O loop poderia ser:
```

Eu diria mais.

**Eu gostaria que esse fosse um dos projetos centrais do Projeto Prometheus.**

Na verdade, acho que ele é perfeito para o seu perfil, porque une praticamente tudo o que você quer construir:

- IA
    
- Marketing
    
- Branding
    
- Newsletter
    
- Second Brain
    
- Automação
    
- Agentes
    
- RAG
    
- Python
    

E o melhor: **é um problema real da sua vida**, não um exercício artificial.

---

# Minha proposta

Vamos criar um projeto contínuo chamado:

# Projeto Atlas

### Agente Editor da Newsletter

(O nome pode ser outro. "Atlas" me veio à cabeça por ser o agente que "carrega" e organiza conhecimento.)

Objetivo:

> Toda semana produzir um rascunho de newsletter no estilo Morning Brew, utilizando notícias atuais + seu Second Brain.

---

# O fluxo seria algo assim

```text
                 Agendamento
                      │
                      ▼
              Início do Agent Loop
                      │
                      ▼
        Pesquisar notícias da semana
          (Perplexity/Search API)
                      │
                      ▼
Selecionar apenas notícias relevantes
                      │
                      ▼
Consultar o Second Brain
(RAG)
                      │
                      ▼
Relacionar notícias com:
• Branding
• Marketing
• IA
• Business
                      │
                      ▼
Escrever primeiro rascunho
                      │
                      ▼
Gerar sugestões de título
                      │
                      ▼
Gerar imagem de capa
                      │
                      ▼
Salvar como Markdown
ou enviar ao Substack
```

---

# Onde cada conceito entra?

Olha que bonito fica o nosso quebra-cabeça.

|Componente|Função|
|---|---|
|**Perplexity**|Buscar notícias recentes|
|**Second Brain**|Trazer seus conhecimentos e frameworks|
|**Chroma (ou outro banco vetorial)**|Fazer o Retrieval das notas|
|**LLM**|Escrever a newsletter|
|**Gerador de imagens**|Criar a capa|
|**Python**|Orquestrar algumas etapas e tratar dados|
|**Google Docs/Markdown/Substack**|Publicar ou salvar o resultado|

Percebe?

É literalmente um agente completo.

---

# E eu faria uma melhoria

Conhecendo você...

Eu faria o agente pensar.

Não apenas escrever.

Exemplo.

Notícia:

> OpenAI lança novo modelo.

O agente poderia pensar.

```text
Essa notícia tem relação com:

↓

Livro Marketing 5.0

↓

Capítulo sobre IA

↓

Suas notas sobre Agentes

↓

Paper "Attention Is All You Need"

↓

Curso Projeto Prometheus
```

A newsletter deixa de ser:

> "Aqui está uma notícia."

E passa a ser:

> "Aqui está uma notícia analisada através do seu conhecimento."

Isso é um diferencial enorme.

---

# E vou propor uma funcionalidade "Caio"

Você sempre gostou de conectar autores que discordam entre si.

Então imagine uma seção fixa da newsletter.

## Perspectivas

Exemplo.

Nova ferramenta de IA.

O agente consulta.

```text
Kotler

↓

Ries

↓

Weinberg

↓

Suas notas
```

Depois escreve.

> Sob a ótica de Kotler...

> Ries provavelmente discordaria porque...

> Weinberg enfatizaria...

Isso seria fantástico.

---

# E outra funcionalidade

Você comentou meses atrás que queria escrever no estilo Morning Brew.

Então podemos criar um agente especializado.

Não apenas:

```text
Escreva newsletter.
```

Mas:

```text
Escreva:

• abertura curta

↓

• notícia

↓

• análise

↓

• aplicação prática

↓

• insight final

↓

• CTA
```

Ou seja.

O agente aprende a arquitetura da sua newsletter.

---

# Sobre usar o Perplexity

Eu gosto bastante da ideia.

Principalmente porque ele é excelente para:

- buscar notícias;
    
- resumir acontecimentos;
    
- localizar fontes recentes.
    

Depois o RAG faz o papel que o Perplexity **não consegue fazer**:

> conectar tudo isso ao seu conhecimento pessoal.

Essa divisão de responsabilidades é muito elegante.

---

# E eu quero ir um passo além

Gostaria que esse fosse **o primeiro grande projeto do Projeto Prometheus**.

Não um exercício.

Um produto.

Ao final dele, você terá um agente que realmente produz algo útil para o seu negócio.

Depois, evoluiremos esse mesmo projeto em etapas, por exemplo:

- **Versão 1:** Busca notícias e gera um rascunho em Markdown.
    
- **Versão 2:** Consulta o Second Brain via RAG e enriquece a análise.
    
- **Versão 3:** Gera uma imagem de capa.
    
- **Versão 4:** Publica automaticamente no Substack (ou deixa um rascunho pronto para revisão).
    
- **Versão 5:** Adiciona memória para evitar repetir temas já abordados.
    
- **Versão 6:** Trabalha em conjunto com outros agentes especializados (pesquisador, editor, revisor e designer).
    

No final do curso, você não terá apenas aprendido conceitos. Você terá construído um sistema que realmente apoia um dos seus objetivos: produzir conteúdo de alta qualidade de forma consistente, aproveitando o patrimônio intelectual que você já acumulou no seu Second Brain.

E, sinceramente, acho que esse é exatamente o tipo de projeto que faz o Projeto Prometheus valer a pena: aprender Engenharia de IA construindo algo que continuará sendo útil muito depois de terminarmos as aulas.

---

# Epílogo 2
```text
Então, vamos lá: Chroma, Pinecone, etc. São ferramentas de Bancos Vetoriais, enquanto LangGraph e LangChain são ferrametnas de estado? 
```

Quase isso. Na verdade, você identificou uma divisão importante, mas há uma pequena correção que vai deixar seu modelo mental perfeito.

Eu faria a seguinte tabela:

|Categoria|Exemplos|Função|
|---|---|---|
|**Banco Vetorial**|Chroma, Pinecone, Weaviate, Qdrant, Milvus|Armazenar embeddings e realizar buscas semânticas (Retrieval).|
|**Framework de Agentes**|LangGraph, LangChain, OpenAI Agents SDK, CrewAI|Construir e coordenar agentes, ferramentas, memória, estado e fluxos.|

Agora vem o detalhe importante.

## O LangGraph **não é uma ferramenta de estado**.

Ele é um **framework para construir agentes baseados em grafos de estados**.

Ou seja, o estado é apenas um dos seus componentes mais importantes.

Pense assim:

```
LangGraph

├── Estado (State)
├── Nós (Nodes)
├── Transições
├── Loops
├── Memória
├── Ferramentas
└── Controle da execução
```

O estado é o "coração" do LangGraph, mas ele faz muito mais do que apenas armazená-lo.

---

## E o LangChain?

O LangChain nasceu antes.

Ele é mais voltado para fornecer componentes reutilizáveis.

Por exemplo:

- conectar LLMs;
- conectar RAGs;
- conectar bancos vetoriais;
- criar prompts;
- gerenciar memória;
- chamar ferramentas.

Ele é uma espécie de "caixa de peças".

Já o LangGraph nasceu porque as pessoas começaram a perceber:

> "Precisamos controlar melhor o fluxo e o estado dos agentes."

Então ele adicionou um modelo baseado em grafos.

---

## Uma analogia que acho que você vai lembrar

Imagine que estamos construindo uma cidade.

### Bancos Vetoriais

São como bibliotecas.

```
📚 Chroma

📚 Pinecone

📚 Weaviate
```

Eles guardam conhecimento.

---

### LangChain

É uma caixa de ferramentas.

```
🧰

LLMs

↓

RAG

↓

Prompts

↓

Ferramentas

↓

Memória
```

Ele ajuda você a montar um sistema.

---

### LangGraph

É o arquiteto da cidade.

Ele decide:

```
Comece aqui

↓

Depois vá para este módulo

↓

Se houver erro

↓

Volte

↓

Se terminar

↓

Finalize
```

Percebe?

Ele controla **o fluxo inteiro**.

---

# Onde entra o OpenAI Agents SDK?

Na mesma categoria do LangGraph.

Ele também é um framework para criar agentes.

Só que com uma filosofia diferente.

Ele abstrai muita coisa.

Você escreve menos código.

---

# Então nosso mapa fica assim

```
                    AGENTES
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
   LangGraph      OpenAI Agents SDK    CrewAI
        │
        ▼
Controla estado, memória, ferramentas,
loops e execução
```

Enquanto isso:

```
                 RAG
                  │
      ┌───────────┼────────────┐
      ▼           ▼            ▼
   Chroma     Pinecone     Qdrant
```

Esses apenas armazenam e recuperam conhecimento.

---

## E vou te dar um pequeno spoiler...

Acho que você vai gostar muito do **LangGraph**.

Sabe por quê?

Porque, conhecendo você, percebo que você pensa em **diagramas, arquitetura e fluxos**. Você gosta de visualizar sistemas antes de implementá-los.

O LangGraph faz exatamente isso.

Em vez de imaginar um programa como uma sequência linear de funções, você passa a enxergá-lo como um **mapa de estados e transições**, muito parecido com os fluxogramas que você já desenhou para os exercícios do Projeto Prometheus.

E há uma notícia boa: quando chegarmos nele, você não estará começando do zero. Sem perceber, você já aprendeu seus principais fundamentos:

- ✅ Estado
- ✅ Ferramentas
- ✅ Memória
- ✅ RAG
- ✅ Orquestrador
- ✅ Agent Loop

Na prática, o LangGraph será apenas a forma de **codificar** tudo isso que você já compreende conceitualmente. É justamente por isso que adiei sua apresentação até agora. Acho que, neste momento do curso, ele finalmente fará sentido para você.