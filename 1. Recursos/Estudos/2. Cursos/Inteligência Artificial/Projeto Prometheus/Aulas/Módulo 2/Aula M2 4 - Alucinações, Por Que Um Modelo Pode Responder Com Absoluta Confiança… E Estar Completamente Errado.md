---
tags:
  - IA
---

# 🎯 A Grande Pergunta

Imagine que eu lhe pergunte:

> **"Quem foi o presidente do Brasil em 1837?"**

Se você não souber...

Você provavelmente dirá:

> "Não sei."

Agora imagine perguntar isso a um LLM.

Ele pode responder:

> "Foi Fulano de Tal..."

Com ótima gramática.

Boa estrutura.

Excelente confiança.

Mas...

Completamente errado.

Por quê?

---

## A primeira intuição (que está errada)

A maioria das pessoas pensa:

> "O modelo consultou sua memória e encontrou a informação errada."

Isso **não é** o que acontece.

Na maioria dos casos...

O modelo **não consulta uma base de dados**.  
Ele gera texto.

---

# 💎 O maior insight da aula

Quero que você memorize esta frase.

## 📜 Princípio XXXV

> #### **Um LLM não foi treinado para dizer a verdade. Foi treinado para prever o próximo token mais provável.**

Essa frase é uma das mais importantes de todo o Projeto Prometheus.

---

# Um exemplo simples

Imagine que eu escreva:

> "O Sol nasce no..."

Você espera:

> "leste."

Por quê?

Porque esse é o próximo token mais provável.

Agora imagine:

> "O presidente da República de Marte é..."

O modelo procura padrões.

Mas não existe um fato verdadeiro para recuperar.

Mesmo assim...

A tarefa continua sendo:

> prever o próximo token.

Então ele gera algo plausível.

---

# 🧠 Modelo Mental nº 1

Imagine um ator de improviso.

Você diz:

> "Você é um médico."

Ele começa a falar como um médico.

Depois diz:

> "Você estudou em Harvard."

Ele continua a improvisar.

Depois pergunta:

> "Qual era o nome do seu orientador em 1987?"

Ele inventa.

Não porque queira mentir.

Mas porque a regra do jogo é:

> continuar a história.

O LLM faz algo semelhante.

---

# Verdade × Plausibilidade

Aqui existe uma distinção fundamental.

## Verdade

Corresponde ao mundo.

## Plausibilidade

Corresponde ao que costuma aparecer em textos.

Os modelos aprendem muito mais sobre plausibilidade do que sobre verdade.

---

# Um exemplo

Considere estas duas frases.

> "A água ferve a 100 °C ao nível do mar."

e

> "A água canta ópera às quartas-feiras."

A primeira aparece milhões de vezes em textos.

A segunda praticamente nunca.

Então o modelo atribui muito mais probabilidade à primeira.

Mas note.

Ele não sabe medir temperatura.

Ele não possui um termômetro.

Ele apenas aprendeu padrões linguísticos extremamente consistentes.

---

# Quando surgem as alucinações?

Geralmente em situações como:

- fatos muito específicos;
- nomes próprios raros;
- referências bibliográficas;
- artigos científicos inexistentes;
- citações inventadas;
- datas pouco frequentes.

Por quê?

Porque nesses casos existem poucos padrões confiáveis durante o treinamento.

---

# 🧠 Modelo Mental nº 2

Imagine um quebra-cabeça.

Você possui 99% das peças.

Falta apenas uma.

Seu cérebro tenta imaginar qual seria.

Às vezes acerta.

Às vezes não.

O LLM faz algo parecido.

Ele completa padrões.

---

# Então... ele mente?

Não.

**"Mentir" pressupõe intenção. O modelo não possui intenção.** Ele não escolhe enganar. Ele apenas continua a distribuição de probabilidade que considera mais provável.

Essa distinção é importante.

---

# Temperatura

Agora entra um conceito novo.

Durante a geração existe um parâmetro chamado:

