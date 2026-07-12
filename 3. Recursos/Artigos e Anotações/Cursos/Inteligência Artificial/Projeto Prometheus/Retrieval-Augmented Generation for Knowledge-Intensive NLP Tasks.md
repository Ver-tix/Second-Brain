---
tags:
  - inteligenciaartificial
---


# O Problema: IAs Fazem Provas "Sem Consulta"
## O Diagnóstico:
Modelos de linguagem pré-treinados (como GPT ou BART) decoram muito conhecimento em seus parâmetros, mas dependem exclusivamente da própria memória para responder
<aside><h3>Sintoma 1: Alucinações</h3><body>Inventam fatos plausíveis quando a memória falha</body></aside>
<aside><h3>Sintoma 2: Amnésia Estática</h3><body>O conhecimento congela na data do treinamento. Não sabem o que aconteceu ontem.</body></aside>
<aside><h3>Sintoma 3: Caixa Preta</h3><body>Não conseguem rastrear de qual "página" tiraram a informação.</body></aside>

---
# Diagnóstico de Sistemas: Os Dois Tipos de Memória

|                   | Memória Paramétrica<br>(O Cérebro)                               | Memória Não-Paramétrica <br>(A Biblioteca)                                    |
| ----------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **O que é?**      | Conhecimento embutido nos "pesos" da rede neural (ex.: T5, BART) | Conhecimento armazenado em um índice externo de vetores (ex.: Wikipedia)      |
| **Transparência** | Opaca (Caixa Preta)                                              | Transparente (Podemos ler o documento)                                        |
| **Atualização**   | Requer meses de treinamento caro                                 | Imediata. Basta trocar os documentos.                                         |
| **Tamanho**       | Limitada pelos bilhões de parâmetros                             | Escalável para bilhões de documentos (21 milhões de artigos usados no estudo) |

---
# A Tese: Apresentando o RAG (O Exame com Consulta)
> E se o aluno mais brilhante da sala pudesse levar a enciclopédia inteira para a prova?

$\text{Buscador Neural e Indice Externo} + \text{Gerador Seq2Seq Pré-Treinado} + \text{Retrieval-Augmented Generation (RAG)}$

<aside><body>O RAG não apenas copia textos. Ele combina a vasta biblioteca do mundo com a fluência gramaticam de um modelo neural para sintetizar respostas factuais.</body></aside>

---
# Dissecando a Arquitetura: Como a Informação Flui
## 1. Entrada ($x$)
A Pergunta (ex.: "defina ouvido médio").
## 2. Entrada (Retriever $p_\eta$)
#### ==Memória Paramétrica==
O codificador busca em 21 milhões de documentos ($z$) e retorna o Top-K mais relevante.
## 3. O Escritor (Generator $p _{\theta}$)
#### ==Memória Paramétrica==
Lê a pergunta original + os documentos recuperados e inicia a redação.
## 4. Marginalização
Pondera as respostas baseadas em diferentes documentos para o consenso matemático.
## 5. Saída ($y$)
#### ==Síntese e Saída (output)==
A resposta final perfeitamente formulada.

---

# Zoom-in: O Arquivista Super-Sônico (DPR)
Como achar a agulha em um palheiro de 21 milhões de artigos em milissegundos? Não procuramos por palavras; procuramos por conceitos matemáticos.
<aside><h3>Representação Densa</h3><body>O Dense Passage retriever (DPR) transformaperguntas e textos da Wikipedia em pontos no espaço (vetores).</body></aside>
<aside><h3>A Busca (MIPS)</h3><body>O sistema mede a "distância" geométrica. Textos que respondem à pergunta ficam fisicamente mais próximos a ela. O Arquivista simplesmente pega os vizinhos mais próximos (Top-K)</body></aside>

---
# Zoom-in: O Escritos com Contexto (BART)
O RAG usa um modelo transformador Seq2Seq de 400 milhões de parâmetros (BART). Seu trabalho não é grifar o texto, mas compreendê-lo![[Pasted image 20260702164142.png]]

---
# Duas Formas de Fazer a Prova: Sequence vs. Token

| RAG-Sequence (O Purista)                                                                                                                                                        | RAG-Token (O Sintetizador)                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| O modelo usa o mesmo documento recuperado para inspirar a frase inteira.<br><br>Ideal para respostas curtas e diretas, calculando a probabilidade da sequência toda de uma vez. | O modelo pode consultar um documento diferente para cada palavra (token) que escreve.<br><br>Permite costurar pedaços de múltiplos textos em uma única resposta genial. |

---
# Estudo de Caso: A Pergunta do "Show do Milhão"
Gerando perguntas complexas no estilo Jeopardy sobre o autor Ernest Hemingway. Observe o 'cérebro' mudando de foco no meio da frase.![[Pasted image 20260702164723.png]]
<aside><b>insight:</b> O RAG-Token costurou ativamente dois fatos de dois artigos diferentes em uma única frase fluida.</aside>

---

# O Boletim de Notas: Eficiência vs. Força Bruta
![[Pasted image 20260702164908.png]]
<aside>O <b>RAG</b> estabeleceu um novo Estado da Arte (SotA) em tarefas de conhecimento intensivo. Ele superou abordagens puramente paramétricas gigantescas, precisando de uma fração minúscula de parâmetros treináveis, simplesmente poque sabe 'pensar'.</aside>

---

# Avaliação Humana: Menos Mentiras, Mais Fatos
![[Pasted image 20260702165254.png]]

<aside>No teste FEVER (Verificação de Fatos), o RAG atinge acurácias fortíssimas classificando verdadeiro/ falso apenas buscando suas próprias evidência na Wikipedia.</aside>

---

# A Vantagem do 'Hot-Swap': Atualizando o Mundo a Frio
O Conhecimento episódico é dissociado do raciocínio linguístico. A memória pode ser atualizada em tempo real.

---

# Síntese: A Sala de Aula do Futuro é Híbrida

<aside><h3>O Fim da Decoreba</h3> Modelos de linguagem não precisam tentar embutir todo o conhecimento do mundo de forma indeficiente em seus próprios pesos</aside>
<aside><h3>Separação de Poderes</h3>Ao isolar o Conhecimento Literário (Biblioteca/Busca) do Raciocínio Linguístico (Cérebro/Gerador), criamos sistemas superiores.</aside>
<aside><h3>O Novo Paradigma</h3>O RAG entrega IA que é atualizável, interpretável, fidedigna e incrivelmente mais eficente</aside>

<aside></aside>
