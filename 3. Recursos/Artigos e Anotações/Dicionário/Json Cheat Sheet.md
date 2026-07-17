---
tags:
  - inteligenciaartificial
  - programação
---
# Cheat Sheet: JSON na Prática

## 1. Estrutura básica

JSON é só **chave: valor**, entre chaves `{ }`.

```json
{
  "chave": "valor"
}
```

- Toda chave fica entre **aspas duplas** `" "` (nunca aspas simples `' '`)
- Chave e valor são separados por **dois pontos** `:`
- Cada par "chave: valor" é separado por **vírgula** `,`
- **O último item NUNCA tem vírgula depois** (erro mais comum)

```json
{
  "nome": "Caio",
  "idade": 30,
  "cidade": "Fortaleza"
}
```

☝️ repare: `"cidade": "Fortaleza"` não tem vírgula depois, porque é o último.

---

## 2. Tipos de valores

|Tipo|Exemplo|Aspas?|
|---|---|---|
|Texto (string)|`"glm-5.2"`|Sim, sempre|
|Número|`42` ou `3.14`|Não|
|Verdadeiro/falso|`true` / `false`|Não|
|Nulo/vazio|`null`|Não|
|Lista (array)|`["a", "b", "c"]`|depende do conteúdo|
|Objeto aninhado|`{ "x": 1 }`|Não (é um bloco)|

```json
{
  "modelo": "glm-5.2",
  "temperatura": 0.7,
  "ativo": true,
  "proxy": null,
  "tags": ["coding", "glm", "producao"]
}
```

---

## 3. Aspas: quando manter, quando não

- **Chaves**: sempre com aspas → `"apiKey"`
- **Valores de texto**: sempre com aspas → `"sk-abc123"`
- **Valores numéricos, true/false, null**: NUNCA com aspas

```json
{
  "apiKey": "sk-abc123",   ✅ texto → com aspas
  "timeout": 30,           ✅ número → sem aspas
  "debug": false           ✅ booleano → sem aspas
}
```

❌ Erro comum: `"timeout": "30"` — isso vira texto, não número (pode quebrar a leitura do programa dependendo do caso)

---

## 4. Objetos aninhados (um "dentro" do outro)

Quando um valor é, ele mesmo, um conjunto de chave:valor, ele fica entre `{ }` também:

```json
{
  "provider": "glm",
  "config": {
    "baseURL": "https://api.z.ai/api/coding/paas/v4",
    "model": "glm-5.2"
  }
}
```

Aqui, `"config"` é uma chave cujo valor é **outro objeto JSON completo**.

---

## 5. Erros mais comuns (e como evitar)

|Erro|Exemplo errado|Correção|
|---|---|---|
|Vírgula sobrando no fim|`{"a": 1, "b": 2,}`|`{"a": 1, "b": 2}`|
|Aspas simples|`{'a': 'b'}`|`{"a": "b"}`|
|Chave sem aspas|`{a: "b"}`|`{"a": "b"}`|
|Esquecer de fechar chave/colchete|`{"a": "b"`|`{"a": "b"}`|
|Barra invertida em caminho Windows|`"C:\Users\caioe"`|`"C:\\Users\\caioe"` (dobrar a barra) ou usar `/`|

⚠️ **Atenção especial pro seu caso**: caminhos do Windows usam `\`, mas dentro de JSON a barra invertida é um caractere especial. Duas opções:

```json
"path": "C:\\Users\\caioe\\.zcode\\cli"
```

ou

```json
"path": "C:/Users/caioe/.zcode/cli"
```

(JSON aceita `/` mesmo no Windows)

---

## 6. Como validar antes de salvar

Antes de salvar o arquivo, cole o conteúdo em um validador online (ex: jsonlint.com) — ele aponta exatamente onde está o erro de sintaxe, linha por linha. É o jeito mais rápido de pegar vírgula sobrando ou aspas esquecidas.

---

## 7. Exemplo completo (parecido com o seu config.json)

```json
{
  "provider": "glm",
  "model": "glm-5.2",
  "apiKey": "sk-sua-chave-aqui",
  "baseURL": "https://api.z.ai/api/coding/paas/v4"
}
```

Checklist rápido antes de salvar:

- [ ] Toda chave tem aspas duplas
- [ ] Todo valor de texto tem aspas duplas
- [ ] Números e true/false/null SEM aspas
- [ ] Sem vírgula depois do último item
- [ ] Toda `{` tem seu `}` correspondente
- [ ] Salvou como `.json`, não `.json.txt`