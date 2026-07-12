## Tags

#inteligenciaartificial #artigo

---

# Módulo 1: O Problema do Fine-Tuning
Historicamente, a **IA dependia do ==Fine Tuining== (ajuste fino)**. Precisávamos de milhares de exemplos específicos para "reprogramar" um modelo para uma única nova tarefa. O problema? **É necessário um ==dataset gigante para cada novo problema==**.

---
## Evolução de Como as Máquinas Aprendem

| Método                       | Atualização <br>de Pesos? | Dados<br>Necessários          | Analogia do Professor                                                                                                          |
| ---------------------------- | ------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Fine-Tuning                  | Sim                       | Milhares                      | *O aluno decora um livro inteiro apenas para passar em uma única prova de múltipla escolha.*                                   |
| Zero-Shot                    | Não                       | Apenas instrução              | *O professor dá uma tarefa totalmente nova na lousa e o aluno resolve usando apenas lógica prévia.*                            |
| One-Shot                     | Não                       | Instrução + 1 Exemplo         | *O professor mostra um único exemplo resolvido na lousa e pede para o aluno fazer o próximo.*                                  |
| Few-Shot <br>(foco do GPT-3) | Não                       | Instrução + 10 a 100 exemplos | *O aluno vê um padrão de 3 a 4 exercícios na lousa e entende instantaneamente a regra sem precisar estudar o livro novamente.* |

---
# Módulo 2: A Receita da Escala (O Motor do GPT-3)

![[Captura de tela 2026-07-03 081207.png]]
<aside>O GPT-3 não é uma arquitetura revolucionária. A revolução é a escala geométrica da arquitetura existente (GPT-2).</aside>

---
## O Conceito Chave: O Modelo como um Meta-Learner
Escalar a rede neural transformou o GPT-3 em um **meta-aprendiz**. Ele possui **dois ciclos operacionais distintos.**
![[Captura de tela 2026-07-03 081436.png]]

---
## A Janela de Contexto na Prática (Few-Shot)
<code>[Instrução] Traduza inglês para francês:</code>  ==Definição da tarefa==

<code>[Exemplo 1] sea otter => loutre de mer</code>       ==O K = 10 a 100 exemplos que ensinam== 
<code>[Exemplo 2] peppermint => menthe poivrée</code>   ==o padrão (condicionamento)==

<code>[Teste] cheese =></code> ==A fronteira onde o modelo completa a sequência==

<aside>Tudo isso deve caber em exatos 2048 tokens. Não há treinamento, apenas predição do próximo caractere baseada no padrão recém-descoberto</aside>

---
# Módulo 3: O Boletim Escolar do GPT-3 (42 Benchmarks)

| Excelente                                                                                                                                                                                                               | Na Média                                                                                                      | Reprovado / Precisa Melhorar                                                                                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| - **LAMBDA:** 76,2% Zero-Shot (Prevendo a última palavra de um parágrafo).<br>- **TriviaQA:** Supera modelos Fine-Tuned de domínios fechados<br>-**Tradução para o Inglês:** Performance estelar de Fr -> En e De -> En | - **SuperGLUE:** Razoável no formato Few-Shot<br>- **ARC CHallenge:** Raciocínio de Ciências da 3ª a 9ªsérie. | - **WiC:** Sentido das palavras. Apenas acerta por pura sorte (50%)<br>- **DROP:** Raciocínio lógico e matemática discreta.<br>Taadução do Inglês: En-Ro perde feio por limitação do tokenizador |

---
## A Mágica Acontece na Escala (SuperGLUE)

![[Captura de tela 2026-07-03 082531.png]]

---
## Onde A Máquina Tropeça (Limitações Estruturais)

| <h4>O Problema do Retrovisor</h4>                                                                                                                                                                    | <h4>Repetição Cega</h4>                                                                                                                                                        | <h4>Falta de Chão (Grounding)</h4>                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| O GPT-3 é estritamente autorregressivo (Lê apenas da esquerda para a direita). Tarefas que exigem olhar para trás e comparar duas partes de um texto (como o benchmark, são pontos cegos anatômicos) | Em textos muito longos, o modelo perde o fio da meada, contradiz a si mesmo repetitivamente e gera frases 'non-sequitur' (sem nenhuma conexão lógica com o parágrafo anterior) | É um cérebro numa jarra. Pesa cada token igualmente e não tem contato com vídeo, físico ou interação no mundo real. Falta o 'senso comum' construído pela experiência física. |

---
# Módulo 4: Impactos Sociais, Ética e Energia
![[Captura de tela 2026-07-03 083119.png | O Custo Ambiental do Pré-Treinamento (Milhares de petaflops/s-dias)]]
O Custo Ambiental do Pré-Treinamento (Milhares de petaflops/s-dias)
<aside><h3>O Contraponto da Eficiência</h3>Apesar do custo da construção ser astronômico, a operação de um modelo gigante já treinado é surpreendentemente barata. Gerar 100 páginas de texto custa apenas cerca de 0,4 kW/h (alguns centavos de energia).</aside>

---
## A Fronteira da Enganação (Fake News)
Até que ponto os humanos conseguem distinguir um artigo de notícias gerado por IA? À medida que o modelo cresce, nossa precisão desaba
![[Captura de tela 2026-07-03 083725.png|692]]

---
## O Radar do Viés: Gênero
<q>Modelos treinados na internet possuem vieses do tamanho da internet.</q>
![[Pasted image 20260703083918.png]]

---
## O Radar do Viés: Raça e Religião

![[Pasted image 20260703084004.png]]

---
# Síntese: A Transição do Hardware para o Software
<aside>A grande descoberta do GPT-3 não é apenas o tamanho, mas uma mudança de fase em como ensinamos as máquinas.</aside>

| O Passado (*Hardwiring*)                                                                                                                                                                                 | O Presente (Softwiring)                                                                                                                                                                                                     |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Fine-Tuning** - mudar o comportamento de uma IA exigia alterar sua própria anatomia física (atualizar milhões de pesos da rede neural). Era como soldar novos cabos na placa-mãe para cada nova tarefa | **Aprendizado In-Context** - Com 175 Bilhões de parâmetros, a IA atingiu uma densidade onde o contexto se tornou a interface. Programar a IA passou a ser um exercício de comunicação humana, não de engenharia de software |
