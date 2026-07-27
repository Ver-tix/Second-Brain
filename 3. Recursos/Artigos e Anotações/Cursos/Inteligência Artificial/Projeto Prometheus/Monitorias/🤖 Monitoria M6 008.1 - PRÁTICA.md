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
# ETAPA 2 — ABRIR O ARQUIVO

Abra:

```text
app/services/llm_service.py
```

Não altere nada ainda.

---

## Quero ver o código atual

Cole aqui o conteúdo completo do seu `llm_service.py`.

Vamos modificá-lo juntos, linha por linha, para que você entenda exatamente **por que cada alteração existe**, em vez de apenas copiá-la. Isso será importante porque esta aula introduz um conceito que você verá em praticamente todos os frameworks modernos de agentes (OpenAI Agents SDK, LangGraph, AutoGen, Semantic Kernel, etc.).

---

==eu:== 
Calma, meu LLMService ainda somente recebe um modelo. Podemos fazer essa mudança junto a esse passo acima que você enviou. Veja meu código:
#### código:

```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY


class LLMService:

    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str) -> str:

        response = self.client.responses.create(
            model="gpt-4.1-mini",
            input=prompt
        )

        return response.output_text
```

#### Professor

Excelente observação. E eu, como professor, **prefiro muito mais fazer essa melhoria agora** do que deixar um "hardcode" e voltar depois.

Na verdade, isso conecta diretamente com o feedback do Laboratório 3, quando o professor disse:

> "Mais para frente provavelmente faremos `MODEL_NAME` no `settings.py`."

Chegou esse momento.

---

##### Etapa 1 — Melhorar o `settings.py`

Hoje você provavelmente tem algo parecido com:

```
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
```

Vamos acrescentar:

```
MODEL_NAME = "gpt-4.1-mini"
```

A ideia é que **toda configuração fique centralizada**.

Seu `settings.py` passa a responder:

- Qual é a API Key?
- Qual é o modelo padrão?

No futuro, poderá responder também:

- temperatura;
- timeout;
- modelo de embeddings;
- provider ativo;
- etc.

Por isso ele se chama **settings**.

---

##### Etapa 2 — Atualizar o `LLMService`

Em vez de:

```
from app.config.settings import OPENAI_API_KEY
```

ficará:

```
from app.config.settings import OPENAI_API_KEY, MODEL_NAME
```

E então:

```
response = self.client.responses.create(
    model=MODEL_NAME,
    input=prompt
)
```

Perceba que já estamos **reduzindo o acoplamento**.

O `LLMService` não conhece mais o nome do modelo.

Ele apenas diz:

> "Use o modelo configurado."

Essa é uma melhoria arquitetural que já estava prevista desde os primeiros laboratórios.

---

##### Ainda não vamos adicionar `tools`

E aqui quero fazer um comentário de professor.

Você pode estar pensando:

> "Já que vamos mexer nesse método, por que não colocamos logo o parâmetro `tools`?"

Porque estamos seguindo uma sequência didática.

Primeiro fazemos um **refactor** simples (tirar o modelo do código).

Depois adicionamos um novo comportamento (Tool Calling).

Misturar as duas mudanças dificultaria entender qual alteração foi responsável por qual efeito.

Essa separação é uma prática muito comum em equipes de software: **um commit para refatoração, outro para nova funcionalidade**.

---

###### Sua missão

Faça essas duas pequenas alterações:

1. Adicione `MODEL_NAME` ao `settings.py`.
2. Atualize o `LLMService` para utilizá-lo.

Depois me envie o código atualizado do `LLMService`.

Só então passaremos para a adição do parâmetro `tools`, que é o verdadeiro foco da Aula 8.

#### Eu:
`Entendo, mas gostaria que ele não tivesse um modelo fixo. Hoje ele está fixo no gpt 4.1 mini, por decisão inicial do professor. Gostaria de opções: O Modelo possui opções. Assim, quando pedir para determinado agente fazer algo, além das ferramentas certas, ele saberá o modelo modelo de LLM para conduzir as etapas`

#### Professor:
Eu iria um passo além da sua ideia.

O professor sugeriu:

```python
MODEL_NAME = "gpt-4.1-mini"
```

Isso resolve o problema do _hardcode_, mas continua assumindo que **existe um modelo padrão para todo o sistema**.

Só que o Prometheus não será um programa simples.

Ele será um **ecossistema de agentes**.

Então eu desenharia a arquitetura pensando nisso.

---

##### Opção 1 — Modelo único (adequado para agora)

```python
MODEL_NAME = "gpt-4.1-mini"
```

É simples.

Serve para um único agente.

---

##### Opção 2 — Catálogo de modelos (o que eu acho que combina com o Prometheus)

Em vez de:

```python
MODEL_NAME = "gpt-4.1-mini"
```

ter algo como:

