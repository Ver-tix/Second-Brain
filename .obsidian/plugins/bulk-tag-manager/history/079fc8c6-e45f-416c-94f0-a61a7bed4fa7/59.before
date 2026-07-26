---
tags:
  - inteligenciaartificial
---

# Primeiro princípio

<h3 align="center">Você nunca mede "o prompt". Você mede <b>o comportamento desejado</b>.</h3>
Por exemplo.

Imagine que você criou um prompt para identificar cláusulas abusivas em contratos.

O que você quer medir?

Pode ser:
- [ ] identificou todas as cláusulas?
- [ ] inventou alguma?
- [ ] explicou corretamente?
- [ ] produziu JSON válido?

Cada uma dessas perguntas gera uma métrica diferente.

---

# Exemplo 1 — Classificação (o mais fácil)

Imagine que você possui 100 contratos.

Um advogado já analisou todos.

Esse é o seu **Ground Truth**.

|Contrato|Verdade|IA|
|---|---|---|
|1|Abusiva|Abusiva|
|2|Normal|Normal|
|3|Abusiva|Normal|
|4|Normal|Normal|
|...|...|...|

Suponha que:

- acertou 92
- errou 8

A precisão (accuracy) seria

$$\text{Accuracy}=\frac{92}{100}=92\%$$

Muito simples.

---

# Exemplo 2 — Extração de informações

Agora imagine.

Você pede:

> Extraia todos os CNPJs do contrato.

O documento possui:

```
15 CNPJs
```

A IA encontrou:

```
13
```

Destes:

```
12 estavam corretos
1 era inventado
```

Agora entram métricas famosas.

### Precision
Dos itens encontrados...

quantos estavam certos?

$$ 
\text{Precision}=\frac{12}{13}  
=92,3\%  
$$

---

### Recall

Dos itens que realmente existiam...

quantos ela encontrou?

$$ 
\text{Recall}=\frac{12}{15}  
=80\%  
$$

Observe.

Ela foi bastante precisa.

Mas deixou passar vários.

---

# Exemplo 3 — Resumo

Agora complica.

Como medir um resumo?

Não existe resposta única.

Você cria critérios.

Exemplo.

Cada resumo recebe notas de 0 a 5 em:

- fidelidade
- cobertura
- clareza
- organização
- ausência de alucinação

Imagine.

|Critério|Nota|
|---|---|
|Fidelidade|5|
|Cobertura|4|
|Clareza|5|
|Organização|5|
|Alucinação|4|

Total:

$$\frac{23}{25} = 92\%$$
Percebe?

Agora você criou uma régua.

---

# Exemplo 4 — Prompt Engineering

Imagine dois prompts.

Prompt A

Prompt B

Você cria 50 casos de teste.

Depois mede.

|Caso|A|B|
|---|---|---|
|1|✅|✅|
|2|❌|✅|
|3|✅|✅|
|4|❌|❌|
|...|...|...|

Resultado.

|Prompt|Acertos|
|---|---|
|A|41|
|B|47|

Logo.

A

$$ 
41/50=82\%  
$$

B

$$ 
47/50=94\%  
$$

Agora você possui um número.

---

# Mas... e respostas abertas?

Aqui entra uma das maiores dificuldades.

Imagine.

Pergunta:

> Explique o que é inflação.

Resposta A.

Excelente.

Resposta B.

Também excelente.

Como medir?

Não existe uma única resposta correta.

Então usamos outra estratégia.

---

# Rubricas

Muito usadas em universidades.

Você define critérios.

Por exemplo.

|Critério|Peso|
|---|---|
|Corretude técnica|40%|
|Completude|25%|
|Clareza|20%|
|Organização|15%|

Cada resposta recebe notas.

No final.

Sai uma pontuação.

---

# E quem atribui essas notas?

Existem quatro possibilidades.

### Humano

Mais confiável.

Mais caro.

---

### Especialista

Excelente.

Muito caro.

---

### Outro LLM

Muito rápido.

Muito barato.

Mas precisa ser validado.

---

### Métricas automáticas

Muito usadas em pesquisa.

Por exemplo:

- ROUGE
    
- BLEU
    
- BERTScore
    

Mas possuem limitações.

---

# Agora vem a parte interessante

Você perguntou:

> "Como eu mediria essa precisão?"

Na empresa...

Você quase nunca mede uma única métrica.

Você cria um **Evaluation Suite**.

Exemplo.

```text
Prompt Jurídico

↓

100 contratos

↓

Precisão de classificação

↓

Precisão na extração

↓

Tempo de resposta

↓

Formato JSON

↓

Alucinação

↓

Nota final
```

Perceba.

O prompt ganha uma espécie de "boletim".

---

# Isso conversa com Engenharia de Software

Você já estudou testes unitários.

Aqui acontece algo parecido.

Não basta dizer:

> "O código parece bom."

Você roda testes.

Prompt Engineering madura funciona igual.

---

# Agora vou te contar uma curiosidade

Empresas como a OpenAI, Anthropic e Google DeepMind possuem **milhares de casos de teste internos**.

Quando um modelo novo é treinado, ele não é avaliado apenas em um benchmark geral.

Ele passa por centenas ou milhares de avaliações especializadas:
- programação;
- matemática;
- direito;
- medicina;
- segurança;
- alucinação;
- aderência às instruções;
- formatação;
- comportamento.

Ou seja, eles não perguntam simplesmente:

> "Este modelo é melhor?"

Eles perguntam:

> **"Melhor em quê?"**

Essa é uma mudança de mentalidade muito importante.

---

## E acho que você vai gostar desta conclusão

Lembra da pergunta que você me fez alguns dias atrás?

> **"Não sei se teria capacidade crítica para avaliar qual prompt ficou melhor."**

Depois desta aula, talvez você perceba que **ninguém avalia "no olho"** em engenharia séria.

Um engenheiro de IA **projeta uma régua antes de comparar os prompts**.

Esse talvez seja o insight mais importante da aula inteira: a qualidade deixa de ser uma opinião e passa a ser uma propriedade observável, definida por critérios explícitos e reproduzíveis. É exatamente essa forma de pensar que diferencia experimentação casual de engenharia.