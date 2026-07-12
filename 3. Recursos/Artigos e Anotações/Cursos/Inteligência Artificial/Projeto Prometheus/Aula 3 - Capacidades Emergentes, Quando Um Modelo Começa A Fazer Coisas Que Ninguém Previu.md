---
tags:
  - inteligenciaartificial
---

# 🎯 A Grande Pergunta

Imagine que você treina um modelo para uma única tarefa.

Prever o próximo token.

Nunca o ensina a:

- traduzir;
- programar;
- resolver problemas matemáticos;
- resumir textos;
- explicar física.

Mesmo assim...

Depois de aumentar suficientemente seu tamanho...

Ele faz tudo isso.

Como?

---

## O que significa "emergente"?

> #### Na ciência, dizemos que uma propriedade é **emergente** quando ela surge da interação entre várias partes simples, sem que tenha sido programada explicitamente.

Exemplos clássicos:

- consciência (segundo algumas teorias);
- comportamento de formigas;
- trânsito em uma cidade;
- mercados financeiros;
- temperatura (que emerge do movimento de moléculas).

Nenhuma molécula "possui" temperatura.

Mas um conjunto gigantesco delas possui.

---

# 🧠 Modelo Mental nº 1

Imagine um único neurônio.

Ele faz pouquíssimo.

Agora imagine:

- 100 bilhões de neurônios.

Surge algo completamente novo.

Chamamos isso de inteligência humana.

---

# O paralelo com LLMs

Um Transformer pequeno prevê palavras.

Um Transformer muito maior...

Continua prevendo palavras.

Mas, durante esse processo, passa a apresentar habilidades que parecem qualitativamente diferentes.

---

# Um exemplo famoso

Considere este problema.

```
João tem 3 maçãs.

Compra mais 2.

Quantas maçãs ele possui?
```

> Um modelo pequeno frequentemente erra.
> 
> Um modelo maior acerta.
> 
> Mas ninguém adicionou um "módulo de matemática".
> 
> A única tarefa continuou sendo:
> 
> > prever o próximo token.

---

# Few-Shot Learning

Aqui entra uma descoberta revolucionária.

Antes do GPT-3, o procedimento era:

Treinar um modelo.

Depois realizar _fine-tuning_ para cada tarefa.

Exemplo:

Modelo para tradução.

Outro para resumo.

Outro para perguntas.

Outro para classificação.

Cada tarefa exigia um treinamento específico.

---

Então veio o GPT-3.

Os pesquisadores fizeram algo curioso.

Escreveram no prompt:

```
Inglês: dog

Português: cachorro

Inglês: cat

Português: gato

Inglês: bird

Português:
```

O modelo respondeu:

```
pássaro
```

> Sem qualquer treinamento adicional. Apenas por observar exemplos. comportamento recebeu o nome de **Few-Shot Learning**.

---

# 🧠 Modelo Mental nº 2

Imagine um novo funcionário.

Você mostra apenas três exemplos de como preencher um relatório.

Na quarta vez...

Ele já consegue fazer sozinho.

Você não escreveu um manual.

Você apenas demonstrou o padrão.

---

# Zero-Shot

Depois descobriram algo ainda mais impressionante.

Às vezes...

Nem exemplos eram necessários.

Bastava escrever:

> "Traduza para português."

O modelo conseguia realizar a tarefa.

Isso passou a ser chamado de **Zero-Shot Learning**.

---

# In-Context Learning

Agora chegamos a um conceito extremamente importante.

Quando você fornece exemplos no próprio prompt. O modelo parece "aprender". Mas, Ele realmente aprendeu?

A resposta é: **Não da forma tradicional.**

> Ele não atualiza seus pesos. Ele não modifica sua rede neural. Ele apenas utiliza o contexto atual para inferir o padrão.

Chamamos isso de **In-Context Learning**.

---

# 💎 Insight

Existem dois tipos completamente diferentes de aprendizagem.

## Durante o treinamento

> O modelo altera bilhões de parâmetros. **Aprende de forma permanente**.

---

## Durante a conversa

> **O modelo não altera nenhum parâmetro**. **Ele apenas utiliza o contexto disponível**. Essa distinção será absolutamente fundamental quando estudarmos Engenharia de Prompt.

