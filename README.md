# Second Brain



Vault pessoal do Obsidian, versionada via Git/GitHub como backup e histórico de evolução.



## Estrutura



- `1. Projetos/` — projetos ativos e em andamento

- `2. Áreas/` — áreas de responsabilidade contínua (hubs de Business, Marketing, IA, Mercado Imobiliário)

- `3. Recursos/` — material de referência, artigos, anotações

- `4. Arquivos/` — imagens, SVGs, e miscelânea 



## Setup técnico

- **Versionamento:** Git + GitHub (repositório privado)
- **Anexos pesados** (.csv, .jpg, .png): versionados via Git LFS
- **Sincronização:** plugin [Obsidian Git](https://github.com/Vinzent03/obsidian-git), com commit e push automáticos a cada \[15/30] minutos



## Workflow de edição

Trabalho no dia a dia é commitado automaticamente pelo plugin Git. Para mudanças maiores ou experimentais, uso branches semanais (`semana-DD-MM`), revisadas com `git diff` antes de mesclar na `main`.



## Notas

- `.obsidian/workspace.json` e cache local são ignorados (não versionados)
- Histórico completo de mudanças disponível via `git log`

## Como usar / manutenção

### Dia a dia
- Edite normalmente pelo Obsidian — o plugin Git cuida do commit e push automático.
- Não é necessário abrir o terminal para uso comum.

### Forçar sincronização manual
Se precisar garantir que tudo foi salvo antes de fechar o PC:
- `Ctrl+P` → "Git: Commit-and-sync"

### Criando uma branch semanal (trabalho experimental/grande)
```powershell
git checkout main
git pull
git checkout -b semana-DD-MM
```

### Revisando o que mudou antes de mesclar
```powershell
git diff main semana-DD-MM
```

### Mesclando a branch semanal (se valeu a pena)
```powershell
git checkout main
git merge semana-DD-MM
git push
git branch -d semana-DD-MM
```

### Descartando a branch semanal (se não valeu)
```powershell
git checkout main
git branch -D semana-DD-MM
```

### Verificando histórico
```powershell
git log --oneline
```

### Se o push falhar por autenticação
Verificar credenciais salvas no Gerenciador de Credenciais do Windows (procurar entradas antigas/erradas do GitHub) antes de gerar um novo token de acesso pessoal.

### Adicionando novo tipo de anexo pesado ao LFS
```powershell
git lfs track "*.extensao"
git add .gitattributes
git commit -m "adiciona nova extensão ao LFS"
```


# 📘 README — Como funciona esta pasta

> Orientação sobre a lógica de **1. Projetos** e sua relação com **2. Áreas** dentro do vault Second Brain (metodologia PARA).

---

## 1. A pilha de conhecimento em "2. Áreas"

As áreas formam uma **pilha de camadas**, onde cada uma se apoiou na anterior — dos fundamentos até o domínio de aplicação.

```
┌─────────────────────────────────────┐
│  4. Mercado Imobiliário             │  ← DOMÍNIO de aplicação (onde tudo converge)
├─────────────────────────────────────┤
│  3. Inteligência Artificial         │  ← MULTIPLICADOR (otimiza e automatiza)
├─────────────────────────────────────┤
│  2. Marketing                       │  ← COMO entregar e comunicar valor
├─────────────────────────────────────┤
│  1. Business                        │  ← O QUE é valor, estratégia, modelos
└─────────────────────────────────────┘
        fundamentos → aplicação
```

| Área                               | Pergunta que responde                                             | Natureza                                                                               |
| ---------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **[[1. Business HUB]]**                | "O que é valor? Como negócios criam e capturam valor?"            | **Fundamentos** — mental models (Porter, VRIO, BMC, Inovação)                          |
| **[[2.1. Marketing HUB]]**               | "Como transformar esse valor em ofertas que os clientes queiram?" | **Aplicação externa** — constrói sobre Business (posicionamos *valor*, não abstrações) |
| **[[3. Inteligência Artificial HUB]]** | "Como fazer mais, mais rápido, com menos esforço?"                | **Multiplicador transversal** — atravessa as duas anteriores                           |
| **[[4. Mercado Imobiliário HUB]]**     | "Onde, concretamente, vou aplicar tudo isso?"                     | **Domínio específico** — o ponto de convergência                                       |


---

## 2. O que é um Projeto (e como esta pasta se preenche)

A diferença entre **Área** e **Projeto** é o *prazo/fim*, não o tema:

> **Área** = competência que você mantém *para sempre*, sem data de fim.
> **Projeto** = esforço com **objetivo específico e data de conclusão**, que *combina* áreas para gerar um entregável.

Em outras palavras, **Projetos são fatias verticais que cortam as Áreas na horizontal**:

```
1.Business   2.Marketing   3.IA    4.Merc.Imob.
    │             │           │          │
    └─────────────┴───┬───────┴──────────┘   ← um PROJETO é esse corte transversal           |
                      │
                entregável concreto + prazo
```

---

## 3. Exemplos concretos (baseados neste vault)

| Projeto | Quais Áreas combina | Entregável |
|---------|--------------------|------------|
| **Incorporação Imobiliária** | Business + Marketing + IA + Mercado Imobiliário | Lançar um empreendimento / criar a operação — *o exemplo perfeito, toca nas 4 áreas* |
| **Projeto Prometheus** | principalmente IA | Concluir a formação em IA (tem fim: quando o curso termina) |
| *(futuro)* "Campanha de pré-lançamento do Empreendimento X" | Marketing + Mercado Imobiliário | Materiais prontos até uma data |
| *(futuro)* "Definir estratégia competitiva do meu negócio" | Business | Documento de estratégia até uma data |

---

## 4. Estrutura sugerida para `1. Projetos/`

```
1. Projetos/
├── Incorporação Imobiliária/
│   ├── Visão e objetivos.md          ← combinação das 4 áreas
│   ├── Estratégia de negócio.md      ← puxa de Business
│   ├── Marketing e vendas.md         ← puxa de Marketing
│   ├── Automações com IA/            ← puxa de IA
│   └── (prompts e materiais relacionados)
│
└── Projeto Prometheus/
    └── (referência da formação em IA)
```

---

## 5. Ciclo de vida no PARA

Quando um projeto **termina**, ele (ou seus artefatos relevantes) migra para `4. Arquivos/` ou vira material de referência em `3. Recursos/`.

```
Projetos (ativos)  →  Arquivos (concluídos)
      ↑
   combinam Áreas para gerar um entregável com prazo
```

---

## Resumo

- **2. Áreas** = conhecimento permanente, em camadas (Business → Marketing → IA → Mercado Imobiliário).
- **1. Projetos** = execução com prazo, que combina Áreas para gerar um entregável concreto.
- Critério para criar um Projeto: **"tem objetivo e prazo, e combina Áreas?"** → se sim, mora aqui.
