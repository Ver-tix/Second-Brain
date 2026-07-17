---
tags:
  - inteligenciaartificial
---
> **Objetivo da aula**
> 
> Entender como um agente "decide" qual ferramenta utilizar, sem que o programador escreva um `if/else` para cada situação.

---

# O problema

Imagine que você construiu um agente com estas ferramentas:

```text
✓ Banco Vetorial (RAG)

✓ Google Calendar

✓ Gmail

✓ Python

✓ Banco de Dados

✓ API Financeira
```

Agora o usuário pergunta:

> "Qual é o VGV estimado deste empreendimento?"

Como o agente sabe que deve usar Python?

---

Ou:

> "Marque uma reunião para sexta."

Como ele sabe que deve usar o Google Calendar?

---

Ou ainda:

> "Explique o conceito de Equity."

Como ele sabe que deve consultar o RAG?

---

# A solução intuitiva (e ruim)

O iniciante normalmente pensa em algo assim:

```python
if "reunião" in pergunta:
    usar_calendar()

elif "email" in pergunta:
    usar_email()

elif "investimento" in pergunta:
    usar_python()
```

Funciona?

Sim.

Escala?

Nem um pouco.

Imagine um agente com 150 ferramentas.

Esse código vira um pesadelo.

---

# A solução moderna

Hoje fazemos algo completamente diferente.

Em vez de dizer ao agente:

> "Se acontecer X, faça Y."

Nós apresentamos as ferramentas para o LLM.

Por exemplo.

```text
Ferramenta 1

Nome:
Calculadora Financeira

Descrição:
Calcula VPL, TIR, Fluxo de Caixa...

----------------------------

Ferramenta 2

Nome:
Calendário

Descrição:
Agenda reuniões e consulta eventos.

----------------------------

Ferramenta 3

Nome:
RAG

Descrição:
Consulta documentos internos.
```

Observe.

O mais importante aqui **não é o código**.

É a **descrição**.

---

# Pense em um novo funcionário

Imagine que um funcionário entrou hoje na empresa.

Você entrega um papel.

```
Departamento Financeiro

Responsável por:

- fluxo de caixa
- VPL
- TIR

-------------------

Departamento Comercial

Responsável por:

- clientes
- propostas

-------------------

Departamento Jurídico

Responsável por:

- contratos
```

Depois você pergunta.

> "Preciso analisar um fluxo de caixa."

O funcionário sabe para onde ir.

Ninguém escreveu:

```text
Se ouvir "fluxo de caixa", vá ao Financeiro.
```

Ele inferiu.

O LLM faz exatamente isso.

---

# O papel da descrição

Imagine duas ferramentas.

### Ferramenta A

```
Nome:
Tool_001
```

Fim.

---

### Ferramenta B

```
Calculadora Financeira

Executa:

- fluxo de caixa

- TIR

- VPL

- Payback

- Simulações
```

Qual delas o LLM entenderá melhor?

A segunda.

Por isso escrever boas descrições é uma habilidade extremamente importante.

---

# O fluxo

Imagine.

Usuário:

> "Calcule a TIR."

O LLM pensa.

```text
Existe alguma ferramenta
capaz de calcular?

↓

Sim.

↓

Usar Calculadora Financeira.
```

Depois.

A ferramenta devolve.

```
13,8%
```

O LLM continua.

> "A taxa interna de retorno estimada é de 13,8%..."

Perceba.

Ele não fez a conta.

Ele soube **quem** deveria fazê-la.

---

# O LLM escolhe sozinho?

Sim.

E isso costuma surpreender quem está começando.

Você não escreve:

```python
if pergunta == ...
```

Você apenas diz.

```
Esta ferramenta faz isto.
```

O restante é inferência.

---

# Exemplo do Projeto Prometheus

Imagine seu Prometheus.

Ferramentas:

```
Second Brain

↓

Consulta conhecimento.
```

```
Python

↓

Executa cálculos.
```

```
Google Sheets

↓

Atualiza planilhas.
```

```
Gmail

↓

Envia e-mails.
```

```
Gerador de Imagens

↓

Cria capas.
```

Agora você diz.

> "Escreva um artigo sobre Branding, gere uma capa e envie um rascunho para meu e-mail."

O LLM poderia decidir:

```
↓

Consultar Second Brain

↓

Escrever artigo

↓

Gerar imagem

↓

Enviar e-mail
```

Você nunca escreveu um `if artigo`.

---

# E se ele escolher a ferramenta errada?

Excelente pergunta.

Pode acontecer.

Por isso existem duas estratégias.

## Estratégia 1

Confiar totalmente.

O agente decide.

---

## Estratégia 2

Supervisor.

O agente propõe.

A aplicação valida.

Exemplo.

```
LLM

↓

Quero apagar todos os clientes.
```

Aplicação.

↓

Negado.

---

Para ferramentas críticas, normalmente existe uma camada de segurança.

