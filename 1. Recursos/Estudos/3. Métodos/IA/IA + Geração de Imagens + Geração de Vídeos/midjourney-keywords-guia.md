---
title: "Midjourney — Guia de Keywords e Lógica de Prompt (v7)"
tags: [midjourney, prompt-engineering, geracao-de-imagem, referencia]
data: 2026-09-19
---

# ## 1. lógica central

Três princípios governam por que certas keywords funcionam e outras não.

### 1.1 Peso posicional
O Midjourney não interpreta sintaxe — ele **pondera tokens**. O que vem primeiro pesa mais.
Sujeito na frente, refinamentos depois, parâmetros no fim.

Mover uma palavra 10 posições para trás reduz materialmente sua influência.

### 1.2 Keyword = âncora de dataset
Cada termo puxa um *cluster* de imagens do treino.
`85mm f/1.4` funciona porque metadados EXIF estavam nas legendas de milhões de retratos reais.
`chiaroscuro` funciona porque é termo catalogado em acervos de arte.

> **Jargão técnico específico vence adjetivo vago.**
> `moody` é difuso e espalhado por tudo.
> `chiaroscuro, single key light, deep shadow falloff` aponta para um cluster estreito e coerente.

### 1.3 Um eixo, uma keyword
Cada categoria é um **eixo independente**.

- Empilhar sinônimos no mesmo eixo (`cinematic, dramatic, epic, moody`) → o modelo faz a média e você perde direção.
- Empilhar eixos diferentes (medium + luz + lente + paleta) → é isso que constrói a imagem.

---

## 2. Hierarquia de impacto

Ordenado por quanto cada eixo realmente muda o pixel.

| # | Eixo | Impacto | Por quê |
|---|------|---------|---------|
| 1 | **Medium / Registro** | Altíssimo | Define o universo inteiro de renderização |
| 2 | **Iluminação** | Altíssimo | Controla forma, volume e emoção simultaneamente |
| 3 | **Lente / Enquadramento** | Alto | Define distância psicológica e distorção |
| 4 | **Paleta / Color grading** | Alto | Carrega o mood sem precisar de adjetivo |
| 5 | **Composição / Ângulo** | Médio-alto | Hierarquia visual e poder do sujeito |
| 6 | **Acabamento / Textura** | Médio | Vende o realismo (grain, halation, bloom) |
| 7 | **Mood (adjetivos)** | **Baixo sozinho** | Só funciona reforçando luz + paleta |

> **Erro nº 1 dos iniciantes:** tentar obter *mood* via adjetivos.
> Mood é **consequência** de luz + paleta + lente.
> `melancholic` sozinho faz pouco.
> `blue hour, desaturated teal palette, 85mm, rain on glass` entrega melancolia de verdade.

---

## 3. Eixo 1 — Medium / Registro

Escolha **um** e não misture registros.

### Fotográfico
`editorial photography` · `documentary photograph` · `studio product photography` ·
`fashion editorial` · `cinematic film still` · `analog photograph` · `35mm film photography`

### Ilustração
`digital illustration` · `gouache painting` · `ink wash` · `watercolor` · `oil painting` ·
`flat vector illustration` · `storybook illustration` · `woodcut engraving` · `risograph print`

### 3D / Concept
`concept art` · `matte painting` · `octane render` · `clay render` · `isometric 3D` ·
`character turnaround sheet`

**Lógica:** o medium é a decisão que trava o `--stylize` correto.

| Medium | Stylize recomendado | Raw? |
|--------|--------------------|------|
| Fotorrealismo / produto | `--s 0–150` | Sim |
| Editorial / cinema | `--s 100–250` | Sim |
| Ilustração / concept | `--s 250–600` | Não |
| Arte experimental | `--s 600–1000` | Não |

> `--s 700` com `studio product photography` faz os dois dials brigarem → resultado sintético.

---

## 4. Eixo 2 — Iluminação (o motor emocional)

### 4.1 Natural / hora do dia

| Keyword | Efeito |
|---------|--------|
| `golden hour` | Quente, direcional, nostálgico |
| `blue hour` | Frio, melancólico, cinematográfico |
| `overcast diffused light` | Neutro, editorial, ideal para produto |
| `dappled light` | Filtrada por folhagem, orgânica |
| `moonlight` | Baixa saturação, misterioso |
| `harsh midday sun` | Sombras duras, estética contemporânea |

### 4.2 Estúdio / retrato

| Keyword | O que faz |
|---------|-----------|
| `Rembrandt lighting` | Triângulo de luz na bochecha, diagonal dramática |
| `loop lighting` | Pequena sombra do nariz — retrato comercial padrão |
| `broad lighting` | Lado mais largo do rosto iluminado, aberto |
| `split lighting` | Metade do rosto na sombra — tensão, ambiguidade |
| `butterfly lighting` | Luz frontal alta — glamour, beleza |
| `rim light` / `backlit` | Contorno luminoso, separa sujeito do fundo |
| `softbox lighting` | Difusa, limpa, e-commerce |

