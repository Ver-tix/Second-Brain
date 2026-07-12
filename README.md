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