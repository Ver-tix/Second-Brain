---
kanban-plugin: board
tags:
  - programação
  - IA
---

## Etapa 1 — Instalar o Git

- [x] 1. Baixe o Instalador em: https://git-scm.com/download/win
- [x] Rode o instalador. Nas opções, os padrões servem, mas fique atento a duas telas:
	
	- **"Adjusting your PATH environment"** → escolha _"Git from the command line and also from 3rd-party software"_
	- **"Choosing the default editor"** → escolha o editor que preferir (VS Code, Notepad++, ou deixe o padrão Vim se não tiver preferência
- [x] 1. Confirme a instalação abrindo o **PowerShell** e rodando:
	
	powershell
	
	```powershell
	   git --version
	```
	
	Deve aparecer algo como `git version 2.4x.x`.
- [x] 4. Configure sua identidade (usada em todo commit):
	
	powershell
	
	```powershell
	   git config --global user.name "Seu Nome"
	   git config --global user.email "seu-email@exemplo.com"
	```


## Etapa 2 — Criar conta e autenticação no GitHub

- [x] 1. Se já não tiver, crie a conta em [https://github.com](https://github.com)
- [x] 2. Para autenticar sem ficar digitando senha toda hora, gere uma chave SSH:
	
	powershell
	
	```powershell
	   ssh-keygen -t ed25519 -C "seu-email@exemplo.com"
	```
	
	Aperte Enter em todas as perguntas (padrões são suficientes).
- [x] 3. Copie a chave pública gerada:
	
	powershell
	
	```powershell
	   Get-Content ~\.ssh\id_ed25519.pub | Set-Clipboard
	```
	
	(isso já copia pro clipboard)
- [x] 4. No GitHub: **Settings → SSH and GPG keys → New SSH key** → cole a chave → salve.
- [x] 5. Teste a conexão:
	
	powershell
	
	```powershell
	   ssh -T git@github.com
	```
	
	Deve responder algo como "Hi seu-usuario! You've successfully authenticated".


## Etapa 3 - Criar o Repositório Privado

- [x] 1. No GitHub, clique em **New repository**
- [x] 2. Nome sugerido: `second-brain` ou `obsidian-vault`
- [x] 3. Marque **Private**
- [x] 4. **Não** marque "Add a README" (sua vault já tem conteúdo — evita conflito no primeiro push)
- [x] 5. Copie a URL SSH do repo (formato `git@github.com:seu-usuario/second-brain.git`)


## Etapa 4 — Preparar a vault antes do primeiro commit

- [x] 1. Na pasta raiz da sua vault, crie um arquivo `.gitignore`:
	
	powershell
	
	```powershell
	cd "C:\caminho\para\sua\vault"
	New-Item .gitignore
	```
- [x] 2. Conteúdo do `.gitignore`:
	
	```
	# Ruído de configuração local do Obsidian
	.obsidian/workspace.json
	.obsidian/workspace-mobile.json
	.obsidian/cache
	
	# Lixeira interna do Obsidian
	.trash/
	
	# Arquivos de sistema
	Thumbs.db
	desktop.ini
	```
- [x] 3. **Se você tiver anexos pesados (PDFs, imagens grandes, vídeos):** configure o Git LFS antes de commitar qualquer coisa.
	
	powershell
	
	```powershell
	git lfs install
	git lfs track "*.pdf" "*.png" "*.jpg" "*.jpeg" "*.mp4"
	```
	
	Isso cria um `.gitattributes` — adicione ele ao commit junto com o `.gitignore`.


## Etapa 5 - Primeiro Commit e Push

- [x] powershell
	
	```powershell
	git init
	git add .gitignore
	git add .gitattributes    # só se estiver usando LFS
	git add .
	git commit -m "commit inicial: migração da vault para o GitHub"
	git branch -M main
	git remote add origin git@github.com:seu-usuario/second-brain.git
	git push -u origin main
	```
	
	Se a vault for grande, o push pode demorar — normal na primeira vez.


## Etapa 6 — Automatizar os próximos commits

- [x] 1. Fazer commit manual toda vez que editar uma nota não é sustentável. Instale o plugin **Obsidian Git**:
	
		1. Dentro do Obsidian: **Settings → Community plugins → Browse**
		2. Busque por "Obsidian Git" → Install → Enable
		3. Nas configurações do plugin, ative:
	    - **Vault backup interval**: ex. `10` (minutos) → faz commit + push automático
	    - **Commit message**: pode deixar o padrão (`vault backup: {{date}}`) ou personalizar
	    - **Auto pull on boot**: útil se um dia você usar em outro device
	
	A partir daqui, seu fluxo é: você escreve → o plugin versiona sozinho.


## Etapa 7 - Documentação do repositório

- [x] Crie um `README.md` na raiz da vault explicando o propósito do repo (útil pro seu "eu do futuro" e caso um dia você queira dar acesso a alguém):
	
	markdown
	
	```markdown
	# Second Brain — Vault Obsidian
	
	Repositório privado usado para versionar e fazer backup da minha
	base de conhecimento pessoal (Obsidian).
	
	## Estrutura
	- `/[sua-pasta-principal]` — notas organizadas por [seu método: PARA, Zettelkasten, etc.]
	- `.obsidian/` — configurações do vault (parcialmente ignoradas via .gitignore)
	
	## Sincronização
	Commits automáticos via plugin **Obsidian Git**, a cada 10 min.
	
	## Backup
	Este repositório é o backup principal na nuvem. [Adicione aqui se
	houver backup secundário, ex: HD externo, outro serviço de nuvem]
	
	## Histórico
	Iniciado em [data]. Ver commits para histórico completo de evolução
	das notas.
	```
- [ ] 2. Adicione e suba:
	
	powershell
	
	```powershell
	git add README.md
	git commit -m "docs: adiciona README explicando o repositório"
	git push
	```


## Pós-Projeto: workflow de branches

- [x] [[LEIA. IMPORTANTE PARA EVITAR DANOS AO PROJETO]]




%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false,false,false,false,false,false],"show-checkboxes":true}
```
%%