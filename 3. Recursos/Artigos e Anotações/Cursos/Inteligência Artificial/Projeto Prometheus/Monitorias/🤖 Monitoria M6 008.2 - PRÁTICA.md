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

---

Pelo material da Aula 8.2, o próximo passo é exatamente implementar a **bifurcação** no `LLMService`: verificar se a resposta da API contém um Tool Call ou uma resposta textual.

Como você ainda não possui créditos na API, **não conseguiremos validar essa lógica executando o programa**, mas podemos escrever a estrutura corretamente.

O fluxo conceitual do método passa a ser:

```
response = OpenAI

↓

A resposta contém Tool Call?

├── SIM
│     ↓
│  executar ToolManager
│     ↓
│  devolver resultado
│
└── NÃO
      ↓
  devolver resposta textual
```

A implementação exata dessa detecção depende da estrutura do objeto retornado pela biblioteca da OpenAI (qual atributo indica um Tool Call). Como **não podemos executar a API** e eu quero permanecer fiel ao material do professor, não vou inventar nomes de atributos que não aparecem na aula.

**Então preciso que você me envie o próximo trecho da Aula 8.2** (a parte em que o professor mostra ou descreve essa detecção). A partir daí implementamos exatamente como ele ensina, sem adivinhar a API e sem correr o risco de construir algo diferente do laboratório.

---

# ETAPA 3 — ALTERAR O CONTRATO DO `LLMService`
Até hoje ele fazia:

```Python
response = self.client.responses.create(...)

return response
```

Mas agora o professor quer algo diferente.

Ele escreveu:

> "O `LLMService` deve ser capaz de informar quando recebeu um Tool Call."

Perceba a palavra:

> **informar**.

Isso significa que o `LLMService` não deve mais devolver apenas um objeto cru da OpenAI.

Ele deve devolver uma informação interpretada.

---

## Como eu estruturaria isso (sem fugir da aula)

Em vez de:

``` Python
return response
```

Ele poderá devolver algo conceitualmente assim:

``` Python
{
    "type": "text",
    "content": ...
}
```

ou

``` Python
{
    "type": "tool_call",
    "tool": ...
}
```

Perceba que ele **não executou** a ferramenta.

Apenas disse:

> "Recebi um Tool Call."

Isso respeita exatamente o que o professor escreveu.

---

## Depois, o MentorAgent fará:

```
LLMService

↓

Resposta

↓

É texto?

↓

SIM

↓

Mostra ao usuário

↓

NÃO

↓

ToolManager

↓

Executa ferramenta
```

Isso é exatamente o diagrama da aula.

---

## Um detalhe importante

Perceba como as responsabilidades ficaram bonitas:

### LLMService

```
Conhece OpenAI

↓

Traduz Response

↓

Entrega informação
```

---

### MentorAgent

```
Coordena o fluxo
```

---

### ToolManager

```
Executa ferramentas
```

Nenhum invade a responsabilidade do outro.

---

## Então vamos para a implementação

A primeira alteração será no `LLMService`.

Mas, como ainda não temos API para testar, **não vamos inventar a forma de detectar um Tool Call na resposta da OpenAI**.

Vamos fazer exatamente como o professor orientou: preparar a arquitetura.

### Minha sugestão

Vamos alterar o retorno do `LLMService` para um formato padronizado (por exemplo, um dicionário com `"type"` e `"content"`), deixando a lógica de detecção marcada como um ponto de evolução para quando tivermos a Aula 8.3 e/ou uma API funcional.

Assim continuamos fiéis ao laboratório, sem codificar em cima de suposições sobre a estrutura da resposta da API.
## Passo 1 - Criar um retorno no `LLMService`
Substitua o final do método por:

```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY, MODEL_NAME


class LLMService:

    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str):

        response = self.client.responses.create(
            model=MODEL_NAME,
            input=prompt,
            tools=[
                {
                    "type": "function",
                    "name": "calculator",
                    "description": "Realiza operações matemáticas básicas entre dois números."
                }
            ]
        )

        # Nesta aula ainda simulamos que veio texto.
        return {
            "type": "text",
            "content": response.output_text
        }
```

## Passo 2 - Atualizar o `MentorAgent`
Troque a parte final do método `ask()` por:
```Python
response = self.llm_service.generate(prompt) 
if response["type"] == "text": 
	self.memory.add_assistant_message(response["content"]) 
	return response["content"] 

elif response["type"] == "tool_call": 
	return self.tool_manager.calculate(response["content"])
```