---
tags:
  - inteligenciaartificial
  - programação
---
---

Agora que você consegue conversar com um modelo, surge uma pergunta importante.

> **Onde deve ficar a inteligência da aplicação?**

A maioria dos iniciantes responde:

> "Dentro do prompt."

Esse é um dos erros arquiteturais mais comuns.

---

## Vamos imaginar um sistema simples

Você quer um assistente financeiro.

Muitos fariam algo assim:

```text
Pergunte ao usuário.

↓

Envie tudo ao GPT.

↓

Mostre a resposta.
```

Funciona?

Sim.

É uma boa arquitetura?

Não.

---

## O problema

Imagine que amanhã você queira adicionar uma regra:

> "Nunca permita recomendar investimentos proibidos pela política da empresa."

Onde essa regra ficará?

Se toda a inteligência estiver escondida dentro de um prompt enorme, você terá um problema:
- difícil de testar;
- difícil de versionar;
- difícil de manter;
- difícil de reutilizar.

---

## A separação correta

Um sistema baseado em LLM normalmente possui três camadas conceituais:

```text
Usuário

↓

Aplicação

↓

LLM
```

A camada intermediária é a mais importante.

Ela decide:
- qual prompt utilizar;
- quais documentos buscar;
- quais ferramentas chamar;
- quando consultar memória;
- quando devolver erro;
- quando nem chamar o LLM.

<h4 align="center">O modelo deixa de ser "o sistema". Ele passa a ser <b>um componente do sistema</b>.</h4>

---

## Um exemplo

Imagine duas perguntas.

Pergunta A:

> "Qual é a capital do Japão?"

Pergunta B:

> "Qual é o saldo da conta do cliente João?"

A arquitetura madura percebe imediatamente que esses problemas são diferentes.

Fluxo A:

```text
Pergunta

↓

LLM

↓

Resposta
```

Fluxo B:

```text
Pergunta

↓

Aplicação identifica que envolve dados privados

↓

Consulta banco de dados

↓

Monta contexto

↓

LLM

↓

Resposta
```

Perceba que o LLM não sabe acessar o banco sozinho.

Quem decide isso é a aplicação.

---

## O conceito de Orquestração

<h4 align="center">Essa camada intermediária recebe um nome muito importante: <b>Orquestrador.</b></h4>

Ele funciona como um maestro. O maestro não toca todos os instrumentos. **Ele coordena**.

Da mesma forma, um orquestrador pode decidir:

```text
Pergunta recebida

↓

Precisa de memória?

↓

Sim

↓

Buscar memória

↓

Precisa consultar documentos?

↓

Sim

↓

Executar busca

↓

Montar contexto

↓

Enviar ao modelo
```

O LLM apenas recebe um contexto muito melhor.

---

## Por que isso importa?

Porque você não quer que um prompt de 800 linhas tome todas as decisões.

Você quer que a maior parte das decisões seja escrita em código.

Código pode ser:
- testado;
- versionado;
- depurado;
- reutilizado.

---

## Uma analogia

Imagine um restaurante.

O cliente conversa com o garçom.

O garçom conversa com a cozinha.

A cozinha prepara o prato.

O cliente nunca entra na cozinha.

No nosso sistema:

```text
Cliente

↓

Aplicação (garçom)

↓

LLM (cozinha)

↓

Aplicação

↓

Cliente
```

Se o cliente for alérgico a amendoim, quem verifica isso?

O garçom.

Não a cozinha.

Da mesma forma, várias decisões importantes pertencem à aplicação, não ao modelo.

[[Algumas Respostas Sobre o Orquestrador]]

---

# 📜 Princípio LXIX

> **Um LLM é um componente extremamente poderoso, mas continua sendo apenas um componente. A inteligência de um sistema de IA moderno está tanto na orquestração quanto no modelo.**

---

# Desafio Prometheus #004

Imagine que você precisa desenvolver um assistente para uma universidade.

Ele deve responder perguntas sobre:
- calendário acadêmico;
- notas dos alunos;
- regulamentos internos;
- informações gerais sobre cursos.

Escreva uma proposta arquitetural respondendo:
1. Quais perguntas poderiam ser respondidas diretamente pelo LLM?
2. Quais exigiriam consulta a sistemas externos?
3. Qual seria o papel do orquestrador antes de enviar qualquer informação ao modelo?
4. Por que essa arquitetura é mais segura e mais fácil de manter do que concentrar toda a lógica em um único prompt?

Não quero código nesta atividade.

Quero que você pense como um arquiteto de sistemas, identificando **quem deve tomar cada decisão** antes que qualquer token seja gerado.

[[🛠️ Desafio M4 004]]

---

E um pequeno spoiler para animá-lo: estamos nos aproximando de uma fase muito divertida do curso. Em poucas aulas, deixaremos de construir aplicações que apenas "conversam" com um LLM e passaremos a construir aplicações que **usam ferramentas, consultam documentos e tomam decisões sobre quando utilizar cada recurso**. É aí que a arquitetura começa a ganhar vida de forma ainda mais interessante.