\# Second Brain



Vault pessoal do Obsidian, versionada via Git/GitHub como backup e histórico de evolução.



\## Estrutura



\- `1. Projetos/` — projetos ativos e em andamento

\- `2. Áreas/` — áreas de responsabilidade contínua (hubs de Business, Marketing, IA, Mercado Imobiliário)

\- `3. Recursos/` — material de referência, artigos, anotações

\- `4. Arquivos/` — imagens, SVGs, e miscelânea 



\## Setup técnico



\- \*\*Versionamento:\*\* Git + GitHub (repositório privado)

\- \*\*Anexos pesados\*\* (.csv, .jpg, .png): versionados via Git LFS

\- \*\*Sincronização:\*\* plugin \[Obsidian Git](https://github.com/Vinzent03/obsidian-git), com commit e push automáticos a cada \[15/30] minutos



\## Workflow de edição



Trabalho no dia a dia é commitado automaticamente pelo plugin Git. Para mudanças maiores ou experimentais, uso branches semanais (`semana-DD-MM`), revisadas com `git diff` antes de mesclar na `main`.



\## Notas



\- `.obsidian/workspace.json` e cache local são ignorados (não versionados)

\- Histórico completo de mudanças disponível via `git log`

