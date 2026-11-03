---
tags:
  - IA
  - programação
  - inovação
---
> **Objetivo da aula**
> 
> Ao final desta aula você entenderá:
> 
> - por que praticamente toda API utiliza JSON;
>     
> - como JSON representa informações;
>     
> - por que Python e LLMs trabalham tão bem com esse formato;
>     
> - e por que você verá JSON durante toda a sua carreira.
>     

---

# O problema

Na aula passada aprendemos que aplicações conversam usando HTTP.

Mas surgiu outro problema.

Imagine que seu programa envia isto para a OpenAI:

```text
Explique Machine Learning.
```

Tudo certo.

Mas agora imagine uma pergunta mais complexa.

Você quer enviar:

- a pergunta;
    
- qual modelo usar;
    
- a temperatura;
    
- o idioma;
    
- o usuário;
    
- o histórico da conversa.
    

Como enviar tudo isso?

Uma frase já não basta.

Precisamos de uma forma organizada de representar dados.

---

# A ideia

Imagine que duas pessoas falam idiomas diferentes.

Uma fala português.

Outra japonês.

Como elas podem trocar informações?

Criando um padrão.

Foi exatamente isso que aconteceu na computação.

Criou-se um formato simples para representar dados.

Esse formato chama-se:

# JSON

**JavaScript Object Notation**

Apesar do nome...

Hoje JSON é utilizado praticamente por todas as linguagens.

Inclusive Python.

---

# Uma analogia

Imagine uma ficha de cadastro.

Ela possui campos.

```text
Nome:

Idade:

Cidade:

Telefone:
```

JSON é exatamente isso.

Só muda a forma de escrever.

---

# O primeiro JSON

Imagine uma pessoa.

Em português diríamos:

```text
Nome: Caio

Idade: 25

Cidade: Fortaleza
```

Em JSON:

```json
{
    "nome": "Caio",
    "idade": 25,
    "cidade": "Fortaleza"
}
```

Percebe?

É apenas uma forma organizada de representar informações.

---

# Chaves e valores

Todo JSON é construído sobre uma ideia muito simples.

```text
chave

↓

valor
```

Por exemplo.

```json
{
    "modelo": "gpt-4.1-mini"
}
```

A chave é:

```text
modelo
```

O valor é:

```text
gpt-4.1-mini
```

---

# Vários campos

Agora imagine uma requisição para um LLM.

```json
{
    "model": "gpt-4.1-mini",
    "input": "Explique RAG.",
    "temperature": 0.2
}
```

Muito mais organizado.

---

# Uma analogia com um formulário

Imagine preencher isto.

```text
Modelo:

Pergunta:

Temperatura:
```

JSON é apenas esse formulário em formato digital.

---

# O que realmente acontece?

Lembra desta linha?

```python
client.responses.create(
    model="gpt-4.1-mini",
    input=question
)
```

Você escreveu Python.

Mas...

O SDK converte isso para algo muito parecido com:

```json
{
    "model": "gpt-4.1-mini",
    "input": "Explique Transformers."
}
```

Depois envia esse JSON usando HTTP.

---

# O servidor responde

A OpenAI também responde usando JSON.

Algo parecido com:

```json
{
    "id": "resp_123",
    "output_text": "Transformers são..."
}
```

O SDK recebe isso.

Transforma novamente em objetos Python.

E você escreve apenas:

```python
response.output_text
```

Percebe?

O SDK está traduzindo JSON para Python.

---

# Uma observação importante

Muita gente pensa:

> "Python conversa com Python."

Na verdade não.

O servidor da OpenAI pode estar escrito em:

- Python;
    
- Go;
    
- Rust;
    
- Java.
    

Seu programa nunca saberá.

Porque ambos conversam usando JSON.

JSON é uma linguagem neutra.

---

# Uma analogia

Imagine dois países.

Um fala português.

Outro alemão.

Ambos resolvem usar inglês para negociar.

JSON é esse inglês.

Não pertence a nenhuma linguagem específica.

---

# Por que JSON venceu?

Poderíamos usar XML.

Ou YAML.

Ou CSV.

Então por que JSON se tornou o padrão?

Porque ele é:

- pequeno;
    
- fácil de ler;
    
- fácil de gerar;
    
- fácil de interpretar;
    
- funciona praticamente em qualquer linguagem.
    

---

# Onde você verá JSON?

Praticamente em todo lugar.

Quando usar:

- OpenAI
    
- Anthropic
    
- Google AI
    
- Stripe
    
- GitHub
    
- Notion
    
- Slack
    
- Discord
    

Todos enviarão e receberão JSON.

---

# Conectando com RAG

Imagine que seu RAG encontrou isto.

```text
Documento A

Documento B

Documento C
```

O orquestrador provavelmente enviará algo parecido com:

```json
{
    "question": "...",
    "documents": [
        "...",
        "...",
        "..."
    ]
}
```

O LLM nem sabe que aquilo veio de um banco vetorial.

