---
tags:
  - programação
  - IA
---


| Categoria                          | Comando                   | O que faz                                        |
| ---------------------------------- | ------------------------- | ------------------------------------------------ |
| 🔍 Verificar versão                | `npm -v`                  | Mostra a versão do npm.                          |
| 🔍 Verificar Node                  | `node -v`                 | Mostra a versão do Node.js.                      |
| 📦 Inicializar projeto             | `npm init`                | Cria um novo projeto interativamente.            |
| 📦 Inicialização rápida            | `npm init -y`             | Cria um `package.json` com valores padrão.       |
| 📥 Instalar pacote                 | `npm install express`     | Instala um pacote no projeto.                    |
| 📥 Instalar (atalho)               | `npm i express`           | Forma abreviada de `npm install`.                |
| 🌍 Instalar globalmente            | `npm install -g nodemon`  | Instala um pacote globalmente.                   |
| 📄 Instalar dependências           | `npm install`             | Instala tudo que está no `package.json`.         |
| 🔄 Atualizar pacote                | `npm update express`      | Atualiza um pacote.                              |
| ❌ Remover pacote                   | `npm uninstall express`   | Remove um pacote.                                |
| 📋 Listar pacotes locais           | `npm list`                | Lista os pacotes do projeto.                     |
| 🌍 Listar pacotes globais          | `npm list -g --depth=0`   | Lista apenas os pacotes globais.                 |
| 🔎 Informações do pacote           | `npm info express`        | Mostra detalhes de um pacote.                    |
| 🔍 Ver dependências desatualizadas | `npm outdated`            | Lista os pacotes que possuem versões mais novas. |
| 🧹 Limpar cache                    | `npm cache clean --force` | Limpa o cache do npm.                            |
| 📂 Ver pasta global                | `npm root -g`             | Mostra onde os pacotes globais estão instalados. |
| 🔄 Atualizar npm                   | `npm install -g npm`      | Atualiza o próprio npm.                          |

# Scripts
Os scripts ficam no arquivo `package.json`.

|Comando|O que faz|
|---|---|
|`npm run dev`|Executa o script `dev`.|
|`npm run start`|Executa o script `start`.|
|`npm run build`|Executa o script de compilação.|
|`npm run test`|Executa os testes.|
|`npm start`|Atalho para `npm run start`.|
|`npm test`|Atalho para `npm run test`.|

Exemplo de `package.json`:

```
{  
	"scripts": 
		{    
			"dev": "node app.js",    
			"start": "node server.js",    
			"test": "jest"  
		}
}
```

# Dependências

|Tipo|Comando|Quando usar|
|---|---|---|
|Dependência de produção|`npm install express`|Necessária para a aplicação funcionar.|
|Dependência de desenvolvimento|`npm install -D nodemon`|Necessária apenas durante o desenvolvimento.|
|Instalação global|`npm install -g nodemon`|Ferramentas disponíveis para todos os projetos.|

---

# Arquivos importantes

|Arquivo|Função|
|---|---|
|`package.json`|Descreve o projeto, scripts e dependências.|
|`package-lock.json`|Registra exatamente as versões instaladas.|
|`node_modules/`|Contém todas as bibliotecas do projeto.|

> **Dica:** normalmente a pasta `node_modules` **não é enviada ao Git**. Basta compartilhar o `package.json` (e o `package-lock.json`) e outro desenvolvedor executa `npm install` para recriá-la.

---

# Pacotes mais usados

|Pacote|Finalidade|
|---|---|
|`express`|Criar APIs e servidores web.|
|`axios`|Fazer requisições HTTP.|
|`dotenv`|Ler variáveis do arquivo `.env`.|
|`openai`|Utilizar a API da OpenAI.|
|`nodemon`|Reiniciar automaticamente o servidor durante o desenvolvimento.|
|`cors`|Configurar CORS em APIs.|
|`mongoose`|Conectar ao MongoDB.|
|`prisma`|ORM para bancos de dados.|
|`typescript`|Desenvolvimento em TypeScript.|
|`jest`|Testes automatizados.|

---

# Fluxo típico de um projeto Node.js

Criar o projeto

```
npm init -y
```

↓

Instalar bibliotecas

```
npm install express dotenv openai
```

↓

Instalar ferramenta de desenvolvimento

```
npm install -D nodemon
```

↓

Executar

```
node app.js
```

ou

```
npm run dev
```

↓

Outro desenvolvedor clona o projeto

```
npm install
```

Todas as dependências serão instaladas automaticamente.

---

# Os 10 comandos que você mais usará

|Comando|Para que serve|
|---|---|
|`npm -v`|Verificar a versão do npm.|
|`npm init -y`|Criar um novo projeto rapidamente.|
|`npm install pacote`|Instalar uma biblioteca.|
|`npm install`|Instalar todas as dependências do projeto.|
|`npm uninstall pacote`|Remover uma biblioteca.|
|`npm update`|Atualizar dependências.|
|`npm run dev`|Executar o ambiente de desenvolvimento.|
|`npm run start`|Iniciar a aplicação.|
|`npm list`|Ver os pacotes instalados.|
|`npm install -g pacote`|Instalar uma ferramenta globalmente.|