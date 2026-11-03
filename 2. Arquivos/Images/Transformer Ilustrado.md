---
tags:
  - IA
---

---
![[Pasted image 20260703142151.png]]

| A Missão                                                                            | A Ruptura                                                                                                                                                        |
| ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tradução de máquina. Transforma uma sequência de entrada em uma sequência de saída. | Diferente das RNNs, que processam palavra por palavra como dominós em queda, o Transformer introduz paralelização massiva. Todos os dados fluem simultaneamente. |
![[Pasted image 20260703142328.png]]

---
# Composição
![[Pasted image 20260703142432.png]]
![[Pasted image 20260703142523.png]]
![[Pasted image 20260703142533.png]]

---
# A Engenharia
![[Pasted image 20260703142830.png]]

| A Engenharia                                                                                                                  | Analogia do Sistema de Biblioteca                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Multiplicamos o Embedding original por matrizes de pesos treinadas para criar três novas abstrações de dimensão reduzida (64) | <ul><li><b>Query (A Pergunta):</b> Estou procurando livros sobre biologia (O que a palavra atual está buscando no contexto)</li><li><b>Key (A Etiqueta):</b> O título na lombada do livro na estante. (O que as outras palavras da frase têm a oferecer)</li><li><b>Value (O Conteúdo):</b> o texto real do livro (A essência semântica da palavra que será extraída caso haja um match)</li></ul> |

---
# Múltiplas Cabeças

| <h3 align= "center">A Fera de Múltiplas Cabeças</h3>                                                                        | <h3 align= "center">Por que 8 Vezes?</h3>                                                                                                                                                                                                             | <h3 align= "center">A Compreensão</h3><br>                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| O Transformer usa 8 cabeças de atenção simultâneas (oito conjuntos de matrizes $Q,$ $K,$ $V$ inicializadas aleatoriamente). | Garante foco múltiplo: Evita que a palavra olhe excessivamente apenas para si mesma. Cria subespaços de representação: Como 8 detetives em uma cena de crime. Um procura sujeitos gramaticais, outro foca no tom emocional, outro na relação de tempo | A rede Feed-Foward precisa de uma única matriz, não oito. Concatenamos os resultados e multiplicamos por $W^0$ para fundir os achados dos "8 detetives". |

---
# Positional Encoding
![[Pasted image 20260703144152.png|697]]

| <h3 align= "center">O Caos do Paralelismo</h3>                                                                                                         | <h3 align= "center">Positional Encoding (O Carimbo de Tempo)</h3>                                                       | <h3 align= "center">A Gemoetriadas Ondas</h3>                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Se todas as palavras entram de uma vez, o modelo não tem noção de tempo espacial. "O gato caça o rato" é matematicamente igual a "O rato caça o gato". | Adicionamos um vetor especial a cada Embedding antes dele entrar no Encoder, injetando o contexto de distância e ordem. | Usando funções de Seno e Cosseno intercaladas, o modelo cria padrões relativos de distância. Isso permite que a rede dimensione o entendimento até para frases mais longas do que as vistas durante o treinamento. |
![[Pasted image 20260703144215.png]]

---
## Onde Fica Cada Componentes

|                          | **Encoder<br>Self-Attention**          | **Decoder<br>Masked Attention**                                                              | **Encoder-Decoder<br>Attention**                                                                                 |
| ------------------------ | -------------------------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Onde <br>FIca**        | Dentro do Encoder                      | Base do Decoder                                                                              | Topo do Decoder                                                                                                  |
| **Visão de<br>Contexto** | Olha para todas as palavras de entrada | Olha apenas para o passado                                                                   | Cruza Entrada e Saída                                                                                            |
| **Mecânica<br>Chave**    | $Q$, $K$, $V$ vêm da mesma frase       | Posições futuras recebem **Máscara (-infinito)** para o modelo não roubar olhando a resposta | As Queries vêm da palavra atual gerada pelo Decoder, mas Keys e Values vêm do topo da pilha do Encoder original. |
![[Pasted image 20260703144604.png]]

---
![[Pasted image 20260703144712.png]]
![[Pasted image 20260703144730.png]]

---
# Attention Is All You Need
<aside><h2>O Triunfo do Paralelismo (Velocidade):</h2> Removendo o gargalo sequencial dos RNNs, permitiu escalar o treinamento usando clusters massivos de GPUs em montanhas de dados. Nascia a base estrutural para GPTs e LLMs modernos.</aside>
<aside><h2>Atenção de Longo Alcance (0(1)):</h2> Não há mais amnésia em textos longos. Cada palavra =, não importa a distância, tem contato matematicamente direto com todas as outras palavras através dos cálculos de matrizes de Atenção.</aside>
