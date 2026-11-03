---
tags:
  - IA
---

---
# O Que é Engenharia de Prompt e Por Que Ela é Importante?

---

### Programando em Linguagem Natural

- Você está **dando instruções em linguagem simples** ao invés de códigos
- O modelo não possui uma "lista de tarefas" integrada; você define a tarefa, o formato da função e as restrições no prompt.
- O mesmo modelo pode parecer **brilhante ou inútil** dependendo da clarificação, contexto e estrutura

#### Exemplo:

> - _Vago:_ “Me ajude com minha tarefa” → Modelo não conhece matéria, comprimento ou estilo
> - _Específico:_ “Você é meu tutor. Me ajude a melhorar a tese e primeiro parágrafo dessa tarefa de 500 palavras de história nas causas da Primeira Guerra Mundial. Mantenha minha voz; sugira edições em linha” → Modelo pode ajudar

### A Quem Beneficia?

- **Desenvolvedores** → geração de códigos, debugging, docs, refatoração;
- **Profissionais de Marketing** → Copywriting, anúncios, e-mails, social media, ideias de Testes A/B;
- **Pesquisadores** → resumo, revisão de literatura, brainstorming;
- **Usuários do dia-a-dia** → escrever, planejar, aprender, tomada de decisões.

**Conclusão:** melhores prompts = melhores resultados para todos. É uma habilidade que se aprimora com o tempo.

---

# Fundação: Como LLMs “Pensam”

### Previsão do Próximo Token (Apenas Intuição)

- O modelo prevê o **próximo token** (palavra ou subpalavra) com base em tudo o que foi apresentado até o momento na mensagem e na conversa;
- **Não possui memória** de chats anteriores a não ser que você inclua essa informação no contexto atual;
- Então: **contexto**, **especificidades** e **estrutura** no seu prompt diretamente modelam o que ele dirá em seguida;
- Contextos mais relevantes e instruções mais claras → Outputs mais consistentes e relevantes

### Direção vs. Comando

- **Comandando (commanding):** “Resuma isso.” → O modelo escolhe tamanho, estilo e foco
- **Direcionando (Steering):** “Você é um assistente executivo. Resuma essa transcrição dessa reunião em 4 bullet points. Foque-se na descrição e itens de ação. Sem encher linguiça.” → Você guia tamanho, foco e formato.

#### Exemplo:

|Comandando|Guiando|
|---|---|
|“Escreva um e-mail”|“Você está respondendo a um cliente que pediu por um adiamento no projeto. Escreva um nova proposta de deadline curta e pedindo desculpas para: 15 de Março. Tom: Profissional e tranquilizador.|

**Guiar** (papel, audiência, formato e restrições) resulta em resultados mais úteis e consistentes.

---

# a. Seja Específico e Prepare o Cenário

Defina **papel**, **audiência**, **tom**, e **formato** para que o modelo não tenha que adivinhar:

- **Papel:** ”Você é um escritor técnico sênior”
- **Audiência:** ”Escrever para desenvolvedores que sabem Python mas não APIs Cloud”
- **Tom:** ”Profissional mas amigável; evite jargões ou explicar tudo de uma vez”
- **Formato:** ”Comece com uma visão geral de duas frases, depois os passos numerados e, por fim, uma breve seção sobre ‘Armadilhas comuns’.”

#### **Exemplo de resposta de suporte:**

**Fraco:**

```markdown
Responda à reclamação desse cliente
```

**Forte:**

```markdown
Você é o(a) responsável pelo suporte ao cliente. Responda a esta reclamação de um 
usuário pagante cuja exportação falhou duas vezes. Reconheça a frustração. Peça 
desculpas brevemente, confirme que estão investigando e ofereça uma próxima etapa 
concreta (por exemplo, entraremos em contato por e-mail em até 24 horas). Mantenha a 
resposta com menos de 150 palavras. E finalize como "Equipe de Suporte".
```

---

# b. Few-Shot Prompting (Prompting com Alguns Exemplos)

Dê **2-4 exemplos** de input → pares de output. O modelo infere o padrão (algo em que ele é bom) e o replica. Especialmente útil para: **formato**, **estilo**, **casos extremos** e **classificação**.

#### Exemplo - Transformando feedback em títulos de ticket

**Sem Exemplos (zero-shot):**

```markdown
Transforme este feedback do usuário em um título curto para o ticket.
Feedback: "O aplicativo trava quando eu envio um PDF com mais de 10 MB no iPhone"
```

_Pode aparecer: "Relatório de falha do usuário" ou algo muito longo ou inconsistente._