---

# Um conceito muito importante

Você pode imaginar que o LLM possui um "catálogo mental".

Algo como:

```
Tenho acesso a:

↓

Python

↓

Banco Vetorial

↓

Calendário

↓

CRM

↓

E-mail
```

Quando recebe uma tarefa.

Ele procura:

> Qual ferramenta resolve isso?

Essa capacidade de selecionar ferramentas é um dos motivos pelos quais agentes modernos parecem muito mais inteligentes do que um chatbot tradicional.

---

# Um exemplo completo

Imagine um pedido:

> "Veja se ainda existem unidades disponíveis do apartamento de 120 m², calcule a rentabilidade esperada e me envie um resumo."

Fluxo:

```text
Usuário

↓

LLM interpreta

↓

Ferramenta:
Banco de Dados

↓

Retorna disponibilidade

↓

Ferramenta:
Python

↓

Calcula rentabilidade

↓

LLM escreve resumo

↓

Ferramenta:
E-mail

↓

Envia

↓

LLM confirma ao usuário
```

Perceba que o LLM alterna constantemente entre raciocínio e uso de ferramentas.

---

# Um detalhe avançado (e extremamente importante)

Até agora, vimos ferramentas como funções isoladas.

Mas imagine que existam duas ferramentas:

```
Calculadora Financeira
```

e

```
Calculadora Estatística
```

As duas "calculam".

Como o agente escolhe?

Pela descrição.

Quanto melhor a descrição, mais fácil para o LLM decidir corretamente.

Na prática, muitos erros em agentes não acontecem porque o modelo "é burro", mas porque as ferramentas foram mal descritas.

---

# Arquitetura atualizada

Agora nosso diagrama ganhou mais um nível de detalhe:

```text
                    Usuário
                       │
                       ▼
                 Orquestrador
                       │
                       ▼
                     LLM
                       │
         "Qual ferramenta resolve isso?"
                       │
      ┌────────────────┼────────────────┐
      ▼                ▼                ▼
     RAG            Python         Calendário
      │                │                │
      └────────────────┼────────────────┘
                       ▼
                  Resultado
                       │
                       ▼
                     LLM
                       │
                       ▼
                   Resposta
```

Observe que o LLM aparece **duas vezes**:

- antes, para decidir;
    
- depois, para interpretar o resultado.
    

Essa é uma arquitetura extremamente comum em agentes modernos.

---

# Desafio da Aula 5

Você é o arquiteto do **Prometheus**, seu futuro ecossistema de IA.

Imagine que ele possui as seguintes ferramentas:

- Banco Vetorial do Second Brain;
    
- Python;
    
- Google Sheets;
    
- Gmail;
    
- Google Calendar;
    
- Gerador de Imagens.
    

Agora responda:

### 1.

Para cada um dos pedidos abaixo, diga **quais ferramentas o agente utilizaria e em que ordem**.

**Pedido A**

> "Escreva um artigo para o Substack explicando a relação entre Branding e Estoicismo e gere uma imagem de capa."

---

**Pedido B**

> "Analise minha carteira de criptomoedas, calcule a distribuição percentual de cada ativo e salve o resultado em uma planilha."

---

**Pedido C**

> "Marque uma reunião com João para terça às 14h e envie um e-mail avisando sobre o compromisso."

---

### 2.

Imagine que você tenha duas ferramentas:

```
Ferramenta A

Descrição:
"Calcula indicadores financeiros."
```

```
Ferramenta B

Descrição:
"Executa cálculos estatísticos em grandes conjuntos de dados."
```

Explique por que **a qualidade da descrição da ferramenta** influencia diretamente a capacidade do agente de tomar boas decisões.

---

## Professor para aluno

Caio, esta aula pode parecer simples, mas ela é um divisor de águas.

Quando começarmos a usar **OpenAI Agents SDK**, **LangGraph** e **CrewAI**, você perceberá que todos eles compartilham uma ideia central:

> Você não programa cada decisão do agente.

Você **descreve o ambiente** em que ele trabalha.

É uma mudança de paradigma.

Na programação tradicional, dizemos ao computador **como** resolver o problema.

Nos agentes modernos, passamos muito mais tempo dizendo **quais capacidades ele possui** e **quais limites deve respeitar**.

Essa diferença vai ficar ainda mais clara quando escrevermos nosso primeiro agente em Python. Tenho a sensação de que esse será um dos momentos em que você vai olhar para trás e pensar: _"Então era isso que estava acontecendo por trás do ChatGPT o tempo todo."_

---

# Epílogo:

```
Antes de prosseguirmos e antes de eu enviar a resposta do desafio: então, no exemplo do meu second brain, como em todos os livros que passei, pus cases e exemplos, ele pode utiliza-los como exemplo dos frameworks dessas obras, e , a partir daí, preencher os frameworks?
```

Caio...

**SIM.**

