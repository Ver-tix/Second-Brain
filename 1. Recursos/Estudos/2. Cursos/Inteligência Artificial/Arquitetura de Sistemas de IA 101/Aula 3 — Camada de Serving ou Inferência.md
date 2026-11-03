---
tags:
  - programação
  - IA
---
## Aula 3 — Camada de Serving/Inferência

Essa aula responde a uma pergunta que parece besta mas não é: **onde exatamente o modelo "vive" e como ele fica disponível pra ser chamado?**

**Duas palavras que você precisa separar bem**

- **Treinamento**: o processo de "ensinar" o modelo, usando os dados de treinamento da aula 2. Isso é feito uma vez (ou de tempos em tempos), custa muito processamento, e demora — pode levar semanas.
- **Inferência**: o processo de **usar** o modelo já treinado pra responder uma pergunta específica. É isso que acontece toda vez que você manda uma pergunta pro LLM e recebe uma resposta. É rápido (segundos) e acontece toda hora.

Tudo que você já fez nos desafios anteriores — chamar o LLM via API — é inferência. Você nunca treinou nada, só usou um modelo já pronto.

**Serving: colocar o modelo "no ar"**

Serving é a infraestrutura que permite que a inferência aconteça de forma confiável, rápida, e escalável — ou seja, que aguente muita gente perguntando ao mesmo tempo sem cair.

```text
Modelo treinado (um arquivo gigante de parâmetros)
   ↓
Camada de Serving
   - recebe a pergunta
   - carrega o modelo em memória (ou já está carregado)
   - roda a inferência
   - devolve a resposta
   ↓
Resposta disponível pra quem chamou (sua aplicação)
```

**Duas formas de ter essa camada de serving: pronta ou própria**

Isso é uma decisão arquitetural real que você vai precisar tomar em projetos futuros:

**Opção 1 — Usar API de terceiro (o que você tem feito até agora)**

Você chama a API da Anthropic, OpenAI, etc. A empresa dona do modelo já cuida de toda a camada de serving pra você — você nem vê essa complexidade, só manda a pergunta e recebe a resposta.

- Vantagem: você não precisa se preocupar com infraestrutura, escala, hardware.
- Desvantagem: você depende de terceiro (custo por uso, disponibilidade fora do seu controle, dado sensível trafega pra fora da sua empresa).

**Opção 2 — Hospedar seu próprio modelo (self-hosting)**

Você mesmo roda o modelo em servidores seus (ou alugados), com sua própria camada de serving.

- Vantagem: controle total, dado sensível não sai da sua infraestrutura, sem custo por chamada (só custo de infraestrutura).
- Desvantagem: você precisa de hardware caro (GPUs), expertise técnica pra manter isso no ar, e escalar isso é trabalho seu.

**Por que isso importa pra arquitetura do sistema**

Essa decisão muda como o resto do seu sistema é desenhado. Se você usa API externa, seu orquestrador simplesmente faz uma chamada HTTP (como você já viu na aula de API) e espera resposta. Se você hospeda seu próprio modelo, o orquestrador chama um endpoint interno da sua própria infraestrutura — mas o conceito de "chamar via API" continua o mesmo, só muda quem está do outro lado.

**Conectando com o hospital**

Pensa num hospital que lida com dado super sensível de paciente. Uma decisão arquitetural real seria: "podemos mandar o prontuário do paciente pra API de um terceiro (tipo OpenAI), ou isso viola política de privacidade/LGPD e exigiria hospedar o modelo internamente?" Essa é literalmente uma decisão de arquitetura de sistema de IA que profissionais da área tomam o tempo todo — não é só teoria de curso.

**Um conceito extra que vale mencionar: latência**

Serving também precisa lidar com o tempo de resposta. Modelo grande demora mais pra responder (mais "pesado" pra rodar). Isso é uma troca (trade-off) que arquitetos de sistema de IA enfrentam sempre: modelo maior geralmente é mais inteligente, mas mais lento e mais caro. Modelo menor é mais rápido e barato, mas pode errar mais. Parte do trabalho de arquitetura é escolher o tamanho certo pro problema certo — nem todo problema precisa do modelo mais poderoso disponível.

---
