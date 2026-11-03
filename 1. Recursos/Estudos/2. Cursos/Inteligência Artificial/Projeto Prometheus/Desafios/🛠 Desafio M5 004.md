---
tags:
  - IA
  - programação
---
```xml
<questoin_set>
Desafio da Aula 4

Imagine que você precisa projetar um **Agente Comercial** para uma incorporadora imobiliária.

Esse agente deverá atender um potencial investidor.

Ele deve ser capaz de:

- responder dúvidas sobre um empreendimento;
    
- consultar a disponibilidade de unidades;
    
- calcular uma simulação de investimento;
    
- enviar uma proposta por e-mail;
    
- agendar uma reunião com um consultor humano.
    

Responda, **sem escrever código**:

1. Quais ferramentas seriam necessárias para esse agente?
    
2. Quais tarefas ficariam sob responsabilidade do LLM?
    
3. Quais tarefas seriam executadas exclusivamente pelas ferramentas?
    
4. Desenhe o fluxo da informação, desde a pergunta do investidor até a resposta final.
</question_set>

<answer_set>
<ol>
<li>Base de dados (dúvidas sobre o empreendimento e consulta da disponibilidade de unidades); Planilhas - Excel ou google sheets, por exemplo - (simulaçãode investimento); e-mail (obviamente para enviar o e-mail) e algum aplcativo de calendário para marcar a reunião
</li>
<li>
Responder dúvidas sobre o emprendimento, calcular simulação de investimento, enviar uma proposta de e-mail e agendar uma reunião com um consultor humano
</li>
<li>Consultar a disponibilidade de unidades (banco de dados pode fazer isso sem uma IA)</li>
<li>Está no fluxograma abaixo:</li>
</ol>
</answer_set>
```

![[Pasted image 20260717095718.png]]

---
# Feedback
Professor corrigindo. 😄

Antes de tudo: **parabéns**. Eu consigo perceber uma mudança importante em relação aos exercícios do início do curso. Antes você tendia a responder "o que faz o agente". Agora você começa a separar **responsabilidades arquiteturais**, que era exatamente o objetivo da aula.

Vamos à correção.

---

# Questão 1 — Ferramentas

Sua resposta:

- Banco de dados
    
- Planilhas
    
- E-mail
    
- Calendário
    

## Correção

**Nota: 9,5/10**

Muito boa.

Eu acrescentaria apenas duas ferramentas:

- **Banco Vetorial (RAG)** → para responder perguntas sobre o empreendimento utilizando materiais comerciais, memorial descritivo, regulamento, FAQ etc.
    
- **CRM** → para registrar que aquele investidor entrou em contato, em que estágio está e quais ações já foram realizadas.
    

Então o ecossistema ficaria algo como:

```text
Banco Vetorial (RAG)

↓

Banco de Dados

↓

Calculadora / Planilha

↓

CRM

↓

E-mail

↓

Calendário
```

---

# Questão 2 — Responsabilidade do LLM

Aqui aconteceu uma confusão muito comum.

Você respondeu:

> Responder dúvidas, calcular simulação, enviar e-mail e agendar reunião.

## Correção

**Nota: 6/10**

O LLM **não envia e-mails**.

O LLM **não agenda reuniões**.

O LLM **não calcula investimentos diretamente** (em um sistema profissional).

Ele faz outra coisa.

Por exemplo:

- interpretar a intenção do usuário;
    
- decidir quais ferramentas usar;
    
- explicar o empreendimento;
    
- escrever o texto do e-mail;
    
- interpretar os resultados das ferramentas;
    
- responder em linguagem natural.
    

Observe a diferença.

### O LLM escreve:

> "Segue sua proposta de investimento..."

Mas quem envia é:

```text
Ferramenta de E-mail
```

---

Outro exemplo.

O LLM pode dizer:

> "Preciso calcular uma TIR."

Mas quem realmente faz o cálculo deveria ser:

```text
Python

ou

Excel

ou

Serviço Financeiro
```

Porque são determinísticos e muito mais confiáveis para cálculos.

---

# Questão 3 — Ferramentas

Você respondeu:

> Consultar disponibilidade de unidades.

## Correção

**Nota: 5/10**

Aqui faltaram algumas responsabilidades.

As ferramentas executariam:

- consultar disponibilidade;
    
- realizar cálculos financeiros;
    
- enviar e-mail;
    
- criar evento no calendário;
    
- registrar interação no CRM;
    
- consultar banco de dados.
    

Perceba uma regra prática.

Sempre pergunte:

> **"Isso altera ou consulta o mundo real?"**

Se sim...

