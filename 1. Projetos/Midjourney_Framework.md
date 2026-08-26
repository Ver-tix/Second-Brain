# Framework Prático — Consistência de Personagens e Estilo no Midjourney

> Consolidação dos dois guias em um sistema único: mesmos parâmetros, mesmo fluxo-base, duas trilhas de aplicação (Marketing/Narrativa e Games).

---

## 0. Ponto de entrada — qual trilha usar

| Se você precisa de... | Use |
|---|---|
| Retratos, séries editoriais, personagens de campanha/narrativa, cenas variadas | **Trilha A** (§4) |
| Turnaround, expression sheet, pose sheet, equipment breakdown para produção de game | **Trilha B** (§5) |

As duas trilhas compartilham o mesmo motor (§1) e o mesmo fluxo de 4 fases (§2). A diferença está só em quanto de fidelidade (`--cw`) cada etapa exige.

---

## 1. O motor de consistência — parâmetros

| Parâmetro | Função | Faixa | Quando usar |
|---|---|---|---|
| `--cref [url]` | Fixa identidade do personagem (rosto/corpo) | — | Sempre que precisar do mesmo personagem em uma cena nova |
| `--cw` | Peso de fidelidade do `--cref` | 0–100 | Ver tabelas por caso de uso em §4 e §5 |
| `--sref [url]` | Fixa estilo visual (pincelada, paleta, luz) | — | Manter direção estética entre imagens |
| `--sw` | Peso do `--sref` | 0–1000 (padrão ~100) | Suba se o estilo "escorregar" entre gerações |
| `--moodboard [code]` | Biblioteca reutilizável de estilo (10–20 imagens) | — | Séries/projetos longos com identidade visual fixa |
| `--mw` | Peso do moodboard | padrão 100; 150–300 para fidelidade alta | Sempre junto com `--moodboard` |
| `--oref [url]` (v7) | Referência "omni" — personagem, objeto ou criatura | — | Evolução do `--cref` em v7 |
| `--seed [n]` | Trava a semente de geração | inteiro | Reproduzir composição/estilo entre variações |
| `(termo:1.5)` | Peso seletivo de uma palavra no prompt | ~0.1–2 | Enfatizar ou reduzir um elemento específico |
| `--style raw` | Reduz a estilização automática do MJ | — | Essencial em assets técnicos (turnaround, produção) |
| `--s [n]` (stylize) | Intensidade artística geral | 0–1000 | Baixo (0–150) para produção técnica; alto (250+) para concept art |
| `--chaos [n]` | Variação entre gerações | 0–100 | Baixo (0–10) para consistência; alto (20–40) para exploração criativa |

**Regra de ouro (vale para as duas trilhas):** depois de aplicar `--cref`, **não descreva o rosto de novo**. Descreva só o que é novo na cena — ação, luz, ângulo, roupa nova. Redescrever o rosto conflita com a referência e gera inconsistência.

---

## 2. Fluxo universal — 4 fases

**Fase 1 — Personagem-mãe (Hero Shot)**
1. Escreva a descrição textual completa (idade, traços, estilo, expressão)
2. Gere 10–30 variações
3. Eleja a melhor e rode "Vary (Strong)" para refinar
4. Extraia a URL da imagem final → essa é sua referência `--cref`

**Fase 2 — Biblioteca de estilo**
- Estilo pontual → `--sref [url]` de uma imagem de referência
- Projeto longo/série → crie um `--moodboard` com 10–20 imagens: mesma paleta, mesmo meio (não misture foto + 3D), curadoria rígida. Copie o código de 8 caracteres.

**Fase 3 — Produção em série**
- Combine `--cref` + `--sref`/`--moodboard` em cada novo prompt
- Ajuste `--cw` conforme a necessidade de cada peça (tabelas em §4/§5)
- Mantenha as mesmas palavras de estilo (lente, iluminação, grade de cor) em todos os prompts da série

**Fase 4 — Validação**
1. Gere 4 variações por prompt
2. Compare 5–8 imagens lado a lado
3. "Vary (Strong)" nas melhores
4. Se a identidade estiver fraca → suba `--cw`; se estiver rígida demais → desça `--cw`

---

## 4. Trilha A — Marketing, Narrativa e Cenários

### Fórmula de prompt
```
[Sujeito + descrição do personagem] + [Ação/pose] + [Cenário] + [Composição] + [Iluminação] + [Estilo] + [Parâmetros]
```

### `--cw` por objetivo
| `--cw` | Efeito |
|---|---|
| 0 | Só estilo geral preservado, rosto pode mudar |
| 50 | Equilíbrio entre consistência e variação natural |
| **75–85** | **Recomendado para séries** — reconhecível, com variação natural de expressão/ângulo |
| 100 | Fidelidade máxima (rosto, cabelo, corpo, roupa) |

### Template — Hero Shot
```
[idade, tipo de cabelo, cor dos olhos, traços distintivos, roupa, expressão], front-facing portrait, neutral background, soft lighting, cinematic photography --ar 4:5 --style raw
```

### Template — Nova cena (cref + sref combinados)
```
[nova cena/ação/iluminação — não redescrever o rosto] --cref [URL hero shot] --cw 80 --sref [URL estilo] --sw 200 --ar 16:9 --v 7
```

### Template — Character sheet simples (multi-ângulo)
```
front view, side view, back view, three-quarter view, character sheet, clean white background --cref [url] --cw 70 --ar 3:2
```

