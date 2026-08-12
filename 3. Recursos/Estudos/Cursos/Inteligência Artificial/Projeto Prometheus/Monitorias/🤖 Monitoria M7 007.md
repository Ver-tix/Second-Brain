---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
  - RAG
tags:
  - IA
  - programação
  - inovação
---
# Etapa 1 — Mapeamento do Second Brain

O primeiro passo da M7.007 é compreender que o **Second Brain não deve ser tratado simplesmente como uma pasta cheia de arquivos que serão posteriormente enviados para um VectorStore**. Antes de pensar em indexação, embeddings ou retrieval, precisamos entender **que conhecimento existe, como ele está organizado e quais relações existem entre suas diferentes formas de representação**. No caso do nosso Second Brain, essa investigação parte da metodologia **P.A.R.A.**, de Tiago Forte, na qual o Vault está organizado em **Projetos, Áreas, Recursos e Arquivos**. Para o Projeto Prometheus, as estruturas mais relevantes são `2. Áreas` e, principalmente, `3. Recursos`, pois é em Recursos que o conhecimento efetivamente está armazenado. Dentro de `Recursos`, a pasta mais importante para o nosso Knowledge System é `Estudos`.

A estrutura de `Estudos` atualmente pode ser representada por seis grandes categorias:

```text
3. Recursos/
└── Estudos/
    ├── Clippings/
    ├── Cursos/
    ├── Dicionário/
    ├── Livros/
    ├── Mapas Mentais importantes/
    └── Métodos/
```

Essas pastas, entretanto, **não representam simplesmente seis tipos equivalentes de documentos**. Ao analisar a estrutura e considerar a função que cada uma desempenha dentro do Second Brain, percebemos que elas possuem naturezas epistemológicas diferentes. Algumas funcionam como fontes de conhecimento, outras como estruturas de aprendizagem, sínteses, representações ou extrações práticas desse conhecimento. Essa distinção será fundamental quando construirmos o pipeline de Knowledge.

A pasta **`Livros`** é a principal fonte de conhecimento atualmente considerada para o Prometheus. Ela contém diversas obras estudadas, como _Administração em Marketing_, _Building Distinctive Assets_, _Building Strong Brands_, _Competitive Advantage_, _Estratégia do Oceano Azul_, _How Brands Become Icons_, _How Brands Grow_, _Managing Brand Equity_, _Marketing Canvas_, _Monetizing Innovation_, _Posicionamento_, _Storybrand_, _Tração_, _ZAG_, entre outras. Algumas obras possuem ainda uma estrutura interna própria, como `Competitive Advantage/Capítulos`, demonstrando que um livro pode ser representado por diversas unidades de conhecimento, e não necessariamente por um único arquivo. Também encontramos materiais que não possuem exatamente a mesma natureza de um livro, como `Análise SWOT.xlsx`, o que mostra que uma pasta organizacional não necessariamente corresponde perfeitamente ao tipo epistemológico de seu conteúdo. Portanto, mesmo dentro de `Livros`, já existe heterogeneidade.

Os **`Cursos`** constituem a segunda grande fonte de conhecimento. Atualmente, essa estrutura contém conteúdos de **Branding**, **Inteligência Artificial** e **Marketing**. Entretanto, dentro da arquitetura conceitual do nosso Second Brain, os cursos não devem ser vistos simplesmente como outra coleção independente de documentos. Eles **complementam os livros**, oferecendo conhecimento estruturado em torno de aulas, conceitos, frameworks e processos de aprendizagem. Dessa forma, um curso pode reforçar, expandir ou organizar conceitos encontrados anteriormente em livros. No futuro, essa relação poderá ser importante para compreender a origem e a contextualização de determinado conhecimento recuperado pelo sistema.

Os **`Mapas Mentais importantes`** representam outra forma de conhecimento. Atualmente existe, por exemplo, um arquivo `.canvas`, `Brand Identity Planning Model`, mostrando que nem todo conhecimento do Second Brain está necessariamente armazenado em Markdown. Os mapas mentais funcionam como **sínteses e representações estruturadas de conhecimento**, complementando principalmente os livros e cursos. Isso é importante porque uma representação visual pode condensar relações entre conceitos que aparecem distribuídas em diversos documentos. Portanto, se futuramente quisermos incorporar essa camada ao Knowledge System, não poderemos assumir que todo conhecimento possui necessariamente o formato `Markdown → texto → embedding`.

Já a pasta **`Métodos`** possui uma função ainda mais específica. Ela contém estruturas como `10 Tipos de Inovação`, `Business Model Canvas`, `Métodos Narrativos`, `Métodos de Criação de Produtos`, `Métodos de Escrita de Conteúdo`, `Repos/Copywriting`, `Value Proposition Design`, além de métodos como `A Estrutura Narrativa de 3 Atos`, `Balanced Scorecard` e `V.R.I.O Framework`. Diferentemente dos livros, os Métodos não representam simplesmente fontes independentes de conhecimento. Dentro da organização do nosso Second Brain, eles são **trechos e conhecimentos extraídos de livros e cursos**, geralmente transformados em algo mais diretamente aplicável. Portanto, existe uma relação de derivação: um método pode ter origem em uma determinada obra ou curso e ser posteriormente separado e organizado como uma unidade própria para utilização prática.