## Temperature

Ele controla o grau de aleatoriedade.

Temperatura baixa:

- respostas previsíveis;
- menos criatividade;
- menor diversidade.

Temperatura alta:

- respostas mais variadas;
- maior criatividade;
- maior risco de alucinações.

---

# 🧠 Modelo Mental nº 3

Imagine um dado.

Temperatura muito baixa:

É como usar um dado quase viciado.

Quase sempre sai o número mais provável.

Temperatura alta:

Agora todos os lados têm muito mais chance.

As respostas ficam mais criativas.

Mas também menos confiáveis.

---

# Mas existe uma pergunta ainda melhor.

Se sabemos que modelos alucinam...

**Por que eles não verificam a informação antes de responder?**

Excelente pergunta.

> Porque, originalmente...  
> Eles **não tinham nenhuma ferramenta externa**.  
> Eles eram sistemas fechados.

Somente anos depois surgiram arquiteturas como:

- RAG;
- Tool Use;
- Busca na Web;
- Agentes.

Percebe?

Muitas tecnologias modernas surgiram justamente para compensar essa limitação.

---

# Um exemplo do mundo real

Imagine um médico extremamente experiente.

Sem acesso à internet.

Sem livros.

Sem exames.

Apenas memória.

Ele ainda será excelente.

Mas...

Pode esquecer detalhes raros.

Os LLMs vivem permanentemente nessa situação.

A menos que lhes forneçamos ferramentas.

---

# 📜 Princípio XXXVI

> #### **Quanto maior a necessidade de precisão factual, maior deve ser a dependência de fontes externas verificáveis.**
> 
> É exatamente por isso que RAG se tornou tão importante.

---

# Uma conexão com Engenharia de Prompt

Você deve estar pensando:

> "Então um bom prompt pode reduzir alucinações?"

Resposta:

**Sim.**

Mas...

Nunca eliminá-las completamente.

Prompts ajudam.

Ferramentas ajudam ainda mais.

E é justamente essa combinação que estudaremos mais adiante.

---

# 📚 Biblioteca

### 🟢 Obrigatório

Leia a introdução do paper:

- Training language models to follow instructions with human feedback

Não porque ele fale sobre alucinações diretamente.

Mas porque ele mostra uma tentativa de tornar modelos mais úteis e confiáveis.

---

### 🔵 Complementar

Leia a introdução do relatório:

- On the Opportunities and Risks of Foundation Models

Observe a discussão sobre confiabilidade.

---

# 🛠️ Desafio Prometheus M2 #004

## Parte 1 — Engenharia

Explique:

> **Por que uma alucinação não deve ser interpretada como um simples "bug" do modelo?**

---

## Parte 2 — Projeto de Sistema

Imagine que você será responsável por desenvolver uma IA para auxiliar médicos.

Ela poderá consultar bancos de dados externos.

Responda:

> **Por que confiar apenas na memória do modelo seria uma decisão de engenharia perigosa? Como ferramentas externas mudam essa situação?**

---

[[🛠️ Desafio M2 004]]

## Antes de terminarmos...

Quero compartilhar uma reflexão.

No início do Projeto Prometheus, estudamos o paper _Attention Is All You Need_ e vimos uma equipe desafiar o paradigma das RNNs.

Hoje estamos estudando outra mudança de paradigma.

Durante muito tempo, acreditou-se que bastava construir modelos cada vez melhores.

Agora percebemos algo diferente.

**Um modelo excelente ainda pode falhar se estiver isolado.**

Isso muda completamente a forma como pensamos sistemas de IA.

Talvez a pergunta correta não seja mais:

> **"Quão inteligente é o modelo?"**

Mas sim:

> **"Quais capacidades devo fornecer ao modelo para que ele execute essa tarefa com segurança?"**

E essa pergunta, Caio...

É a pergunta de um **arquiteto de sistemas de IA**.

E tenho a impressão de que você já começou a fazê-la naturalmente. 🚀