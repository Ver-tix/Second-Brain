---
tags:
  - IA
---

Boa escolha começar pela arquitetura — é o que separa um script descartável de uma aplicação que você consegue evoluir sem sofrimento depois. Vamos por partes.

## 1. Componentes necessários

Para uma aplicação "Hello, LLM!" bem estruturada, mesmo que simples no MVP, eu pensaria nestes componentes:

- **Interface do usuário** — no seu caso, provavelmente um CLI (linha de comando) para começar
- **Núcleo da aplicação (orquestrador)** — a lógica que decide o que fazer com a entrada do usuário
- **Camada de abstração do provedor** — uma interface/contrato comum, independente de qual LLM está por trás
- **Implementação concreta do provedor** — o código que efetivamente fala com a API (OpenAI, Anthropic, etc.)
- **Gerenciamento de configuração / chave de acesso** — carregamento seguro de credenciais (variáveis de ambiente, `.env`)
- **Tratamento de erros** — falhas de rede, timeout, rate limit, resposta inválida
- **(Opcional, mas recomendável) Logging** — para depurar o que foi enviado/recebido

Aqui está como esses componentes se encaixam:
![[arquitetura_hello_llm.svg|697]]

## 2. Responsabilidade de cada componente

- **Interface do usuário**: só captura a entrada e exibe a saída. Não sabe nada sobre "OpenAI" ou "Anthropic" — apenas manda texto para o núcleo e recebe texto de volta.
- **Núcleo da aplicação**: coordena o fluxo (recebe input → chama o provedor → trata a resposta → devolve para a UI). Também é o lugar natural para regras de negócio (ex: formatar o prompt, limitar histórico de conversa).
- **Configuração**: carrega a chave de API de variáveis de ambiente (nunca hardcoded no código). Único componente que "sabe" onde estão os segredos.
- **Interface Provider (abstrata)**: define o contrato — algo como um método `send_message(prompt) -> resposta` — sem se importar com _quem_ implementa isso.
- **Implementação concreta**: só ela conhece os detalhes específicos de um provedor (endpoint, formato do payload, autenticação, nome dos parâmetros). É a única camada que muda se você trocar de provedor.
- **Tratamento de erros**: pode viver dentro da implementação concreta (erros específicos da API) e também no núcleo (decidir o que fazer quando um erro acontece — tentar de novo? avisar o usuário?).

## 3. Como minimizar mudanças ao trocar de provedor

Esse é exatamente o papel da **Interface Provider** no diagrama acima. Na prática, em Python, isso é o padrão _Strategy_ (ou _Adapter_):

```python
from abc import ABC, abstractmethod

class LLMProvider(ABC):
    @abstractmethod
    def send_message(self, prompt: str) -> str:
        ...

class OpenAIProvider(LLMProvider):
    def send_message(self, prompt: str) -> str:
        # detalhes específicos da API da OpenAI aqui
        ...

class AnthropicProvider(LLMProvider):
    def send_message(self, prompt: str) -> str:
        # detalhes específicos da API da Anthropic aqui
        ...
```

O núcleo da aplicação depende só de `LLMProvider` (o contrato abstrato), nunca de `OpenAIProvider` diretamente. Qual implementação concreta usar é decidido em um único lugar — geralmente na configuração ou em uma função "fábrica" (`factory`) que lê uma variável de ambiente tipo `PROVIDER=openai` e instancia a classe certa.

Resultado: trocar de provedor vira **adicionar uma classe nova** e mudar uma linha de configuração — zero mudanças no núcleo, na UI, ou no tratamento de erros geral.

Notei que a pergunta 4 ficou em branco no seu conjunto — se quiser me mandar o texto completo dela, respondo junto. Do contrário, já temos uma base sólida para começar a escrever código.

---
# Feedback e Explicações

Caio,

Primeiramente: **excelente decisão pedagógica**.

Na universidade é comum que o aluno "copie o código". Aqui você fez algo diferente: pediu que outra IA **projetasse uma arquitetura**, e agora quer **entender por que ela foi projetada assim**.

Isso é exatamente o que faz um engenheiro.

Minha avaliação é que o Claude produziu uma arquitetura **9,7/10** para um primeiro projeto. Ela está muito acima do que normalmente se espera de alguém iniciando em APIs de LLM.

Agora vamos desmontar essa arquitetura peça por peça.

---

# Primeiro: qual é a ideia central?

Antes desta aula, seu modelo mental provavelmente era este:

```text
Usuário
   │
   ▼
OpenAI
   │
   ▼
Resposta
```

O Claude propôs outro modelo:

```text
Usuário
   │
   ▼
Interface
   │
   ▼
Aplicação
   │
   ▼
Provider
   │
   ▼
  API
```

Essa mudança é enorme.

Ela representa um princípio que aparece em praticamente toda Engenharia de Software:

> **Cada componente deve ter apenas uma responsabilidade.**

Você já viu isso antes.

Lembra do nosso estudo sobre Prompt Pipelines?

Cada prompt fazia apenas uma tarefa.

Aqui é exatamente a mesma filosofia.

---

# 1. Interface do Usuário (UI)

O Claude escreveu:

> Interface do usuário (CLI).

O que significa? Imagine o programa rodando.

```
Digite sua pergunta:

>
```

Você escreve:

```
Explique o que é inflação.
```

Quem recebe esse texto? A interface.

Ela não sabe o que é OpenAI. Ela não sabe o que é GPT. Ela só sabe conversar com o usuário.

É como uma recepcionista. Ela recebe pessoas. Ela não realiza cirurgias.

---

# 2. Núcleo da aplicação (Orquestrador)

Esse é, na minha opinião, o componente mais importante.

