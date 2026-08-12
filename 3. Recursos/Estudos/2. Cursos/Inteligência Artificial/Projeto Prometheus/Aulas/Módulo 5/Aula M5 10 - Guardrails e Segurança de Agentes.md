---
tags:
  - IA
  - programação
  - inovação
---
# Projeto Prometheus

# Módulo 5 — Aula 10

# Guardrails: Como Impedir que um Agente Faça Besteira

> **Objetivo da aula**
> 
> Entender por que agentes precisam de limites, como esses limites são implementados e quais decisões nunca devem ficar totalmente sob responsabilidade da IA.

---

# O nascimento do problema

Vamos imaginar o Prometheus-Office.

Você pede:

> "Envie este contrato ao cliente."

Parece simples.

Mas imagine que o agente:

- enviou a versão errada;
    
- esqueceu um anexo;
    
- enviou para o e-mail errado;
    
- alterou uma cláusula automaticamente;
    
- apagou o contrato anterior.
    

De repente...

Não estamos mais falando de uma resposta errada.

Estamos falando de um prejuízo real.

---

# Quanto mais autonomia...

...maior o risco.

Visualmente:

```text
Chatbot
│
├── Apenas responde perguntas
│
│    risco baixo
│
▼

Agente

│
├── Consulta banco de dados
├── Envia e-mails
├── Agenda reuniões
├── Move arquivos
├── Executa código
│
│    risco médio
│
▼

Sistema Autônomo

│
├── Decide sozinho
├── Executa pagamentos
├── Assina contratos
├── Publica conteúdos
│
│    risco alto
▼
```

Perceba.

Quanto maior o poder...

Maior a responsabilidade.

---

# O que são Guardrails?

Guardrails significam, literalmente:

> **"Trilhos de proteção."**

Pense numa estrada de montanha.

```text
█████████████████████

───────────────

█████████████████████
```

Os carros possuem liberdade.

Mas dentro de limites.

Os guardrails fazem exatamente isso.

O agente continua inteligente.

Mas deixa de ser livre para fazer qualquer coisa.

---

# Existem vários tipos de Guardrails

Vamos classificá-los.

---

# 1. Guardrails de Ferramenta

O agente pode usar apenas determinadas ferramentas.

Exemplo.

```text
Prometheus-Mentor

Pode:

✔ consultar RAG

✔ gerar exercícios

✔ responder perguntas

Não pode:

✘ apagar arquivos

✘ enviar e-mails

✘ executar Python
```

Observe.

A limitação não está no LLM.

Está na arquitetura.

---

# 2. Guardrails de Permissão

Imagine.

O Prometheus-Office possui acesso ao Google Drive.

Mas...

Pode apenas:

```text
Ler documentos.
```

Não pode:

```text
Excluir documentos.
```

Mesma ferramenta.

Permissões diferentes.

---

# 3. Guardrails de Conteúdo

Imagine.

Você pede.

> "Apague todas as notas do meu Second Brain."

O sistema poderia responder.

```text
Essa ação exige confirmação.
```

Ou até recusar.

---

# 4. Guardrails Humanos

Esse é um dos mais importantes.

Chamamos de:

## Human in the Loop

Visualmente.

```text
Agente

↓

Terminou.

↓

Humano aprova?

↓

SIM

↓

Executar.
```

Isso aparece muito em empresas.

Exemplos.

- pagamentos;
    
- contratos;
    
- diagnósticos médicos;
    
- decisões jurídicas.
    

---

# Exemplo no Prometheus-Office

Imagine.

O agente revisou um contrato.

Encontrou uma cláusula problemática.

Ele poderia:

```text
Alterar automaticamente.
```

Mas...

Será que deveria?

Talvez seja melhor.

```text
Encontrada cláusula de risco.

↓

Sugestão de alteração.

↓

Aguardando aprovação humana.
```

Perceba.

O agente auxilia.

Quem decide é você.

---

# Autonomia não é tudo ou nada

Esse é um erro comum.

Na verdade existe um espectro.

```text
Nível 0

↓

Responder perguntas.

────────────────────────

Nível 1

↓

Consultar ferramentas.

────────────────────────

Nível 2

↓

Executar pequenas ações.

────────────────────────

Nível 3

↓

Executar ações importantes.

Com confirmação.

────────────────────────

Nível 4

↓

Autonomia completa.
```

Nem todo agente precisa chegar ao nível 4.

