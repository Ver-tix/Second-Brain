---
tags:
  - programação
  - IA
---
## Aula 2 — Separação em camadas

Se na aula 1 o princípio foi "cada responsabilidade mora em um lugar", agora vem a pergunta prática: **quais lugares?**

A resposta clássica, usada há décadas em praticamente todo sistema, são três camadas:

```text
Apresentação (o que o usuário vê e usa)

↓

Lógica / Aplicação (as regras, as decisões)

↓

Dados (onde tudo fica guardado de verdade)
```

Vamos destrinchar cada uma com exemplo do hospital, que você já conhece.

**Camada de Apresentação**

É a parte que interage com quem está usando o sistema. Uma tela, um chat, um app. No seu caso, é a interface onde o médico digita "qual o resultado do exame do paciente X".

Essa camada **não deveria tomar nenhuma decisão importante**. Ela só recebe a pergunta e mostra a resposta. Se ela começar a decidir "ah, esse médico pode ver isso", você já perdeu a separação — a regra de autorização vazou pro lugar errado.

**Camada de Lógica (ou Aplicação)**

É o cérebro. É aqui que mora tudo que você já viu nos desafios:

- Verificar se o usuário tem permissão
- Decidir se precisa consultar banco de dados
- Decidir se chama o LLM ou não
- Montar o contexto certo
- Tratar erro

Essa é a camada que a gente chamou de "orquestrador" nos desafios anteriores. Orquestrador não é um conceito separado de arquitetura — ele **é** a camada de lógica, só que aplicada especificamente a sistemas com LLM.

**Camada de Dados**

É onde a verdade mora. Banco de dados com o resultado real do exame, a lista real de medicamentos, o histórico real do paciente. Essa camada só sabe guardar e devolver dado — ela não interpreta nada, não decide nada.

**Por que separar assim?**

Pega o exemplo do hospital: se amanhã a política de segurança do hospital mudar (ex: "agora precisa de dupla autenticação para ver resultado de exame"), essa mudança acontece **só na camada de lógica**. A tela não muda. O banco de dados não muda. Você mexe em um lugar só, e o resto do sistema nem percebe.

Se tudo estivesse misturado numa coisa só, essa mudança de regra poderia quebrar a tela, quebrar a busca no banco, quebrar tudo — porque ninguém saberia exatamente onde aquela regra "morava".

**Uma forma de enxergar isso:**

- Apresentação = a boca e os ouvidos (fala e escuta)
- Lógica = o cérebro (decide)
- Dados = a memória de longo prazo (guarda fatos)

Um corpo funciona porque essas três coisas são partes diferentes, com trabalhos diferentes. Ninguém quer que a boca decida o que vai ser lembrado, nem que a memória decida o que vai ser dito. Cada parte faz uma coisa.
