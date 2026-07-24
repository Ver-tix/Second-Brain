---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---

# ETAPA 1
## Etapa 1.1

Dentro de:

```text
app/memory/
```

crie o arquivo:

```text
conversation_memory.py
```

Sem código ainda.

✅ Quando criar, me avise.

---

## Etapa 1.2 — Vamos pensar antes de programar

O professor pediu uma classe:

```python
class ConversationMemory:
```

Antes de escrever uma linha, quero que você pense como arquiteto.

Imagine a conversa:

```text
Usuário:
Meu nome é Caio.

Assistente:
Prazer!

Usuário:
Explique RAG.

Assistente:
...
```

A memória precisa guardar isso.

**Pergunta:**

Na sua opinião, o que seria mais simples para armazenar esse histórico?

Algo como:

```python
[
    "Meu nome é Caio.",
    "Prazer!",
    "Explique RAG.",
    "..."
]
```

ou

```python
[
    "Usuário: Meu nome é Caio.",
    "Assistente: Prazer!",
    "Usuário: Explique RAG.",
    "Assistente: ..."
]
```

Qual das duas você escolheria e por quê?

> 💡 Não existe uma "pegadinha" aqui. Quero entender seu raciocínio antes de implementarmos a classe. Isso vai nos ajudar a manter a arquitetura coerente com o restante do projeto.

# Resposta à Pergunta
eu: `A segunda. Lembrei do KV Cache - Key Value Cache.`

Gostei muito da sua resposta. E, curiosamente, ela mostra que você já está começando a conectar módulos diferentes do curso.

Mas vou fazer uma pequena correção conceitual.

### A escolha

Eu também escolheria a segunda:

```
[
    "Usuário: Meu nome é Caio.",
    "Assistente: Prazer!",
    "Usuário: Explique RAG.",
    "Assistente: ..."
]
```

Porque ela já deixa explícito **quem disse o quê**. Depois, basta juntar tudo em uma string para enviar ao PromptBuilder.

---

### Sobre o KV Cache

A associação foi boa, mas **não é exatamente o mesmo conceito**.

O KV Cache guarda **representações internas (embeddings/vetores de atenção)** calculadas pelo Transformer para acelerar a inferência.

Aqui, estamos guardando **mensagens da conversa**, ou seja, dados da aplicação.

É uma memória de software, não uma memória do modelo.

---
# ETAPA 2
No arquivo:

`app/memory/conversation_memory.py`

Comece apenas com a estrutura:

```Python 
class ConversationMemory:

    def __init__(self):
        self.history = []
```

### O que isso faz?

Quando criarmos:

```python
memory = ConversationMemory()
```

o Python executará automaticamente:

```python
self.history = []
```

ou seja, cada instância terá sua própria lista de histórico.
### Minha pergunta (bem rápida)

Por que usamos:

```
self.history = []
```

e **não**:

```
history = []
```

Quero ver se você já pegou a ideia de atributos de instância. 😊
# Resposta à Segunda Pergunta
eu: `porque, diferentemente de dos métodos estáticos, classes "normais" precisam de instâncias`

Você está **no caminho certo**, mas a resposta ainda não é exatamente essa. 😄

O fato de a classe precisar de instâncias explica **por que existe `self`**, mas não explica por que usamos `self.history`.

A ideia principal é:

- `history = []` cria apenas uma **variável local** dentro do `__init__()`. Quando o construtor termina, ela desaparece.
    
- `self.history = []` cria um **atributo do objeto**, que continua existindo enquanto a instância existir.
    

Por exemplo:

```python
memory = ConversationMemory()
```

Após isso, o objeto `memory` fica assim, conceitualmente:

```text
memory
│
└── history
      └── []
```

Mais tarde, quando chamarmos outro método da mesma instância:

```python
memory.add_user_message("Olá!")
```

esse método conseguirá acessar a mesma lista:

```python
self.history
```

porque ela pertence ao objeto.

---

🎯 Uma forma de decorar:

- **Variável local** → vive apenas durante a execução da função.
    
- **`self.alguma_coisa`** → pertence ao objeto e pode ser usada por **todos os métodos da classe**.
    

Esse é um dos conceitos mais importantes da Programação Orientada a Objetos. Quando ele "clicar", muita coisa vai começar a fazer sentido. Agora vamos construir os métodos que vão manipular essa `history`. 🚀
# ETAPA 3

Agora vamos implementar os métodos da `ConversationMemory`. O laboratório pede que ela seja capaz de:
- guardar mensagens;
- devolver todo o histórico para ser enviado ao `PromptBuilder`.

## Etapa 3.1 - Primeiro método: Mensagem do Usuário

```python
def add_user_message(self, message: str):
    self.history.append(f"Usuário: {message}")
```

### O que acontece aqui?

Se o usuário digitar:

```
O que é um Transformer?
```

A lista passa de:

```Python
[]
```

para:

```Python
[
    "Usuário: O que é um Transformer?"
]
```

## Etapa 3.2 - Segundo método: Mensagem do Assistente
Agora o segundo método:

```python
def add_assistant_message(self, message: str):
    self.history.append(f"Assistente: {message}")
```

Depois da resposta do modelo, teremos:

```python
[
    "Usuário: O que é um Transformer?",
    "Assistente: Um Transformer é..."
]
```

## Minha pergunta

Por que criamos **dois métodos**:

