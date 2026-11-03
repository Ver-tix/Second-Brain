---
tags:
  - programação
  - IA
---


Tudo que você aprendeu até agora (camadas, orquestrador, API, banco de dados, auth) continua valendo **integralmente**. Arquitetura de Sistemas de IA não é uma coisa nova do zero — é a arquitetura de software de sempre, com uma peça a mais que exige cuidado redobrado: **um componente que não é determinístico**.

**O que isso quer dizer, na prática**

Num sistema de software tradicional, se você manda a mesma entrada duas vezes, recebe a mesma saída duas vezes. Uma função que soma dois números sempre vai dar o mesmo resultado. Isso é determinismo — e é o que te permite testar, prever, confiar.

Um LLM (ou qualquer modelo de IA) **não garante isso**. Mande a mesma pergunta duas vezes e pode vir uma resposta ligeiramente diferente. Isso muda a forma como você precisa pensar arquitetura, porque agora você tem um componente no meio do seu sistema que se comporta mais como "um funcionário que pode ter dias diferentes" do que como "uma calculadora".

**O mapa que você já conhece, agora expandido**

Lembra do mapa da aula 6 anterior? Ele mostrava Usuário → Apresentação → Lógica (orquestrador) → LLM → Lógica → Usuário. Isso é, na verdade, um **zoom** dentro de uma parte maior. O quadro completo de um Sistema de IA é assim:

text

```text
Dados
  ↓
Modelo (treinado ou escolhido pronto, tipo LLM via API)
  ↓
Camada de Serving (coloca o modelo "no ar", disponível pra ser chamado)
  ↓
Orquestração (o que você já aprendeu: decide quando chamar o modelo, com qual contexto)
  ↓
Aplicação / Usuário
  ↓
Feedback (o que aconteceu depois da resposta volta pro sistema)
  ↓
(volta pros Dados, fechando o ciclo)
```

Tudo que a gente viu até aqui (aulas 1-6) foi um mergulho fundo só na parte de "Orquestração". Agora vamos abrir as outras partes desse ciclo maior.

**Por que essa peça extra — o não-determinismo — muda tanto a arquitetura**

Porque ela te obriga a adicionar camadas que um sistema tradicional não precisa:

- Você precisa de **guardrails**: uma camada que verifica a saída do modelo antes de entregar, porque ele pode errar de formas imprevisíveis (aula 4).
- Você precisa de **avaliação contínua**: não basta testar uma vez e "funcionou", porque o comportamento pode mudar com o tempo, com dado novo, com atualização do modelo (aula 5).
- Você precisa de **feedback loop**: o sistema precisa aprender ou pelo menos registrar o que deu certo e errado, pra melhorar (também aula 5/6).

Um sistema tradicional (tipo o site de compras de uma loja) não precisa de nada disso, porque o comportamento dele é previsível por natureza. Um sistema de IA precisa, porque o coração dele é probabilístico.

**Uma analogia pra fixar**

Pensa num sistema tradicional como uma máquina de vending: você aperta o botão A2, sai sempre o mesmo salgadinho. Um sistema de IA é mais como contratar um atendente novo: ele geralmente acerta, mas você precisa de supervisão (guardrails), avaliação de desempenho (observabilidade) e treinamento contínuo (feedback/MLOps) — porque ele não é uma máquina fixa, é algo que "se comporta".