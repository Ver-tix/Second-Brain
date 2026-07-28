---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
# ETAPA 1 — REMOVER `ToolDecision`

O desafio pede explicitamente:

> **"Remova a dependência de `ToolDecision` do `MentorAgent`."**

Então a primeira missão é bem objetiva.

### O que deve desaparecer do `MentorAgent`?

Você deverá remover:

1. O import:

```Python
from app.services.tool_decision import ToolDecision
```

2. A linha:

```Python
tool = ToolDecision.decide(question)
```

3. Todo o bloco:

```Python
if tool == "calculator":
    ...
```

Ao final dessa etapa, o `MentorAgent` **não decidirá mais absolutamente nada** sobre ferramentas.

Ele apenas enviará a requisição ao `LLMService`.

> **Ainda não implemente a nova lógica. Apenas remova a antiga.**

Quando terminar essa etapa, me envie o novo `MentorAgent`. A partir daí construiremos juntos a nova responsabilidade do `LLMService`, que será o verdadeiro protagonista da Aula 8.2.

# Então, qual é o primeiro passo?

Eu faria exatamente isto:

De:

```Python
def generate(self, prompt: str) -> str:
```

Para:

```Python
def generate(self, prompt: str):
```

(removemos temporariamente o `-> str`)

E, no final:

De:

```Python
return response.output_text
```

Para:

```Python
return response
```

Perceba que ainda **não estamos implementando Tool Calling**.

Estamos apenas parando de jogar informação fora.

É um pequeno refactor que prepara o restante da Aula 8.2.

---
# Próximo passo

Agora chegamos ao coração da Aula 8.2.

O desafio diz:

> **"Faça com que o `LLMService` detecte quando a resposta da API contém um tool call em vez de uma resposta textual."**

Antes de escrever código, porém, quero fazer do jeito que temos feito em todos os laboratórios.

**Não vamos adivinhar a estrutura da resposta da OpenAI.**

Como estamos estudando arquitetura e não "tentativa e erro", a próxima coisa que faremos é **inspecionar o objeto `response`** para descobrir como ele representa um Tool Call.

Essa é exatamente a postura de um engenheiro: primeiro observar a estrutura dos dados, depois escrever a lógica que os interpreta. Isso evita criar código baseado em suposições.

---
# ETAPA 2 - INSPECIONAR O OBJETO `Response`

Até agora nós sempre fizemos:

```python
return response.output_text
```

Nunca olhamos para o restante do objeto.

Agora precisamos conhecê-lo.

## Minha proposta

Vamos fazer um experimento.

Temporariamente, substitua o final do método por:

```python
print(response)

return response
```

ou, melhor ainda:

```python
print(response.model_dump())

return response
```

Se a sua versão da biblioteca OpenAI suportar `model_dump()`, ela imprimirá a estrutura inteira da resposta.

---

# Por que fazer isso?

Porque eu **não quero adivinhar**.

Quero descobrir.

Imagine que você abriu uma caixa-preta.

Primeiro olhamos o conteúdo.

Depois decidimos como interpretar.

Essa é uma prática excelente em engenharia.

---

# O que esperamos encontrar?

Algo conceitualmente parecido com:

```
Response
│
├── output
│
├── output_text
│
├── tool_calls
│
└── ...
```

**Não estou afirmando que é exatamente assim.**

Estou dizendo que **vamos descobrir**.

---

# O experimento

Infelizmente, aqui temos um obstáculo.

Sua API Key ainda está inválida (erro 401 que vimos anteriormente), então você não conseguirá executar essa chamada até configurar uma chave válida.

Por isso, temos duas alternativas:

### Opção A (a que eu prefiro)

Quando você tiver a API funcionando, executamos esse experimento e observamos a resposta real.

### Opção B

Continuamos acompanhando a arquitetura da aula e, quando chegarmos ao momento de testar, ajustamos conforme a estrutura real retornada pela API.

---

## Minha recomendação

Eu seguiria a **Opção B**.

O professor deixou claro que o foco desta aula é a **mudança arquitetural**, não os detalhes da API.

Então podemos continuar implementando o fluxo e, quando você configurar uma API Key válida, validaremos tudo na prática.

**Vamos seguir assim?** Acho que é a forma mais produtiva de não interromper o laboratório por causa de uma dependência externa.