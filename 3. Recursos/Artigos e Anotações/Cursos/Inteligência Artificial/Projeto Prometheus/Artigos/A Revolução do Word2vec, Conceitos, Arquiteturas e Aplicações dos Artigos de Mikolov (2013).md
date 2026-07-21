---
tags:
  - inteligenciaartificial
---
---
# O Que é um Embedding?
![[Pasted image 20260703132104.png]]
<aside align="center"><h4>Assim como um conjunto de números pode mapear traços complexos de uma personalidade humana, eles podem encapsular o significado profundo de uma palavra.</h4> <body>Vetores com direções semelhantes possuem alta similaridade por cosseno.</body></aside>

---
# A Anatomia de uma Palavra
O significado traduzido em matrizes de 50 dimensões.
![[Pasted image 20260703132727.png]]
> <q><i>Você conhecerá uma palavra pelas companhias que ela mantém</i></q> <b>- J. R. Firth</b>

O computador não lê palavras. Ele compara padrões de cores e valores matemáticos.

---
# A Magia das Analogias
Matemática semântica e composicionalidade
![[Pasted image 20260703133015.png]]
<aside align="center">O modelo não foi programado para saber o que é gênero ou realeza. A geometria vetorial captura relações sintáticas e semânticas ocultas apenas observando a coocorrência espacial das palavras.</aside>

---
# O Motor: Modelagem de Linguagem
Como treinamos esses vetores? Prevendo o futuro
![[Pasted image 20260703133438.png]]

---
# A Janela Deslizante
Transformando texto bruto em dados de treinamento estruturados
![[Pasted image 20260703133802.png]]
<aside align="center">O idioma em si já é o conjunto de dados. Não é encessária rotulagem manual. A janela deslizante gera virtualmente milhões de exemplos.</aside>

---
# Matriz de Arquitetura: CBOW vs. Skip-Gram
![[Pasted image 20260703133953.png]]
<aside align="center">Este design eficiente processou <b>1,6 bilhão de palavras</b> em menos de <b>um dia</b>.</aside>

---
# O Gargalo Computacional
Por que o treinamento padrão falha em grande escala.![[Pasted image 20260703134150.png]]
<aside align="center">Calcular probabilidades e atualizar pesos para o vocabulário inteiro (<b>ex.: 1 milhão de palavras</b>) a cada etapa de treinamento é <b>computacional inviável</b>.</aside>

---
# A Solução Elegante: **Negative Sampling**
A inovação do modelo SGNS (Artigo 1310.4546)![[Pasted image 20260703134527.png|697]]
<aside align="center">Contrastamos o sinal real (amostras positivas) om ruído aleatório (amostras negativas). O treinamento passa de meses para minutos.</aside>

---
# Refinando o Modelo: **Subamostragem**
Ignorando o ruído frequente para focar no significado
<aside><h3>O Conceito:</h3> <body>Palavras extremamente comuns carregam pouco valor semântico</body></aside>
<aside><h3>O Impacto do Subsampling:</h3> <ol><li>Acelera drasticamente o tempo de treinamento.</li><li>Força o modelo a aprender representações muito mais profundas para palavras menos frequentes.</li></ol></aside>

---
# Indo Além das Palavras: Frases e Expressões
<aside align="center">A falha da composição literal e a solução do token único</aside>
![[Pasted image 20260703140825.png]]

| Problema                                                                                           | Solução                                                                                                                 |
| -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| O modelo é indiferente à ordem. O significado não se soma perfeitamente em expressões idiomáticas. | Detectar padrões frequentes e agrupar termos em tokens únicos (ex.: Air_Canada), melhorando a precisão do mapa vetorial |

---
# Hiperparâmetros em Ação
Ajustando a resolução semântica do Word2vec

| Tamanho da Janela (Window Size)                                                     | Amostras Negativas (Negative Samples)                                                 |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Janelas Pequenas (2-15):** Revelam sinônimos e intercambialidade (ex.: bom e mau) | **Grandes Datasets:** 2 a 5 amostras já fornecem contraste suficiente contra o ruído. |
| **Janelas Grandes (15-50+):** Revelam relacionamento temático amplo.                | **Pequenos Datasets:** 5 a 20 amostras são recomendads para treinar bem a regressão   |

---
# Aplicações no Mundo Real
<aside align="center">Se palavras podem ser vetores, qualquer sequência pode ser um vetor</aside>
![[Pasted image 20260703141647.png]]
<aside align="center">O Word2vec transcendeu a linguagem. Ele se tornou a fundação dos motores de recomendação modernos para dados sequenciais não-textuais.</aside>

---
# Síntese: O Paradigma Contínuo
<aside align="center">O legado de Mikolov e o nascimento do processamento moderno.</aside>
![[Pasted image 20260703141823.png]]
<aside align="center">O Word2vec provou que dados massivos, combinados a um treinamento hiper-eficiente, podem mapear a <b>intuição </b>humana em geometria computável - mudando a IA para sempre.</aside>