### 4.3 Cinematográfica / atmosférica
`volumetric lighting` (god rays) · `chiaroscuro` (contraste extremo luz-sombra) ·
`low-key lighting` (predominância de sombra) · `high-key lighting` (predominância de luz, publicitário) ·
`practical lights` (fontes visíveis no quadro: luminárias, neon) · `cinematic haze` ·
`god rays through fog`

### 4.4 Qualidade da luz (modificadores)
`soft shadows` ↔ `hard shadows` · `diffused` ↔ `directional` · `dimly lit` ·
`overexposed highlights` · `deep shadow falloff`

### 4.5 Especiais
`neon glow` · `lens flare` · `caustics` (padrões de luz na água) ·
`subsurface scattering` (pele/cera translúcida) · `bioluminescence` ·
`iridescent` · `prismatic refraction`

**Lógica:** a iluminação define **forma** (onde cai a sombra = onde o olho lê volume) e
**tom emocional** ao mesmo tempo. É o eixo com melhor retorno por palavra gasta.

> Combine sempre **fonte + qualidade**:
> `single key light, soft falloff` é mais forte que qualquer dos dois isolado.

---

## 5. Eixo 3 — Photoframing (lente, plano, ângulo)

### 5.1 Plano (distância)

```
extreme close-up → close-up portrait → medium shot → cowboy shot
→ full body shot → wide shot → extreme wide / establishing shot
```

### 5.2 Ângulo (poder e psicologia)

| Keyword | Efeito narrativo |
|---------|------------------|
| `low-angle shot` | Sujeito dominante, heroico, monumental |
| `high-angle shot` | Sujeito vulnerável, diminuído |
| `eye-level shot` | Neutro, empático, documental |
| `dutch angle` | Tensão, desequilíbrio, instabilidade |
| `bird's-eye view` / `top-down flat lay` | Organização, padrão, controle |
| `over-the-shoulder` | Narrativo, relacional |
| `worm's-eye view` | Escala extrema |

### 5.3 Lentes (a keyword mais subestimada)

| Lente | O que o modelo entrega |
|-------|------------------------|
| `14–24mm ultra-wide` | Distorção de perspectiva, ambiente dominante |
| `35mm` | Repórter, contexto + sujeito, naturalismo |
| `50mm` | Perspectiva do olho humano, neutra |
| `85mm` | **Retrato padrão** — compressão lisonjeira do rosto |
| `135mm` / `70–200mm telephoto` | Comprime o fundo, isola o sujeito, look editorial |
| `100mm macro` | Textura extrema, detalhe |
| `tilt-shift` | Efeito miniatura, foco seletivo |

### 5.4 Profundidade de campo
`shallow depth of field` · `f/1.4` / `f/1.8` · `creamy bokeh` · `deep focus` · `focus stacking`

**Lógica:** a lente é o eixo que mais separa "imagem de IA" de "fotografia".
O modelo aprendeu a correlação entre distância focal e geometria facial real.
`85mm f/1.8` não é decoração — reconstrói a compressão ótica correta.

> ⚠️ `bokeh` + `deep focus` no mesmo prompt se anulam.

---

## 6. Eixo 4 — Paleta e color grading

`muted earth tones` · `desaturated palette` · `monochromatic blue` · `warm golden tones` ·
`teal and orange grade` · `pastel palette` · `high contrast black and white` · `sepia toned` ·
`neon magenta and cyan` · `limited palette, two-tone`

**Lógica:** a paleta é o veículo mais eficiente de mood, porque é um eixo que o modelo
controla **globalmente**. Especificar paleta libera você de gastar tokens com adjetivos emocionais.

---

## 7. Eixo 5 — Composição e ambiente

`rule of thirds` · `centered symmetrical composition` · `negative space` · `leading lines` ·
`framed by foreground elements` · `shot through a window` · `silhouette against` ·
`minimal background` · `cluttered maximalist scene` · `volumetric fog` · `atmospheric perspective`

---

## 8. Eixo 6 — Acabamento e textura

`film grain` · `halation` (sangramento de luz em filme) · `chromatic aberration` ·
`motion blur` · `long exposure light trails` · `scanned negative` ·
`anamorphic lens flare` · `slight vignetting`

### Filmes como âncora de dataset

| Filme | Assinatura |
|-------|-----------|
| `Kodak Portra 400` | Tons de pele quentes, baixo contraste |
| `Cinestill 800T` | Halation vermelha, noturno, tungstênio |
| `Fuji Velvia 50` | Saturação alta, paisagem |
| `Ilford HP5` | P&B granulado, documental |

**Lógica:** imperfeição óptica é o sinal mais forte de "isto foi capturado, não gerado".

---

## 9. Eixo 7 — Mood (use como reforço, nunca como base)

`melancholic` · `serene` · `ominous` · `whimsical` · `austere` · `intimate` ·
`epic` · `nostalgic` · `clinical` · `oppressive` · `euphoric`

**Regra:** **um** adjetivo de mood, curto, e só depois de já ter definido luz e paleta.
Se o mood contradiz a luz, **a luz ganha**.

---

## 10. Parâmetros (v7)