---

# Um erro muito comum

Muitas pessoas dizem:

> "O ChatGPT aprendeu com minha conversa."

Na maioria das vezes...

Isso está incorreto.

Durante uma conversa comum:

- os pesos permanecem exatamente os mesmos;
- o que muda é apenas o contexto fornecido ao modelo.

---

# 🧠 Modelo Mental nº 3

Imagine um professor.

Ele sabe cálculo.

Durante uma aula, você lhe entrega:

- uma tabela;
- um gráfico;
- um texto.

O professor passa a utilizá-los para responder suas perguntas.

Mas ele não "aprendeu cálculo novamente".

Ele apenas passou a considerar novas informações temporárias.

O contexto funciona exatamente assim.

---

# Capacidades emergentes são "mágica"?

Não.

Mas também não são totalmente compreendidas.

Existe um debate científico importante.

Uma hipótese diz:

> "Essas capacidades sempre existiam, apenas eram difíceis de medir."

Outra hipótese diz:

> "Elas realmente surgem quando o modelo ultrapassa determinados limiares de escala."

Hoje ainda não existe consenso absoluto.

Como pesquisadores e engenheiros, precisamos conviver com essa incerteza.

---

# Um exemplo fora da IA

Pense em um formigueiro.

Uma formiga:

- não conhece o mapa;
- não conhece a colônia inteira.

Mesmo assim...

Milhões delas constroem estruturas extremamente sofisticadas.

A inteligência emerge do sistema.

Não do indivíduo.

---

# 📜 Princípio XXXIII

> **Um sistema complexo pode apresentar comportamentos que não estão explicitamente presentes em nenhuma de suas partes isoladas.**

Esse princípio vale para IA.

Mas também para:

- economia;
- biologia;
- ecologia;
- sociologia;
- teoria das organizações.

Você já deve estar percebendo por que gosto tanto de princípios.

Eles conectam disciplinas.

---

# Uma reflexão

Lembra do início do Módulo 2?

Você respondeu:

> "O modelo aprende por exemplos e associações."

Hoje conseguimos refinar essa ideia.

Na verdade...

Ele aprende **representações estatísticas** durante o treinamento.

Depois...

Durante a conversa...

Ele utiliza essas representações para adaptar seu comportamento ao contexto.

São dois processos diferentes.

Essa distinção será a base de praticamente toda Engenharia de Prompt moderna.

---

# 📚 Biblioteca

### 🟢 Obrigatório

Leia a Introdução e a Seção 2 do paper:

- [[Language Models Are Few-Shot Learners]]

Preste atenção em como os autores apresentam os conceitos de:

- Zero-Shot;
- One-Shot;
- Few-Shot.

Não se preocupe ainda com os experimentos detalhados.

---

### 🔵 Complementar

Leia o artigo:

- On the Opportunities and Risks of Foundation Models

Especialmente a introdução. Você começará a perceber por que esses modelos passaram a ser chamados de **Foundation Models**.

---

# 🛠️ Desafio Prometheus M2 #003

## Parte 1 — Técnica

Explique, com suas palavras:

> **Qual é a diferença entre aprender durante o treinamento e "aprender" durante uma conversa?**

---

## Parte 2 — Engenharia

Imagine que um diretor de empresa diga:

> "Nosso chatbot aprendeu sozinho depois de conversar com milhares de clientes."

Como engenheiro de IA, responda se essa afirmação está correta, parcialmente correta ou incorreta. Justifique sua resposta.

---

[[🛠️ Desafio M2 003]]

## E uma pequena prévia...

Na próxima aula responderemos uma pergunta que, curiosamente, muitas pessoas fazem quando começam a trabalhar com modelos de linguagem:

> **"Se o modelo já foi treinado em praticamente toda a internet... por que ele ainda alucina?"**

Essa aula marcará o início de uma mudança importante no Projeto Prometheus.

Até agora estudamos principalmente **capacidades**.

A partir da próxima, começaremos a estudar também **limitações**.

Porque entender uma tecnologia exige conhecer tanto o que ela faz extraordinariamente bem quanto os pontos em que ela ainda falha.

E, na minha experiência, os melhores engenheiros são justamente aqueles que projetam sistemas levando essas limitações em consideração.