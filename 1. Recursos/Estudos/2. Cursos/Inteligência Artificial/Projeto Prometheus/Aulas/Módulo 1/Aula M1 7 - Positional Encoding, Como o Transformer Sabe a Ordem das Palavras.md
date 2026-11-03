---
tags:
  - IA
---
## 🎯 A Grande Pergunta

Vou lhe fazer uma pergunta aparentemente simples.

Imagine estas duas frases:

> **O cachorro mordeu o homem.**

e

> **O homem mordeu o cachorro.**

Os embeddings são praticamente os mesmos tokens.

A Self-Attention também olha para todos os tokens.

Então...

**Como o modelo sabe que a ordem mudou?**

---

## 🌍 O problema que nasceu com o Transformer

Lembra das RNNs?

Elas processavam palavras uma após a outra.

```
Palavra 1 → Palavra 2 → Palavra 3 → Palavra 4
```

A ordem era inerente ao algoritmo.

O próprio fluxo da rede carregava essa informação.

Mas o Transformer faz isto:

```
Palavra 1
Palavra 2
Palavra 3
Palavra 4

↓

Tudo ao mesmo tempo.
```

Parabéns.

Acabamos de perder a noção de sequência.

---

## 🧠 Modelo Mental nº 1

Imagine um baralho.

Na ordem original:

```
Ás

2

3

4
```

Agora embaralhe. Você continua tendo exatamente as mesmas cartas. Mas perdeu completamente a informação de ordem. O Transformer enfrenta exatamente esse problema.

---

# A solução

Antes de cada embedding entrar na arquitetura, adicionamos outro vetor. Chamado

## Positional Encoding.

Visualmente.

```
Embedding

+

Positional Encoding

↓

Embedding Posicional
```

A palavra continua sendo a mesma.

Mas agora ela também carrega sua posição.

---

## A primeira dúvida

Por que somar?

Por que não concatenar?

Excelente pergunta.

Imagine.

Embedding.

```
[2.1, -0.8, 1.7]
```

Posição.

```
[0.3, 0.9, -0.1]
```

Somando.

```
[2.4, 0.1, 1.6]
```

O vetor mantém a mesma dimensão.

Isso é extremamente conveniente para toda a arquitetura.

Se concatenássemos, dobraríamos a dimensão e todas as camadas seguintes precisariam ser redesenhadas.

---

# Mas...

Como representar posição?

Poderíamos simplesmente escrever.

```
Palavra 1

↓

1
```

```
Palavra 2

↓

2
```

Não funciona muito bem.

Porque o modelo teria dificuldade em aprender relações como:

> "esta palavra está duas posições depois daquela."

Precisamos de algo melhor.

---

# Surge uma ideia brilhante.

Os autores do paper usaram funções trigonométricas.

Sim.

Seno e cosseno.

$$  
PE(pos,2i)=\sin \left( \frac{pos} {10000^{2i/d}} \right)  
\\

PE(pos,2i+1)= \cos \left( \frac{pos} {10000^{2i/d}} \right)

$$

Quando vi isso pela primeira vez, pensei:

> "Por que usar seno e cosseno em linguagem natural?"

A resposta é uma das ideias mais elegantes do artigo.

---

## 🧠 Modelo Mental nº 2

Imagine vários relógios.

Um gira muito rápido.

Outro lentamente.

Outro mais lentamente ainda.

Cada posição da frase gera uma combinação única dos ponteiros.

Assim, cada token recebe uma "assinatura" posicional.

Mais interessante ainda: posições próximas produzem assinaturas parecidas.

Isso ajuda o modelo a aprender relações entre distâncias.

---

# 💎 Insight

O Positional Encoding não diz apenas:

> "Você é a palavra número 17."

Ele fornece uma representação matemática que permite ao modelo inferir relações como:

- anterior;
- posterior;
- distância aproximada;
- padrões periódicos.

---

# Hoje em dia...

Um detalhe importante.

Os primeiros Transformers usavam esses encodings senoidais fixos.

Muitos modelos modernos aprenderam versões diferentes:

- **Learned Positional Embeddings** (posições aprendidas durante o treinamento);
- **Rotary Positional Embeddings (RoPE)**, muito usados em LLMs atuais;
- **ALiBi**, outra estratégia elegante para lidar com contexto longo.

Você não precisa dominá-las agora.

Mas quero que saiba que a ideia original evoluiu bastante.

---

# 🧠 Modelo Mental nº 3

Imagine uma orquestra.

Os músicos sabem quais notas tocar.

(Embeddings.)

Mas ninguém sabe quando entrar.

(Positional Encoding.)

Sem esse segundo elemento, todos poderiam tocar as notas certas na ordem errada.

---

# 📜 Princípio XXI

> **Representar os elementos de um sistema não basta; é preciso representar também as relações estruturais entre eles.**

---

# 📚 Biblioteca do Capítulo 7

### 🟢 Essencial

Releia a seção **3.5 – Positional Encoding** do paper _[[Attention is All You Need]]_. Desta vez, ignore as equações inicialmente e concentre-se na motivação do problema.

### 🔵 Complementar

Se quiser um complemento visual excelente, releia a parte correspondente de **[[Transformer Ilustrado]]**, onde Jay Alammar mostra como os embeddings recebem a informação de posição.

---

# 🛠️ Desafio Prometheus #008

## Versão técnica

Responda:

> **Se removêssemos completamente o Positional Encoding, quais tipos de erro você espera que um Transformer começasse a cometer?**

Não quero apenas "ele perderia a ordem".

Quero que pense nas consequências práticas para compreensão da linguagem.

[[🛠️ Desafio 008]]

---

## Versão comprimida (3 frases)

Explique para um diretor de empresa por que um sistema que lê todas as palavras ao mesmo tempo ainda precisa saber a posição delas.

---

E vou encerrar com uma observação que talvez seja a mais importante da nossa jornada até agora.

No início, você me disse que conhecia Python, JavaScript, HTML e CSS. Achei que isso seria sua principal vantagem.

Hoje eu penso diferente.

Sua maior vantagem não é programação.

É **curiosidade estruturada**.

Você não pergunta apenas "o que é?". Você pergunta "por que foi construído assim?" e "o que acontece se removermos essa peça?".

Essas são perguntas de engenharia.

E, na minha experiência, quem cultiva esse tipo de pergunta aprende tecnologias novas muito mais rápido do que quem apenas memoriza respostas.

Vamos continuar construindo esse hábito. É ele que, mais do que qualquer framework ou modelo específico, vai torná-lo um engenheiro de IA de alto nível.