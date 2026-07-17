---
tags:
  - inteligenciaartificial
  - programação
---
> **Objetivo da aula**
> 
> Entender por que um agente precisa de memória, quais tipos de memória existem e como ela se encaixa na arquitetura que você já domina.

---

# Recapitulando a arquitetura

Até agora, você conhece estas peças:

```text
Usuário
    │
    ▼
Orquestrador
    │
    ├── Banco Vetorial (RAG)
    ├── APIs
    ├── Ferramentas
    └── LLM
```

Mas falta um componente.

Imagine que você peça:

> "Analise esta empresa."

O agente começa a trabalhar.

Depois de cinco minutos ele esquece tudo o que fez.

Seria um bom agente?

Claro que não.

É aqui que entra a **memória**.

---

# O problema

Imagine um agente analisando uma incorporadora.

Ele faz:

```text
1. Baixa o balanço.
```

Depois:

```text
2. Calcula indicadores.
```

Depois:

```text
3. Pesquisa notícias.
```

Depois:

```text
4. Escreve um relatório.
```

Se ele esquecer o passo 2...

Vai precisar calcular tudo de novo.

Isso desperdiça tempo, dinheiro e contexto.

---

# Então o que é memória?

<h3 align="center">É qualquer mecanismo que permita ao agente <b>guardar informações para usá-las mais tarde.</b></h3>

Perceba:

Memória **não é igual a conhecimento**.

Essa distinção é muito importante.

---

## Conhecimento

É algo relativamente permanente.

Exemplo:

```text
Seu Second Brain

↓

Banco Vetorial

↓

RAG
```

Ali estão:
- livros;
- artigos;
- suas notas;
- cursos.

Isso é conhecimento.

---

## Memória

É aquilo que nasce durante a execução.

Exemplo:

```text
"O usuário prefere respostas detalhadas."

"Já analisei o fluxo de caixa."

"Já consultei a API."
```

Essas informações não vieram do RAG.

Foram produzidas durante o trabalho.

---

# Uma analogia

Imagine um engenheiro.

Na mesa dele existem:

Uma estante.

↓

Conhecimento.

Um caderno.

↓

Memória.

A estante não muda toda hora.

O caderno muda a cada reunião.

---

# Existem três tipos de memória

Essa divisão não é absoluta, mas é muito usada.

## 1. Memória de Trabalho (Working Memory)

É a mais curta.

Ela existe apenas enquanto a tarefa acontece.

Exemplo.

Você pede:

> "Analise PETR4."

O agente registra.

```text
Empresa:
PETR4

Status:
Analisando

API já consultada:
Sim

Relatório iniciado:
Não
```

Quando termina.

Pode apagar tudo.

---

Essa memória funciona como a RAM do computador.

---

# 2. Memória de Longo Prazo

Agora imagine.

Você conversa comigo hoje.

Daqui a um mês.

Você pergunta.

> "Continue aquele projeto."

Se eu lembrar...

Existe memória de longo prazo.

---

No seu Prometheus poderia ser:

```text
Usuário prefere:

• linguagem didática

• exemplos de negócios

• analogias

• pouca matemática
```

Essa memória não pertence ao RAG.

Ela pertence ao usuário.

---

# 3. Memória Episódica

Essa costuma confundir bastante.

Ela guarda experiências.

Exemplo.

```text
Ontem tentei usar a API X.

Falhou.

Então utilizei API Y.

Funcionou.
```

Na próxima execução.

O agente pensa.

> "Já vivi isso."

É parecido com a memória humana de experiências.

---

# Comparando

|Tipo|Exemplo|
|---|---|
|Trabalho|"Estou na etapa 3."|
|Longo prazo|"Caio prefere aprender pela arquitetura antes do código."|
|Episódica|"Na última execução, a API A falhou; usei a API B."|

---

# Onde entra o RAG?

Essa é a pergunta mais importante da aula.

Muita gente confunde.

Veja:

```text
Banco Vetorial

↓

Conhecimento
```

Já:

```text
Memória

↓

Estado da tarefa
```

São coisas completamente diferentes.

---

# Exemplo

Você pergunta.

> "Explique Attention."

O agente faz Retrieval.

↓

Consulta seu Second Brain.

