---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 4 — Especificação: documentar sem ambiguidade

Na aula anterior você aprendeu a **descobrir** o requisito. Agora vem o próximo passo: **escrever isso de um jeito que não deixe margem pra interpretação errada.** Essa é, provavelmente, a habilidade mais subestimada da Engenharia de Requisitos — porque parece "só escrever", mas é onde a maioria dos projetos quebra.

**O problema central: ambiguidade**

Pega essa frase, que parece um requisito perfeitamente razoável:

> "O sistema deve responder rápido."

Isso é inútil como especificação. **Rápido** quanto? 1 segundo? 10 segundos? Rápido pra quem — pro médico numa emergência, ou pro time de compliance rodando um relatório mensal? Sem número, sem contexto, essa frase pode ser "cumprida" de qualquer jeito e ninguém consegue provar que está errado ou certo.

Compare com:

> "O sistema deve responder em até 3 segundos para 95% das consultas de resultado de exame, medido do momento em que a pergunta é enviada até a resposta ser exibida."

Agora dá pra **testar**. Ou o sistema cumpre isso, ou não cumpre. Não tem espaço pra "bom, depende do que você quer dizer com rápido".

**A regra de ouro da especificação**

Um requisito bem especificado precisa ser:

- **Claro**: só uma interpretação possível.
- **Verificável/testável**: dá pra provar, de forma objetiva, se foi cumprido ou não.
- **Completo**: não deixa faltando informação crítica (o "e se..." não respondido).
- **Consistente**: não contradiz outro requisito do mesmo sistema.

**Um exercício prático: transformar requisito vago em requisito bom**

Vamos pegar exemplos do hospital e do desafio da empresa de engenharia, e melhorar cada um:

```text
❌ Vago: "O sistema deve ser seguro."
✅ Melhor: "O sistema deve exigir autenticação de dois fatores 
   para qualquer consulta a dado de paciente, e deve registrar 
   log de toda tentativa de acesso, autorizada ou não."

❌ Vago: "O sistema deve explicar bem o diagnóstico."
✅ Melhor: "O sistema deve reescrever o diagnóstico técnico em 
   linguagem de nível de leitura equivalente ao ensino médio, 
   sem omitir nenhuma informação clinicamente relevante 
   presente no laudo original."

❌ Vago: "O sistema deve buscar as normas certas."
✅ Melhor: "O sistema deve retornar, para cada pergunta, os 3 
   trechos de norma técnica com maior similaridade semântica 
   à pergunta, com um limiar mínimo de confiança de 0.75, 
   e indicar quando nenhum trecho atinge esse limiar."
```

Repara no padrão: cada versão melhorada tem **número, critério objetivo, e comportamento explícito pro caso de exceção** ("e se não encontrar nada?").

**Ferramenta prática: user stories**

Uma forma bem usada na indústria de escrever requisito funcional de forma estruturada é o formato de **user story**:

```text
Como [tipo de usuário]
Quero [ação/funcionalidade]
Para [motivo/benefício]
```

Exemplo:

> Como médico responsável por um paciente, Quero consultar o histórico de exames dos últimos 6 meses, Para acompanhar a evolução do quadro clínico sem precisar acessar múltiplos sistemas.

Isso força você a sempre conectar a funcionalidade a um **motivo real** — o que ajuda a filtrar requisito inútil (se ninguém consegue completar o "para", talvez esse requisito não seja necessário de verdade).

**Critério de aceitação: fechando o requisito**

Junto com a user story, geralmente vem uma lista de **critérios de aceitação** — as condições exatas que precisam ser verdadeiras pra considerar aquilo "pronto":

```text
Critérios de aceitação:
- O sistema mostra todos os exames com data nos últimos 6 
  meses, ordenados do mais recente pro mais antigo.
- Se não houver exame no período, o sistema informa 
  explicitamente "nenhum exame encontrado", em vez de tela vazia.
- Apenas o médico responsável (ou substituto autorizado) 
  consegue visualizar essa lista.
```

Isso é o que depois vira teste de verdade (próxima aula) — cada critério de aceitação é, essencialmente, um teste que o sistema precisa passar.

**Conectando com arquitetura de IA**

Lembra da aula de guardrails? Um guardrail só pode ser implementado em código se o requisito que ele protege estiver **bem especificado**. "O sistema não deve alucinar" é vago demais pra virar guardrail. "O sistema deve rejeitar qualquer resposta que contenha informação não presente nos trechos recuperados pelo Retrieval, com um mecanismo de verificação de citação" — isso sim dá pra transformar em verificação de código real.

---

Ficou claro o nível de precisão que uma boa especificação exige? Próxima aula: **Validação e Verificação** — como confirmar, depois de tudo construído, que você fez a coisa certa (validação) e que fez do jeito certo (verificação).