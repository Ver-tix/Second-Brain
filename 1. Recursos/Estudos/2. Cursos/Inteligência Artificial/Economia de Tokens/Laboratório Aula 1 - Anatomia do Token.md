---
tags:
  - IA
  - programação
  - inovação
---
## Laboratório Prático: Economia de Tokens na prática

Vamos pegar os dois eixos que a Aula 1 revisada explicou (custo de leitura O(N²) vs. custo de raciocínio/saída memory-bound) e transformar isso em código que você mesmo vai rodar e observar.

O objetivo deste lab é simples: **você vai literalmente ver, em números, o efeito de um contexto bem cortado (tipo RAG) versus jogar o vault inteiro pro modelo.**

---

### Passo 0 — O que vamos construir

Um script Python que:

1. Lê algumas "notas" fake do seu Second Brain (vamos simular um vault pequeno).
2. Conta quantos tokens cada nota tem.
3. Compara o custo de mandar o vault inteiro vs. mandar só as notas relevantes (RAG simplificado, sem IA de verdade — só filtro por palavra-chave, pra você entender o princípio antes de complicar).
4. Calcula o custo monetário estimado (input vs output, com a proporção ~5x que a Aula 1 mencionou).

Nada disso usa API de LLM ainda — é só para você **enxergar os números do Eixo 1 (entrada)** antes de conectar com o Eixo 2 (saída).

---

### Passo 1 — Instalar a biblioteca de contagem de tokens

>A Anthropic e a OpenAI não contam tokens do jeito que a gente conta palavras. Existe uma biblioteca chamada `tiktoken` que faz essa contagem de forma parecida com o que os modelos realmente usam.

Abra seu terminal (dentro da pasta `hello_llm/`, ou em qualquer pasta que você use pra estudar) e rode:

```bash
pip install tiktoken
```

Isso baixa a biblioteca. Depois que rodar sem erro, me avisa que a gente segue pro Passo 2.

---
### Passo 1 (continuação) — Instalando o tiktoken

Você vai usar o terminal. Se você ainda não sabe onde abrir, me diz qual editor/ambiente você está usando (VS Code, terminal do Windows, Mac, etc.) que eu te mostro o caminho exato. Se você já sabe abrir um terminal, siga:

**1.** Abra o terminal na pasta do seu projeto `hello_llm/` (ou qualquer pasta de estudo).

**2.** Digite exatamente isto e aperte Enter:

bash

```bash
pip install tiktoken
```

**3.** Espere o download/instalação terminar. Você vai ver várias linhas de texto passando e, no final, algo como:

```
Successfully installed tiktoken-x.x.x
```

Isso confirma que funcionou.

**Possíveis erros comuns:**

- Se aparecer `pip: command not found` → o Python pode não estar no PATH, ou você precisa usar `pip3` em vez de `pip`.
- Se aparecer algo sobre permissão negada → tente `pip install --user tiktoken`.

Tenta rodar e me conta o que aconteceu (colar a mensagem de erro, se der erro, me ajuda a te guiar certo).