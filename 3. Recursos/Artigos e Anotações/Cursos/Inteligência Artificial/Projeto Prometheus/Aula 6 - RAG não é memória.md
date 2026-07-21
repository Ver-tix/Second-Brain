---
tags:
  - inteligenciaartificial
---
Existe uma frase que você verá constantemente na internet:

> "RAG dá memória ao modelo."

Ela está...

**tecnicamente errada.**

<h4 align="center">RAG não altera a memória do modelo. RAG altera o <b>contexto</b> recebido durante a inferência.</h4> 

Essa diferença parece pequena. Mas ela muda toda a arquitetura.

---

# Vamos lembrar como um LLM responde

Imagine que o modelo recebeu:

> "Quem fundou a OpenAI?"

Ele responde usando:

- pesos;
- padrões aprendidos;
- contexto da conversa.

Nada mais.

Agora imagine outra pergunta.

> "Qual é a política interna da Empresa XYZ sobre férias?"

O modelo nunca viu esse documento.

Como responder?

---

# A primeira ideia (errada)

Muita gente pensa:

```text
Vamos treinar o modelo novamente.
```

Mas isso gera problemas.

- caro;
- lento;
- conhecimento continua congelado após o treino;
- precisa repetir o processo sempre que um documento mudar.

---

# A segunda ideia (melhor)

Antes de perguntar ao modelo:

```text
Usuário

↓

Buscar documentos

↓

Encontrar trechos relevantes

↓

Enviar junto com a pergunta

↓

LLM

↓

Resposta
```

O modelo continua exatamente igual.

Os pesos continuam iguais.

A diferença é que agora ele recebeu um contexto muito melhor.

---

# O que significa Retrieval?

Retrieval significa:

> **Encontrar a informação certa antes da geração.**

Não é geração.

É busca.

---

# O que significa Generation?

Depois da busca:

O modelo recebe algo como:

```text
Documento:

"O calendário acadêmico determina que as matrículas encerram em 15 de agosto."

Pergunta:

"Até quando posso me matricular?"
```

Agora ele responde:

> "Segundo o calendário acadêmico, as matrículas encerram em 15 de agosto."

Observe.

Ele não inventou.

Ele utilizou o contexto.

---

# O pipeline de RAG

Quase todos os sistemas seguem algo parecido com:

```text
Pergunta

↓

Embedding

↓

Busca vetorial

↓

Documentos relevantes

↓

Contexto

↓

LLM

↓

Resposta
```

Perceba uma coisa.

O LLM aparece quase no final.

Grande parte do trabalho acontece antes.

---

# Uma analogia

Imagine uma prova de Direito.

Sem consulta.

Você responde apenas pela memória.

Agora imagine a mesma prova.

Mas você pode consultar o código civil.

Seu cérebro mudou?

Não.

O contexto disponível mudou.

RAG faz exatamente isso.

---

# Um erro muito comum

Muitos iniciantes imaginam isto:

```text
Documento

↓

LLM

↓

Pronto.
```

Na prática, isso quase nunca acontece.

Por quê?

Porque empresas possuem:

- milhares de PDFs;
    
- milhões de páginas;
    
- documentos enormes.
    

Não cabe tudo na janela de contexto.

Então primeiro precisamos descobrir:

> **Quais partes realmente importam?**

---

# O papel da busca

Imagine um hospital.

Existem 300 mil prontuários.

O médico pergunta:

> "Qual foi o último exame de creatinina do paciente João?"

Seria absurdo enviar os 300 mil prontuários para o modelo.

A busca primeiro encontra:

```text
Paciente João

↓

Exames

↓

Creatinina

↓

Último resultado
```

Só então o modelo explica.

---

# O papel do LLM

Perceba algo importante.

No RAG:

O modelo não é o bibliotecário.

O modelo é o professor.

Quem encontra o livro é o sistema.

Quem explica o livro é o modelo.

---

# 📜 Princípio LXXI

> **RAG não aumenta o conhecimento interno do modelo; aumenta a qualidade do contexto disponível durante a inferência.**

---

# Um insight para conectar com tudo o que você estudou

Lembra do Módulo 2?

Você aprendeu que:

- o conhecimento do modelo fica congelado;
    
- modelos possuem cutoff;
    
- ferramentas externas reduzem alucinações.
    

Na época, isso parecia apenas teoria.

Agora você consegue enxergar a consequência arquitetural:

O problema **não é** que o modelo "esquece".

O problema é que a aplicação precisa fornecer a informação correta no momento certo.

É exatamente isso que o RAG resolve.

---

# Desafio Prometheus #006

Imagine que você precisa construir um assistente para uma empresa de engenharia.

Ela possui:
- 15 mil normas técnicas;
- milhares de contratos;
- centenas de manuais de equipamentos;
- procedimentos internos atualizados toda semana.

Responda: 

1. Por que seria uma má ideia tentar resolver esse problema apenas aumentando o tamanho do LLM?
2. Explique, passo a passo, como um pipeline de RAG resolveria essa situação.
3. Qual é a responsabilidade da etapa de **Retrieval** e qual é a responsabilidade da etapa de **Generation**?
4. Por que dizemos que RAG melhora a qualidade das respostas **sem alterar um único peso do modelo**?

[[🛠 Desafio M4 006]]

---

## Uma observação final

Talvez você tenha notado algo interessante.

Há poucas semanas, "RAG" parecia apenas uma sigla.

Hoje, depois de tudo o que estudamos sobre orquestração, separação de responsabilidades, contexto, memória e arquitetura, o conceito começa a parecer quase inevitável.

Essa sensação é intencional.

Os módulos do Prometheus foram organizados para que cada conceito preparasse o terreno para o próximo. Quando você finalmente chega ao RAG, ele deixa de parecer uma técnica isolada e passa a ser uma consequência natural de tudo o que veio antes.

E, se eu puder deixar um pequeno spoiler: quando dominarmos RAG, estaremos a um passo de entrar no universo dos **agentes de IA**. É lá que praticamente todas as peças que você estudou até agora começam a se encaixar em um único sistema. 

[[❓Um Questionamento Importante - Projeto Prometheus M4 Aula 6]]