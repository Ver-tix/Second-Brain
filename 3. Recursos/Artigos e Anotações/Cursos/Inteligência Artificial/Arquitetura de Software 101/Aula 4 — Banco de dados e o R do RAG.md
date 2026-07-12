---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 4 — Banco de dados e o "R" do RAG

**O que é um banco de dados, na prática**

É só um lugar organizado pra guardar informação de forma que dê pra buscar rápido e com precisão depois. Pensa numa planilha gigante, só que muito mais rápida e com regras de organização.

Existem dois tipos principais que você vai ouvir falar o tempo todo:

**Banco de dados relacional (SQL)**

Organiza tudo em tabelas, tipo planilhas conectadas entre si.

```text
Tabela Pacientes          Tabela Exames
-----------------         -----------------
id | nome                 id | paciente_id | resultado
1  | João                 1  | 1           | Glicemia: 95
2  | Maria                2  | 1           | Hemograma: normal
```

Repara que a tabela de Exames tem uma coluna `paciente_id` que "aponta" pra tabela de Pacientes. Isso é o "relacional" — as tabelas se conectam por essas referências. Você pergunta "quais exames do paciente 1?" e o banco cruza as tabelas e devolve certinho.

É exatamente esse tipo de estrutura que guarda resultado de exame, medicamento, matrícula de aluno, nota — dado estruturado, fixo, que você busca com precisão cirúrgica.

**Banco de dados vetorial (o que interessa pro seu módulo de LLM)**

Esse é diferente. Em vez de guardar dado em tabelas com colunas fixas, ele guarda **texto convertido em números** (chamado de "embedding" — um vetor que representa o significado daquele texto).

Por quê isso importa? Porque texto solto — um regulamento inteiro, um manual, um contrato — não cabe bem numa tabela com colunas fixas tipo "nome, id, valor". Mas você ainda quer conseguir _buscar_ dentro desse texto por significado, não só por palavra exata.

**Aqui entra o RAG (Retrieval-Augmented Generation)**

Lembra lá na aula 1, quando você mencionou RAG? Agora dá pra explicar direito, porque você já tem o vocabulário:

```text
Pergunta chega: "o que diz o regulamento sobre trancamento de matrícula?"

↓

Aplicação transforma a pergunta em vetor (embedding)

↓

Aplicação busca no banco vetorial: quais trechos do regulamento têm significado parecido com essa pergunta?

↓

Banco devolve os 2-3 trechos mais relevantes (texto puro, ainda não é resposta)

↓

Aplicação monta um contexto: "aqui estão os trechos relevantes do regulamento: [trechos]. Responda a pergunta do usuário usando só essas informações."

↓

LLM recebe esse contexto pronto e escreve a resposta final
```

RAG é literalmente **"R"** de Retrieval (busca) + **"AG"** de Augmented Generation (o LLM gerando texto com um contexto "aumentado", enriquecido pela busca). É a camada de lógica (o orquestrador que você já conhece) decidindo buscar num banco vetorial antes de acionar o LLM — exatamente o mesmo padrão do Fluxo B lá do desafio da universidade, só que agora você sabe o nome técnico de cada peça.

**Por que isso é importante e não é só "detalhe técnico"**

Porque sem isso, o LLM só tem duas opções ruins: ou ele **inventa** a resposta (alucina) porque não tem o regulamento de verdade, ou você **cola o regulamento inteiro** no prompt toda vez (caro, lento, e ainda assim limitado em tamanho). RAG resolve isso buscando só o pedaço relevante, na hora certa.

**Conectando os dois tipos de banco no hospital:**

- Resultado de exame do paciente → banco relacional (dado estruturado, precisa ser exato, não "parecido")
- Protocolos médicos, bulas de remédio, manuais internos → banco vetorial, buscado via RAG (texto longo, você quer o trecho com significado relevante)