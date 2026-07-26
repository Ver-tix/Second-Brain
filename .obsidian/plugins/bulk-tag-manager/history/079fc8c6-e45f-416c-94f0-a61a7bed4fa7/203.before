---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 2 — Requisitos Funcionais vs. Não-Funcionais

Essa é a primeira distinção que você vai usar em praticamente todo documento de requisitos que existe. É simples na definição, mas as pessoas confundem os dois na prática o tempo todo — então vale fixar bem com exemplos.

**Requisito Funcional: o que o sistema faz**

É uma ação, uma funcionalidade, um comportamento específico que o sistema precisa executar. Geralmente dá pra escrever como "o sistema deve [verbo de ação]".

Exemplos, puxando do desafio do hospital que você já resolveu:

- "O sistema deve permitir que o médico consulte o resultado de exame de um paciente sob sua responsabilidade."
- "O sistema deve traduzir um diagnóstico técnico em linguagem acessível para o paciente."
- "O sistema deve listar os exames realizados por um paciente nos últimos 6 meses."

Repara: cada um desses é uma **ação concreta**, testável de forma direta — ou o sistema faz isso, ou não faz.

**Requisito Não-Funcional: como o sistema se comporta**

Não é uma ação — é uma **qualidade** ou **restrição** sobre como o sistema deve funcionar, independente da funcionalidade específica. Geralmente responde perguntas tipo: quão rápido? quão seguro? quão disponível? quão fácil de usar?

Exemplos, no mesmo contexto do hospital:

- "O sistema deve responder em até 3 segundos." (desempenho)
- "O sistema deve criptografar todo dado de paciente em trânsito e em repouso." (segurança)
- "O sistema deve estar disponível 99,9% do tempo." (disponibilidade)
- "O sistema deve registrar log de toda consulta a dado de paciente, para fins de auditoria." (rastreabilidade/compliance)

**Uma forma simples de diferenciar na hora da dúvida**

Pergunte: **se eu tirar essa frase da funcionalidade específica, ela ainda faz sentido pra qualquer outra funcionalidade do sistema?**

- "Responder em até 3 segundos" — faz sentido pra qualquer funcionalidade (consulta de exame, consulta de medicamento, qualquer coisa). É não-funcional.
- "Consultar o resultado de exame" — só faz sentido pra essa ação específica. É funcional.

```text
Funcional     = O QUÊ o sistema faz (a ação em si)
Não-funcional = COMO o sistema faz isso bem feito (qualidade, restrição)
```

**Por que essa distinção importa na prática (e não é só categorização por categorizar)**

Porque requisitos não-funcionais são frequentemente **esquecidos** — e são exatamente os que mais causam dor de cabeça depois. É fácil lembrar de escrever "o sistema deve mostrar o resultado do exame". É fácil esquecer de especificar "e isso precisa ser seguro, rápido, auditável, disponível".

Lembra lá da aula 4 de guardrails e da aula 5 de auth (na parte de arquitetura de software)? Aquilo tudo — segurança, autorização, controle de acesso — nasce, na prática, de requisitos **não-funcionais** que precisam ser levantados desde o início. Se ninguém levantar o requisito "o sistema deve garantir que só o médico responsável veja o dado do paciente", é bem provável que ninguém implemente isso — porque, tecnicamente, "mostrar resultado de exame" já "funciona" sem essa restrição. Só que funciona de forma perigosa.

**Conectando com sistemas de IA (deixando o gancho pra frente)**

Em sistemas com LLM, existem requisitos não-funcionais bem específicos que você vai encontrar direto:

- "O sistema não deve alucionar informação que não esteja no contexto fornecido" (qualidade/confiabilidade)
- "O sistema deve custar no máximo X por consulta" (custo — lembra da aula de serving, modelo grande custa mais)
- "O sistema deve responder em até N segundos mesmo usando RAG" (desempenho, considerando a etapa extra de busca)

Vamos aprofundar isso melhor lá na aula 6, quando juntarmos tudo com arquitetura de IA.