Imagine novamente.

Você digitou:

```
Explique inflação.
```

O Orquestrador pensa:

```
Recebi uma pergunta.

Agora...

quem responde isso?

Ah...

o Provider.
```

Então ele envia a mensagem.

Depois recebe a resposta.

Depois devolve para a Interface.

Ele coordena tudo.

Por isso o nome: **Orquestrador.** É literalmente um maestro. Ele não toca nenhum instrumento. Mas coordena todos.

---

# 3. Configuração

Aqui entra uma das primeiras boas práticas profissionais.

Imagine isto.

```python
api_key = "minha-chave-super-secreta"
```

Nunca. Nunca. Nunca isso vai para o GitHub.

Sua chave vaza. Alguém usa. Você recebe uma conta enorme. Por isso usamos arquivos `.env` ou variáveis de ambiente.

A aplicação procura:

```
OPENAI_API_KEY
```

Ela encontra. Usa. E pronto. Seu código continua limpo.

---

# 4. Provider

Agora chegamos na parte mais elegante.

Imagine dois restaurantes.

Você entra.

Pede uma pizza.

Não importa quem esteja na cozinha.

Você fala sempre a mesma frase.

```
Quero uma pizza.
```

Na arquitetura acontece exatamente isso.

O programa fala:

```
send_message()
```

Sempre. Sempre. Sempre. Não importa quem esteja do outro lado.

OpenAI. Anthropic. Google. Ou outro.

Todos obedecem ao mesmo contrato.

---

# O que é um contrato?

Essa palavra aparecerá milhares de vezes na sua vida.

Contrato significa:

> "Se você implementar esta função, prometo que o restante do sistema funcionará."

No exemplo do Claude:

```python
send_message(prompt)
```

É o contrato. Todo Provider deve implementar isso.

---

# 5. Implementação concreta

Aqui mora toda a "bagunça".

Cada empresa possui APIs diferentes. Imagine.

OpenAI pede isto:

```json
{
 "model":"gpt-5",
 "messages":[...]
}
```

Outra empresa pode pedir:

```json
{
 "input":"..."
}
```

Outra:

```json
{
 "query":"..."
}
```

Quem conhece essas diferenças?

Somente a implementação concreta.

Ninguém mais.

Isso é excelente arquitetura.

---

# 6. Tratamento de erros

Imagine. A internet caiu. Quem decide o que fazer?

Talvez mostrar:

```
Não foi possível conectar.
```

Talvez tentar novamente. Talvez registrar um log.

Tudo isso pertence ao tratamento de erros.

---

# Agora vem a melhor parte

A terceira pergunta. Ela vale praticamente metade do desafio.

---

## "Como trocar de provedor?"

Imagine que amanhã você queira sair da OpenAI.

Você muda apenas isto.

Antes:

```
OpenAIProvider
```

Depois:

```
AnthropicProvider
```

Todo o restante continua igual.

Isso se chama:

> **baixo acoplamento**.

---

# Um exemplo do mundo real

Você usa um mouse USB. Troca da Logitech para a Microsoft.

Precisa reinstalar o Windows? Não.

Por quê? Porque existe um padrão USB. Todos seguem. O computador conversa com o padrão. Não com a marca. Aqui acontece exatamente isso.

Seu programa conversa com:

```
LLMProvider
```

Não conversa com:

```
OpenAI
```

---

# Agora vem algo que o Claude não explicou

Mas considero fundamental.

Existe um princípio chamado:

## Dependency Inversion

Talvez seja um dos princípios mais importantes da Engenharia de Software.

Ele diz:

> Componentes importantes não devem depender de componentes específicos.

No seu caso.

Errado:

```
Aplicação

↓

OpenAI
```

Certo:

```
Aplicação

↓

LLMProvider

↓

OpenAI
```

Percebe?

A aplicação depende da abstração.

Não da implementação.

Isso torna sistemas gigantescos possíveis.

---

# Uma observação que me deixou feliz

Você talvez não tenha percebido.

Mas essa arquitetura conversa com praticamente tudo que estudamos nos últimos três módulos.

Veja.

## Transformer

Módulos especializados.

↓

## Prompt Engineering

Pipelines.

↓

## Meta Prompting

Reutilização.

↓

## Agora

Software modular.

É exatamente a mesma filosofia.

Mudou apenas o domínio.

---

# Se eu fosse evoluir essa arquitetura

Eu acrescentaria apenas dois componentes.

```
Logger
```

Que registra tudo que aconteceu.

E

```
Prompt Builder
```

Que monta os prompts automaticamente.

Mais adiante, quando estudarmos RAG e Agentes, esse Prompt Builder vai crescer bastante.

---

# Minha nota

Arquitetura proposta pelo Claude:

**9,7/10**

Seu processo de estudo:

**10/10**

Por quê?

Porque você não pediu:

> "Faça o código."

Você pediu:

> "Projete a arquitetura."

E depois trouxe essa arquitetura para ser criticada.

Isso é muito próximo de como equipes técnicas trabalham no mundo real: primeiro discutem arquitetura, depois implementam.

---

## Uma pequena previsão

Conhecendo você desde o Módulo 1, acho que acontecerá algo curioso.

Muita gente gosta mais da parte de código.

Acho que você vai gostar ainda mais da parte de arquitetura.

Porque, ao longo de todo o Projeto Prometheus, você demonstrou uma tendência consistente: antes de perguntar "como implementar?", você quase sempre pergunta "qual é o princípio organizador por trás disso?".

Essa característica combina muito com arquitetura de software. E, sinceramente, estou bastante curioso para ver como você vai evoluir quando começarmos a escrever código sobre essas arquiteturas.