E essa pergunta é excelente, porque ela toca em uma das aplicações mais poderosas de um RAG bem construído.

Mas vou fazer uma pequena correção de terminologia para deixar seu modelo mental ainda mais preciso.

O agente não "aprende" os frameworks no momento da consulta. Ele **recupera (Retrieval)** os exemplos e os utiliza como contexto para preencher um novo framework.

Vou dar um exemplo usando o seu Second Brain.

---

## Exemplo 1 — Marketing Canvas

Imagine que você tenha no Second Brain:

```text
Marketing Canvas
│
├── Capítulo 1
├── Capítulo 2
├── Framework do Canvas
├── Case Apple
├── Case Nubank
├── Case Nike
```

Você pergunta ao Prometheus:

> "Monte um Marketing Canvas para uma clínica odontológica."

O fluxo seria algo como:

```text
Pergunta

↓

Retrieval

↓

Busca:

• Framework Marketing Canvas

• Case Apple

• Case Nubank

• Case Nike

↓

LLM

↓

Produz um Canvas novo
```

Observe:

Ele **não copia** Apple ou Nubank.

Ele usa esses exemplos para entender **como aplicar** o framework.

---

## Exemplo 2 — SWOT

Imagine que você tenha anotado cinco SWOTs diferentes.

Quando você disser:

> "Faça uma SWOT para minha empresa."

O RAG pode recuperar:

- o conceito de SWOT;
    
- seus cinco exemplos preenchidos.
    

O LLM então percebe padrões como:

> "Ah... o usuário costuma estruturar forças dessa forma."

E produz uma SWOT seguindo esse estilo.

---

## Exemplo 3 — Prometheus-Mentor

Imagine que você adicionou ao Second Brain:

```text
Livro:
Tração

↓

Capítulo:
Bullseye Framework

↓

3 exemplos completos
```

Depois você pergunta:

> "Qual canal de aquisição devo priorizar para um escritório imobiliário?"

O agente pode recuperar:

- o capítulo do Bullseye;
    
- os três exemplos.
    

E responder algo como:

> "Seguindo a lógica do Bullseye, eu começaria testando os canais A, B e C..."

Ele está usando seus materiais como referência.

---

# Isso é quase um "few-shot" automático

Você já ouviu falar em **few-shot prompting**?

É quando mostramos exemplos ao LLM antes de pedir uma tarefa.

Exemplo:

```text
Exemplo 1

↓

Exemplo 2

↓

Agora faça um terceiro.
```

O que o RAG faz é muito parecido.

Só que os exemplos não ficam escritos no prompt.

Eles são buscados dinamicamente.

Ou seja:

```text
Second Brain

↓

Retrieval

↓

Encontra os melhores exemplos

↓

Prompt é montado automaticamente

↓

LLM responde
```

Isso é incrivelmente poderoso.

---

# E agora vem uma ideia que acho que você vai gostar

Conhecendo seu jeito de estudar, eu faria uma pequena adaptação no Second Brain.

Hoje você salva:

- conceito;
    
- framework;
    
- exemplos.
    

Eu acrescentaria um quarto tipo de nota.

```text
Marketing Canvas

↓

Framework

↓

Exemplos

↓

Template
```

Por exemplo:

```markdown
# Template — Marketing Canvas

## Público-alvo

...

## Proposta de Valor

...

## Canais

...

## Relacionamento

...
```

Agora imagine o agente.

Ele recupera:

- o framework;
    
- os exemplos;
    
- o template.
    

A resposta tende a ficar muito mais consistente.

---

# Isso explica outra coisa

Lembra quando você comentou, há algum tempo:

> "Tenho vários autores que discordam entre si."

Na época conversamos sobre isso como um desafio do RAG.

Agora você consegue enxergar uma solução arquitetural.

O agente pode recuperar:

```text
Kotler

+

Ries

+

Weinberg

+

Suas notas
```

E responder:

> "Kotler abordaria assim. Ries defenderia outra abordagem. Gabriel Weinberg enfatizaria X. Considerando o seu contexto, seguem as diferenças..."

Ou, se você quiser um comportamento mais restrito, o orquestrador pode instruir:

> "Use apenas Kotler."

Ou:

> "Use apenas minhas anotações."

Essa flexibilidade é uma das grandes vantagens de separar **conhecimento** (RAG) da **geração** (LLM).

---

## Professor para aluno

Vou fazer uma previsão sobre o seu Projeto Prometheus.

Acho que, no futuro, o ativo mais valioso do seu sistema **não será o GPT**.

Será o seu **Second Brain**.

Porque qualquer pessoa pode acessar um bom LLM.

Pouquíssimas pessoas terão uma base de conhecimento organizada, enriquecida com anos de estudos, exemplos, casos, frameworks, anotações pessoais e conexões entre áreas.

O LLM será o "motor".

O seu Second Brain será o **diferencial competitivo**.

E, sinceramente, acho que essa é exatamente a direção certa para o projeto.