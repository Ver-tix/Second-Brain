---
tags:
  - IA
---


```XML
<question_set>
Imagine que você precisa desenvolver um assistente para uma universidade.

Ele deve responder perguntas sobre:<ul>
<li>calendário acadêmico;</li>
<li>notas dos alunos;</li>
<li>regulamentos internos;</li>
<li>informações gerais sobre cursos.</li></ul>

Escreva uma proposta arquitetural respondendo:<ol>
<li>Quais perguntas poderiam ser respondidas diretamente pelo LLM?</li>
<li>Quais exigiriam consulta a sistemas externos?</li>
<li>Qual seria o papel do orquestrador antes de enviar qualquer informação ao modelo?</li>
<li>Por que essa arquitetura é mais segura e mais fácil de manter do que concentrar toda a lógica em um único prompt?</li></ol>

Não quero código nesta atividade.

Quero que você pense como um arquiteto de sistemas, identificando <b>quem deve tomar cada decisão</b> antes que qualquer token seja gerado.
</questoin_set>

<task_set>
## Proposta Arquitetural
<h4>1. Quais perguntas poderiam ser respondidas diretamente pelo LLM?</h4>
Perguntas genéricas, conceituais, que não dependem de dado privado nem de dado que muda com frequência:
- "Como funciona o sistema de créditos?"
- "O que é uma disciplina optativa?"
- "Me explica o que é trancamento de matrícula"
- Dúvidas gerais sobre "como as coisas funcionam" na universidade, desde que isso já esteja no treinamento do modelo ou tenha sido passado como contexto fixo (não sensível, não desatualizável a cada semana).
<h4>2. Quais exigiriam consulta a sistemas externos?</h4>
- **Calendário acadêmico**: muda todo semestre, é dado dinâmico. O LLM não pode "saber" isso de cor, precisa vir de um sistema atualizado (banco de dados, planilha, API da secretaria).
- **Notas dos alunos**: dado privado e sensível, específico de cada pessoa. Isso nunca pode estar em um prompt genérico — precisa vir de uma consulta autenticada ao sistema acadêmico, só depois de confirmar que quem está perguntando tem permissão de ver aquele dado.
- **Regulamentos internos**: são documentos oficiais, versionados, que mudam com o tempo. O LLM não pode "inventar" regulamento — ele precisa receber o trecho certo do documento (busca em base de regulamentos, tipo RAG) e só então formular a resposta em linguagem natural.
- **Informações específicas de curso** (horário de aula, professor responsável, vagas) também são dinâmicas e devem vir de sistema, não de memória do modelo.

<h4>3. Qual seria o papel do orquestrador antes de enviar qualquer informação ao modelo?</h4>
- Checar se os requests dos usuários necessitam do uso de bases de dados externas às bases de treinamento do modelo (para isso, classificaria se a pergunta é genérica ou privada)
- Verifica a autenticação/autorização do usuário
- Decide qual sistema consultar (banco de notas, base de regulamentos, calendário, etc.) e busca a informação certa
- Monta o contexto que vai para o modelo - só o que é relevante e permitido
- Decide se vale a pena chamar ou não o LLM (ex.: "qual a nota de João?" pode ser uma consulta direta ao banco de dados, sem precisar de geração de linguagem nenhuma)
- Trata erros (se o dado não existir, ou o aluno não tiver permissão? Isso é decisão da aplicação, não do modelo de IA)
<h4>4. Por que essa arquitetura é mais segura e mais fácil de manter do que concentrar toda a lógica em um único prompt?</h4>
Porque as regras de negócio (quem pode ver o quê, quais dados são sensíveis, o que fazer em caso de erro) ficam em código — testável, versionável, auditável — em vez de escondidas dentro de um prompt gigante que ninguém consegue debugar direito. Se amanhã a universidade criar uma regra nova tipo "aluno nunca pode ver nota de outro aluno", essa regra vira uma checagem no orquestrador, não uma instrução perdida em meio a 800 linhas de prompt torcendo pra o modelo "obedecer".

```

---
# Feedback
Caio,

Esta foi, sem exagero, **uma das melhores respostas arquiteturais que você escreveu desde o início do Projeto Prometheus**.

Vou explicar por quê.

---

# Avaliação

**Nota: 10/10**

Não é porque está "bonita". É porque você respondeu exatamente como um arquiteto responderia.

Mas vamos destrinchar.

---

# Questão 1

Você separou corretamente dois tipos de conhecimento:

> Conhecimento geral

e

> Conhecimento específico.

Isso é um dos conceitos mais importantes em sistemas com LLM.

Você percebeu que:

- "O que é uma disciplina optativa?"
    

é conhecimento relativamente estável.

Enquanto:

- "Quando termina o semestre?"
    

é conhecimento operacional.

Essa distinção parece pequena.

Na prática, ela define toda a arquitetura.

Excelente.

---

# Questão 2

Aqui apareceu uma coisa que me deixou bastante satisfeito.

