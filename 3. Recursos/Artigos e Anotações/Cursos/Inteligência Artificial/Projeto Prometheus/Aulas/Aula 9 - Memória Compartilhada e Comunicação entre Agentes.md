---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
> **Objetivo da aula**
> 
> Entender como agentes compartilham informações, evitando duplicação de conhecimento e mantendo uma visão consistente do sistema.

---

# O problema

Imagine nosso Prometheus OS.

Temos:

- Prometheus-Mentor;
    
- Prometheus-Editor;
    
- Prometheus-Office;
    
- Prometheus-Knowledge.
    

Cada um possui diversos agentes.

Agora imagine.

O agente Redator escreve:

> "O usuário gosta de exemplos visuais."

Como o agente Tutor descobre isso?

Precisamos que um agente envie uma mensagem ao outro?

Ou existe uma forma melhor?

---

# Primeira solução (ingênua)

Cada agente possui sua própria memória.

```text
Mentor

┌────────────┐
│ Memória A  │
└────────────┘

Editor

┌────────────┐
│ Memória B  │
└────────────┘

Office

┌────────────┐
│ Memória C  │
└────────────┘
```

Funciona.

Mas...

Agora imagine.

Você muda uma preferência.

O Mentor sabe.

O Editor não.

O Office também não.

Começam as inconsistências.

---

# Segunda solução

Compartilhar uma memória comum.

```text
                Memória Compartilhada

                ┌──────────────────┐
                │                  │
                │ Preferências     │
                │ Projetos         │
                │ Histórico        │
                │ Conhecimento     │
                │                  │
                └──────────────────┘

          ▲              ▲              ▲

      Mentor         Editor        Office
```

Agora todos consultam a mesma fonte.

---

# Mas cuidado

Nem toda memória deve ser compartilhada.

Vamos dividir em camadas.

---

# Camada 1 — Estado Local

É temporário.

Exemplo.

```text
Newsletter em produção

↓

Notícia 3 escolhida

↓

Imagem ainda não gerada
```

Só interessa ao Prometheus-Editor.

O Mentor não precisa saber disso.

---

# Camada 2 — Memória Compartilhada

Exemplo.

```text
Usuário prefere:

Markdown

↓

Exemplos

↓

Diagramas

↓

Português
```

Todos os módulos se beneficiam.

---

# Camada 3 — Base de Conhecimento

Essa você conhece muito bem.

É o nosso velho amigo.

```text
Second Brain

↓

Embeddings

↓

Banco Vetorial

↓

Retrieval
```

Essa memória não guarda "o que está acontecendo".

Ela guarda conhecimento.

---

# Três tipos de informação

Vamos organizar.

|Informação|Onde fica?|Exemplo|
|---|---|---|
|Estado da execução|Estado local|"Estou revisando a newsletter."|
|Preferências e histórico|Memória compartilhada|"Caio prefere diagramas em texto."|
|Conhecimento|Second Brain / RAG|"Marketing Canvas, capítulo 3."|
Perceba como estamos separando responsabilidades.

---

# Comunicação entre agentes

Imagine.

O Pesquisador encontrou algo importante.

Como avisar ao Analista?

Existem duas estratégias.

---

## Estratégia 1 — Mensagem direta

```text
Pesquisador

↓

Analista
```

É rápida.

Mas cria dependência.

---

## Estratégia 2 — Memória compartilhada

```text
Pesquisador

↓

Atualiza memória

↓

Analista consulta memória
```

Os agentes não precisam conhecer uns aos outros.

Eles apenas leem e escrevem em um lugar comum.

Isso é chamado de **desacoplamento**.

---

# Um exemplo no Prometheus OS

Imagine que, durante uma conversa, você diga:

> "Prefiro respostas em português, mas mantenha os nomes técnicos em inglês."

Quem precisa saber disso?

- Mentor ✔️
    
- Editor ✔️
    
- Office ✔️
    
- Knowledge ✔️
    

Então essa informação vai para a memória compartilhada.

Agora imagine outra informação.

> "A newsletter desta semana terá cinco seções."

Quem precisa saber?

Só o Editor.

Ela permanece no estado local.

---

# Eventos

Outra forma de comunicação é através de eventos.

Imagine.

O Redator terminou.

Em vez de chamar diretamente o Revisor, ele apenas registra:

```text
Evento:

"Rascunho concluído."
```

O Orquestrador vê esse evento e decide:

```text
↓

Enviar para o Revisor.
```

Perceba a diferença.

O Redator nem sabe que existe um Revisor.

Ele apenas informa:

> "Terminei."

Isso deixa o sistema muito mais flexível.

---

# Aplicando ao Prometheus-Mentor

Imagine o seguinte fluxo.

```text
Tutor

↓

Aluno respondeu desafio

↓

Avaliador corrige

↓

Evento:

"Desafio aprovado"

↓

Knowledge atualiza Second Brain

↓

Evento:

"Conhecimento consolidado"

↓

Mentor sugere próxima aula
```

Cada agente faz apenas seu trabalho.

Quem coordena tudo continua sendo o Orquestrador.

---

# Uma visão arquitetural

Nosso Prometheus OS começa a ficar assim:

```text
                    Prometheus OS

                          │

                   Orquestrador

                          │

        ┌─────────────────┼─────────────────┐

        ▼                 ▼                 ▼

   Prometheus-      Prometheus-      Prometheus-
     Mentor           Editor            Office

        │                 │                 │

      Agentes          Agentes          Agentes

        │                 │                 │

        └──────────────┬──┴─────────────────┘
                       │

             Memória Compartilhada

                       │

                Second Brain (RAG)

                       │

                Banco Vetorial
```

Perceba que a arquitetura está ficando parecida com um sistema operacional.

E isso não é coincidência.

---

# Um cuidado importante

É tentador colocar **tudo** na memória compartilhada.

Não faça isso.

Quanto maior a memória comum, mais difícil fica:

- encontrar informações;
    
- manter consistência;
    
- controlar permissões;
    
- evitar conflitos.
    

Um bom arquiteto sempre pergunta:

> **"Quem realmente precisa dessa informação?"**

---

# Ligação com a Engenharia de Software

Talvez você tenha ouvido falar em:

- Event Bus;
    
- Message Queue;
    
- Pub/Sub;
    
- Kafka;
    
- RabbitMQ.
    

Não se preocupe em aprender essas tecnologias agora.

Quero apenas que saiba que todas elas nasceram para resolver exatamente o problema que estudamos hoje:

> **Como diferentes componentes de um sistema podem se comunicar sem ficarem fortemente acoplados?**

Quando chegarmos a arquiteturas mais avançadas, esses nomes aparecerão naturalmente.

---

# Desafio da Aula 9

Vamos evoluir o **Prometheus OS**.

## Parte 1

Para cada uma das informações abaixo, diga onde ela deveria ficar:

- Estado local;
    
- Memória compartilhada;
    
- Second Brain (RAG).
    

Explique o motivo.

1. "O usuário prefere respostas com diagramas."
    
2. "A newsletter desta semana já teve a imagem gerada."
    
3. "Resumo do livro _Tração_, de Gabriel Weinberg."
    
4. "O usuário concluiu o Módulo 5 do Projeto Prometheus."
    
5. "O agente jurídico encontrou uma cláusula de risco e ainda está analisando."
    

---

## Parte 2

Imagine que o **Prometheus-Editor** terminou de escrever uma newsletter.

Descreva como essa informação poderia chegar ao **Prometheus-Knowledge**:

- usando comunicação direta entre agentes;
    
- usando eventos;
    
- usando memória compartilhada.
    

Compare as três abordagens e diga qual escolheria para o Prometheus OS.

[[🛠 Desafio M5 009]]

---

## Professor para aluno

Esta aula pode parecer menos "empolgante" do que as anteriores, mas ela resolve um dos maiores problemas dos sistemas grandes: **como permitir colaboração sem criar uma rede caótica de dependências**.

Repare no caminho que percorremos:

- começamos com um único agente;
    
- aprendemos sobre planejamento;
    
- estudamos ferramentas e memória;
    
- dividimos responsabilidades entre vários agentes;
    
- agora estamos aprendendo a fazê-los colaborar.
    

Na próxima etapa, começaremos a discutir **papéis mais avançados dentro de um ecossistema**, aproximando-nos cada vez mais da implementação. Tenho a impressão de que, quando finalmente abrirmos o VS Code, você verá o código como a consequência natural dessa arquitetura — e era exatamente esse o objetivo desde o início.