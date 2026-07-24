---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---

# Visão Técnica
O custo é por token processado pelo transformer, mas input e output têm **perfis computacionais diferentes**:

## Input (prefill)
- Processado em paralelo — todos os tokens de uma vez, uma única passada forward
- Custo computacional ~O(n²) em atenção (cada token atende a todos os outros), mas é _paralelizável_, então o custo de latência não escala tão mal quanto o de output
- É por isso que input é mais barato: a GPU processa tudo de uma vez, throughput alto
---
## Output (decode)
- **Autorregressivo** — um token por vez, cada novo token exige uma passada forward completa
- **Não paralelizável** entre tokens (token N+1 depende do token N)
- Cada token gerado recalcula atenção sobre todo o contexto acumulado (mitigado pelo **KV cache**, que guarda as projeções K/V dos tokens anteriores pra não recomputar)
- Mesmo com KV cache, o _decode_ é memory-bound (bandwidth-limited), não compute-bound — por isso é mais lento e mais caro por token
---
## **Prompt caching ≠ KV cache, mas usa o mesmo princípio**

- O prompt caching da Anthropic persiste as ativações (KV) de um prefixo fixo do prompt entre requisições
- Se seu system prompt + few-shot examples do Prometheus-Editor não mudam, a API reaproveita o estado computado — você paga só pela leitura do cache (fração do preço), não pelo recompute do prefill inteiro
- Implicação de design: **estruture o prompt com o conteúdo fixo primeiro, o variável por último** — cache hit precisa de prefixo idêntico byte-a-byte

## **Consequência prática pra você**: 
Se seu ecossistema de agentes gera 12 agentes com um system prompt compartilhado (ex: identidade da marca, tom de voz), isso deveria ser um bloco cacheado único, reutilizado por todos os agentes — não reescrito por instância.

## O Dois Eixos
Existem **dois eixos de custo** independentes, e sua conversa com a outra LLM captou isso bem, mas a conclusão final ("é melhor gastar na entrada") precisa de um ajuste fino.

| Eixo 1 — Custo de leitura (prefill/input)                                                                    | Eixo 2 — Custo de raciocínio (decode/output)                                                                                                             |
| ------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Self-attention sobre um contexto de tamanho N custa O(N²) em FLOPs (cada token atende a todos os outros)     | Autorregressivo: token N+1 depende do token N, uma passada forward por token                                                                             |
| Mas é **paralelizável**: a GPU processa a sequência inteira numa única passada forward, matriz contra matriz | Cada passo ainda faz atenção sobre todo o contexto acumulado (mitigado por KV cache — reaproveita as projeções K/V já calculadas, não recomputa do zero) |
| Alto throughput, mesmo sendo O(N²) algoritmicamente — é _compute-bound_, e hardware moderno é bom nisso      | Mesmo com KV cache, decode é **memory-bound** (bandwidth-limited, não compute-bound), e sequencial — não dá pra paralelizar entre tokens                 |
### Um Erro Comum
Há quem pense:

> Se o gasto é menor na entrada O(N²), já que a saída é O(N), não seria melhor gastar na entrada?

Mas **complexidade algorítmica ≠ custo monetário por token**. Os dois eixos são otimizados por hardware de formas diferentes:
- Input é O(N²) mas paralelo → barato por token (throughput alto)
- Output é "linear" em passos mas sequencial e memory-bound → caro por token, e por isso a Anthropic cobra ~5x mais no output que no input
<aside>
<h4 align="center">Ou seja: não é "gaste mais na entrada porque é mais barato algoritmicamente" — é "minimize os dois, mas por razões diferentes":</h4>
<ol><li>Minimize input porque cresce quadraticamente em FLOPs (mesmo sendo paralelo, em contextos muito longos o custo real dispara)</li><li>Minimize output porque cada token gerado é caro e lento por natureza sequencial/memory-bound</li></ol>
</aside>

- 
- 
# Agora, Ensinando a Leigos