Aliás...

Pouquíssimos deveriam.

---

# Aplicando ao Prometheus OS

Vamos classificar.

## Prometheus-Mentor

Pode.

✔ explicar.

✔ gerar exercícios.

✔ consultar RAG.

Não pode.

✘ modificar seu Second Brain automaticamente.

---

## Prometheus-Knowledge

Pode.

✔ sugerir notas.

✔ gerar Markdown.

✔ criar links.

Mas...

Antes de alterar centenas de arquivos.

↓

Confirmação.

---

## Prometheus-Editor

Pode.

✔ escrever newsletter.

✔ gerar imagens.

✔ revisar texto.

Mas...

Antes de publicar automaticamente.

↓

Confirmação.

---

## Prometheus-Office

Pode.

✔ calcular cronogramas.

✔ revisar contratos.

✔ gerar planilhas.

Jamais deveria.

✘ assinar contratos.

✘ transferir dinheiro.

✘ alterar cláusulas críticas sem aprovação.

---

# Um conceito muito importante

Você já percebeu que estamos construindo agentes.

Agora pense.

Quem define os guardrails?

O próprio agente?

Não.

Quem define é...

## O arquiteto.

O LLM não sabe quais riscos existem no seu escritório.

Você sabe.

Logo.

Os limites vêm da arquitetura.

Não da inteligência artificial.

---

# Um erro comum

Imagine um prompt.

```text
"Nunca envie e-mails errados."
```

Isso é um guardrail?

Não.

É apenas uma instrução.

Um guardrail de verdade seria.

```text
A ferramenta de e-mail exige:

destinatário

↓

confirmação

↓

envio
```

Perceba a diferença.

O guardrail está na aplicação.

Não na boa vontade do modelo.

---

# Ligando com tudo que vimos

Até agora temos.

```text
Usuário

↓

Orquestrador

↓

Planejamento

↓

Agentes

↓

Ferramentas

↓

Guardrails

↓

Execução

↓

Eventos

↓

Memória

↓

RAG
```

Os guardrails envolvem todas as ações.

---

# Um caso real

Imagine o Prometheus-Knowledge.

Ele gera uma nota nova.

Em vez de salvar diretamente.

Ele faz.

```text
Gerar Markdown

↓

Mostrar diferença

↓

"Você deseja aplicar?"

↓

SIM

↓

Salvar.
```

Isso é muito mais seguro.

---

# Filosofia da Engenharia de IA

Quero que você guarde esta frase.

> **Quanto maior a autonomia de um agente, maior deve ser o investimento em mecanismos de controle.**

Essa é uma das regras mais importantes da Engenharia de Sistemas Inteligentes.

---

# Desafio da Aula 10

Agora vamos projetar os **guardrails do Prometheus OS**.

## Parte 1

Para cada módulo abaixo, classifique as ações em três categorias:

- Pode executar sozinho.
    
- Deve pedir confirmação.
    
- Nunca deve executar.
    

### Prometheus-Mentor

### Prometheus-Knowledge

### Prometheus-Editor

### Prometheus-Office

Justifique suas decisões.

---

## Parte 2

Imagine que o **Prometheus-Knowledge** detecte automaticamente que 200 notas do seu Second Brain poderiam ser reorganizadas.

Projete um fluxo seguro para essa operação.

Explique:

- como o agente detecta a oportunidade;
    
- como apresenta a proposta;
    
- em que momento entra o Human in the Loop;
    
- como evitar alterações irreversíveis.
    

---

## Parte 3

Explique por que um bom guardrail deve ser implementado **na arquitetura** e não apenas escrito no prompt do agente.

[[🛠 Desafio M5 010]]

---

# Professor para aluno

Caio, esta aula tem um significado especial porque toca em algo que vai muito além da tecnologia.

Até agora falamos sobre como tornar agentes mais capazes.

Hoje começamos a discutir como torná-los **confiáveis**.

Na prática, um arquiteto de IA não é apenas alguém que faz sistemas inteligentes. É alguém que decide **quais decisões continuarão sendo humanas**.

Essa talvez seja a maior responsabilidade de quem projeta sistemas inteligentes.

E, olhando para o Prometheus OS que estamos desenhando, fico feliz por estarmos tratando desse assunto **antes** de escrever qualquer código. Porque é muito mais fácil construir um sistema seguro desde o início do que tentar adicionar segurança depois que ele já está em funcionamento.