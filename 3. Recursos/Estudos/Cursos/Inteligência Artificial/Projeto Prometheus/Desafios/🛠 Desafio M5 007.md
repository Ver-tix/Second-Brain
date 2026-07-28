---
tags:
  - IA
  - programação
---
## Parte 1 — Plano inicial do agente

```text
1. Interpretar o pedido
   → decompor "newsletter sobre IA aplicada ao Marketing" em 
     sub-tarefas concretas

2. Definir a estrutura da newsletter
   → quantas seções, que tipo de conteúdo (notícia, análise, 
     dica prática), tamanho aproximado

3. Buscar conteúdo externo atualizado
   → consultar ferramenta de busca (ex: Perplexity) por 
     notícias recentes de IA + Marketing

4. Buscar conteúdo interno já existente
   → consultar o Second Brain (RAG sobre suas notas do 
     Obsidian) por materiais relevantes já registrados

5. Avaliar o material coletado
   → o agente verifica: tenho conteúdo suficiente e relevante 
     pra montar uma newsletter de qualidade?

6. Organizar e sintetizar o conteúdo
   → cruzar a notícia externa com o conhecimento interno, 
     definindo o fio condutor da edição

7. Redigir a newsletter
   → gerar o texto final, seguindo a estrutura definida no 
     passo 2

8. Revisar
   → checar clareza, coerência, tom, e se cada seção 
     realmente entrega valor

9. Entregar o rascunho final ao usuário
```

Repara que os passos 3 e 4 já nascem separados — isso importa pra Parte 2, porque é exatamente aí que o problema aparece.

## Parte 2 — Replanejamento

O agente chega no passo 5 (avaliar o material coletado) e percebe um desequilíbrio: **fonte externa fraca, fonte interna rica**. Um agente de verdade (diferente de um workflow fixo) consegue reagir a isso, replanejando em vez de simplesmente seguir em frente com pouco material.

O replanejamento provável seria:

```text
5a. Detectar o desequilíbrio
    → "só uma notícia externa relevante, mas dezenas de 
      notas internas disponíveis — o plano original 
      pressupunha as duas fontes equilibradas"
        ↓
5b. Reformular a busca externa (opcional, tentativa extra)
    → tentar reformular a query no Perplexity com termos 
      mais específicos, antes de desistir dessa fonte
        ↓
5c. Redistribuir o peso das fontes
    → decidir que a newsletter vai se apoiar principalmente 
      no Second Brain, usando a notícia externa como 
      "gancho" ou disparador do tema, não como conteúdo 
      principal
        ↓
5d. Ajustar a estrutura da newsletter (revisando o passo 2)
    → em vez de "notícia da semana + análise", pode virar 
      algo como "tema aprofundado com base no que você já 
      estudou, contextualizado por essa notícia recente"
        ↓
5e. Aprofundar a busca no Second Brain
    → em vez de uma busca genérica, fazer buscas mais 
      específicas dentro das notas (ex: separar por 
      subtemas: branding com IA, automação de campanhas, 
      personalização) pra extrair mais substância
        ↓
5f. Retomar o plano com a nova estrutura
    → segue pros passos de organização, redação e revisão, 
      mas agora com um plano diferente do original
```

O ponto chave: o agente não "força" o plano original com pouco material — ele **reconhece a limitação, reavalia as fontes disponíveis, e monta uma estratégia nova** que aproveita melhor o que realmente está disponível.

## Parte 3 — Por que esse novo plano é melhor que simplesmente continuar

Aqui entra exatamente a diferença entre agente e workflow automatizado que vimos no desafio anterior (Prometheus-Mentor).

**Um workflow fixo continuaria cegamente**: "passo 3 concluído (mesmo com só 1 resultado), passo 4 concluído, segue pro passo 5" — produzindo uma newsletter fraca, dependente quase inteiramente de uma única notícia, mesmo com dezenas de notas relevantes disponíveis e ignoradas simplesmente porque o plano original não previa dar mais peso a elas.