Para ele...

É apenas JSON.

---

# Conectando com Agentes

Imagine um agente.

Ele chama uma ferramenta.

A ferramenta responde:

```json
{
    "temperatura": 31,
    "cidade": "Fortaleza"
}
```

Depois chama outra.

```json
{
    "agenda": [
        "...",
        "...",
        "..."
    ]
}
```

Tudo continua sendo JSON.

---

# Uma observação muito importante

Quero plantar uma semente.

Você estudou XML no Módulo 3.

Naquela época usamos XML para organizar prompts.

Agora aparece JSON.

Qual a diferença?

---

## XML organiza documentos.

Ele é excelente para representar estruturas hierárquicas complexas.

Por exemplo.

```xml
<context>

<role>

<constraints>

<examples>
```

Muito bom para prompts.

---

## JSON organiza dados.

Por exemplo.

```json
{
    "nome": "...",
    "idade": 25,
    "curso": "Marketing"
}
```

Muito melhor para aplicações.

---

Nenhum é melhor.

Cada um resolve um problema diferente.

---

# Voltando ao HelloLLM

Agora podemos desenhar seu projeto quase completamente.

```text
main.py

↓

OpenAIProvider

↓

SDK

↓

JSON

↓

HTTP

↓

API

↓

Servidor

↓

LLM

↓

JSON

↓

SDK

↓

Python

↓

Resposta
```

Perceba que JSON aparece duas vezes.

Na ida.

E na volta.

---

# Uma curiosidade

Quando você abre o DevTools do navegador (F12) e olha a aba **Network**, verá dezenas de requisições.

Ao clicar nelas...

Na maioria das vezes aparecerá exatamente isto.

JSON.

Você começará a enxergá-lo em todos os lugares.

---

# O grande insight

Até agora estudamos várias tecnologias diferentes.

Mas perceba um padrão.

Cada uma resolve um problema específico.

|Problema|Solução|
|---|---|
|Como aplicações conversam?|HTTP|
|Como acessam serviços?|API|
|Como organizam serviços?|Endpoints|
|Como provam identidade?|API Key|
|Como transportam dados?|JSON|

Em mais detalhes:

|Camada|Pergunta que responde|Responsabilidade|Analogia|
|---|---|---|---|
|**HTTP**|**Como** cliente e servidor conversam?|Define o **protocolo de comunicação**: como enviar requisições, receber respostas, indicar erros, etc.|As regras dos Correios (como enviar uma encomenda).|
|**JSON**|**Como** os dados são representados?|Define o **formato dos dados** transportados na comunicação.|O conteúdo organizado dentro da caixa enviada pelos Correios.|
|**API**|**Quais serviços** estão disponíveis?|Expõe funcionalidades de um sistema de forma organizada e padronizada.|A recepção de um hotel ou o garçom de um restaurante, que recebe seu pedido e o encaminha ao setor correto.|
|**Endpoint**|**Qual serviço específico** será utilizado?|Representa uma funcionalidade específica dentro da API.|Uma sala específica dentro de um prédio (Financeiro, RH, Engenharia).|
|**SDK**|**Como facilitar** o uso da API?|Abstrai a complexidade da API e do HTTP, oferecendo funções prontas na linguagem de programação.|O painel de um carro: você dirige usando volante e pedais, sem controlar diretamente o motor.|
|**API Key**|**Quem está usando** a API?|Autentica a aplicação perante o servidor e permite controlar permissões e cobrança.|A carteirinha de um clube ou um crachá de identificação.|
|**Servidor**|**Quem executa** o serviço?|Processa a requisição, aplica as regras de negócio e gera a resposta.|A cozinha de um restaurante.|
|**LLM**|**Quem gera** a resposta inteligente?|Interpreta o contexto recebido e produz texto, código, imagens etc.|O chef especializado que prepara o prato.|

Isso é Engenharia de Software.

Não decorar ferramentas.

Mas entender **qual problema cada tecnologia resolveu**.

---

# A ideia mais importante da aula

Se eu tivesse que resumir JSON em uma frase, seria esta:

> **JSON não existe para humanos nem para Python; ele existe para que sistemas diferentes consigam trocar informações de maneira padronizada.**

É por isso que ele está presente em praticamente toda aplicação moderna.

---

### Um resumo em uma frase (para fixação)

- **HTTP** → _Como conversamos?_
- **JSON** → _Como organizamos os dados dessa conversa?_
- **API** → _Quais serviços você pode solicitar?_
- **Endpoint** → _Qual serviço específico você quer usar?_
- **SDK** → _Como tornar tudo isso fácil para o programador?_
- **API Key** → _Quem é você e pode usar esse serviço?_
- **Servidor** → _Quem executa o trabalho?_
- **LLM** → _Quem produz a resposta inteligente?_

> **Observe um padrão:** cada camada responde a uma única pergunta fundamental da arquitetura. É exatamente essa separação de responsabilidades que torna sistemas complexos compreensíveis e escaláveis.