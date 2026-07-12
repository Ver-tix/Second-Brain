---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 5 — Observabilidade e Avaliação

Aqui entra uma pergunta que separa sistema de brinquedo de sistema de produção: **como você sabe, com o tempo, se o sistema continua funcionando bem?**

**Por que isso é diferente de software tradicional**

Num sistema tradicional, você testa uma vez ("essa função soma certo?") e, se passou, você confia que sempre vai passar — porque é determinístico, lembra da aula 1? Com IA, isso não é suficiente. O comportamento pode mudar por vários motivos: o modelo por trás da API é atualizado pela empresa que o mantém, o tipo de pergunta que os usuários fazem muda com o tempo, o banco vetorial do RAG vai crescendo e mudando. Um sistema que funcionava bem há três meses pode estar performando pior hoje, e ninguém percebe se não estiver observando.

**Observabilidade: enxergar o que está acontecendo**

É a prática de registrar e monitorar o que o sistema está fazendo, em tempo real, pra conseguir investigar problemas depois.

```text
Cada interação do sistema deveria registrar:
- qual foi a pergunta
- qual contexto foi montado e enviado ao modelo (aquele RAG, aquele dado buscado)
- qual foi a resposta do modelo
- quanto tempo levou (latência)
- quanto custou (tokens usados)
- se algum guardrail foi acionado (bloqueou algo?)
- se o usuário deu algum feedback (👍/👎, por exemplo)
```

Isso tudo vira **log** — um registro guardado que permite, depois, você investigar: "por que essa resposta saiu errada semana passada?" Sem esse registro, você fica no escuro, tentando adivinhar o que aconteceu.

**Avaliação: medir qualidade de forma sistemática**

Observabilidade te dá o dado bruto. Avaliação é o processo de **usar esse dado pra medir se o sistema está bom**, de forma estruturada — não só "parece que está indo bem".

Duas formas comuns de avaliar:

- **Avaliação humana**: pessoas revisam uma amostra de respostas e classificam (correto/incorreto, apropriado/inapropriado). É mais confiável, mas cara e lenta — não escala pra todo volume.
- **Avaliação automatizada**: usar métricas ou até outro LLM pra avaliar as respostas do primeiro (chamado às vezes de "LLM as a judge"). É mais barato e escala melhor, mas tem suas próprias limitações — afinal, você está usando um componente não-determinístico pra avaliar outro.

**Um conceito chave: dataset de avaliação (benchmark interno)**

Uma prática comum é montar um conjunto fixo de perguntas com respostas "corretas" conhecidas (chamado de golden dataset ou conjunto de teste), e rodar o sistema contra esse conjunto regularmente. Se o desempenho cair nesse conjunto fixo, é sinal de alerta — algo mudou (modelo, dado, RAG) e precisa de investigação, antes que o usuário real perceba o problema.

```text
Sistema é modificado (mudou o modelo, mudou o prompt, mudou o RAG)
     ↓
Roda contra o conjunto de teste fixo
     ↓
Compara desempenho: melhorou, piorou, ou igual?
     ↓
Só libera a mudança em produção se o resultado for aceitável
```

Isso é o equivalente, em sistemas de IA, ao que teste automatizado é em software tradicional — só que em vez de "passou/falhou" binário, geralmente é uma métrica de qualidade (% de respostas corretas, por exemplo).

**Conectando com os exemplos anteriores**

No hospital, você não quer descobrir que o sistema começou a alucinar diagnóstico errado só quando um médico reclamar. Com observabilidade e avaliação contínua, você detecta a degradação de qualidade antes que isso vire um problema real — monitorando as respostas, comparando contra o conjunto de teste, e revisando os casos em que o guardrail (aula 4) foi acionado.

**A relação entre as três últimas aulas**

Guardrails (aula 4) atuam **na hora**, bloqueando problema em tempo real. Observabilidade e avaliação (essa aula) atuam **ao longo do tempo**, detectando tendências e degradação que um guardrail pontual não pegaria sozinho. Os dois são complementares: um protege o momento, o outro protege a trajetória.

---

Última aula dessa parte: **MLOps** — como tudo isso (dados, modelo, serving, guardrails, avaliação) se organiza como um ciclo de vida contínuo, não como algo que você constrói uma vez e esquece. Depois disso, partimos pra Engenharia de Requisitos.