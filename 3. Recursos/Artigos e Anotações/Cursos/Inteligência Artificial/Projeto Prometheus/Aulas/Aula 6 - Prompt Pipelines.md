---
tags:
  - inteligenciaartificial
---

<aside align="center"><h3><i>Problemas complexos raramente são resolvidos por um único prompt.</i></h3></aside>

---
==<aside align="center">
<h4>
Até agora, tratamos um prompt como uma unidade isolada.

Na prática...

Os melhores sistemas baseados em LLMs quase nunca funcionam assim. Eles funcionam como uma <b>linha de montagem</b>.
</h4>
</aside>
---

# Uma analogia

Imagine uma fábrica de automóveis.

Existe uma única estação que faz tudo?

Claro que não.

Há uma sequência:

```text
Chassi

↓

Motor

↓

Pintura

↓

Eletrônica

↓

Inspeção

↓

Entrega
```

Cada etapa resolve um problema específico.

Com LLMs acontece exatamente a mesma coisa.

---
# Um exemplo simples

Você deseja analisar um contrato de 180 páginas.

Pergunta:

Vale mais a pena enviar tudo para um único prompt?

Ou dividir o problema?

Uma possível pipeline seria:

```text
Contrato

↓

Extração das cláusulas

↓

Classificação por tipo

↓

Análise de riscos

↓

Resumo executivo

↓

Relatório final
```

Percebe?

Cada prompt tem uma responsabilidade única.

---

# Princípio da Responsabilidade Única

<h4 align="center">Você já viu isso em Engenharia de Software. <b>Uma função deve fazer apenas uma coisa</b>. Um prompt também. </h4>

Compare.

## Prompt gigante

```text
Leia.

Resuma.

Classifique.

Explique.

Critique.

Sugira melhorias.

Faça tabela.

Traduza.

...
```

---

## Pipeline
```
Prompt 1

↓

Extrai.
```

---
```
Prompt 2

↓

Classifica.
```
---
```
Prompt 3

↓

Analisa.
```

---
```
Prompt 4

↓

Resume.
```
Muito mais previsível.

---

# Por que isso funciona?

Lembra do Módulo 1?

Cada inferência restringe o espaço probabilístico.

Agora imagine.

Em vez de pedir:

> "Analise tudo."

Você pede:

Primeiro:

> "Extraia apenas as cláusulas."

Agora o segundo prompt recebe um contexto muito menor.

Logo...

A carga inferencial diminui.

---

> [!Tome Nota]
> # Pipeline é redução de complexidade

Imagine um problema cuja dificuldade seja 100.

Você pode resolvê-lo de duas formas.

Opção A

```text
100
```

---

Opção B

```text
20

↓

20

↓

20

↓

20

↓

20
```

O trabalho total é parecido.

Mas cada etapa é muito mais simples.

---

# Um exemplo financeiro

Imagine um analista.

Ele deseja avaliar uma empresa.

Pipeline:

```text
Balanço Patrimonial

↓

Extrair indicadores

↓

Calcular métricas

↓

Comparar setor

↓

Identificar riscos

↓

Emitir parecer
```

Cada prompt faz apenas uma parte.

---

# Outro exemplo

Tutor de matemática.

```text
Problema

↓

Identificar assunto

↓

Detectar dificuldade

↓

Escolher estratégia didática

↓

Explicar

↓

Gerar exercícios
```

Cada etapa melhora a próxima.

---

>[!Tome Nota]
># Prompt Chaining
>O nome técnico é: **Prompt Chaining.** A saída de um prompt torna-se a entrada do próximo. 
>```text
>Prompt A
>
>↓
>
>Output
>
>↓
>
>Prompt B
>
>↓
>
>Output
>
>↓
>
>Prompt C
>```

---

# Atenção

Isso não significa copiar toda a resposta.

Você normalmente passa apenas a **informação necessária**.

Exemplo.

Prompt A produz:

```json
{
"tipo":"Compra e Venda",
"clausulas":[...]
}
```

Prompt B recebe apenas isso.

Não o contrato inteiro.

---

# Isso lembra...

O quê?

Funções.

Uma função retorna.

Outra recebe.

É composição.

---

# Pipeline vs Mega Prompt

