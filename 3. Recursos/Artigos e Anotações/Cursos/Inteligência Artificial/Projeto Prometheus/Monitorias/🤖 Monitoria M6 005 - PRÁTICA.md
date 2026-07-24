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