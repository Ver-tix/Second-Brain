**Executa totalmente no local.** Esta sessão inicia um servidor de caixa de ferramentas do MCP **local** que lê um banco de dados SQLite **local** (`destinations.db`). Não é necessário o Google Cloud, faturamento ou créditos. O único download externo é o binário da caixa de ferramentas do MCP de código aberto.

## Etapa 1: preparar o banco de dados local

👉💻 Na raiz do repositório, crie o banco de dados de exemplo:

```bash
cd ~/adk_tutorialsource .adk_env/bin/activatechmod +x setup_trip_database.py./setup_trip_database.py
```

Isso cria `destinations.db` em `~/adk_tutorial/`.

⚠️ **O banco de dados de exemplo contém apenas quatro cidades: Paris, Roma, Nova York e Tóquio** (tipos de atração: marco, museu, parque). Os comandos que fazem referência a outras cidades (por exemplo, São Francisco ou Sunnyvale) vão retornar **nenhum resultado** corretamente, porque essas linhas não existem. Use as quatro cidades acima ao testar esta sessão.

## Etapa 2: instalar e executar o servidor da caixa de ferramentas do MCP

👉💻 Faça o download do binário da caixa de ferramentas do MCP para **seu sistema operacional**:

```bash
cd ~/adk_tutorial/mcp_tool_boxexport VERSION=0.16.0# Choose the line that matches your machine:export OS=linux/amd64      # Cloud Shell or Linux (x86_64)# export OS=darwin/arm64   # macOS Apple Silicon (M1/M2/M3)# export OS=darwin/amd64   # macOS Intelcurl -O https://storage.googleapis.com/genai-toolbox/v$VERSION/$OS/toolbox
```

depois de concluir o download e executar

```bash
chmod +x toolbox
```

Não sabe qual Mac você tem? Execute `uname -m` — `arm64` = Apple Silicon, `x86_64` = Intel. No **Windows**, faça o download de `.../windows/amd64/toolbox.exe` e execute-o no PowerShell.

## Etapa 3

**Em um terminal** Execute o seguinte comando (deixe-o em execução. O agente se conecta a ele em `http://127.0.0.1:7001`):

```bash
cd ~/adk_tutorialsource .adk_env/bin/activatecd ~/adk_tutorial/mcp_tool_box./toolbox --tools-file "trip_tools.yaml" --port 7001
```

**Em outro terminal** Execute o seguinte comando

```bash
cd ~/adk_tutorialsource .adk_env/bin/activatecd ~/adk_tutorial/g_agents_mcppython main.py
```

👉 **Comandos de teste** (use as cidades que existem no banco de dados de exemplo: Paris, Roma, Nova York ou Tóquio):

```
What are the top-rated things to do in Tokyo?
```

```
Show me the museums in Rome.
```

```
What can I do in New York for under 25 dollars?
```

**Solução de problemas da sessão 7**

- **"Conexão recusada" / o agente não encontra ferramentas:** o servidor da caixa de ferramentas no primeiro terminal não está em execução. Reinicie-o e confirme se ele está escutando na porta `7001`.
- **Cada consulta retorna resultados vazios**:confirme se você executou a **etapa 1** para que `~/adk_tutorial/destinations.db` exista e se o comando usa uma das quatro cidades (Paris, Roma, Nova York, Tóquio).
- **`toolbox: cannot execute binary file`** **no macOS**:você fez o download da versão do Linux. Execute novamente a **etapa 2** com `OS=darwin/arm64` (Apple Silicon) ou `OS=darwin/amd64` (Intel).