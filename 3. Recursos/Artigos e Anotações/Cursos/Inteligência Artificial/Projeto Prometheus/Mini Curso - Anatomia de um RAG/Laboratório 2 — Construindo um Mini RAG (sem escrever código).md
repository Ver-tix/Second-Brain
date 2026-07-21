---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
---

# Laboratório 2 — Construindo um Mini RAG (sem escrever código)

> **Objetivo**
> 
> Ao final desta aula, você conseguirá acompanhar mentalmente o caminho percorrido por uma pergunta, desde o momento em que ela é feita até a resposta final.

---

# O cenário

Imagine que você possui um vault extremamente pequeno.

```
Second Brain

├── Branding.md
├── Attention.md
└── Estoicismo.md
```

Dentro de Branding.md existe:

```markdown
# Branding

Branding é o processo de construir uma percepção consistente da marca.
```

Dentro de Attention.md:

```markdown
# Attention

Attention permite que um Transformer descubra quais palavras merecem mais atenção.
```

Dentro de Estoicismo.md:

```markdown
# Estoicismo

Os estóicos acreditavam que devemos controlar apenas aquilo que depende de nós.
```

---

# Primeira etapa

Você liga o sistema pela primeira vez.

O sistema pensa:

> "Ainda não existe índice."

Então inicia a indexação.

```
Branding.md

↓

Chunking
```

Resultado

```
Chunk 1

Branding é o processo...
```

---

Depois:

```
Chunk

↓

Embedding
```

Resultado

```
[0.18, -0.41, 0.72...]
```

---

Depois:

```
Adicionar ao Chroma
```

Agora existe um registro.

```
ID: 1

Texto:
Branding é...

Embedding:
[...]

Metadata:
Arquivo = Branding.md
```

---

Ele faz isso com TODAS as notas.

No final temos algo assim.

```
Branding

↓

●

Attention

↓

                    ●

Estoicismo

↓

                              ●
```

Pronto.

O índice foi criado.

---

# Agora vem a parte interessante

Você faz uma pergunta.

```
"O que é Branding?"
```

Quem recebe?

Não é o GPT.

Quem recebe é a aplicação.

---

Ela pensa.

```
Usuário perguntou algo.
```

↓

```
Preciso consultar conhecimento?
```

↓

```
Sim.
```

↓

```
Executar Retrieval.
```

---

Primeira ação.

Transformar a pergunta em embedding.

```
"O que é Branding?"

↓

Embedding

↓

[0.20, -0.39, ...]
```

---

Agora acontece a comparação.

```
Pergunta

↓

Banco Vetorial

↓

Quem está mais próximo?
```

Resultado.

```
Branding

98%

Attention

32%

Estoicismo

11%
```

---

O banco devolve apenas isto.

```
Texto:

Branding é o processo de construir...

Arquivo:

Branding.md
```

Observe.

Ele devolveu o texto.

Não o vetor.

---

Agora a aplicação monta o prompt.

Algo parecido com isto.

```
Você é um professor.

Explique usando apenas o contexto.

Contexto:

Branding é o processo...

Pergunta:

O que é Branding?
```

---

Agora.

Finalmente.

O GPT entra.

Ele responde.

```
Branding é o processo de construir...
```

---

# Perceba uma coisa importante

O GPT nunca viu isto.

```
Second Brain
```

Nem isto.

```
Markdown
```

Nem isto.

```
Embedding
```

Nem isto.

```
Banco Vetorial
```

Ele viu apenas.

```
Contexto
```

---

# Agora vamos complicar um pouco

Você pergunta.

```
Escreva um artigo relacionando Branding e Estoicismo.
```

O Retrieval faz.

```
Quem fala de Branding?
```

↓

Encontrou Branding.

Depois.

```
Quem fala de Estoicismo?
```

↓

Encontrou Estoicismo.

Agora ele entrega ambos.

```
Contexto

Branding...

+

Estoicismo...
```

<h5>O GPT conecta os dois.</h5>

---

# E se não existir resposta?

Você pergunta.

```
Quem descobriu a Penicilina?
```

O banco procura.

```
Branding

↓

Não.

Attention

↓

Não.

Estoicismo

↓

Não.
```

Resultado.

```
Nenhum chunk encontrado.
```

Agora existem duas possibilidades.

---

## Sistema ruim

Mesmo assim chama o GPT.

O GPT inventa.

---

## Sistema bom

A aplicação pensa.

```
Nenhum contexto.

↓

Responder:

"Não encontrei essa informação."
```

Nem chama o GPT.

---

# Agora chegamos à grande descoberta

Até hoje talvez você imaginasse isto.

```
Pergunta

↓

GPT

↓

Resposta
```

Na verdade acontece isto.

```
Pergunta

↓

Aplicação

↓

Retrieval

↓

Banco Vetorial

↓

Contexto

↓

GPT

↓

Resposta
```

Isso muda completamente a forma de enxergar agentes.

---

# Agora vou mostrar uma arquitetura REAL

Imagine seu Prometheus daqui a um ano.

```
                 Você

                    │

                    ▼

        "Escreva um artigo"

                    │

                    ▼

             Orquestrador

                    │

        "Sobre qual assunto?"

                    │

                    ▼

          Retrieval Marketing

                    │

                    ▼

             Banco Vetorial

                    │

      Retorna 12 chunks relevantes

                    │

                    ▼

           GPT escreve artigo

                    │

                    ▼

              Você revisa
```

Percebe?

O GPT trabalhou apenas nos **últimos 10%** do processo.

---

# Um insight que talvez seja o mais importante do minicurso

Quando você começou o Projeto Prometheus, provavelmente pensava:

> "A IA é o GPT."

Hoje, espero que você pense:

> **"O GPT é apenas o motor linguístico de um sistema muito maior."**

Essa mudança de mentalidade é exatamente o que diferencia quem **usa** IA de quem **projeta sistemas de IA**.

---

# O que faremos no Laboratório 3

Agora que você entende todo o fluxo, faremos algo extremamente próximo da sua realidade.

Não construiremos um RAG genérico.

Vamos responder à pergunta:

> **"Como transformar o seu Second Brain em uma base de conhecimento para agentes de IA?"**

Vamos falar de:

- Como aproveitar os seus headers (`#`, `##`, `###`) no chunking.
    
- O papel das tags e dos links `[[ ]]` do Obsidian.
    
- Como estruturar coleções por domínio (Marketing, IA, Imobiliário, Filosofia etc.).
    
- Como manter o banco vetorial sincronizado com o Obsidian.
    
- E, principalmente, **qual arquitetura eu escolheria para o Projeto Prometheus**, pensando no ecossistema de agentes que você quer construir.
    

Na minha opinião, esse será o laboratório mais valioso de todo o mini-módulo, porque deixaremos de falar de um RAG abstrato e começaremos a desenhar **o seu RAG**.