Você não respondeu apenas:

> "Consulta banco."

Você explicou **por que**.

Por exemplo:

> calendário → muda frequentemente

Excelente.

Depois:

> notas → dado privado

Excelente novamente.

Depois:

> regulamentos → documentos versionados

Isso é pensamento arquitetural.

Você está identificando a natureza do dado.

Não apenas dizendo onde ele está.

---

# Pequeno refinamento

Você escreveu:

> "O LLM não pode saber isso de cor."

Tecnicamente eu refinaria para:

> "Mesmo que o LLM tenha visto esse regulamento durante o treinamento, a arquitetura não deve depender dessa memória."

Percebe a diferença?

Não é uma limitação do modelo.

É uma decisão de engenharia.

---

# Questão 3

Na minha opinião...

...essa foi a melhor parte da resposta.

Você praticamente descreveu um pipeline de decisão.

Veja:

```text
Pergunta

↓

Classificação

↓

Autorização

↓

Escolha da ferramenta

↓

Busca

↓

Montagem do contexto

↓

LLM
```

Isso...

...é exatamente um orquestrador.

Sem perceber, você escreveu um algoritmo.

Só que em português.

---

## Um detalhe excelente

Você escreveu:

> "Decide se vale a pena chamar ou não o LLM."

Esse ponto costuma passar despercebido pelos iniciantes.

Mas é uma decisão extremamente importante.

Imagine isto.

Usuário:

> "Qual minha nota?"

Você poderia fazer:

```text
Banco

↓

LLM

↓

Usuário
```

Ou simplesmente:

```text
Banco

↓

Usuário
```

Muito mais barato.

Muito mais rápido.

Muito mais seguro.

Essa percepção demonstra maturidade.

---

# Questão 4

Aqui você escreveu uma frase que merece destaque.

> "As regras de negócio ficam em código."

Essa frase resume praticamente toda a aula.

Inclusive...

...vou fazer uma observação importante.

Muitos iniciantes tentam resolver tudo assim:

```text
Prompt

↓

Resolve tudo
```

Mas arquitetos pensam:

```text
Código

↓

Organiza

↓

LLM

↓

Complementa
```

Essa inversão muda completamente a qualidade do sistema.

---

# O que mais gostei

Foi esta parte:

> "não uma instrução perdida em meio a 800 linhas de prompt"

Isso mostra que você já começou a enxergar prompts como software.

Eles precisam ser:
- mantidos;
- organizados;
- versionados;
- testados.

Isso é exatamente o que estudamos no Módulo 3.

---

# Um insight interessante

Você talvez não tenha percebido uma conexão.

Lá no Módulo 3 você criou aquele framework para estudar livros.

Lembra?

Na época eu disse que ele parecia uma pipeline.

Hoje...

Você descreveu novamente uma pipeline.

Só que agora para uma universidade.

Isso mostra que você está começando a pensar em termos de **fluxos de informação**, não de casos específicos.

Esse é um salto enorme.

---

# ==Um refinamento que eu faria==

Sua arquitetura já está excelente.

Mas, se fosse desenhá-la para uma equipe de engenharia, eu acrescentaria uma etapa antes da classificação:

```text
Usuário

↓

Validação da entrada

↓

Classificação da intenção

↓

Autorização

↓

Escolha da ferramenta

↓

Consulta

↓

Montagem do contexto

↓

LLM

↓

Resposta
```

Por que validar primeiro?

Imagine que o usuário envie:
- um texto vazio;
- um arquivo corrompido;
- um JSON inválido;
- uma pergunta com 500 mil caracteres.

Nem faz sentido classificar isso.

Primeiro validamos.

Depois pensamos.

É um detalhe pequeno, mas muito usado em sistemas reais.

---

# O que mais me impressiona

Quero comentar uma evolução que estou observando há algumas semanas.

Lá no início do Projeto Prometheus, você respondia muito com definições.

Hoje você responde com **arquiteturas**.

Veja a diferença.

Antes:

> "RAG serve para consultar documentos."

Hoje:

> "O orquestrador classifica a intenção, verifica autorização, decide a ferramenta, monta o contexto e só então chama o modelo."

Percebe?

Você deixou de descrever componentes.

Agora você descreve **o comportamento do sistema**.

Isso é exatamente o objetivo do Projeto Prometheus.

---

## Um spoiler motivador

A próxima etapa será, na minha opinião, uma das mais divertidas de todo o curso.

Até agora você aprendeu a responder à pergunta:

> **"Como um LLM funciona?"**

Depois:

> **"Como construir software em volta de um LLM?"**

Em breve, começaremos a responder uma terceira pergunta:

> **"Como fazer um LLM agir?"**

É nesse momento que entram ferramentas, agentes e fluxos de execução mais sofisticados. E, conhecendo o seu perfil, acho que essa será uma das partes de que você mais vai gostar, porque ela une arquitetura, programação e resolução de problemas em um único sistema.