---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 3 — Cliente-servidor, API, e onde o LLM mora nisso tudo

**Cliente-servidor: o básico**

Quase todo sistema moderno funciona assim:

```text
Cliente (quem pede)

↓ pede algo

Servidor (quem responde)

↓ devolve algo

Cliente
```

O "cliente" pode ser o app do médico, o chat que o usuário usa. O "servidor" é onde a lógica de verdade roda — normalmente longe fisicamente do cliente, numa máquina em algum datacenter.

**API: a "linguagem" entre eles**

API é só um contrato: "se você me mandar isso, eu te devolvo aquilo". Tipo um cardápio de restaurante. Você não entra na cozinha pra pedir o prato — você usa o cardápio (a API) pra pedir, e a cozinha (o servidor) prepara e devolve.

Exemplo prático: quando a camada de lógica do hospital precisa buscar o resultado de um exame, ela não "invade" o banco de dados do laboratório diretamente — ela chama uma API do sistema laboratorial, tipo "me dá o exame do paciente com ID 4521", e recebe uma resposta estruturada de volta.

**Onde entra o LLM nisso tudo — aqui é a parte que você pediu**

O modelo de linguagem (LLM), na prática, é **só mais um servidor que você chama via API**. Não tem mistério nenhum: sua aplicação (a camada de lógica) faz uma chamada pra API da OpenAI, da Anthropic, etc., manda um texto, e recebe um texto de volta. É exatamente o mesmo padrão cliente-servidor que você acabou de aprender.

```text
Aplicação (camada de lógica)

↓ chama via API

LLM (servidor de linguagem)

↓ devolve texto gerado

Aplicação
```

Repara: isso é literalmente a mesma estrutura da API do laboratório. A diferença é só o que o servidor faz — um devolve dado bruto, o outro devolve texto gerado.

**E o "agente de IA"? Onde isso entra?**

Um agente de IA nada mais é do que **a camada de lógica automatizando decisões que antes um humano tomava**, usando o LLM em loop. Em vez de uma única chamada "pergunta → resposta", o agente faz várias idas e vindas:

```text
Pergunta chega

↓

Aplicação decide: preciso de mais informação?

↓ sim

Aplicação chama uma ferramenta (ex: buscar exame no banco)

↓

Resultado volta pra aplicação

↓

Aplicação manda esse resultado pro LLM: "agora, com esse dado, o que fazer?"

↓

LLM responde: "preciso buscar mais uma coisa" OU "já posso responder"

↓ (repete até ter tudo que precisa)

Resposta final é montada
```

Ou seja: **agente = orquestrador + LLM chamado várias vezes em loop, decidindo passo a passo o que fazer.** Não é uma camada nova na arquitetura — é uma forma mais sofisticada de implementar a camada de lógica que você já conhece desde a aula 1.

No exemplo do hospital: em vez da aplicação decidir tudo de forma fixa em código ("se for pergunta sobre exame, busca aqui"), um agente permite que o próprio LLM ajude a decidir "preciso buscar o exame, depois o medicamento, depois cruzar os dois pra responder". Mas atenção: **decisões de permissão e segurança continuam sendo da aplicação**, nunca do agente/LLM sozinho — isso não muda, é só a parte de "qual ferramenta chamar em qual ordem" que fica mais flexível.

**Resumindo a aula 3:**

|Conceito|O que é|
|---|---|
|Cliente-servidor|Quem pede vs quem responde|
|API|O contrato/cardápio de como pedir|
|LLM na prática|Um servidor chamado via API, como qualquer outro|
|Agente|A camada de lógica usando o LLM em loop pra decidir passos, não só responder uma vez|