```python
LLM_MODELS = {
    "fast": "gpt-4.1-mini",
    "reasoning": "o4-mini",
    "deep_reasoning": "o3",
    "creative": "gpt-5",
}
```

Assim o código poderia fazer:

```python
self.llm_service.generate(
    prompt,
    model=LLM_MODELS["reasoning"]
)
```

---

##### Ou ainda melhor...

Eu faria o `LLMService` nem conhecer um modelo padrão.

```python
def generate(
    self,
    prompt: str,
    model: str
) -> str:
```

E quem escolhe o modelo?

Não o serviço.

O agente.

Exemplo:

```python
mentor.ask(...)
```

↓

```python
LLMService.generate(
    prompt,
    model=LLM_MODELS["reasoning"]
)
```

Enquanto outro agente faria:

```python
EditorAgent

↓

LLMService.generate(
    prompt,
    model=LLM_MODELS["creative"]
)
```

---

##### E eu iria ainda mais longe...

Lembra da discussão que tivemos sobre o futuro?

Você comentou que imagina usar:

- ChatGPT
    
- Claude
    
- Gemini
    
- Grok
    
- Perplexity
    

Então, em vez de um catálogo de **modelos**, eu começaria a pensar em um catálogo de **provedores**.

Algo como:

```python
LLM_CONFIGS = {
    "mentor": {
        "provider": "openai",
        "model": "o4-mini"
    },
    "editor": {
        "provider": "anthropic",
        "model": "claude-sonnet-4"
    },
    "research": {
        "provider": "perplexity",
        "model": "sonar-pro"
    }
}
```

Percebe a diferença?

O `LLMService` deixa de ser "o serviço da OpenAI" e passa a ser "o serviço que conversa com um provedor configurado".

Isso conversa diretamente com algo que o seu professor já adiantou no Laboratório 3:

```text
MentorAgent

↓

LLMService

↓

Provider

↓

OpenAI
```

ou

```text
↓

Claude
```

ou

```text
↓

Gemini
```

Na época era apenas um desenho arquitetural. Agora você já está começando a enxergar **por que** essa camada existe.

---

##### Minha recomendação para o Projeto Prometheus

**Para a aula**, eu seguiria o professor e colocaria um `MODEL_NAME` simples. Não vale a pena desviar do roteiro pedagógico.

**Para o Prometheus OS**, eu anotaria essa ideia como uma evolução futura:

```text
Agent
    │
    ▼
LLMService
    │
    ▼
LLMConfig
    │
    ▼
Provider
    │
    ▼
Modelo
```

Assim, cada agente poderá usar o **provedor**, o **modelo** e até os **parâmetros** (temperatura, ferramentas, custo, contexto etc.) mais adequados para sua função.

E eu gosto da sua direção de pensamento. Você não está mais perguntando "qual modelo usar?", mas sim "quem deve ser responsável por escolher o modelo?". Essa é uma pergunta de arquitetura, não apenas de implementação.

Eu montaria pensando no **Prometheus OS**, não apenas no Prometheus-Mentor. Ou seja, uma arquitetura que possa sobreviver por anos.

A ideia principal é separar três conceitos diferentes:

1. **Agente** → escolhe sua estratégia.
    
2. **Configuração** → define qual provedor/modelo usar.
    
3. **LLMService** → apenas executa a chamada.
    

---

##### Estrutura

```text
app/
│
├── config/
│   ├── settings.py
│   ├── llm_catalog.py      ← catálogo de provedores/modelos
│   └── agent_profiles.py   ← qual agente usa qual configuração
│
├── services/
│   ├── llm_service.py
│   └── ...
│
├── providers/
│   ├── openai_provider.py
│   ├── anthropic_provider.py
│   ├── google_provider.py
│   ├── perplexity_provider.py
│   └── xai_provider.py
```

---

##### llm_catalog.py

Aqui cadastramos **todos os modelos conhecidos pelo sistema**.

```python
LLM_CATALOG = {

    "gpt_fast": {
        "provider": "openai",
        "model": "gpt-4.1-mini"
    },

    "gpt_reasoning": {
        "provider": "openai",
        "model": "o4-mini"
    },

    "gpt_deep_reasoning": {
        "provider": "openai",
        "model": "o3"
    },

    "claude_fast": {
        "provider": "anthropic",
        "model": "claude-3.5-haiku"
    },

    "claude_reasoning": {
        "provider": "anthropic",
        "model": "claude-sonnet-4"
    },

    "gemini_fast": {
        "provider": "google",
        "model": "gemini-2.5-flash"
    },

    "gemini_pro": {
        "provider": "google",
        "model": "gemini-2.5-pro"
    },

    "perplexity_search": {
        "provider": "perplexity",
        "model": "sonar-pro"
    },

    "grok_realtime": {
        "provider": "xai",
        "model": "grok"
    }
}
```