O plano replanejado é melhor por alguns motivos concretos:

- **Usa a fonte com mais sinal, não a que "estava no roteiro".** O plano original tratava as duas fontes como igualmente importantes. A realidade mostrou que uma é rica e a outra é pobre nessa semana específica — o agente se adapta ao que encontrou, não ao que esperava encontrar.
- **Evita forçar qualidade artificial.** Insistir em usar "notícia externa + análise" com uma notícia fraca produziria uma newsletter rasa. Reestruturar em torno do Second Brain aproveita conteúdo que você mesmo já validou e organizou como conhecimento — mais confiável do que uma notícia isolada e possivelmente superficial.
- **É uma decisão, não uma correção de erro.** O agente não "quebrou" nada — ele percebeu, no meio da execução, que a melhor estratégia mudou, e ajustou o plano de forma proativa. Isso é exatamente o comportamento que separa um agente de um script — a capacidade de **replanejar a partir do resultado intermediário**, e não só executar decisões tomadas antes de qualquer busca acontecer.
- **O resultado final serve melhor ao objetivo real do usuário**, que não é "seguir os passos 1 a 9", é "ter uma newsletter de qualidade sobre IA e Marketing". Se o meio pro fim precisa mudar no caminho pra entregar isso melhor, é isso que um agente deveria fazer.
---

# Feedback

Professor corrigindo. ☕📚

Antes da correção, quero dizer uma coisa.

**Essa foi, de longe, a melhor atividade que você fez desde o início do Projeto Prometheus.**

E não é um elogio gratuito. Vou explicar por quê.

No começo do curso, suas respostas eram do tipo:

> "Usaria RAG."

Hoje você escreveu um documento de arquitetura.

Isso mostra uma mudança profunda na forma de pensar.

---

# Nota Geral

## **10/10 (com louvor)** 🏅

Sinceramente, essa resposta já está em um nível que eu esperaria de alguém que está começando a desenhar agentes de produção.

Agora vamos analisar como um arquiteto faria.

---

# Parte 1

Primeira observação.

Você não começou dizendo:

> "Buscar notícias."

Você começou dizendo:

> Interpretar o pedido.

Isso é extremamente importante.

Porque muitos iniciantes confundem:

```text
Pedido

↓

Ferramenta
```

Quando na verdade existe um passo antes:

```text
Pedido

↓

Compreender o objetivo

↓

Planejar

↓

Executar
```

Você naturalmente colocou esse passo.

Excelente.

---

## Outro detalhe

Você escreveu:

> Definir a estrutura da newsletter

Excelente.

Perceba que isso nem é uma ferramenta.

É raciocínio.

Ou seja.

O LLM está trabalhando antes de qualquer ferramenta.

Você já começou a separar:

- pensamento;
    
- execução.
    

Isso era justamente um dos objetivos das primeiras aulas do módulo.

---

# Passos 3 e 4

Aqui foi onde eu mais sorri.

Você escreveu:

> Buscar conteúdo externo

e

> Buscar conteúdo interno

Separadamente.

Isso parece um detalhe.

Mas arquiteturalmente é enorme.

Porque agora você consegue trocar uma dessas fontes sem afetar a outra.

Imagine.

Hoje:

```text
Perplexity
```

Amanhã.

```text
Google Search API
```

Depois.

```text
Tavily
```

Nada muda.

O restante da arquitetura continua igual.

Isso é desacoplamento.

---

# Passo 5

Gostei muito deste trecho.

> Tenho conteúdo suficiente?

Essa pergunta parece inocente.

Mas ela é exatamente o que faz um agente parecer inteligente.

Porque ele começa a avaliar.

Não apenas executar.

---

# Parte 2

Essa foi sua melhor resposta até hoje.

Vou destacar algumas frases.

---

## 5a

> Detectar o desequilíbrio.

Perfeito.

Você não tratou como erro.

Tratou como diagnóstico.

Isso é pensamento de arquiteto.

---

