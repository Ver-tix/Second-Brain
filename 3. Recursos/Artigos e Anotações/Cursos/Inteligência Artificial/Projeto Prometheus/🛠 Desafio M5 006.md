---
tags:
  - inteligenciaartificial
  - programação
---
Esse desafio é ótimo porque ele testa exatamente a diferença entre "workflow automatizado" e "agente de verdade" — que é o ponto mais sutil de tudo que vimos até agora. Vamos por partes.

## 1. O Agent Loop, passo a passo

```text
1. Usuário faz o pedido completo (aprender Black-Scholes: 
   conceito + exemplo + exercício + correção)
        ↓
2. Orquestrador + LLM decompõe o pedido em sub-tarefas
   ("preciso: explicar conceito, dar exemplo, propor 
   exercício, e depois corrigir uma resposta que ainda 
   nem existe")
        ↓
3. LLM decide a primeira ação: buscar informação sobre 
   Black-Scholes (RAG)
        ↓
4. Retrieval busca no banco vetorial os trechos relevantes 
   sobre o modelo Black-Scholes
        ↓
5. LLM recebe os trechos e gera a explicação do conceito
        ↓
6. Agente decide a próxima ação: gerar um exemplo prático 
   coerente com a explicação que acabou de dar
        ↓
7. LLM gera o exemplo (pode reaproveitar o mesmo contexto 
   do RAG, sem precisar buscar de novo)
        ↓
8. Agente decide a próxima ação: propor um exercício
        ↓
9. LLM gera o exercício e apresenta ao usuário
        ↓
10. Agente PAUSA o loop — precisa esperar a resposta do 
    usuário antes de continuar (não dá pra corrigir uma 
    resposta que ainda não existe)
        ↓
11. Usuário responde ao exercício
        ↓
12. Agente retoma o loop: LLM compara a resposta do usuário 
    com o gabarito/critério correto
        ↓
13. LLM gera o feedback/correção
        ↓
14. Fim do loop, resposta final entregue
```

Repara na etapa 10 — isso é crucial e volto nisso na pergunta 4.

## 2. Quando consulta RAG, usa memória, e toma nova decisão

**Consulta ao RAG:**

- Na etapa 4, ao buscar o conteúdo teórico sobre Black-Scholes (fórmula, premissas, variáveis do modelo). É aqui que o agente busca informação que ele não tem garantia de saber "de cor" com precisão — fórmulas financeiras são exatamente o tipo de coisa em que você não quer confiar na memória do modelo, quer buscar a fonte certa (lembra da aula sobre por que RAG existe).

**Uso de memória:**

- Depois da etapa 5, o agente precisa **lembrar** o que ele mesmo já explicou, pra construir o exemplo prático de forma coerente com a explicação (não pode contradizer o que acabou de dizer).
- Na etapa 12, o agente precisa lembrar **qual foi o exercício que ele mesmo propôs**, pra poder corrigir a resposta do usuário com o critério certo. Sem memória dessa etapa anterior, ele não teria como saber o que estava sendo corrigido.
- Isso é memória de curto prazo (dentro da mesma conversa/sessão) — diferente de memória de longo prazo (tipo lembrar que esse usuário sempre erra derivadas, de sessões passadas).

**Nova decisão antes de continuar:**

- Depois da etapa 4 (Retrieval): decidir se o resultado encontrado é suficiente pra gerar uma boa explicação, ou se precisa buscar de novo com outra query (isso conecta direto com a pergunta 3).
- Na etapa 6: decidir que tipo de exemplo prático fazer sentido (ex: exemplo de opção de compra vs. venda) — isso não estava especificado no pedido original, o agente precisa decidir sozinho.
- Na etapa 10: decidir que **precisa parar e esperar input do usuário** antes de seguir pra correção — essa é a decisão mais importante do fluxo inteiro.

## 3. O que o agente faz se o Retrieval retornar poucos resultados

Aqui está a diferença prática entre agente e workflow fixo. Um **workflow automatizado** seguiria cegamente: "buscou, achou pouco, usa o que tem e segue em frente" — porque workflow não tem capacidade de avaliar a qualidade do que recebeu, só executa passos fixos.

Um **agente** consegue **avaliar o próprio resultado** e reagir de formas diferentes:

- **Reformular a busca**: perceber que a query original ("Black-Scholes") pode ter sido genérica demais, e tentar de novo com termos mais específicos (ex: "fórmula Black-Scholes precificação de opções", "premissas do modelo Black-Scholes").
- **Buscar em outra fonte**: se o banco vetorial específico não tem material suficiente, decidir complementar com uma busca na web (se essa ferramenta estiver disponível) antes de responder.
- **Avisar o usuário sobre a limitação**: em vez de inventar informação pra preencher a lacuna (alucinação), o agente pode decidir dizer algo como "encontrei informação limitada sobre esse tópico específico, vou explicar com base no que tenho, mas recomendo verificar fontes adicionais."
- **Ajustar o escopo**: se realmente não achar material bom o suficiente pra um exemplo prático elaborado, decidir simplificar o exemplo em vez de forçar algo mal fundamentado.

O ponto chave: o agente **reavalia e decide o próximo passo com base no que encontrou**, em vez de seguir um script fixo de "passo 1, passo 2, passo 3" sem checar se o resultado de cada passo faz sentido.

## 4. Por que isso é um agente, e não só um workflow automatizado

A diferença central é: **um workflow segue uma sequência fixa e pré-determinada de passos. Um agente decide, a cada etapa, qual é o próximo passo — com base no que aconteceu até ali.**

Alguns sinais concretos, olhando o fluxo que desenhamos:

- **Bifurcação condicional real**: se o Retrieval trouxer pouco resultado, o agente muda de estratégia (pergunta 3). Um workflow fixo não teria essa flexibilidade — ele executaria a próxima etapa de qualquer jeito, mesmo com dado ruim.
- **Pausa e retomada com estado**: o agente precisa parar no meio do processo (esperar a resposta do exercício), guardar contexto, e retomar depois sabendo exatamente onde parou e o que precisa fazer a seguir. Isso exige memória e decisão, não é só "próximo passo da esteira".
- **Sequência não é fixa no código**: num workflow tradicional, alguém programou explicitamente "passo 1 → passo 2 → passo 3" em código. Num agente, é o **LLM, a cada iteração do loop**, que decide qual é a próxima ação — a sequência emerge da decisão do modelo, não de um script rígido escrito antecipadamente.
- **O número de idas e vindas não é conhecido de antemão**: se o Retrieval falhar uma vez, pode ser preciso tentar de novo — o workflow de um agente pode ter 4 passos numa execução e 7 passos em outra, dependendo do que acontece no meio do caminho.

Isso conecta direto com o princípio que abriu todo esse módulo, lá na primeira aula sobre arquitetura: a inteligência não está isolada num prompt gigante nem num script fixo — ela está distribuída entre a orquestração **e** as decisões que o próprio modelo toma, iteração após iteração, reagindo ao que encontra pelo caminho.