Provavelmente é trabalho de uma ferramenta.

---

# Questão 4 — Fluxograma

Infelizmente a imagem incorporada ao Markdown não veio junto no upload, então eu não consegui visualizar seu fluxograma. No arquivo aparece apenas a referência:

```text
![[Pasted image 20260717095718.png]]
```

Então **não vou descontar pontos** nessa questão. Se quiser, envie apenas a imagem depois que eu faço a análise.

---

# O aprendizado mais importante

Quero insistir numa ideia porque ela será recorrente durante todo o curso.

Você ainda pensa um pouco assim:

```text
LLM

↓

Faz tudo
```

Mas a arquitetura correta é:

```text
LLM

↓

Decide

↓

Ferramenta executa

↓

LLM interpreta

↓

Ferramenta executa

↓

LLM responde
```

Essa diferença parece pequena.

Mas ela separa um chatbot de um agente.

---

# Nota Final

|Questão|Nota|
|---|--:|
|Ferramentas|9,5|
|Responsabilidade do LLM|6,0|
|Responsabilidade das Ferramentas|5,0|
|Fluxograma|Não avaliado (imagem ausente)|

**Nota parcial:** **8,2/10**

---

# Feedback do professor

Quero destacar uma evolução que me deixou bastante satisfeito.

Há algumas semanas você provavelmente responderia:

> "O agente faz isso."

Agora você naturalmente começou a responder:

> "Banco de dados faz isso."
> 
> "Planilha faz aquilo."
> 
> "Calendário faz aquilo outro."

Isso significa que você está começando a **enxergar um sistema distribuído**, e não apenas "uma IA". Essa mudança de mentalidade é um dos maiores objetivos deste módulo.

O que ainda precisamos refinar é a fronteira entre **decidir** e **executar**:

- **LLM:** interpreta, raciocina, planeja e comunica.
    
- **Ferramentas:** executam operações concretas (consultar, calcular, enviar, registrar, agendar).
    
- **Orquestrador:** coordena quem entra em ação e em que ordem.
    

Essa separação vai aparecer repetidamente nas próximas aulas, e tenho certeza de que em pouco tempo ela ficará completamente natural para você.

---
# Feedback 2

Primeiro: **seu fluxograma ficou excelente.**

Na verdade, depois de vê-lo, eu mudaria a nota da Questão 4 para **10/10**.

O que eu gostei nele:

- ✅ Existe um ponto de entrada (pergunta do investidor).
    
- ✅ O orquestrador consulta o LLM para decidir.
    
- ✅ A ferramenta retorna **dado bruto**, e não uma resposta pronta (essa observação é muito madura).
    
- ✅ O LLM interpreta o resultado antes de responder.
    
- ✅ Você ainda colocou um loop de repetição caso sejam necessárias várias ferramentas.
    

Esse último detalhe é exatamente o que acontece em muitos agentes modernos.

---

Agora vamos ao que realmente importa.

Você disse:

> "Fiquei abaixo da média em duas questões, e gostaria que me explicasse melhor."

**Excelente postura.** É exatamente assim que quero que você encare os desafios. A nota é apenas um indicador; o aprendizado vem de entender o erro.

---

# Questão 2 — O que é responsabilidade do LLM?

Você respondeu:

> "Responder dúvidas, calcular simulação, enviar proposta por e-mail e agendar reunião."

Vamos analisar cada uma.

---

## Responder dúvidas

✅ Correto.

Exemplo:

Usuário:

> "Qual a diferença entre a Torre A e a Torre B?"

O LLM pode:

- interpretar a pergunta;
    
- consultar o RAG;
    
- produzir uma explicação.
    

Perfeito.

---

## Calcular uma simulação

Aqui está o ponto mais importante.

Imagine que o usuário diga:

> "Tenho R$ 500.000. Monte uma projeção de investimento."

O LLM sabe matemática?

Sim.

Mas...

**Devemos deixá-lo fazer isso?**

Na maioria dos casos...

**Não.**

---

Por quê?

Imagine que o GPT faça:

```text
Taxa interna de retorno

13,48%
```

Como você prova isso?

Não consegue.

Agora imagine.

O LLM diz:

> "Preciso calcular."

↓

Chama Python.

↓

Python faz:

```python
np.irr(...)
```

↓

Retorna:

```text
13,482913%
```

↓

O LLM responde:

> "A taxa interna de retorno estimada é 13,48%."

Percebe?

O LLM continua sendo o cérebro.

Mas quem fez a conta foi uma ferramenta determinística.

---

## Enviar e-mail

Aqui está outra diferença enorme.

Imagine.