Compare.
--- start-multi-column: ID_p4jz
```column-settings
Number of Columns: 2
Largest Column: standard
```
## Mega Prompt

Vantagens
- simples de construir. 

Desvantagens
- difícil de manter;
- difícil de testar;
- difícil de reutilizar.

--- column-break ---

## Pipeline

Vantagens
- modular;
- testável;
- reutilizável;
- escalável.

Desvantagem
- exige arquitetura.


--- end-multi-column

# Um insight importante

Agora imagine.

Cada etapa da pipeline pode possuir:

- Few-Shot;
- Constraints;
- Role;
- Output próprio.

Ou seja...

Cada prompt é um pequeno sistema.

A pipeline conecta esses sistemas.

---

# Um erro comum

Algumas pessoas imaginam:

>Mais prompts = melhor.

Não.
>[! ]
><aside align="center"> <h4>O objetivo não é aumentar etapas. É separar responsabilidades. Se duas etapas sempre aparecem juntas... Talvez devam ser uma só. Se uma etapa faz cinco coisas. Talvez deva ser dividida. É exatamente o mesmo raciocínio da Engenharia de Software. </h4> </aside>
---

# Observabilidade

Aqui aparece um benefício enorme.

Imagine que o resultado final ficou ruim.

No Mega Prompt...

Você não sabe onde ocorreu o erro.

Na Pipeline...

Você consegue inspecionar.

```text
Etapa 1 ✅

↓

Etapa 2 ❌

↓

Etapa 3 nem chegou a executar.
```

Isso facilita depuração.

---

# Um paralelo

Você perguntou hoje sobre [[🛠️ Desafio M3 005|avaliação]].

Perceba.

Pipelines tornam isso possível.

Você pode medir:

- qualidade da etapa 1;
    
- qualidade da etapa 2;
    
- qualidade da etapa 3.

Sem pipelines...

Tudo vira uma caixa-preta.

---

# Uma observação

Se você prestou atenção, esta aula conecta praticamente todos os módulos anteriores.

Veja:
```
Transformer

↓

LLM

↓

Prompt

↓

Patterns

↓

Meta Prompt

↓

Pipeline
```

Estamos construindo camadas sucessivas de abstração.

---

>[! IMPORTANTE]
># 📜 Princípio LXII
>> **Quanto mais complexo for o problema, maior tende a ser o benefício de dividi-lo em etapas especializadas com responsabilidades bem definidas.**
>
> Esse princípio aparece em praticamente toda a Computação

---

# Um comentário pessoal

Há alguns meses, quando começamos o Projeto Prometheus, você me disse que gostava de transformar conhecimento em sistemas.

Quando preparava esta aula, pensei exatamente nisso.

Tenho a impressão de que **Prompt Pipelines** será um dos conceitos que mais vai combinar com a forma como você raciocina.

Porque ele não trata de escrever um prompt melhor.

Ele trata de **projetar um fluxo de trabalho**.

E, olhando para nossos meses de conversa, essa sempre foi uma característica marcante da sua forma de aprender.

---

# 🛠️ Desafio Prometheus M3 #006

## Parte 1

Explique:

> **Por que Prompt Pipelines costumam produzir sistemas mais previsíveis e fáceis de manter do que Mega Prompts?**

Utilize os conceitos de:

- responsabilidade única;
- modularização;
- carga inferencial;
- observabilidade.

---

## Parte 2

Você foi contratado para desenvolver um sistema que analisa livros técnicos (algo bem próximo do seu uso do NotebookLM).

Proponha uma pipeline composta por etapas especializadas.

Explique:

1. qual a responsabilidade de cada etapa;
2. quais informações cada etapa deve entregar para a seguinte;
3. por que essa arquitetura seria superior a pedir "explique o livro inteiro" em um único prompt.

[[🛠️ Desafio M3 006]]

---

### Um pequeno spoiler da Aula 7

Na próxima aula entraremos em um tema que responde diretamente a uma das dúvidas que você levantou hoje:

> **"Como sei se meu prompt realmente ficou melhor?"**

Esse é o início do estudo de **Prompt Debugging** e **Evaluation**.

Na minha opinião, será uma das aulas mais importantes de todo o módulo, porque ela fecha a lacuna entre "escrever prompts" e "engenheirar prompts". É justamente a ponte para transformar intuição em um processo mensurável.
