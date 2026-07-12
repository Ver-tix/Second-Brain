---
tags:
  - programação
  - inteligenciaartificial
---
## Aula 2 — Dados: o combustível do sistema

Se na aula 1 você viu que o não-determinismo é o que torna um sistema de IA diferente, agora vem a explicação de **de onde vem esse comportamento**: dos dados. Existe um ditado no meio de IA que resume isso bem: "garbage in, garbage out" — lixo entra, lixo sai. Vale entender por quê.

**Dois papéis diferentes que dado tem num sistema de IA**

É importante você separar bem duas coisas que se confundem fácil:

1. **Dado de treinamento**: o que foi usado pra construir ou ajustar o modelo em si.
2. **Dado de contexto (em tempo real)**: o que você busca e entrega pro modelo na hora da pergunta — que é o RAG que você já aprendeu na aula 4 da parte anterior.

No seu módulo prático, você provavelmente vai lidar quase só com o segundo tipo (você não vai treinar um LLM do zero), mas é essencial entender os dois pra não confundir os conceitos quando aparecerem.

```text
Dado de treinamento
   → usado uma vez (ou de tempos em tempos) pra criar/ajustar o modelo
   → fica "dentro" do modelo, viraram parâmetros

Dado de contexto
   → usado a cada pergunta, em tempo real
   → nunca "entra" no modelo permanentemente, só é emprestado pra aquela resposta
```

**Qualidade de dado: o problema real**

Um modelo de IA aprende **padrões** a partir dos dados que recebe. Se o dado de treinamento tiver viés, erro, ou lacuna, o modelo carrega isso pra sempre em seu comportamento. Exemplo clássico: se um sistema de IA pra triagem hospitalar foi treinado majoritariamente com dados de um perfil de paciente, ele pode performar pior com pacientes de outro perfil. Isso não é "bug" no sentido tradicional de programação — é uma limitação estrutural que vem do próprio dado usado.

No dado de contexto (RAG) o problema é parecido, só que na hora: se o banco vetorial que você monta tiver informação desatualizada, errada ou mal categorizada, o LLM vai receber lixo como contexto — e mesmo sendo um modelo ótimo, ele vai gerar uma resposta ruim, porque a matéria-prima que ele recebeu estava ruim.

**Pipeline de dados: como isso vira arquitetura**

Isso não é fluffy, vira decisão concreta de sistema. Um pipeline de dados típico:

```text
Coleta (de onde vem o dado bruto — sistema legado, documentos, formulários)
   ↓
Limpeza (remove duplicado, corrige formato, trata dado faltante)
   ↓
Transformação (formata pro uso — ex: converte texto em embedding pro banco vetorial)
   ↓
Armazenamento (banco relacional ou vetorial, como você já viu)
   ↓
Disponibilização (a camada de lógica/orquestrador acessa esse dado quando precisa)
```

Repara que essas etapas de coleta/limpeza/transformação **não são feitas na hora da pergunta do usuário** — elas acontecem antes, como preparação. É um processo separado, muitas vezes chamado de "pipeline offline", em contraste com o fluxo "online" (em tempo real) que você já viu no orquestrador.

**Conectando com os desafios anteriores**

No desafio do hospital, o "banco de protocolos médicos" que alimenta o RAG não aparece do nada — alguém (ou algum processo automatizado) precisou coletar os documentos, limpar formatação, quebrar em pedaços (chamado de "chunking", você provavelmente vai ouvir esse termo), transformar em embedding, e só então guardar no banco vetorial. Se esse processo de preparação for malfeito, o sistema inteiro sofre — mesmo que o orquestrador e o LLM estejam perfeitos.

**Por que isso é "a parte mais negligenciada"**

Porque é trabalho menos glamouroso — não é a parte que "conversa" ou "responde". É faxina, organização, verificação. Mas é a parte que mais frequentemente quebra sistemas de IA na prática. Um princípio bom pra guardar: **a qualidade do seu sistema de IA tem um teto definido pela qualidade dos seus dados** — não importa quão bom seja o modelo ou quão esperto seja o orquestrador, se o dado na base for ruim, o resultado também será.

---

Próxima aula: **camada de serving/inferência** — onde o modelo efetivamente "roda" e fica disponível pra ser chamado, incluindo a diferença entre usar um modelo pronto via API (como você tem feito) versus hospedar seu próprio modelo.