| Flag | Faixa | Padrão | Uso |
|------|-------|--------|-----|
| `--ar` | W:H | 1:1 | `1:1` catálogo · `4:5` retrato · `16:9` cinema · `9:16` stories |
| `--stylize` / `--s` | 0–1000 | ~100 | 0–150 fotorrealismo · 200–500 ilustração |
| `--style raw` | on/off | off | Reduz a "embelezação" do MJ — essencial para foto e produto |
| `--chaos` / `--c` | 0–100 | 0 | 0–15 grade consistente · 40+ exploração |
| `--weird` | 0–3000 | 0 | Estranheza estética controlada |
| `--no` | lista | — | `--no text, watermark, extra fingers` |
| `--sref` | URL / código | — | Trava **estilo** (paleta, grade, acabamento) |
| `--sw` | 0–1000 | 100 | Força do style reference |
| `--oref` | URL | — | Trava **identidade** (rosto / objeto) — v7 |
| `--ow` | 0–1000 | 100 | Força do omni reference |
| `--q` | 0.5 / 1 / 2 | 1 | Suba só na arte final |
| `--tile` | — | — | Padrão contínuo (têxtil, textura) |
| `--v 7` | — | conta | Fixe sempre em templates compartilhados |

### 10.1 Sintaxe avançada

| Recurso | Sintaxe | Uso |
|---------|---------|-----|
| Peso multi-prompt | `ancient cathedral::2 fog::1` | Dobra o peso do primeiro conceito |
| Peso negativo | `crowd::-0.5` | Alternativa cirúrgica ao `--no` |
| Permutação | `a {red, blue, green} car` | Gera 3 jobs — ideal para testar um eixo isolado |

### 10.2 Regras de sintaxe (críticas)

1. Espaço antes de cada `--`
2. Sem espaço dentro do traço (`- -ar` falha)
3. **Sem vírgulas** entre parâmetros
4. Nenhuma palavra de cena depois do bloco de parâmetros
5. Tudo em uma linha, parâmetros sempre no fim

---

## 11. Fórmula

```
[MEDIUM] of [SUJEITO + AÇÃO], [AMBIENTE],
[ILUMINAÇÃO: fonte + qualidade], [LENTE + PLANO + ÂNGULO],
[PALETA], [COMPOSIÇÃO], [ACABAMENTO], [1 MOOD]
--ar X:Y --style raw --s N --v 7
```

### Exemplo A — Retrato fotorrealista

```
editorial photograph of a weathered fisherman mending a net,
harbor at dawn, blue hour with single practical lantern as key light,
soft shadow falloff, 85mm f/1.8, medium close-up, eye-level,
desaturated teal and amber palette, negative space to the left,
Kodak Portra 400 grain, quiet dignity
--ar 4:5 --style raw --s 80 --v 7
```

### Exemplo B — Ilustração / concept

```
ink wash and gouache illustration of an armored cherub drawing a longbow,
storm clouds behind, hard directional backlight with rim separation,
low-angle heroic framing, two-tone limited palette of bone white and oxblood,
centered symmetrical composition, visible brush texture, austere
--ar 2:3 --s 450 --v 7
```

### Exemplo C — Produto / e-commerce

```
studio product photography of a matte ceramic carafe on seamless backdrop,
softbox key light with white bounce fill, soft shadows,
100mm macro, eye-level, deep focus,
muted warm neutral palette, centered composition, clinical
--ar 1:1 --style raw --s 50 --v 7
```

---

## 12. Conflitos que arruínam prompts

| Conflito | O que acontece |
|----------|----------------|
| `--s` alto + `--style raw` + fotografia | Os dials brigam → resultado sintético |
| `bokeh` + `deep focus` | Se anulam, foco imprevisível |
| `cinematic` + `studio product shot` | Registros incompatíveis |
| 4 sinônimos de mood juntos | Média difusa, perde direção |
| `--sw` e `--ow` ambos no máximo | Referências competem, imagem quebra |
| Mudar 5 flags após 1 grade ruim | Você perde a causalidade |

---

## 13. Metodologia de iteração

1. **Trave** `--ar` e `--v 7`
2. **Defina** o medium e a banda de `--stylize` correspondente
3. **Gere** e avalie nesta ordem: **sujeito → luz → enquadramento**
4. **Ajuste um único eixo por iteração** (use permutações para isolar variáveis)
5. **Só então** adicione `--sref` / `--oref` para consistência de série

---

## Fontes

- [Midjourney Prompt Guide 2026: Parameters & v7 Syntax — PromptMake](https://promptmake.net/blog/midjourney-prompt-guide-2026)
- [The Complete Midjourney Prompt Guide — Apiframe](https://apiframe.ai/guides/the-complete-midjourney-prompt-guide)
- [50+ Midjourney Lighting Prompts — Aiarty](https://www.aiarty.com/midjourney-prompts/midjourney-lighting-prompts.htm)
- [8 Pro Midjourney Camera Prompts to Master in 2026 — Promptaa](https://promptaa.com/blog/midjourney-camera-prompts)
- [Midjourney v7 Guide: --sref, --cref, Draft Mode & New Parameters — PromptMake](https://promptmake.net/blog/midjourney-v7-guide)