Essa relação entre as estruturas revela algo importante: o Second Brain possui **proveniência e relações entre unidades de conhecimento**. Um livro pode originar determinado método; um curso pode complementar conceitos apresentados em um livro; um mapa mental pode sintetizar conhecimentos provenientes de uma ou várias fontes. Assim, não temos simplesmente:

```text
Livro
Curso
Método
Mapa Mental
```

como quatro conjuntos completamente independentes. Temos algo mais próximo de uma rede:

```text
                 LIVROS
                /      \
               ↓        ↓
           CURSOS      MÉTODOS
               ↓        ↑
               └────┐   │
                    ↓   │
              MAPAS MENTAIS
```

Essa estrutura será extremamente importante para a evolução do Prometheus. Um futuro Knowledge System não deveria considerar apenas o conteúdo textual de uma unidade, mas potencialmente também informações sobre **sua origem, sua relação com outras unidades e sua função dentro do conhecimento acumulado**. Um método, por exemplo, não possui necessariamente o mesmo significado epistemológico de uma obra inteira. Uma síntese não é simplesmente uma nova fonte independente. Um mapa mental pode representar uma organização própria de conhecimentos existentes em diferentes fontes.

Por outro lado, ainda existem duas categorias de `Estudos` que não foram analisadas profundamente nesta primeira etapa: **`Clippings`** e **`Dicionário`**. Portanto, o levantamento realizado até agora é suficiente para compreender o núcleo mais importante do Knowledge Source, especialmente `Livros`, `Cursos`, `Mapas Mentais importantes` e `Métodos`, mas ainda não devemos considerar o mapeamento completo de `Estudos` encerrado até analisarmos essas duas estruturas.

A partir desse levantamento, podemos estabelecer uma distinção inicial entre a estrutura organizacional e o conhecimento propriamente dito. A pasta **`Áreas`** contém principalmente os **MOCs (Maps of Content)**, funcionando como uma camada de organização, navegação e conexão temática. Já `Recursos/Estudos` contém as diferentes formas pelas quais o conhecimento efetivamente foi acumulado. Portanto, um MOC de Marketing, por exemplo, não deve ser automaticamente interpretado da mesma maneira que um livro de Marketing. O primeiro organiza e aponta para conhecimento; o segundo constitui uma fonte desse conhecimento.

Essa distinção nos leva a uma conclusão fundamental para a M7.007:

> **O Second Brain não é o VectorStore.**

O Second Brain é a **fonte de conhecimento**, enquanto o VectorStore será futuramente apenas uma **representação operacional desse conhecimento destinada à recuperação semântica**. Isso significa que não devemos simplesmente "jogar o Obsidian no VectorStore". Antes disso, precisamos compreender o que cada unidade representa, de onde ela veio, qual é sua relação com outras unidades e qual tratamento deve receber.

Nosso mapa atual pode, portanto, ser sintetizado da seguinte maneira:

```text
SECOND BRAIN
│
├── 2. Áreas
│   └── MOCs
│       └── Organização e navegação do conhecimento
│
└── 3. Recursos
    └── Estudos
        │
        ├── Livros
        │   └── Fontes de conhecimento
        │
        ├── Cursos
        │   └── Conhecimento estruturado
        │       que complementa os livros
        │
        ├── Mapas Mentais importantes
        │   └── Sínteses e representações
        │       que complementam livros e cursos
        │
        ├── Métodos
        │   └── Conhecimento/trechos extraídos
        │       de livros e cursos
        │
        ├── Clippings
        │   └── Ainda a analisar
        │
        └── Dicionário
            └── Ainda a analisar
```

Assim, a **Etapa 1 — Mapear o Second Brain** nos permite chegar a uma primeira visão do que o Prometheus poderá considerar como sua fonte de conhecimento. Mais importante do que simplesmente conhecer as pastas é perceber que o nosso Second Brain já possui uma **estrutura de conhecimento com diferentes níveis de representação, origem, síntese e aplicação**. Os livros fornecem grande parte das fontes estudadas; os cursos complementam e estruturam esse conhecimento; os mapas mentais sintetizam e relacionam conceitos; e os métodos extraem partes desse conhecimento para aplicação prática. As Áreas, por sua vez, organizam e conectam esse universo por meio dos MOCs.

Essa descoberta é importante porque muda a natureza do problema que estamos tentando resolver. **Não estamos tentando transformar arquivos em embeddings. Estamos tentando transformar um sistema pessoal de conhecimento em uma Knowledge Base que preserve o contexto necessário para que agentes possam utilizá-lo.**

E é justamente essa compreensão que prepara a próxima etapa da M7.007: **identificar os diferentes tipos de conhecimento existentes dentro dessa estrutura e decidir se todos eles devem ser tratados da mesma maneira pelo RAG.**