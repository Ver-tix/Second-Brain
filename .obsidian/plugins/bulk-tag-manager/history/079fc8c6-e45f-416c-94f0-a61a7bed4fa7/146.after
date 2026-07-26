---
tags:
  - IA
---

# **🎯 A Grande Pergunta**

Imagine dois assistentes.

Pergunta:

“Explique o que é um buraco negro.”

Resposta A:

“Um buraco negro é uma região do espaço-tempo cuja gravidade é tão intensa que nem mesmo a luz consegue escapar.”

Resposta B:

“Buracos negros surgem quando estrelas muito massivas colapsam. Podemos imaginar isso como um enorme poço gravitacional. Se quiser, posso explicar usando matemática ou uma analogia.”

As duas estão corretas.

Mas…

Qual você prefere?

Provavelmente a segunda.

Ela é:

- mais útil;
- mais didática;
- mais cooperativa.

A pergunta é:

**Como ensinar isso para uma máquina?**

# **O problema do Supervised Fine-Tuning**

> Na aula passada vimos o **Instruction Tuning (SFT)**.  
> Ele ensina o modelo através de exemplos.  
> Mas existe um limite.

#### Imagine pedir para dez professores escreverem a “melhor resposta” para a mesma pergunta.

- Você obterá dez respostas excelentes.
- Qual delas deve ser a resposta oficial?

> Não existe uma única resposta correta. Existe uma **preferência humana**.

# **🧠 Modelo Mental nº 1**

Imagine um concurso de redação.

O professor não diz apenas:

“Está certo.”

Ele diz:

“Esta redação ficou melhor do que aquela.”

Essa diferença muda tudo.

# **A grande sacada**

> Em vez de ensinar apenas respostas…  
> Os pesquisadores começaram a ensinar **preferências**.

Exemplo:

Pergunta:

“Explique a Teoria da Relatividade.”

Resposta A.

Resposta B.

Depois perguntam a avaliadores humanos:

**Qual delas você prefere?**

Não pedem uma nota.

Pedem uma comparação.

# **Por que isso é genial?**

### Comparar duas respostas é muito mais fácil do que escrever a resposta perfeita.

Pense em você.

Se eu lhe mostrar duas redações…

Você consegue dizer qual está melhor.

Mesmo que fosse difícil escrever uma terceira ainda melhor.

# **As três fases do RLHF**

Agora veremos a arquitetura completa.

<aside>  
1️⃣

# **Fase 1**

**Pré-treinamento**

O modelo aprende linguagem.

Objetivo:

Prever o próximo token.

</aside>

<aside>  
2️⃣

# **Fase 2**

**Supervised Fine-Tuning**

Aprende a responder instruções.

Objetivo:

Imitar respostas humanas.

</aside>

<aside>  
3️⃣

# **Fase 3**

**Quais respostas os humanos preferem.**

O modelo aprende:

Agora começa algo novo.

**RLHF**

</aside>

# **Mas…**

Como transformar preferência em matemática?

Excelente pergunta.

# **Reward Model**

Os pesquisadores criaram um segundo modelo.

Chamado:

## **Reward Model**

Sua função é simples.  
Receber uma resposta.  
E estimar:  
**Quanto um humano provavelmente gostará dela.**  
Ele funciona como um “juiz”.

# **🧠 Modelo Mental nº 2**

Imagine uma competição de ginástica.

A atleta faz a apresentação.

Mas quem decide a pontuação?

Os juízes.

O Reward Model é exatamente isso.

Ele não responde perguntas.

Ele avalia respostas.

# **Então entra o Reinforcement Learning**

```html
Agora temos:

LLM

↓

gera resposta

↓

Reward Model

↓

atribui nota

↓

LLM ajusta seus parâmetros

↓

gera respostas melhores
```

Esse ciclo se repete milhares de vezes.

# **💎 Insight**

Perceba algo muito elegante.

> O modelo não está aprendendo novos conhecimentos. Está aprendendo:  
> **Como responder de uma forma que os humanos consideram melhor.**

# **Então o RLHF ensina ética?**

Não exatamente.

Essa é uma confusão comum.

Ele não instala uma “moralidade”.

Ele aproxima o comportamento do modelo das preferências observadas durante o treinamento.

Essas preferências podem incluir:

