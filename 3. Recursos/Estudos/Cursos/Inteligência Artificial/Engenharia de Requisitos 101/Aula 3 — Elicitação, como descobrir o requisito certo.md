---
tags:
  - IA
  - programação
---
## Aula 3 — Elicitação: como descobrir o requisito certo

Até agora você viu **o que é** um requisito e **como classificá-lo** (funcional/não-funcional). Essa aula responde a uma pergunta mais difícil: **de onde esses requisitos vêm, na prática, quando ninguém te entrega prontos?**

**O problema real da elicitação**

Elicitação é o processo de extrair requisitos das pessoas que vão usar ou são afetadas pelo sistema (chamadas de **stakeholders** — termo que você vai ver bastante). O problema é que, na prática, essas pessoas quase nunca sabem descrever exatamente o que precisam.

Isso acontece por alguns motivos bem previsíveis:

- **A pessoa sabe o problema, não a solução.** O médico sabe que "perco muito tempo procurando resultado de exame espalhado em três sistemas diferentes" — mas não sabe articular isso como "preciso de um requisito funcional de consulta unificada".
- **A pessoa assume que algo é óbvio e não menciona.** Ninguém vai falar "ah, e é importante que dado de paciente não vaze" — porque pra ela isso é tão básico que nem passa pela cabeça que precisaria ser dito. Só que, do lado de quem constrói o sistema, isso precisa estar explícito.
- **Pessoas diferentes querem coisas diferentes (e às vezes conflitantes).** O médico quer acesso rápido e amplo ao histórico do paciente. O time de compliance quer o mínimo de acesso possível, restrito ao necessário. Os dois estão certos do próprio ponto de vista — é trabalho da elicitação descobrir esse conflito **antes** de construir o sistema, não depois.

**Técnicas comuns de elicitação**

Não tem uma fórmula mágica, mas existem técnicas que ajudam a puxar essa informação de forma mais estruturada:

- **Entrevista**: conversa direta com o stakeholder, com perguntas abertas ("me conta como é seu dia a dia hoje, sem o sistema") e específicas ("com que frequência você precisa consultar exame de outro médico?").
- **Observação (ou "shadowing")**: acompanhar a pessoa fazendo o trabalho de verdade, no dia a dia, porque muitas vezes ela faz coisas no automático que nem lembraria de mencionar numa entrevista.
- **Questionário**: útil quando você precisa ouvir muita gente (ex: todos os médicos do hospital), mas perde profundidade comparado à entrevista.
- **Análise de documentos existentes**: olhar processos, formulários, sistemas antigos que já existem — muita coisa que o sistema precisa fazer já está "escondida" em como o processo funciona hoje, mesmo sem sistema nenhum.
- **Prototipagem**: mostrar algo tangível (mesmo que simples, tipo uma tela desenhada) pra pessoa reagir. Muita gente só consegue dizer "não é bem isso que eu queria" quando vê algo concreto na frente — é mais fácil reagir do que imaginar do zero.

**Um princípio importante: requisito implícito vs. explícito**

```text
Requisito explícito = o que o stakeholder disse com todas as letras
Requisito implícito = o que ele espera, mas nem pensou em dizer, 
                       porque pra ele é "óbvio demais"
```

Boa parte do trabalho de elicitação é **transformar requisito implícito em explícito** — porque um requisito que não está escrito não pode ser verificado, testado, nem cobrado depois. "Óbvio" pra quem pediu não é óbvio pra quem constrói.

**Conflito de stakeholders: o caso mais delicado**

Voltando ao hospital: imagina esses três grupos com necessidades diferentes sobre o mesmo sistema:

- **Médico**: "quero acesso rápido, sem burocracia, a qualquer dado do paciente que eu atenda."
- **Time de TI/segurança**: "quero controle rígido de quem acessa o quê, com log de tudo."
- **Paciente** (ou o hospital representando o interesse dele): "quero que meu dado só seja visto por quem realmente precisa."

Nenhum dos três está errado. O trabalho de elicitação (e depois de especificação, que é a próxima aula) é **negociar** esse conflito, geralmente encontrando um meio-termo documentado — não ignorando nenhum lado.

**Conectando com o que você já sabe**

Repara que essa negociação vira, na prática, exatamente os requisitos não-funcionais de segurança/autorização que você viu na aula 2 — e que depois viram, na arquitetura (aula 5 da parte anterior), a camada de autenticação/autorização de verdade. O caminho completo é: **elicitação descobre o conflito → especificação documenta a regra → arquitetura implementa a regra em código.** Cada disciplina passa o bastão pra próxima.

---

Ficou claro o processo? Próxima aula: **Especificação** — como pegar tudo que foi descoberto na elicitação e documentar de um jeito claro, sem ambiguidade, e que dê pra testar depois.