Observe que **não existe lógica** aqui.

É apenas um catálogo.

---

##### agent_profiles.py

Agora vem a parte que eu considero mais elegante.

Cada agente possui seu perfil.

```python
AGENT_PROFILES = {

    "mentor": {
        "llm": "gpt_reasoning"
    },

    "editor": {
        "llm": "claude_reasoning"
    },

    "researcher": {
        "llm": "perplexity_search"
    },

    "social_media": {
        "llm": "grok_realtime"
    }
}
```

Perceba a diferença.

O Mentor não conhece OpenAI.

Ele conhece apenas:

```python
mentor
```

↓

```python
gpt_reasoning
```

---

##### LLMService

Agora o serviço fica extremamente limpo.

```text
Agent

↓

LLMService.generate(
    profile="mentor"
)
```

O serviço faz:

```text
mentor

↓

AGENT_PROFILES

↓

gpt_reasoning

↓

LLM_CATALOG

↓

provider=openai

↓

model=o4-mini
```

Tudo automaticamente.

---

##### Amanhã...

Imagine que você decide trocar o Mentor.

De:

```text
GPT
```

para

```text
Claude
```

Você altera apenas:

```python
AGENT_PROFILES
```

De:

```python
"mentor": {
    "llm": "gpt_reasoning"
}
```

Para:

```python
"mentor": {
    "llm": "claude_reasoning"
}
```

Fim.

Nenhuma linha do agente muda.

---

##### Melhor ainda

Cada perfil pode crescer.

```python
"mentor": {

    "llm": "gpt_reasoning",

    "temperature": 0.2,

    "max_tokens": 4000,

    "tools": [
        "calculator",
        "memory"
    ]
}
```

Enquanto:

```python
"editor": {

    "llm": "claude_reasoning",

    "temperature": 0.9,

    "tools": [
        "web",
        "style_checker"
    ]
}
```

---

##### Mais para frente...

Quando chegarmos ao Prometheus OS completo, eu faria isso.

```python
"researcher": {

    "llm": "perplexity_search",

    "fallback": "gpt_reasoning",

    "tools": [
        "web",
        "memory",
        "citation"
    ]
}
```

Se um provedor cair...

↓

usa outro automaticamente.

---

##### E finalmente...

No Prometheus OS eu não teria apenas um catálogo de modelos.

Eu teria uma **Constituição do Agente**.

```text
Agent Profile
        │
        ├── Provider
        ├── Modelo
        ├── Temperatura
        ├── Prompt Base
        ├── Ferramentas
        ├── Memória
        ├── Políticas
        ├── Limites de custo
        ├── Fallbacks
        ├── RAG
        └── Permissões
```

Ou seja, cada agente seria descrito por um único objeto de configuração.

---

###### Minha opinião

Essa é a primeira ideia durante o Projeto Prometheus em que eu diria: **vale a pena guardar**.

Ela resolve um problema que inevitavelmente aparecerá quando você tiver dezenas de agentes, vários provedores e centenas de modelos. Em vez de espalhar decisões pelo código, toda a identidade operacional de um agente fica concentrada em um perfil. A partir daí, trocar de modelo, adicionar um fallback, ajustar temperatura ou conceder novas ferramentas passa a ser uma alteração de configuração, não uma refatoração do sistema.

Mais interessante ainda: essa arquitetura é compatível com o caminho que o professor vem construindo. Ela não contradiz as aulas atuais; apenas estende naturalmente a camada de `Provider` que ele já antecipou e prepara o terreno para o Prometheus OS.


---
# CONTINUANDO A ETAPA 2
## Passo 1 — Atualizar `settings.py`

Vamos apenas remover o hardcode da API e do modelo.

Ele deve ficar conceitualmente assim:

```Python
load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

MODEL_NAME = "gpt-4.1-mini"
```

Nada além disso.

> **Mesmo tendo criado `llm_catalog.py`, ainda não vamos usá-lo.**
> 
> Por quê? Porque primeiro queremos eliminar o hardcode do `LLMService`. Depois, em um segundo momento, podemos trocar `MODEL_NAME` por uma consulta ao catálogo. É uma refatoração por etapas.

---

## Passo 2 — Atualizar `llm_service.py`

Hoje você tem:

```Python
from app.config.settings import OPENAI_API_KEY
```

Troque por:

```python
from app.config.settings import OPENAI_API_KEY, MODEL_NAME
```

Depois altere:

```python
response = self.client.responses.create(
    model="gpt-4.1-mini",
    input=prompt
)
```

para:

```Python
response = self.client.responses.create(
    model=MODEL_NAME,
    input=prompt
)
```

Perceba que ainda **não usamos** `llm_catalog.py` nem `agent_profiles.py`.

---

## Por que não usar agora?

Porque isso seria uma mudança arquitetural maior.