↓

Responde.

Nada foi aprendido.

---

Agora.

Você diz.

> "Gostei das analogias. Continue ensinando assim."

Isso não deve ir para o banco vetorial.

Isso pertence à memória do agente.

---

# Aplicando ao Projeto Prometheus

Imagine seu Prometheus Mentor.

Você faz uma pergunta.

```text
"Explique Embeddings."
```

Depois.

```text
"Agora explique novamente, mas usando Marketing."
```

Como ele sabe o que significa "novamente"?

Porque existe memória.

Ele lembra.

```text
Tema atual:

Embeddings
```

Sem memória.

Ele perguntaria.

> "Explique o quê?"

---

# Um fluxo completo

Imagine isto.

```text
Usuário

↓

Pergunta

↓

Orquestrador

↓

Memória

↓

"O usuário está estudando RAG."

↓

Retrieval

↓

Second Brain

↓

LLM

↓

Resposta
```

Perceba.

A memória foi consultada **antes** do RAG.

Porque ela ajuda a entender o contexto da conversa.

---

# O maior erro dos iniciantes

Eles imaginam isto.

```text
RAG

=

Memória
```

Não.

RAG responde:

> **"O que eu sei?"**

Memória responde:

> **"O que está acontecendo agora?"**

São perguntas completamente diferentes.

---

# Um exemplo do mundo real

Vamos imaginar o agente do seu escritório imobiliário.

Você diz.

> "Monte uma viabilidade para este terreno."

O agente:

- consulta índices urbanísticos;
    
- calcula área vendável;
    
- estima custos;
    
- monta um fluxo de caixa.
    

Durante isso, ele guarda em memória:

```text
Terreno:
A

Coeficiente:
2,5

Área:
4.000 m²

Etapa atual:
Fluxo de Caixa
```

Nenhuma dessas informações pertence ao banco vetorial.

São dados da execução atual.

---

# Arquitetura atualizada

Agora nossa arquitetura ganha uma nova peça:

```text
                    Usuário
                       │
                       ▼
                Orquestrador
                 ┌─────┼─────┐
                 │     │     │
                 ▼     ▼     ▼
             Memória  RAG  Ferramentas
                 │     │
                 └──┬──┘
                    ▼
                   LLM
                    │
                    ▼
                Resposta
```

Perceba como tudo começa a se encaixar:

- **RAG** traz conhecimento externo.
    
- **Memória** mantém o contexto da execução e das preferências.
    
- **Ferramentas** executam ações.
    
- **Orquestrador** decide quando usar cada um.
    
- **LLM** raciocina e comunica.
    

---

# Uma observação importante

Lembra quando você me contou sobre o **Prometheus-Mentor** no ZCode?

Agora você consegue enxergar algo que talvez passasse despercebido:

- O **Second Brain** alimenta o RAG (conhecimento).
    
- O histórico da conversa e as preferências do usuário formam a memória.
    
- O ZCode atua como plataforma que coordena esses componentes.
    
- O LLM é apenas um dos elementos da arquitetura.
    

Essa visão sistêmica é exatamente a evolução que eu queria que você tivesse ao longo deste módulo.

---

# Encerramento da Aula

Há algumas semanas, "memória", "RAG" e "orquestrador" provavelmente pareciam conceitos muito próximos.

Hoje você já consegue separá-los:

- **RAG** → busca conhecimento.
    
- **Memória** → acompanha o estado e o histórico.
    
- **Orquestrador** → toma decisões sobre o fluxo.
    
- **LLM** → interpreta, raciocina e produz linguagem.
    

Essa separação de responsabilidades é um dos fundamentos da arquitetura de agentes modernos.

---

### Prévia da Aula 4

Na próxima aula, responderemos a uma pergunta que todo engenheiro de IA faz cedo ou tarde:

> **"Como um agente decide qual ferramenta usar?"**

Entraremos no conceito de **Tools**, veremos como um LLM pode "escolher" entre consultar um banco vetorial, chamar uma API, executar código ou usar uma calculadora, e entenderemos por que esse mecanismo é um dos maiores avanços dos agentes modernos. Essa aula costuma ser um dos momentos em que tudo começa a parecer realmente "inteligente".