```
add_user_message()
```

e

```
add_assistant_message()
```

em vez de apenas um:

```
add_message()
```

Não existe resposta única, mas quero ver seu raciocínio arquitetural antes de continuarmos. 😊
# Resposta à Pergunta 3: 
eu: `Porque faz-se necessário saber não só o que o usuário pediu, mas como a IA respondeu também. Isso é importante para sabermos o feedback do usuário depois disso: se foi útil ou não a resposta`

Sua resposta foi boa, mas ela respondeu uma pergunta **ligeiramente diferente** da que eu fiz. 😄

Você explicou **por que precisamos guardar as mensagens do usuário e do assistente**, e concordo 100%.

Mas eu queria saber:

> **Por que criamos dois métodos diferentes, em vez de um só?**

Por exemplo, poderíamos fazer:

```
def add_message(self, role: str, message: str):
    self.history.append(f"{role}: {message}")
```

Isso também funcionaria.

Então, por que o professor preferiu:

```
add_user_message()
```

e

```
add_assistant_message()
```

A minha resposta seria:

- porque a API da classe fica mais clara;
- evita erros (ninguém esquece de escrever `"Usuário"` ou `"Assistente"` corretamente);
- quem usa a classe não precisa saber como a mensagem é armazenada internamente.

Em outras palavras, o código fica mais expressivo:

```
memory.add_user_message(question)
```

é muito mais legível do que:

```
memory.add_message("Usuário", question)
```

Você quase não percebe, mas isso é um princípio muito valorizado em engenharia de software:

> **Faça a interface da classe expressar a intenção, e esconda os detalhes da implementação.**


# ETAPA 4
## Agora vamos para o último método da classe

Adicione:

```python
def get_history(self) -> str:
    return "\n".join(self.history)
```

### O que ele faz?

Suponha que `self.history` seja:

```python
[
    "Usuário: O que é um Transformer?",
    "Assistente: Um Transformer é...",
    "Usuário: Explique RAG."
]
```

Quando chamarmos:

```python
memory.get_history()
```

o retorno será uma única string:

```
Usuário: O que é um Transformer?
Assistente: Um Transformer é...
Usuário: Explique RAG.
```

Percebe por que isso é útil?

O `PromptBuilder` não quer uma lista de Python. Ele quer um **texto** para incorporar ao prompt. Assim, a `ConversationMemory` entrega o histórico exatamente no formato que os próximos componentes da arquitetura precisam consumir.

Quando você terminar esse método, seguimos para a parte que eu considero a mais interessante da aula: **ligar a memória ao `PromptBuilder`**, para que o Prometheus finalmente comece a "lembrar" da conversa. 🚀

# ETAPA 5 - LIGANDO A MEMÓRIA AO `PromptBuilder`

## Etapa 5.1 - Alterando o `PromptBuilder`
`app/prompts/mentor_prompt.py`:
Hoje, seu método deve estar parecido com:

```python
@staticmethod
def build(question: str) -> str:
```

Vamos mudar a assinatura para:

```python
@staticmethod
def build(history: str, question: str) -> str:
```

## Minha pergunta
Por que adicionamos o parâmetro `history`?

Porque agora o PromptBuilder precisa de **duas informações** para montar o prompt:
- o histórico da conversa;
- a nova pergunta.

Ele continua sendo apenas um "montador de prompts", mas agora tem mais matéria-prima para trabalhar.

## Etapa 5.2 - Alterando o `prompt`

Onde antes havia apenas:

```python
Pergunta do usuário:

{question}
```

vamos incluir o histórico antes da pergunta:

```python 
prompt = f"""
Você é o Prometheus-Mentor.

Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara, didática e organizada.

Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da definição técnica.

Histórico da conversa:

{history}

Nova pergunta do usuário:

{question}
```

### O que isso muda?

Antes, o modelo recebia algo como:

```
O que é RAG?
```

Agora ele receberá:

```
Histórico:

Usuário: O que é um Transformer?
Assistente: ...

Usuário: Explique atenção.
Assistente: ...

Nova pergunta:

Como isso se relaciona ao RAG?
```

Agora o modelo consegue entender que **"isso"** se refere ao assunto da conversa.

---

🎓 **Observe um detalhe arquitetural importante:**

O `PromptBuilder` **não sabe de onde veio o histórico**.

Ele apenas recebe uma `str`.

Amanhã, esse histórico poderá vir:
- de uma lista (como hoje);
- de um banco de dados;
- de um banco vetorial;
- do seu Second Brain.

E o `PromptBuilder` continuará funcionando exatamente igual.

Essa é uma demonstração muito bonita do princípio da **inversão de dependência**: o componente não depende da implementação da memória, apenas da informação que ela fornece.

Quando terminar essa alteração, nós iremos modificar o `MentorAgent`. E aí você vai ver como todos os componentes começam a conversar entre si de forma muito elegante. É a minha parte favorita do laboratório. 🚀

# ETAPA 6
Agora chegamos ao coração do laboratório.

Até agora, o `MentorAgent` fazia algo parecido com isto:

```
Pergunta
    ↓
PromptBuilder
    ↓
LLMService
```

Agora ele será o maestro da orquestra.

```
Pergunta
    ↓
ConversationMemory
    ↓
PromptBuilder
    ↓
LLMService
    ↓
ConversationMemory
```

Percebe? Ele passa a coordenar todo o fluxo.