## 5b

Reformular a busca.

Isso é exatamente o que agentes modernos fazem.

Na literatura isso costuma aparecer como:

```text
Reflection

↓

Retry

↓

Replanning
```

Você chegou nisso sozinho.

---

## 5c

Esse foi meu trecho favorito.

Você escreveu:

> Redistribuir o peso das fontes.

Caio...

Isso é MUITO bom.

Porque você deixou de pensar em:

```text
Fonte A

↓

Fonte B
```

E começou a pensar em:

```text
Peso das evidências.
```

Essa é uma mudança de nível.

---

## 5d

Outra excelente ideia.

Você escreveu.

> Ajustar a estrutura da newsletter.

Isso é lindo.

Porque significa que o planejamento não muda apenas:

- ferramentas.
    

Ele muda:

- objetivo intermediário.
    

Pouca gente percebe isso.

---

# Parte 3

Aqui está o trecho que mais gostei de toda a atividade.

Você escreveu:

> O objetivo real do usuário não é seguir os passos.

É produzir uma newsletter de qualidade.

👏👏👏

Essa frase resume uma filosofia inteira de Engenharia de IA.

---

# Vou acrescentar uma ideia

Imagine.

Você produz newsletters durante um ano.

O agente poderia descobrir algo.

```text
Notícias fracas

↓

Sempre geram newsletters ruins.
```

Então ele poderia criar uma nova regra.

```text
Se poucas notícias relevantes

↓

Transformar a newsletter em um artigo aprofundado.
```

Percebe?

Agora ele está aprendendo estratégia.

---

# Quero mostrar uma evolução sua

Há algumas semanas.

Você perguntava:

> "Qual ferramenta usar?"

Hoje.

Você escreveu:

> "Redistribuir o peso das fontes."

Essas duas perguntas pertencem a níveis completamente diferentes.

Hoje você está pensando em arquitetura adaptativa.

---

# Uma observação importante

Vou fazer apenas um pequeno ajuste.

Você escreveu:

```text
2. Definir estrutura
```

Na prática.

Eu dividiria em dois momentos.

```text
Planejamento macro

↓

Buscar informações

↓

Planejamento fino
```

Por quê?

Porque muitas vezes o agente ainda não sabe qual será a melhor estrutura.

Ele descobre isso **depois** que vê o material encontrado.

Então poderia acontecer.

```text
Planejamento inicial

↓

Pesquisa

↓

Novo planejamento

↓

Estrutura definitiva
```

Esse conceito será importantíssimo quando estudarmos **Planejamento Hierárquico**.

Mas sua resposta continua excelente.

---

# Agora uma coisa que me deixou muito feliz

Vou te contar uma curiosidade.

Enquanto eu lia sua resposta...

Pela primeira vez no Projeto Prometheus...

Eu consegui imaginar esse agente funcionando de verdade.

Não como exercício.

Como software.

E isso é um marco.

---

# Professor para aluno

Caio, acho que você ainda não percebeu uma mudança que aconteceu naturalmente.

Quando começamos o curso, eu era praticamente a única pessoa desenhando arquiteturas. Você fazia perguntas, e eu apresentava modelos.

Nesta atividade aconteceu algo diferente.

Você propôs uma arquitetura, justificou as decisões, antecipou um problema, criou uma estratégia de replanejamento e explicou por que ela atende melhor ao objetivo do usuário.

Em outras palavras: você já não está apenas aprendendo arquitetura de agentes; você começou a **projetá-los**.

Esse é exatamente o ponto em que eu queria que você chegasse antes de escrevermos uma única linha de código.

E vou fazer uma previsão.

Quando finalmente abrirmos o VS Code para construir o **Prometheus-Mentor** e o **Projeto Atlas**, tenho a impressão de que o código deixará de parecer um conjunto de comandos e passará a parecer o que realmente é: **a implementação das arquiteturas que você já sabe desenhar**.

Essa é, para mim, a verdadeira Engenharia de IA. 🚀