### Exemplo aplicado (personagem de narrativa)
- **Moodboard:** `cyberpunk-neon-2026`
- **Hero shot:** `young hacker, 25 years old, silver dyed hair, cybernetic eye implant, black leather jacket, neon-lit alley, dramatic rim lighting, cinematic photography --ar 4:5 --v 7`
- **Cena:** `same character walking through futuristic street market, holographic signs, rain, night, low angle shot --cref [URL] --cw 75 --moodboard [CODE] --mw 250 --ar 16:9 --v 7`

---

## 5. Trilha B — Character Sheets para Games

### Os 5 tipos de sheet

| Tipo | Para quê | `--cw` | `--ar` |
|---|---|---|---|
| **Turnaround** (front/side/back/3-quarter, T-pose) | Modelagem 3D | 90–100 | 16:9 ou 2:1 |
| **Expression** (neutral/happy/angry/sad/surprised…) | Rigging facial, animação | 75–85 | 3:2 |
| **Pose/Action** (idle, combate, correndo, pulando) | Animação de movimento | 70–80 | 16:9 |
| **Equipment breakdown** (armas, armadura, variações de cor) | Assets de produção | 95–100 | 3:2 |
| **Full Production** (tudo combinado) | Documento de referência único | 85–90 | 16:9 ou 3:2 |

Sempre com `--style raw` e fundo branco/neutro limpo (facilita extração para modeladores).

### Templates prontos

**Turnaround:**
```
character turnaround model sheet of [DESCRIÇÃO], front view, side view, back view in a row, T-pose, consistent outfit, plain white background, game concept art reference sheet --ar 16:9 --style raw --s 200 --cref [url] --cw 95 --v 7
```

**Expression sheet:**
```
character expression sheet of [DESCRIÇÃO], neutral happy angry sad surprised determined expressions, multiple facial expressions grid, clean white background, game animation reference --ar 3:2 --style raw --s 200 --cref [url] --cw 80 --v 7
```

**Pose sheet:**
```
character pose sheet of [DESCRIÇÃO], idle pose, combat stance, running pose, jumping pose, dynamic action poses, multiple poses on one sheet, game animation reference, clean background --ar 16:9 --style raw --s 200 --cref [url] --cw 75 --v 7
```

**Equipment breakdown:**
```
character equipment breakdown sheet, [DESCRIÇÃO] armor and weapons, detailed close-ups, color variations, accessories and gear, technical illustration style, game production asset, white background --ar 3:2 --style raw --s 250 --cref [url] --cw 100 --v 7
```

**Full production sheet:**
```
official character design sheet of [DESCRIÇÃO], front view side view back view, multiple expressions, pose variations, equipment details, color palette, clean layout, white background, professional game concept art, technical reference document --ar 16:9 --style raw --s 250 --cref [url] --cw 90 --v 7
```

### Técnica avançada — Multi-Reference Blending
Para consistência ultra-precisa: gere 3 ângulos base (front, perfil, 3/4) com mesma luz/fundo/expressão, extraia as 3 URLs e combine:
```
[nova cena/pose] --cref [URL1] [URL2] [URL3] --cw 85 --ar 16:9 --style raw
```
O Midjourney faz a média das 3 referências → personagem mais estável em múltiplos ângulos.

### Pipeline de estúdio (3 fases)
1. **Concept** — 20–30 variações, `--chaos 20–40` para explorar livremente, eleger o hero shot
2. **Production** — turnaround (`cw 95–100`) → expression (`cw 80–85`) → pose (`cw 75–80`), validando consistência entre todas
3. **Export** — turnaround para modeladores 3D, expression para rigging facial, pose para animações base

### Múltiplos personagens na mesma cena
```
two characters standing together, [PERSONAGEM 1] and [PERSONAGEM 2], team pose, clean white background --cref [URL1] [URL2] --cw 85 --ar 16:9 --style raw --v 7
```

---

## 6. Erros comuns → correção

| Erro | Sintoma | Correção |
|---|---|---|
| `--cw` alto demais em pose/expression | Personagem fica rígido, poses não variam | Baixar para 70–85 |
| `--cw` baixo demais em turnaround/equipment | Outfit e proporções mudam entre vistas | Subir para 90–100 |
| Não usar `--style raw` em assets técnicos | Estilo artístico automático interfere na consistência | Sempre `--style raw` em turnaround/produção |
| Fundo complexo | Difícil extrair para modeladores | `clean white background` / `plain background` |
| Redescrever o rosto no prompt | Conflita com `--cref`, gera inconsistência | Descrever só cena/ação/luz — deixar o `--cref` cuidar da identidade |
| Moodboard com mídias/paletas misturadas | Estilo "escorrega" entre imagens | Curadoria rígida: mesma paleta, mesmo meio (foto OU 3D, nunca os dois) |

---

## 7. Checklist final

- [ ] Personagem reconhecível em todas as imagens da série?
- [ ] Estilo (paleta, luz, textura) coerente entre peças?
- [ ] Expressões/poses variam naturalmente sem perder identidade?
- [ ] `--cw` calibrado por tipo de peça (não um valor único para tudo)?
- [ ] Prompts descrevem só o que é **novo** (nunca redescrevem o rosto)?
- [ ] Moodboard com 10–20 imagens coerentes, mesmo meio?
- [ ] (Games) Fundo limpo, `--style raw`, proporções consistentes entre vistas?
- [ ] Resolução suficiente para o uso final (upscale se for para modelagem)?

---

## 8. Referência rápida — pesos por caso de uso (tabela unificada)

| Objetivo | `--cw` |
|---|---|
| Série editorial/marketing (padrão) | 75–85 |
| Turnaround / equipment (games) | 90–100 |
| Expression sheet (games) | 75–85 |
| Pose sheet (games) | 70–80 |
| Full production sheet (games) | 85–90 |
| Multi-reference blending (3 ângulos) | 85 |
| Múltiplos personagens na mesma cena | 85 |
