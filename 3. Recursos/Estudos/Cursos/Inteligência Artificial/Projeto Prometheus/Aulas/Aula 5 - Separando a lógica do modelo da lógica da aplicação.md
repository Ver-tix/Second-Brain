---
tags:
  - IA
---
Na aula passada vimos o conceito de orquestração.

Hoje veremos um princípio ainda mais importante:

> **O LLM não deve tomar decisões que pertencem à aplicação.**

Essa frase parece simples.

Mas ela separa aplicações amadoras de aplicações profissionais.

---

# Um exemplo

Imagine que você esteja construindo um assistente para uma empresa.

O usuário escreve:

> "Quero atualizar meu endereço."

A primeira ideia de muitos desenvolvedores é:

```text
Usuário
    ↓
LLM
    ↓
Banco de Dados
```

Mas existe um problema.

Quem decidiu que essa operação era permitida?

O modelo?

Não deveria.

---

# A arquitetura correta

O fluxo correto é:

```text
Usuário

↓

Aplicação

↓

Autenticação

↓

Autorização

↓

Validação

↓

Banco de Dados

↓

LLM (se necessário)

↓

Resposta
```

Observe algo curioso.

Em alguns casos...

...o LLM aparece **depois** do banco.

---

# Por quê?

Imagine:

O banco retorna:

```text
Rua: Av. Santos Dumont

Número: 2500

Cidade: Fortaleza
```

O usuário não quer receber JSON.

Ele quer ler:

> "Seu endereço foi atualizado para Av. Santos Dumont, nº 2500, Fortaleza."

Quem transforma dados em linguagem natural?

O LLM.

Quem altera dados?

A aplicação.

Essa divisão de responsabilidades é essencial.

---

# O anti-pattern mais comum

Veja este prompt:

```text
Você é um assistente bancário.

Sempre verifique se o usuário pode acessar a conta.

Depois consulte o saldo.

Depois valide as permissões.

Depois...
```

Parece bom.

Mas existe um problema.

O modelo não consulta banco.

Não autentica.

Não controla permissões.

Você está pedindo para o modelo **imaginar** um processo que pertence ao sistema.

---

# A responsabilidade de cada camada

Pense assim:

## A aplicação responde:

- Quem é o usuário?
    
- Ele está autenticado?
    
- Ele pode acessar esse dado?
    
- Qual ferramenta deve ser utilizada?
    
- Qual documento deve ser consultado?
    
- O resultado foi encontrado?
    

---

## O LLM responde:

- Como explicar?
    
- Como resumir?
    
- Como organizar?
    
- Como adaptar a linguagem ao usuário?
    
- Como transformar dados em texto?
    

---

# Um princípio importante

Sempre pergunte:

> **Essa decisão depende de regras de negócio ou de linguagem?**

Se depende de regras de negócio...

→ Código.

Se depende de linguagem...

→ LLM.

---

# Exemplo

Pergunta:

> "Posso cancelar minha matrícula?"

A aplicação pode descobrir:

```text
Aluno inadimplente.

Regra:

Não pode cancelar.
```

Quem decide isso?

Código.

Depois o LLM recebe:

```text
Situação:

Aluno inadimplente.

Regra:

Cancelamento indisponível.

Explique isso educadamente.
```

Agora sim.

O modelo faz aquilo em que é excelente:

comunicação.

---

# Um padrão arquitetural

Sistemas modernos normalmente seguem algo parecido com:

```text
Entrada

↓

Validação

↓

Classificação

↓

Escolha da ferramenta

↓

Execução

↓

LLM

↓

Formatação

↓

Saída
```

Perceba que o LLM aparece quase no final.

Isso surpreende muitos iniciantes.

---

# 📜 Princípio LXX

> **Quanto mais crítica for uma decisão, menos ela deve depender exclusivamente do modelo.**

---

# Desafio Prometheus #005

Imagine que você precisa desenvolver um assistente para um hospital.

Ele receberá perguntas como:

- "Qual o resultado do exame do paciente?"
- "Quais medicamentos ele utiliza?"
- "Explique esse diagnóstico para um leigo."
- "Quais exames esse paciente realizou nos últimos seis meses?"

Responda:
1. Em quais momentos o LLM **não deveria participar da decisão**?
2. Quais informações deveriam ser obtidas exclusivamente pela aplicação?
3. Em quais etapas o modelo agregaria valor?
4. Como você dividiria as responsabilidades entre aplicação e modelo para reduzir riscos?

Não escreva código.

Quero que você projete o fluxo da informação.

[[🛠️ Desafio M4 005]]

---

## Reflexão final

Você talvez tenha percebido que, nas últimas aulas, quase não falamos sobre prompts.

Isso é intencional.

Existe um erro muito comum na comunidade de IA: acreditar que sistemas inteligentes são construídos apenas com prompts sofisticados.

Na prática, os melhores sistemas são resultado da combinação de:

- boa arquitetura;
    
- regras de negócio bem definidas;
    
- uso criterioso de ferramentas;
    
- e um LLM utilizado exatamente onde ele gera mais valor.
    

Se eu tivesse que resumir sua evolução desde o Módulo 1, diria que você deixou de perguntar **"como conversar com um modelo?"** e começou a perguntar **"como construir um sistema em que o modelo é apenas uma parte?"**.

Essa mudança de perspectiva é um dos maiores marcos na formação de um engenheiro de IA.