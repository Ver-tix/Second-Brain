---
tags:
  - programação
  - IA
---
## Aula 6 — MLOps: o ciclo de vida contínuo

Essa aula fecha o quadro, juntando tudo que você viu nas últimas cinco aulas numa ideia central: **um sistema de IA nunca está "pronto"**. Ele é um ciclo que se repete e se retroalimenta, e MLOps é o nome dado à prática de gerenciar esse ciclo de forma organizada.

**De onde vem o nome**

MLOps = Machine Learning + Operations. É a junção de duas disciplinas: construir o modelo/sistema de IA, e operar isso no mundo real, de forma confiável, ao longo do tempo. É uma variação de um conceito mais antigo do mundo de software chamado DevOps (Development + Operations) — a ideia geral é a mesma: não basta construir, precisa manter funcionando bem, de forma repetível.

**O ciclo completo, juntando todas as aulas anteriores**

```text
1. Dados (aula 2)
   coleta, limpeza, preparação
        ↓
2. Modelo (treinado ou escolhido pronto via API)
        ↓
3. Serving (aula 3)
   modelo disponível pra ser chamado
        ↓
4. Orquestração + Guardrails (aulas anteriores + aula 4)
   sistema em uso real, com proteções ativas
        ↓
5. Observabilidade (aula 5)
   registra tudo que acontece
        ↓
6. Avaliação (aula 5)
   mede qualidade de forma sistemática
        ↓
7. Feedback
   o que foi observado e avaliado informa: 
   o dado precisa mudar? o modelo precisa trocar? 
   o guardrail precisa ser ajustado?
        ↓
volta pro passo 1 (Dados) — o ciclo recomeça
```

Note que isso é literalmente um círculo, não uma linha reta. Um sistema de IA bem operado está constantemente passando por esse ciclo — nunca é "lançou e esqueceu".

**Por que isso é uma decisão de arquitetura, não só "operação"**

Porque se você não desenhar o sistema desde o início pensando nesse ciclo, fica muito caro e difícil de adicionar depois. Por exemplo: se você não pensou em observabilidade (logging) desde o começo, quando o sistema já estiver em produção com problema, você não vai ter dado nenhum pra investigar o que aconteceu. Isso precisa estar na arquitetura desde o primeiro dia, não ser um "detalhe pra depois".

**Versionamento: uma peça prática do MLOps**

Um conceito que aparece bastante: assim como código é versionado (você provavelmente já ouviu falar de Git), num sistema de IA você também versiona:

- O modelo usado (ex: "essa resposta foi gerada com Claude versão X")
- O conjunto de dados do RAG (ex: "o banco vetorial na versão de julho tinha esses documentos")
- Os prompts do orquestrador (ex: "o prompt do guardrail mudou na versão 3")

Isso importa porque, se algo der errado, você precisa conseguir voltar atrás ("rollback") pra uma versão anterior que funcionava, e também precisa conseguir comparar: "o problema começou depois que trocamos o modelo, ou depois que atualizamos o RAG?"

**Conectando com tudo que você já viu**

Esse ciclo de MLOps é, na prática, o "corpo inteiro" do sistema de IA — enquanto as aulas anteriores da parte de Arquitetura de Software (camadas, orquestrador, auth) descreviam o "sistema nervoso" que decide o fluxo de uma pergunta específica. Os dois се encaixam: o orquestrador roda dentro do passo 4 desse ciclo maior, toda vez que uma pergunta chega.

**Resumo de vocabulário da parte de Arquitetura de IA:**

- **Treinamento vs inferência** = ensinar o modelo vs usar o modelo já pronto
- **Serving** = infraestrutura que deixa o modelo disponível pra ser chamado
- **Guardrail** = verificação em código, na entrada ou saída, que protege contra erro do modelo
- **Observabilidade** = registrar o que acontece, pra poder investigar depois
- **Avaliação** = medir qualidade de forma sistemática e contínua
- **MLOps** = o ciclo de vida completo, incluindo dados, modelo, serving, guardrails, avaliação e feedback, se repetindo continuamente

---

Isso fecha a parte de Arquitetura de Sistemas de IA. Você agora tem o vocabulário e o mapa mental pra entender qualquer desafio prático que envolva "onde cada peça do sistema de IA mora e por quê".

Vamos então para **Engenharia de Requisitos** — quer que eu já comece a aula 1, ou prefere primeiro me contar se você já tem algum contato com esse tema (tipo, já ouviu falar de "requisito funcional" e "não-funcional") pra eu calibrar o ponto de partida?