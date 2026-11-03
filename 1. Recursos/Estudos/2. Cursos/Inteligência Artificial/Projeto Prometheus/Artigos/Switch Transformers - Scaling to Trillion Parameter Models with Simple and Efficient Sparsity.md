---
tags:
  - IA
---

<aside><h2>Como Escalar par trilhões de parâmetros quebrando a relação entre tamanho e custo operacional</h2> </aside>

---
# Evolução: O Conselho Médico (Mixture of Experts, ou MoE):
A primeira tentativa de especialização **dividiu o modelo**, mas multiplicou a **burocracia**
![[Pasted image 20260703091131.png]]

---
# A Triagem Cirúrgica: O Switch Transformer
**Simplificar para escalar.** Ao rotear cada paciente para exatamente um especialista (k=1), cortamos a comunicação pela metade e mantemos a precisão
![[Pasted image 20260703091302.png]]

---
# Matriz Diagnóstica de Arquiteturas

|                                 | Modelo Denso (T5) |      MoE Tradicional      |    Switch Transformer     |
| :-----------------------------: | :---------------: | :-----------------------: | :-----------------------: |
|    **Decisão de Roteamento**    |       Todos       |           Top-2           |        Top-1 (k=1)        |
|   **Custo <br>(FLOPs/Token)**   | Sobe com o modelo | Constante <br>(Duplicado) | Constante <br>(Otimizado) |
|        **Estabilidade**         |       Alta        |           Baixa           |           Alta            |
| **Velocidade <br>(Pré-Treino)** |        1x         |            ∼5x            |        <h3>7x</h3>        |
<aside><b>Conclusão:</b> O Switch entrega a melhor relação entre escala e velocidade de terinamento.</aside>

---
# A Matriz de Congestionamento (Fator de Capacidade)
Chips possuem memória estática. Como lidar com um dia atípico na clínica?
![[Pasted image 20260703091844.png]]

---
# A Gangorra do Balanceamento de Carga
A perda auxiliar (Auxiliary Loss) pune o sistema se o fluxo de pacientes for mal distribuído.
![[Pasted image 20260703091950.png]]

---
# Cirurgia de Precisão (selective Precision)
Aliando a velocidade do formato bfloat16 com a estabilidade do float32.
![[Pasted image 20260703092055.png]]

---
# Prevenindo o Decoreba (Regularização)
Modelos esparsos com bilhões de parâmetros tendem ao overfitting. Como ensiná-los a generalizar?
![[Pasted image 20260703092440.png]]

---
# A Síntese: O Paradoxo da Escala Desacoplada
Tradicionalmente, adicionar parâmetros significava aumentar o tempo de computação. O Switch corta esse cordão umbilical.![[Pasted image 20260703092610.png]]

---
# A Arquitetura Lego (Paralelismo Tridimensional)

![[Pasted image 20260703092705.png]]
A Mágica da Escala: No Switch-C, alocamos 2048 especialistas em 2048 núcleos físicos. Máxima eficiência sem quebrar os pesos densos

---
# Aceleração Universal de Pré-Treinamento
A mesma energia. A mesma qualidade. Uma fração do tempo.
![[Pasted image 20260703092840.png]]
O Modelo esparso atinge a mesma capacidade cognitiva do modelo denso em 1/7 do tempo, mantendo o consumo de FLOPs constante.

---
# Fluência Global: O Teste das 101 Línguas
Modelos esparsos são aprendizes formidáveis em cenários multitarefa.
![[Pasted image 20260703093204.png|688]]
<aside><h3>5x</h3><body>Aceleração média global.</body><h3>91%</h3><body>Das línguas (do Espanhol ao Iorubá) alcançaram no mínimo 4x de aceleração.</body></aside>

---
# A Matriz de Destilação
Como Transferir a inteligência de um hospital inteiro para o cérebro de um único clinico geral.
![[Pasted image 20260703093513.png]]

<aside><b>Resultados da Destilação:</b> <br> <ul><li><b>Compreensão Total:</b> Redução de 99% no tamanho do modelo.</li><li><b>Retenção:</b> O modelo diminuto preserva 30% dos ganhos de qualidade exclusivos ao professor gigante.</li></ul></aside>

---
# Quebrando a Barreira do Trilhão
A Combinação das técnicas permitiu criar os maiores modelos da história do NLP
![[Pasted image 20260703093800.png]]
<aside><b>O Desacoplamento Comprovado:</b> O modelo colosso quebra a barreira do trilhão usando uma fração do custo computacional da arquitetura densa.</aside>

---
# Fim da Computação Estática

| Simplicidade Vence                                                                                                                         | Escala Desacoplada                                                                                                                                 | Adaptabilidade                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Substituir roteamentos complexos por um único especialista (k=1) corta o atrito computacional e gera até 7x mais velocidade de treinamento | O uso de matrizes de roteamento permite que a contagem de parâmetros aumente exponencialmente enquanto a energia por inferência permanece estática | De clusters pequenos (2 especialistas) a supercomputadores processando 1.6 trilhão de parâmetros, a esparsidade é universal e destilável. |
> <cite>A inteligência superior não resulta de ativar todas as células simultaneamente, mas de enviar o sinal perfeito, para o lugar exato, na hora certa.</cite>
