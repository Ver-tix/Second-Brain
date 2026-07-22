---
tags:
  - programação
  - inteligenciaartificial
---

| Categoria             | Comando                      | O que faz                                     |
| --------------------- | ---------------------------- | --------------------------------------------- |
| 📂 Localização        | `pwd`                        | Mostra a pasta atual.                         |
| 📂 Localização        | `Get-Location`               | Equivalente ao `pwd`.                         |
| 📁 Listar arquivos    | `ls`                         | Lista arquivos e pastas.                      |
| 📁 Listar arquivos    | `tree`                       | Lista arquivos e pastas.                      |
| 📁 Listar arquivos    | `dir`                        | Mesmo resultado do `ls`.                      |
| 📁 Listar arquivos    | `Get-ChildItem`              | Nome completo do `ls`.                        |
| 📂 Navegação          | `cd Pasta`                   | Entra em uma pasta.                           |
| 📂 Navegação          | `cd ..`                      | Volta um nível.                               |
| 📂 Navegação          | `cd ..\..`                   | Volta dois níveis.                            |
| 📂 Navegação          | `cd C:\Caminho`              | Vai para um caminho específico.               |
| 📁 Criar pasta        | `mkdir MinhaPasta`           | Cria uma pasta.                               |
| 📄 Criar arquivo      | `New-Item arquivo.txt`       | Cria um arquivo vazio.                        |
| 🗑️ Remover           | `Remove-Item arquivo.txt`    | Remove um arquivo.                            |
| 🗑️ Remover           | `Remove-Item Pasta -Recurse` | Remove uma pasta e todo o conteúdo.           |
| 📋 Copiar             | `Copy-Item origem destino`   | Copia arquivos ou pastas.                     |
| 🚚 Mover              | `Move-Item origem destino`   | Move arquivos ou pastas.                      |
| ✏️ Renomear           | `Rename-Item antigo novo`    | Renomeia arquivo ou pasta.                    |
| 📖 Ler arquivo        | `cat arquivo.txt`            | Exibe o conteúdo do arquivo.                  |
| 📖 Ler arquivo        | `Get-Content arquivo.txt`    | Mesmo que `cat`.                              |
| 📝 Editar             | `notepad arquivo.txt`        | Abre o arquivo no Bloco de Notas.             |
| ⚙️ Processos          | `Get-Process`                | Lista os processos em execução.               |
| ⚙️ Processos          | `Get-Process chrome`         | Procura um processo específico.               |
| ❌ Encerrar processo   | `Stop-Process -Name chrome`  | Finaliza um processo.                         |
| 🔧 Serviços           | `Get-Service`                | Lista os serviços do Windows.                 |
| 🔧 Serviços           | `Get-Service *sql*`          | Filtra serviços pelo nome.                    |
| 🌐 Rede               | `ipconfig`                   | Exibe informações da rede.                    |
| 🌐 Rede               | `ping google.com`            | Testa a conexão com um servidor.              |
| 🌐 Rede               | `netstat -ano`               | Mostra conexões e portas abertas.             |
| ❓ Ajuda               | `Get-Help comando`           | Exibe ajuda de um comando.                    |
| ❓ Ajuda               | `Get-Help comando -Examples` | Mostra exemplos de uso.                       |
| 🔎 Descobrir comandos | `Get-Command`                | Lista todos os comandos disponíveis.          |
| 🔎 Pesquisar comandos | `Get-Command *git*`          | Procura comandos relacionados ao termo.       |
| 💾 Variáveis          | `$nome = "Caio"`             | Cria uma variável.                            |
| 💾 Variáveis          | `$nome`                      | Exibe o valor da variável.                    |
| 🚀 Abrir VS Code      | `code .`                     | Abre a pasta atual no VS Code.                |
| 📂 Abrir Explorer     | `explorer .`                 | Abre a pasta atual no Explorador de Arquivos. |
| 🌍 Abrir navegador    | `start https://google.com`   | Abre uma URL no navegador padrão.             |
| 🟢 Node.js            | `node -v`                    | Mostra a versão do Node.js.                   |
| 🟢 npm                | `npm -v`                     | Mostra a versão do npm.                       |
| 📦 npm                | `npm init`                   | Inicializa um projeto Node.js.                |
| 📦 npm                | `npm install pacote`         | Instala um pacote.                            |
| ▶️ Executar Node      | `node app.js`                | Executa um arquivo JavaScript.                |
| 🌱 Git                | `git status`                 | Mostra o estado do repositório.               |
| 🌱 Git                | `git add .`                  | Adiciona todas as alterações.                 |
| 🌱 Git                | `git commit -m "mensagem"`   | Cria um commit.                               |
| 🌱 Git                | `git push`                   | Envia alterações ao repositório remoto.       |
| 🧹 Limpeza            | `cls` ou `clear`             | Limpa a tela do terminal.                     |
| 📜 Histórico          | `history`                    | Mostra os comandos executados anteriormente.  |

# ⭐ Os 10 Comandos que Você Mais Usará
| Comando       | Para que serve                    |
| ------------- | --------------------------------- |
| `pwd`         | Saber em qual pasta você está.    |
| `ls`          | Ver os arquivos da pasta atual.   |
| `mkdir`       | Criar uma nova pasta.             |
| `cd`          | Entrar em uma pasta.              |
| `cd ..`       | Voltar uma pasta.                 |
| `code .`      | Abrir o projeto no VS Code.       |
| `node app.js` | Executar uma aplicação Node.js.   |
| `npm install` | Instalar dependências do projeto. |
| `git status`  | Verificar o estado do Git.        |
| `cls`         | Limpar a tela.                    |
