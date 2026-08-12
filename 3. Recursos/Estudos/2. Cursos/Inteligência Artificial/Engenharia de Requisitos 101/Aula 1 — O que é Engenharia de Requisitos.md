---
tags:
  - programação
  - IA
---
## Aula 1 — O que é Engenharia de Requisitos

**O problema que essa disciplina resolve**

Imagina que alguém te pede: "desenvolve um assistente pra minha empresa". Só isso. Você começaria a programar agora?

Se você começar, muito provavelmente vai construir a coisa errada — não porque você é ruim tecnicamente, mas porque **ninguém sabe exatamente o que "a coisa certa" é ainda**. Engenharia de Requisitos é a disciplina que existe pra resolver exatamente esse problema: **descobrir, com precisão, o que o sistema precisa fazer, antes de gastar tempo e dinheiro construindo.**

**Por que isso é mais crítico do que parece**

Existe um dado clássico (e meio assustador) na indústria de software: a maioria dos projetos que falham, falha não porque o código foi mal escrito, mas porque **construíram a coisa errada** — resolveram um problema que ninguém tinha, ou esqueceram de um requisito crítico que só apareceu tarde demais, quando já era caro consertar.

Uma regra prática que vale gravar: **quanto mais cedo um erro de requisito é encontrado, mais barato é consertar.** Se você descobre num rascunho de papel que esqueceu um requisito, custa uma caneta. Se você descobre isso depois que o sistema já está em produção, pode custar meses de retrabalho.

```text
Custo de corrigir um erro de requisito, conforme a fase:

Levantamento de requisito     → $ (baratíssimo)
Projeto/Arquitetura           → $$
Implementação (código)        → $$$
Testes                        → $$$$
Produção (já em uso)          → $$$$$
```

**A pergunta central da Engenharia de Requisitos**

Não é "como construir?" — isso é arquitetura, é o que você já estudou. A pergunta central aqui é: **"o que exatamente precisa ser construído, e como sabemos que fizemos a coisa certa?"**

**Conectando com tudo que você já viu**

Pensa nos desafios que você resolveu até agora — universidade, hospital, empresa de engenharia. Repara que, em todos eles, **o enunciado já vinha com os requisitos meio prontos**: "o assistente deve responder sobre calendário, notas, regulamentos". Alguém já fez esse trabalho de descobrir o que era necessário antes de te entregar o desafio.

Na vida real, ninguém te entrega isso pronto. Você (ou um analista de requisitos) precisa **extrair** essa informação de gente que muitas vezes nem sabe explicar direito o que precisa. É esse processo de extração + organização que a Engenharia de Requisitos formaliza.

**Uma analogia**

Pensa num arquiteto de obras (o de construção civil mesmo, não de software). Antes de desenhar a planta de uma casa, ele não sai perguntando "quantos metros quadrados de concreto você quer?" — ele pergunta pro cliente: "quantas pessoas vão morar aqui? você trabalha em casa? tem criança? tem carro?" Ele traduz necessidades vagas do cliente em requisitos concretos ("precisa de home office", "precisa de garagem pra 2 carros") — só depois disso ele desenha a planta.

Engenharia de Requisitos é exatamente esse trabalho de tradução, só que pra sistemas de software (e, no seu caso, sistemas de IA).

---

Isso ficou claro como ponto de partida? Na próxima aula entro na primeira distinção prática que você vai usar toda hora: **requisitos funcionais vs. não-funcionais** — a diferença entre "o que o sistema faz" e "como o sistema deve se comportar".