O GPT escreve:

```text
Prezado João,

Segue a proposta...
```

Ele fez o quê?

**Escreveu.**

Agora.

Quem enviou?

Ninguém.

Enviar é outra ação.

---

Imagine um computador sem internet.

O GPT consegue escrever um e-mail.

Mas não consegue enviá-lo.

Porque enviar exige:

```text
SMTP

ou

Gmail API

ou

Outlook API
```

Isso é ferramenta.

---

## Agendar reunião

Mesma ideia.

O GPT pode escrever:

> "Sugiro agendar para sexta às 15h."

Mas quem cria o evento?

Google Calendar.

Microsoft Calendar.

Calendly.

Ferramenta.

---

# Então o que faz o LLM?

Pense nele como um gerente.

O gerente diz:

> "Precisamos calcular."

Quem calcula?

O contador.

---

O gerente diz:

> "Precisamos enviar isso."

Quem envia?

A secretária.

---

O gerente diz:

> "Precisamos consultar o estoque."

Quem consulta?

O sistema.

---

O gerente continua pensando.

Mas ele não executa.

---

# Questão 3 — O que fazem as ferramentas?

Você respondeu apenas:

> Consultar disponibilidade.

Na verdade...

As ferramentas fazem praticamente todas as ações concretas.

Vou dar um exemplo completo.

---

Imagine que o usuário diga:

> "Gostei do apartamento 901. Me envie uma proposta e marque uma reunião."

O agente começa.

---

## Passo 1

LLM interpreta.

```text
Usuário quer:

- proposta

- reunião
```

---

## Passo 2

Ferramenta.

Consulta:

```text
Banco de Dados
```

↓

Apartamento disponível?

↓

Sim.

---

## Passo 3

Ferramenta.

Calculadora.

↓

Calcula financiamento.

---

## Passo 4

LLM.

Recebe os valores.

↓

Escreve uma proposta bonita.

---

## Passo 5

Ferramenta.

API do Gmail.

↓

Envia.

---

## Passo 6

Ferramenta.

Google Calendar.

↓

Agenda.

---

## Passo 7

LLM.

Informa.

> "Pronto! Sua proposta foi enviada e a reunião foi marcada."

---

Observe a alternância.

```text
LLM

↓

Ferramenta

↓

LLM

↓

Ferramenta

↓

LLM

↓

Ferramenta

↓

LLM
```

Essa alternância é praticamente a essência dos agentes modernos.

---

# Um teste mental

Vou te ensinar um truque que eu mesmo uso quando projeto agentes.

Sempre faça esta pergunta:

> **"Isso muda alguma coisa fora da cabeça do agente?"**

Se a resposta for **SIM**...

É ferramenta.

Exemplos:

- enviar e-mail ✅
    
- apagar arquivo ✅
    
- consultar banco ✅
    
- chamar API ✅
    
- criar reunião ✅
    
- executar Python ✅
    

Tudo isso modifica ou consulta o mundo externo.

---

Agora outra pergunta.

> **"Isso é raciocínio, interpretação ou comunicação?"**

Então normalmente é LLM.

Exemplos:

- interpretar intenção ✅
    
- resumir texto ✅
    
- explicar conceitos ✅
    
- escrever proposta ✅
    
- decidir qual ferramenta usar ✅
    

---

# Vou te mostrar uma "regra de ouro"

Ela vai aparecer várias vezes daqui para frente.

```text
LLM

PENSA
DECIDE
INTERPRETA
ESCREVE

↓

Ferramenta

CONSULTA
CALCULA
EXECUTA
MODIFICA
```

Se você decorar apenas essa tabela, já vai acertar a maioria das questões sobre arquitetura de agentes.

---

## Professor para aluno

Caio, vou te fazer um elogio que considero mais importante do que a nota.

Há cerca de um mês, você me fazia perguntas como:

> "Então o ChatGPT já não é o LLM?"

Hoje, no seu fluxograma, você desenhou naturalmente:

- entrada do sistema;
    
- orquestrador;
    
- decisão;
    
- ferramenta;
    
- retorno de dado bruto;
    
- LLM compondo a resposta.
    

Isso mostra que você já não está pensando em "uma IA que responde". Você está pensando em **um sistema composto por componentes**.

Essa mudança de mentalidade é uma das mais difíceis de adquirir em Engenharia de IA. O restante — Python, LangGraph, OpenAI Agents SDK, CrewAI — são ferramentas que você aprenderá depois para implementar exatamente essas ideias. O mais difícil é aprender a enxergar a arquitetura. E nisso, sinceramente, você está evoluindo muito.