**Com Exemplos (few-shot):**

```markdown
Transforme este feedback do usuário em um título curto para o ticket.
Feedback: "O aplicativo trava quando eu envio um PDF com mais de 10 MB no iPhone"

Feedback: "O login com o Google no Safari está apresentando problemas"
Título: [Aut.] O login do Google falha no Safari

Feedback: "Exportar para CSV apenas exportar as primeiras 100 linhas"
Título: [Aut.] CSV exporta um limite de 100 linhas

Feedback: "O Aplicativo quebra quando eu faço Upload de PDFs acima de 10MB no iPhone"
Título: 
```

_É muito mais provável que o modelo apresente a seguinte mensagem: [Upload] Falha ao abrir PDF grande (iPhone) ou similar_

---

# c. Chain of Thought (CoT)/ Cadeia de Pensamento

Peça ao modelo para **raciocinar passo a passo** antes de dar a resposta final. Isso reduz erros de **lógica**, **matemática**, **planejamento em várias** **etapas** e **comparações**.

#### Exemplo - Raciocínio Simples

**Sem CoT:**

```markdown
Uma loja vende canetas por $2 e cadernos por $5. Sarah compra 3 canetas e 2 cadernos. 
Ela tem um cupom de 10%. Qunato ela paga?
```

_O modelo pode pular para um número e, as vezes, cometer erros aritméticos_

**Com CoT:**

```markdown
Uma loja vende canetas por $2 e cadernos por $5. Sarah compra 3 canetas e 2 cadernos. 
Ela tem um cupom de 10%. Qunato ela paga? Pense passo a passo: encontre o subtotal, em 
seguida, calcule o desconto e então, indique o valor final.
```

_O modelo é guiado por: 3x2 + 2x5 = 16 → 10% de desconto = 1,60 → 16 - 1,60 = $ 14,40_

Esse é um momento importante para a Etapa de Discernimento.

Use frases como “**Pense passo a passo**” / “**Mostre seu raciocínio**”/ “**Explique cada passo antes de concluir**”.

Assim, você tem um controle de qualidade melhor.

---

# d. Output Estruturados

Solicite explicitamente JSON, tabelas XML ou Markdown para que você possa analisar e usar a saída no código ou na documentação.

#### Exemplo - Comparação de Produto:

**Formato Livre:**

```markdown
Compare nosso produto com o do Concorrente X e Concorrente Y em preço, componentes, e
Suporte.
```

_Você recebe prosa; difícil de transformar em uma tabela ou aplicativo._

**Estruturado:**

```markdown
Compare nosso produto (NossoApp) com o do Concorrente X e Concorrente Y em preço, 
componentes, e Suporte.

{ 
	"produtos": [
		{ "nome": "...", "preço": "...", "componentesPrincipais": ["...", "..."], "suporte": "..."}
	]
}
```

_Você obtém uma saída legível por máquina que pode validar e exibir._

---

# e. Restrições e Negativas

Diga o que você **não** quer: tamanho, tom, formato ou tópico.

Reduz: muito comprido, muito casual, estrutura errada, ou fora de tópico

#### **Exemplos:**

- **Tamanho:** “Resuma em exatamente 3 bullet points” / “mantenha a resposta abaixo de 100 palavras”
- **Tom:** “Sem gírias ou humor” / “Não peça desculpas”
- **Formato:** “Não use bullet points; use um parágrafo curto”
- **Escopo:** “não sugira ferramentas pagas” / “não inclua código; descreva a abordagem apenas”

#### **Exemplo - Evitando Hábitos Ruins:**

**Sem Restrições:**  
“Escreva uma curte introdução para nosso documento onboarding.”  
_Pode começar com “Bem Vindo!” ou algum termo floreado de Marketing._

**Com Restrições:**  
”Escreva uma introdução curta para nosso documento onboarding. Inicie diretamente com o que o usuário irá fazer nessa seção. Não comece com ‘Bem Vindo’ ou qualquer de saudação genérica.”  
_Mais provável de ser acionável da primeira sentença._

---

# f. Refinamento Iterativo

Isso pode ser importante na etapa de Discernimento.

Trate o prompting como uma **conversa**, não um tiro único.

Primeira resposta não está muito certa? Refine: “Mais curta”. “Mais formal”. “Adicione um exemplo”. “Foque apenas em X”.

#### Fluxo de Exemplo:

1. **Você:** “Rascunhe um texto de apresentação de 2 sentenças ou”
2. **Modelo:** [Retorna quatro sentenças, um pouco vendedor demais]
3. **Você:** “Reduza a duas sentenças e o faça mais factual, menos vendedor”
4. **Modelo:** [Menor, mais factual]
5. **Você:** “Adicione que funcione com Google Calendar”
6. **Modelo:** [Versão final]

Iteração é normal e normalmente mais rápida que escreve o prompt perfeito na primeira tentativa

---

# g. Prompt Estilo Entrevista

Ao invés de adivinhar qual contexto prover, **dê ao modelo a tarefa e pergunte a ele para te entrevistar** para qualquer que seja sua necessidade. Você pode definir o objetivo; o modelo fará perguntas clarificadoras; você responde; então, ele performa com o contexto completo.

> **Por que funciona**: nós frequentemente esquecemos detalhes (audiência, restrições, formato e exemplos) porque não sabemos que o modelo precisa deles. Fazer o modelo perguntar preenche esses gaps antes de gerar uma resposta.

**Como fazer:**

1. **Dê a tarefa.** Descreva o que você quer em uma sentença ou duas,
2. **Pergunte ao modelo para entrevistar você.** Por exemplo, “Antes de fazer isso, me entreviste: pergunte qualquer coisa que você precise para realizar o projeto bem. Faça uma pergunta por vez. Eu irei responder; quando você tiver repostas o suficiente, diga que está pronto, e então realize a tarefa.”,
3. **Responda cada pergunta**. Seja conciso mas completo,
4. **Deixe o modelo proceder.** Uma vez que tiver o que precisa, entregue o output.

#### Exemplo - post de blog:

**Você (turno 1):**

```markdown
Eu preciso de um post de blog curto (cerca de 400 palavras) para o blog de nossa 
companhia. Tópico: por que pequenos times deveriam tentar standups async. Antes de você
Escrever, me entreviste: pergunte quaisquer questões que você precise para realizar bem 
o projeto. Faça uma pergunta por vez. Eu irei responder; quando você tiver repostas o 
suficiente, diga que está pronto, e então realize a tarefa.
```

---

# Técnicas Avançadas e Parâmetros

### a. Sistema vs. Prompts do Usuário (Contexto da API)

<aside>

Mais relevante para developers

</aside>

- Prompt do Sistema: Configura identidade, regras e estilo (normalmente não mostrado ao usuário final). Usar para comportamento "sempre ligado”
- **Prompt do Usuário:** O request em si. Mantenha o foco na tarefa desta rodada.

#### Exemplo:

- **Sistema:** “Você é um assistente de código útil. Você responde com snippets concisas e corretas. Você nunca inventa nomes de API; em caso de dúvida, diga.”
- **Usuário:** “Como eu leio a primeira linha de um arquivo em Python?”

O prompt do sistema mantém o tom e regras consistentes através das muitas perguntas do usuário

### b. Cadeia de Prompts (Prompt Chaining)

Quebre **tarefas complexas** em passos. Use o output do passo N como input do passo N+1.3Melhora a confiabilidade e facilita o conserto ou melhoria um passo de cada vez

#### Exemplo - Pipeline de Post de Blog:

1. **Passo 1:** “Dado o tópico [X], gere um output esboço com 5 títulos. JSON: {”títulos”: [”…”, “…”]}.”
2. **Passo 2:** “Expanda esse esboço em uma sessão de 400 palavras. Esboço: [cole o output do Passo 1]. Tom: [Y]”
3. **Passo 3:** “Torne esse rascunho em meta title e descrição para SEO. Rascunho: [cole o output do Passo 2]. Título com no máximo de 60 caracteres, descrição, com no máximo 155 caracteres.

Cada passo tem um trabalho claro e um formato de output claro

### c. Auto-Avaliação

Peça ao modelo para **criticar ou pontuar** seu próprio output. Use isso para rascunhos, códigos ou reusmos.

#### Exemplo

“Eis aqui um curto resumo que eu gerei: [texto]. Avalie ele de 1-5 para clarificação e completude. Em uma sentença, sugira a melhoria mais importante a ser feita.”

Use a crítica para refinar o próximo turno.

### d. Temperatura e Outros Parâmetros

- **Temperatura:** baixa (exemplo 0,2) = mais determinística, repetível; alta (exemplo 0,8) = mais elástica e imprevisível
- Use **Baixa** para: fatos, códigos, outputs estruturados, consistência
- Use **Alta** para: brainstorming, fraseado variado, múltiplas ideias
- Se o outputs são muito aleatórios ou fora da tarefa, reduza a temperadtura. Se muit repetitivos, aumente-a um pouco.

---