A sequência fica muito mais limpa assim:

1. ✅ Tirar o hardcode (`MODEL_NAME`).
2. ✅ Implementar Tool Calling (objetivo da aula).
3. ✅ Depois evoluir o `LLMService` para consultar `agent_profiles.py` e `llm_catalog.py`.

Assim, cada passo tem um único objetivo.

# ETAPA 3 - ADICIONAR O PARÂMETRO `tools`
Perfeito. Agora começa a parte que diferencia **Tool Decision** de **Tool Calling**.

Até agora, nossa chamada era:

```python
response = self.client.responses.create(
    model=MODEL_NAME,
    input=prompt
)
```

Agora vamos ensinar o LLM que **existem ferramentas disponíveis**.

---

# Etapa 3 — Adicionar o parâmetro `tools`

A chamada ficará assim (por enquanto com apenas uma ferramenta):

```python
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
```

**Ainda não copie. Vamos entender primeiro.**

---

# Linha por linha

## `tools=[...]`

Este parâmetro envia ao modelo uma lista de ferramentas disponíveis.

Até então o GPT pensava:

> "Só posso responder com texto."

Agora ele pensa:

> "Além de responder, posso chamar funções."

---

## `"type": "function"`

Estamos dizendo:

> "Esta ferramenta é uma função que pode ser chamada."

No futuro teremos várias:

```text
calculator
weather
search
memory
database
python
...
```

Todas serão do tipo `"function"`.

---

## `"name": "calculator"`

Este é o identificador.

Se o modelo decidir usá-la, ele devolverá algo como:

```json
{
  "name": "calculator",
  ...
}
```

Perceba que **ele não chama `CalculatorTool`**.

Ele chama pelo nome público da ferramenta.

---

## `"description"`

Talvez a linha mais importante.

O modelo lê essa descrição antes de decidir.

Por exemplo:

```text
Realiza operações matemáticas básicas.
```

É isso que permite ao LLM pensar:

> "A pergunta envolve matemática. Vou usar essa ferramenta."

---

# Uma observação importante

Você provavelmente está pensando:

> "Cadê os parâmetros da função?"

Excelente pergunta.

Ainda faltam.

Uma ferramenta de Tool Calling normalmente precisa informar também:

- quais argumentos recebe;
    
- quais são obrigatórios;
    
- seus tipos (`number`, `string`, etc.).
    

Mas quero seguir exatamente o ritmo da aula.

### Minha pergunta

**O professor, na Aula 8, já pede para definir os parâmetros (`parameters`, `properties`, `required`) da função `calculator`, ou nesta primeira implementação ele apenas apresenta `type`, `name` e `description`?**

Quero acompanhar fielmente o material, sem adiantar conteúdo. 

### ==Resposta do Professor Principal:==
Eis a resposta do professor sobre a aula 8:
"""
Excelente pergunta do monitor. E a resposta é:

> **Não. Nesta primeira implementação da Aula 8, ainda não.**

Ele deve acompanhar exatamente esta progressão:

#### Aula 8 (primeiro contato com Tool Calling)

Apresentamos apenas o conceito de que uma ferramenta possui uma identidade:

```python
{
    "type": "function",
    "name": "calculator",
    "description": "Executa operações matemáticas básicas."
}
```

O objetivo pedagógico desta aula é responder apenas à pergunta:

> **"Como o LLM sabe que uma ferramenta existe?"**

Ainda **não** queremos aprofundar em como o LLM sabe **quais argumentos** enviar.

---

#### Aula seguinte (ou segunda parte de Tool Calling)

Só então introduzimos:

* `parameters`
* `properties`
* `required`
* JSON Schema
* validação de argumentos

A pergunta passa a ser:

> **"Como o LLM chama corretamente a ferramenta?"**

Essa é uma etapa conceitualmente diferente da descoberta da ferramenta.

---

#### Por que separar?

Porque há dois conceitos distintos:

1. **Descoberta da ferramenta**

   * nome
   * descrição
   * tipo

2. **Contrato da ferramenta**

   * parâmetros
   * tipos
   * obrigatórios
   * validação

Misturar ambos na primeira aula aumenta a carga cognitiva sem necessidade. Como combinamos para o Projeto Prometheus, queremos que cada laboratório introduza apenas um conceito novo de cada vez, construindo a arquitetura em camadas.

---

# Próximo passo

Agora vem uma das partes mais interessantes da Aula 8.

Até aqui nós apenas dissemos ao GPT:

> "Existe uma ferramenta chamada `calculator`."

Mas ainda não ensinamos:

> "Como chamá-la."

Esse será exatamente o próximo conceito introduzido pelo professor: transformar a descrição da ferramenta em um **contrato** que o modelo possa seguir. É aí que entram `parameters`, `properties` e `required`, que ele deixou propositalmente para a próxima etapa.