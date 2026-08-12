---
tags:
  - programação
  - IA
---
## Aula 6 — O mapa mental completo (juntando tudo)

Essa é a aula de fechar o ciclo. Vamos pegar as cinco peças que você já aprendeu e montar o quadro completo que serve pra resolver qualquer desafio novo do seu curso.

**As peças que você já tem:**

1. Arquitetura = decidir onde cada responsabilidade mora
2. Camadas = apresentação, lógica, dados
3. Cliente-servidor/API = como as camadas se comunicam, e o LLM é só mais um servidor chamado via API
4. Banco de dados = relacional (dado exato) ou vetorial (busca por significado, via RAG)
5. Autenticação/autorização = quem é você, e o que você pode ver

**O mapa mental unificado:**

```text
Usuário faz uma pergunta
         ↓
┌─────────────────────────────────────┐
│  CAMADA DE APRESENTAÇÃO              │
│  Só recebe a pergunta, não decide nada│
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│  CAMADA DE LÓGICA (orquestrador)     │
│                                       │
│  1. Quem é essa pessoa? (autenticação)│
│  2. Ela pode pedir isso? (autorização)│
│  3. Que tipo de pergunta é essa?      │
│     → genérica → pode ir direto ao LLM│
│     → específica → precisa buscar dado│
│  4. Se precisa buscar:                │
│     → dado exato → banco relacional   │
│     → texto/significado → banco vetorial (RAG)│
│  5. Monta o contexto certo, só com o  │
│     que é permitido e relevante       │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│  LLM (chamado via API)               │
│  Recebe contexto pronto e confiável  │
│  Só escreve/organiza/traduz          │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│  CAMADA DE LÓGICA (de novo)          │
│  Revisa a resposta antes de entregar │
└─────────────────────────────────────┘
         ↓
Resposta final ao usuário
```

**Por que esse mapa resolve praticamente qualquer desafio do seu curso:**

Repara que os dois desafios que você já resolveu (universidade e hospital) são só esse mesmo mapa aplicado com nomes diferentes:

|Peça do mapa|Universidade|Hospital|
|---|---|---|
|Autenticação|Aluno logado|Médico logado|
|Autorização|Aluno só vê a própria nota|Médico só vê paciente sob sua responsabilidade|
|Dado exato (relacional)|Nota, calendário|Resultado de exame, medicamento|
|Texto/significado (RAG)|Regulamento interno|Protocolo médico, bula|
|LLM só escreve|Explica um regulamento em linguagem simples|Explica diagnóstico pra leigo|

Sempre que você receber um desafio novo, a receita é: **pega a pergunta do enunciado e encaixa ela nesse mapa, peça por peça.** Pergunte pra você mesmo: essa informação é autenticação, autorização, dado exato, texto pra buscar por significado, ou trabalho de escrita do LLM? Na maioria das vezes, a resposta certa "cai" naturalmente quando você faz essa pergunta pra cada parte do enunciado.

**Um resumo de vocabulário pra você não travar na prova/desafio:**

- **Orquestrador** = a camada de lógica, focada em decidir o fluxo quando tem LLM envolvido
- **Agente** = orquestrador usando o LLM em loop pra decidir passos
- **RAG** = buscar texto relevante num banco vetorial antes de mandar pro LLM
- **Autenticação** = "quem é você"
- **Autorização** = "o que você pode ver/fazer"