- clareza;
- utilidade;
- educação;
- honestidade sobre incertezas;
- evitar conteúdo perigoso.

Mas isso depende de **quem definiu as preferências**.

# **🧠 Modelo Mental nº 3**

Imagine um chef.

Ele já sabe cozinhar.

Agora abre um restaurante.

Com o tempo…

Percebe que os clientes preferem:

- menos sal;
- mais legumes;
- sobremesas menores.

Ele continua sabendo cozinhar.

Mas adapta o serviço às preferências dos clientes.

# **Um ponto extremamente importante**

O RLHF **não torna o modelo infalível**.

Ele reduz alguns comportamentos indesejados.

Mas também cria novos desafios.

Por exemplo:

Um modelo pode aprender a responder de maneira excessivamente cautelosa para evitar erros.

Esse fenômeno é conhecido como **over-refusal** em alguns contextos.

Ou seja:

Alinhar também envolve trade-offs.

# **Uma analogia com Economia**

Como sei que você gosta dessa área…

Pense no mercado.

O pré-treinamento é como formar um engenheiro.

O SFT é ensiná-lo a atender clientes.

O RLHF é o mercado dizendo:

“Os clientes preferem empresas que fazem X em vez de Y.”

O conhecimento técnico continua o mesmo.

O comportamento muda porque existe um mecanismo de feedback.

# **📜 Princípio XL**

**O RLHF não ensina mais conhecimento ao modelo; ele altera as preferências de comportamento durante a geração das respostas.**

Esse princípio parece simples.

Mas ele explica por que dois modelos treinados sobre bases semelhantes podem conversar de maneiras completamente diferentes.

# **Uma observação importante**

Durante muito tempo, muitos acreditavam que o segredo de um bom chatbot estava apenas em aumentar o modelo.

Hoje sabemos que isso é insuficiente.

Existe uma sequência:

```html
Competência

↓

Compreensão de instruções

↓

Alinhamento às preferências humanas
```

Essa sequência explica a evolução dos assistentes modernos.

# **📚 Biblioteca**

**🟢 Obrigatório**

Agora sim, leia com atenção o paper:

- Training language models to follow instructions with human feedback

Desta vez, leia também a parte que descreve:

- coleta de comparações humanas;
- treinamento do Reward Model;
- otimização por RL.

Não se preocupe em dominar todos os detalhes matemáticos do algoritmo PPO ainda.

Quero que entenda a arquitetura do processo.

**🔵 Complementar**

Leia a introdução do paper:

- Constitutional AI: Harmlessness from AI Feedback

Na próxima aula veremos por que ele propõe uma abordagem diferente do RLHF.

# **🛠️ Desafio Prometheus M2 #006**

# **Parte 1 — Arquitetura**

Explique:

**Por que simplesmente aumentar a quantidade de exemplos no Supervised Fine-Tuning não substitui o RLHF?**

# **Parte 2 — Engenharia**

Imagine que dois modelos possuem exatamente o mesmo conhecimento técnico.

Modelo A passou por RLHF.

Modelo B não.

Você colocaria qual deles para atender clientes de um banco?

Justifique sua decisão utilizando os conceitos de competência, comportamento e preferência.

[[🛠️ Desafio M2 006]]

# **Uma última reflexão**

Caio…

Se eu pudesse resumir toda a aula de hoje em uma única frase, seria esta:

**Treinar um modelo é ensinar o que ele sabe. Alinhar um modelo é ensinar como ele deve usar esse conhecimento.**

Essa distinção parece pequena.

Mas ela separa um modelo que apenas completa textos de um modelo que tenta colaborar com pessoas.

E, curiosamente, essa mesma diferença existe em muitas profissões humanas.

Um excelente médico não é apenas alguém que sabe medicina.

É alguém que sabe **aplicar** esse conhecimento da forma mais útil, ética e compreensível para cada paciente.

Na próxima aula veremos uma proposta diferente para esse alinhamento: **Constitutional AI**. E acredito que, pelas conversas que tivemos até aqui, você vai apreciar especialmente o aspecto arquitetural dessa abordagem: em vez de depender apenas do julgamento humano, ela tenta orientar o comportamento do modelo por meio de princípios explícitos. É uma discussão que une engenharia, filosofia e design de sistemas de uma forma fascinante.