---
tags:
  - programação
  - inteligenciaartificial
---
| Categoria                     | Comando                                    | O que faz                                              |
| ----------------------------- | ------------------------------------------ | ------------------------------------------------------ |
| 🔍 Verificar versão           | `pip --version`                            | Mostra a versão do pip instalada.                      |
| 🔍 Verificar versão           | `python -m pip --version`                  | Forma recomendada (garante o pip do Python utilizado). |
| 📦 Instalar pacote            | `pip install requests`                     | Instala um pacote.                                     |
| 📦 Instalar versão específica | `pip install requests==2.32.3`             | Instala uma versão específica.                         |
| 📦 Instalar versão mínima     | `pip install "requests>=2.0"`              | Instala uma versão igual ou superior à especificada.   |
| 🔄 Atualizar pacote           | `pip install --upgrade requests`           | Atualiza um pacote para a versão mais recente.         |
| ❌ Remover pacote              | `pip uninstall requests`                   | Remove um pacote instalado.                            |
| 📋 Listar pacotes             | `pip list`                                 | Lista todos os pacotes instalados.                     |
| 📄 Gerar dependências         | `pip freeze`                               | Lista os pacotes em formato para `requirements.txt`.   |
| 💾 Salvar dependências        | `pip freeze > requirements.txt`            | Cria o arquivo de dependências do projeto.             |
| 📥 Instalar dependências      | `pip install -r requirements.txt`          | Instala todos os pacotes listados no arquivo.          |
| 🔎 Informações do pacote      | `pip show requests`                        | Mostra detalhes de um pacote instalado.                |
| 🔍 Procurar pacote instalado  | `pip list \| findstr requests` _(Windows)_ | Filtra a lista de pacotes.                             |
| ⚠️ Verificar conflitos        | `pip check`                                | Verifica dependências quebradas ou incompatíveis.      |
| 🧹 Limpar cache               | `pip cache purge`                          | Remove o cache de downloads do pip.                    |
| 📂 Ver cache                  | `pip cache dir`                            | Mostra onde o cache do pip está armazenado.            |
| 📥 Baixar sem instalar        | `pip download requests`                    | Baixa o pacote sem instalá-lo.                         |
| 📤 Reinstalar pacote          | `pip install --force-reinstall requests`   | Reinstala completamente um pacote.                     |
| 🔄 Atualizar o próprio pip    | `python -m pip install --upgrade pip`      | Atualiza o gerenciador pip.                            |

# Ambientes Virtuais (Essencial)
|Comando|O que faz|
|---|---|
|`python -m venv .venv`|Cria um ambiente virtual chamado `.venv`.|
|`.venv\Scripts\activate` _(Windows)_|Ativa o ambiente virtual.|
|`deactivate`|Sai do ambiente virtual.|
|`pip install pacote`|Instala o pacote apenas dentro do ambiente virtual.|
# Pacotes Mais Usados
|Pacote|Finalidade|
|---|---|
|`requests`|Requisições HTTP.|
|`pandas`|Manipulação de dados.|
|`numpy`|Computação numérica.|
|`matplotlib`|Gráficos.|
|`scikit-learn`|Machine Learning.|
|`python-dotenv`|Carregar variáveis do arquivo `.env`.|
|`openai`|Utilizar a API da OpenAI.|
|`jupyter`|Notebooks interativos.|
|`fastapi`|Desenvolvimento de APIs.|
|`streamlit`|Criar aplicações web em Python.|

# Fluxo Típico de um Projeto Python

```
python -m venv .venv
```

↓

```
.venv\Scripts\activate
```

↓

```
pip install openai python-dotenv
```

↓

```
pip freeze > requirements.txt
```

↓

Outro desenvolvedor clona o projeto:

```
pip install -r requirements.txt
```

# Os 10 comandos que você mais usará

| Comando                               | Para que serve                                |
| ------------------------------------- | --------------------------------------------- |
| `pip --version`                       | Verificar se o pip está instalado.            |
| `pip install pacote`                  | Instalar uma biblioteca.                      |
| `pip install --upgrade pacote`        | Atualizar uma biblioteca.                     |
| `pip uninstall pacote`                | Remover uma biblioteca.                       |
| `pip list`                            | Ver os pacotes instalados.                    |
| `pip show pacote`                     | Obter informações sobre um pacote.            |
| `pip freeze`                          | Listar dependências do projeto.               |
| `pip freeze > requirements.txt`       | Salvar as dependências em um arquivo.         |
| `pip install -r requirements.txt`     | Instalar todas as dependências de um projeto. |
| `python -m pip install --upgrade pip` | Atualizar o próprio pip.                      |