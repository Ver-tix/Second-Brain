---
tags:
  - IA
  - programação
---
## 1. O que é um .env

É um arquivo de texto simples que guarda **variáveis de ambiente** — geralmente chaves de API, senhas, URLs e configurações que um programa lê ao iniciar. Diferente do JSON, não tem chaves `{ }`, nem vírgulas, nem aspas obrigatórias.

env

```env
API_KEY=sk-abc123
PORT=3000
DEBUG=true
```

---

## 2. Sintaxe básica

env

```env
NOME_DA_VARIAVEL=valor
```

- **Sem espaço** antes e depois do `=` (o espaço pode quebrar a leitura, dependendo do programa)
- Por convenção, nomes de variável ficam em **MAIÚSCULO com underscore** (`API_KEY`, não `apiKey`)
- Uma variável por linha
- Não tem vírgula no final, nem entre linhas

env

```env
✅ API_KEY=sk-abc123
❌ API_KEY = sk-abc123
❌ API_KEY=sk-abc123,
```

---

## 3. Quando usar aspas

Na maioria dos casos **não precisa de aspas**. Só use quando o valor tiver espaço ou caracteres especiais:

env

```env
NOME=Caio
FRASE="Olá, mundo"
CAMINHO="C:\Users\caioe\projeto"
```

Sem aspas, um valor com espaço pode ser cortado no meio:

env

```env
❌ SAUDACAO=Olá mundo        (pode ler só "Olá")
✅ SAUDACAO="Olá mundo"       (lê a frase inteira)
```

---

## 4. Comentários

Linhas começando com `#` são ignoradas — úteis pra explicar o que cada variável faz:

env

```env
# Chave da API da Z.ai (GLM Coding Plan)
ZAI_API_KEY=sk-abc123

# Porta do servidor local
PORT=3000
```

---

## 5. Tipos de valor (tudo é texto, na prática)

Diferente do JSON, no `.env` **tudo é lido como texto (string)** — não existe número, booleano ou null "de verdade". O programa que lê o `.env` é quem decide converter `"true"` em booleano ou `"3000"` em número.

env

```env
DEBUG=true       ← isso é lido como o texto "true", não o valor booleano
PORT=3000        ← isso é lido como o texto "3000", não o número 3000
VAZIO=           ← valor vazio (string vazia)
```

---

## 6. Erros mais comuns

|Erro|Exemplo errado|Correção|
|---|---|---|
|Espaço em volta do `=`|`API_KEY = sk-123`|`API_KEY=sk-123`|
|Aspas desnecessárias|`PORT="3000"`|`PORT=3000` (funciona, mas é redundante)|
|Faltando aspas em valor com espaço|`NOME=Caio Silva`|`NOME="Caio Silva"`|
|Variável duplicada|`PORT=3000` ... `PORT=4000`|Deixe só uma ocorrência (a última geralmente prevalece, mas não conte com isso)|
|Salvar como `.env.txt`|—|Salvar como `.env`, sem extensão extra (ver seção 8)|
|Espaço em branco extra no fim da linha|`API_KEY=sk-123`|Apagar espaço final (pode ir junto do valor sem querer)|

---

## 7. Nomeação de variáveis por convenção

env

```env
# Chaves de API sempre com sufixo _API_KEY ou _KEY
OPENAI_API_KEY=sk-...
ZAI_API_KEY=sk-...

# URLs com sufixo _URL
BASE_URL=https://api.z.ai/api/coding/paas/v4

# Flags booleanas (mesmo sendo texto) em maiúsculo
DEBUG=true
ENABLE_CACHE=false
```

---

## 8. Como criar/salvar o arquivo (Windows)

O nome do arquivo é literalmente **`.env`** — sem nome antes do ponto, e sem extensão depois.

1. Abra o Bloco de Notas
2. Cole o conteúdo
3. **Arquivo → Salvar como**
4. No campo nome, digite: `.env` (com o ponto na frente)
5. Em "Tipo", troque pra **"Todos os arquivos (_._)"** — senão vira `.env.txt`
6. Salve na pasta que o programa espera (geralmente a raiz do projeto)

⚠️ O Windows Explorer às vezes esconde ou reclama de arquivos que começam com ponto e não têm nome antes — pelo Bloco de Notas normalmente funciona sem problema.

---

## 9. Exemplo completo (parecido com seu caso do ZCode/GLM)

env

```env
# Configuração do provider GLM (Z.ai)
ZAI_API_KEY=sk-sua-chave-aqui
ZAI_BASE_URL=https://api.z.ai/api/coding/paas/v4
ZAI_MODEL=glm-5.2
```

Checklist rápido antes de salvar:

- [ ]  Sem espaço em volta do `=`
- [ ]  Aspas só quando o valor tem espaço ou caractere especial
- [ ]  Uma variável por linha
- [ ]  Sem vírgulas
- [ ]  Nome do arquivo é exatamente `.env` (não `.env.txt`)
- [ ]  Comentários com `#` na frente

---

## 10. JSON vs .env — diferença rápida

||JSON|.env|
|---|---|---|
|Estrutura|`{ "chave": "valor" }`|`CHAVE=valor`|
|Aspas em texto|Sempre|Só se tiver espaço|
|Números/booleanos|Tipados (`42`, `true`)|Sempre texto (`"42"`, `"true"`)|
|Aninhamento (objeto dentro de objeto)|Sim|Não existe|
|Uso comum|Configs estruturadas, APIs|Variáveis de ambiente, segredos|