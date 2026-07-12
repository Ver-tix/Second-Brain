---
tags:
  - inteligenciaartificial
---
## O espectro de autonomia (a indústria formaliza isso)

| Nível                    | Modelo                                                        | Onde o humano está                    |
| ------------------------ | ------------------------------------------------------------- | ------------------------------------- |
| Human-in-the-loop (HITL) | agente age, humano aprova cada passo sensível                 | dentro do fluxo, em portões           |
| Human-on-the-loop (HOTL) | agente age sozinho num escopo delimitado, humano supervisiona | monitorando, intervindo só em exceção |
| Human-out-of-the-loop    | autonomia total                                               | ausente — raro e perigoso             |
Na pequena empresa da [[🤖 Desafio M4 008 ZCode|Questão 2]], o desenho saudável é **híbrido**: :
- _HITL_ para compra e comunicação (Camada 3), 
- _HOTL_ para relatórios e atualização de planilha (Camada 2 autônoma + Camada 4 de auditoria). 

Total autonomia (`out-of-the-loop`) quase nunca é o objetivo certo — é geralmente um erro disfarçado de eficiência.
## Os dois erros clássicos (e como evitá-los)

- **Excesso de automação:** remover o humano dos portões achando que o agente "já está bom". Resultado: o primeiro caso fora do padrão vira desastre, e ninguém estava olhando.
- **Excesso de desconfiança:** transformar o agente numa máquina de pedir aprovação para _tudo_. Resultado: o humano vira um carimbo que aprova sem ler, e o agente perde toda a utilidade.

## O ponto de equilíbrio é: 
Colocar o humano **onde a decisão é irreversível ou onde falta informação ao agente** — e soltá-lo onde a operação é reversível e rotineira.