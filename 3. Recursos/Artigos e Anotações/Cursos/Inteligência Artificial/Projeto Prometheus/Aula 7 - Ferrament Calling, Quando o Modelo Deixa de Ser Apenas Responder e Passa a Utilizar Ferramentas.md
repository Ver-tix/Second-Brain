---
tags:
  - inteligenciaartificial
  - programação
---


Até agora vimos uma ideia muito importante:

> **O conhecimento não precisa estar dentro do modelo.**

Foi exatamente isso que o RAG resolveu.

<h3 align="center">Mas agora surge outro problema: </h3>
Imagine que um usuário pergunte:

> "Qual é a cotação atual do dólar?"

Mesmo usando RAG, isso continua ruim. Por quê?

Porque essa informação muda a cada segundo.

Outro exemplo. O usuário pergunta:

> "Agende uma reunião amanhã às 14h."

Um LLM consegue escrever um ótimo texto dizendo:

> "Claro! A reunião foi agendada."

Mas. Ele realmente agendou? Não. Ele apenas gerou texto.

---

## A grande limitação

Até agora nosso fluxo era:

```text
Usuário
   ↓
  LLM
   ↓
 Texto
```

Mesmo quando usamos RAG:

```text
Usuário
   ↓
Busca em documentos
   ↓
  LLM
   ↓
 Texto
```

<h3 align="center">Ainda existe apenas uma saída:<br>
texto. </h3>

Mas sistemas reais precisam fazer mais. Precisam:
- consultar APIs;
- acessar bancos de dados;
- enviar e-mails;
- criar arquivos;
- fazer cálculos;
- consultar agenda;
- chamar outros sistemas.

<h4 align="center">O LLM sozinho não faz nada disso.</h4>

---

# Surge então o Tool Calling

<h3 align="center">Em vez de apenas responder, o modelo passa a poder dizer:
<i>"Para responder corretamente, preciso utilizar esta ferramenta."</i></h3>

O fluxo muda completamente.

```text
Usuário

↓

LLM analisa o pedido

↓

Ferramenta necessária?

↓

SIM

↓

Executar ferramenta

↓

Resultado

↓

LLM gera resposta
```

Observe uma mudança importante.

O LLM continua sem acessar a internet sozinho.

Continua sem abrir banco de dados.

Continua sem acessar APIs.

Quem faz isso continua sendo a aplicação.

O modelo apenas indica:

> "Agora chame esta ferramenta."

---

# Analogia: o arquiteto e os especialistas

Imagine um arquiteto construindo uma casa.

Ele não:
- fabrica cimento;
- instala fiação;
- pinta paredes.

Mas ele sabe quando chamar:
- o eletricista;
- o encanador;
- o pedreiro.

O LLM faz exatamente isso.

Ele não executa.

Ele coordena.

---

# Um exemplo completo

Usuário:

> "Qual foi meu faturamento do mês passado?"

Fluxo:

```text
Usuário

↓

LLM entende:
"preciso consultar ERP"

↓

Aplicação consulta ERP

↓

ERP retorna:

R$ 428.000

↓

LLM recebe:

"O faturamento foi R$ 428.000."

↓

LLM responde:

"No mês passado sua empresa faturou R$ 428.000. Comparado ao mês anterior houve crescimento de 8%."
```

Perceba:

O LLM nunca soube o faturamento.

Ele apenas utilizou a ferramenta correta.

---

# Outro exemplo

Usuário:

> "Marque uma consulta para sexta-feira."

Fluxo:

```text
Usuário

↓

LLM

↓

Ferramenta Agenda()

↓

Agenda confirma horário

↓

LLM responde
```

Sem ferramenta...

O modelo apenas fingiria.

Com ferramenta...

A consulta realmente acontece.

---

# Onde entra o orquestrador?

Lembra que falamos dele nas aulas anteriores?

Agora ele ganha ainda mais importância.

```text
Usuário

↓

Orquestrador

↓

LLM

↓

"Use ferramenta X"

↓

Orquestrador

↓

Executa ferramenta

↓

Resultado

↓

LLM

↓

Resposta final
```

O LLM nunca chama diretamente uma API.

Quem faz isso é o orquestrador.

---

# Conexão com tudo o que você já estudou

Veja como os conceitos estão convergindo:

- **Prompt Engineering:** ensina o modelo a interpretar corretamente a intenção do usuário.
    
- **RAG:** fornece conhecimento atualizado e contextual.
    
- **Tool Calling:** permite agir sobre sistemas externos.
    
- **Orquestração:** decide como tudo isso se conecta.
    
- **Agentes (próxima aula):** adicionam planejamento e execução de múltiplas etapas.
    

Perceba como cada módulo resolveu uma limitação específica do anterior.

---

# Princípio Prometheus XLV

> **Um LLM não se torna mais inteligente ao ganhar ferramentas; ele se torna mais útil. O conhecimento continua vindo dos pesos e do contexto, mas a capacidade de agir passa a depender da arquitetura do sistema ao seu redor.**

---

# Desafio Prometheus #007

## Questão 1

Explique:

> **Por que Tool Calling não transforma o LLM em um programa que executa ações diretamente?**

Utilize os conceitos de:

- separação de responsabilidades;
- orquestrador;
- ferramentas;
- segurança.

---

## Questão 2

Imagine que você está projetando um assistente para uma corretora de investimentos.

Ele poderá:

- consultar cotações em tempo real;
- calcular risco de carteira;
- enviar ordens de compra;
- explicar conceitos financeiros.

Projete a arquitetura respondendo:

1. Quais tarefas devem ser executadas exclusivamente pela aplicação?
2. Quais tarefas agregam valor quando realizadas pelo LLM?
3. Em que momento o orquestrador deve intervir?
4. Por que permitir que o modelo execute diretamente uma ordem de compra seria uma decisão arquitetural perigosa?

---

💡 **Uma observação pessoal, como seu professor neste projeto:** acredito que você vai gostar muito das próximas duas aulas. Desde o início do Prometheus, percebo que você tem uma inclinação natural para pensar em fluxos, módulos e responsabilidades. Ferrament Calling e Agentes são justamente a materialização dessas ideias em software. Tenho a impressão de que muitas peças que pareciam isoladas até agora começarão a formar um único sistema coerente na sua mente.