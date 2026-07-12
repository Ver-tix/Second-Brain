---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 5 — Autenticação e Autorização

Essas duas palavras parecem sinônimo, mas são coisas bem diferentes — e a confusão entre elas é uma das causas mais comuns de falha de segurança em sistemas reais. Vale a pena fixar bem.

**Autenticação: "quem é você?"**

É o processo de confirmar a identidade de quem está usando o sistema. Login e senha, biometria, token de acesso — tudo isso é autenticação. A pergunta que ela responde é só uma: **você é mesmo quem diz que é?**

**Autorização: "o que você pode fazer?"**

É diferente. Depois que o sistema já sabe quem você é, ele precisa decidir **o que essa pessoa específica tem permissão de ver ou fazer**. Um médico autenticado pode ver o prontuário de qualquer paciente dele. Um paciente autenticado só pode ver o próprio prontuário. Os dois estão autenticados (o sistema sabe quem são), mas têm autorizações completamente diferentes.

```text
Usuário faz login

↓

AUTENTICAÇÃO: "você é o Dr. João Silva, CRM 12345? confirmado."

↓

Usuário pergunta: "qual o resultado do exame do paciente Maria?"

↓

AUTORIZAÇÃO: "Dr. João Silva pode ver dados da paciente Maria? 
             ele é o médico responsável por ela? sim → libera
             não é → nega, mesmo estando autenticado"
```

Repara: autenticação acontece **uma vez** (no login). Autorização acontece **toda vez** que alguém pede pra ver ou fazer algo específico.

**Onde isso vive na arquitetura**

Lembra da divisão em camadas da aula 2? Autenticação e autorização moram **na camada de lógica/aplicação**, nunca na camada de apresentação e nunca no LLM.

Isso é crítico especialmente com LLM, porque o modelo **não tem noção nenhuma de quem está falando com ele**. Se você simplesmente jogar a pergunta "qual o resultado do exame da Maria?" direto pro LLM sem passar por uma checagem antes, ele vai tentar responder — porque ele não sabe (e não tem como saber) se quem perguntou tem o direito de saber isso.

```text
❌ ERRADO:
Usuário → LLM (com acesso direto ao banco) → Resposta

✅ CERTO:
Usuário → Aplicação verifica autenticação e autorização → 
         só então busca o dado → só então monta contexto pro LLM → Resposta
```

**Conectando com os desafios que você já resolveu**

No desafio do hospital, quando você escreveu "o LLM não deveria participar da decisão de quem pode ver o quê" — isso é exatamente autorização acontecendo na camada certa. Você já tinha essa intuição certa mesmo sem saber o nome técnico.

**E com agente de IA (lembrando a aula 3)?**

Mesmo quando um agente decide sozinho quais ferramentas chamar e em que ordem, a checagem de autorização **não pode ser delegada ao agente**. Isso é regra fixa, testável, que fica em código de verdade — nunca em uma instrução tipo "só responda se o usuário tiver permissão" dentro do prompt. Um prompt pode ser ignorado ou manipulado (isso se chama prompt injection, você provavelmente vai ver isso em algum módulo futuro); uma checagem de autorização em código, não.
