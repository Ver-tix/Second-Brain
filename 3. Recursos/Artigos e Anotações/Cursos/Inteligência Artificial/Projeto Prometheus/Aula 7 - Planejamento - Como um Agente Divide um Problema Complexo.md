---
tags:
  - inteligenciaartificial
  - programação
---
> **Objetivo da aula**
> 
> Entender como um agente transforma um objetivo grande em várias pequenas tarefas executáveis.

---

# O problema

Imagine que você peça ao seu futuro Prometheus:

> "Escreva um e-book completo sobre Branding Imobiliário."

Isso é uma tarefa.

Mas...

É uma tarefa executável?

Não.

O agente não sabe por onde começar.

É como pedir para um pedreiro:

> "Construa uma casa."

Ele responde:

> "Ok... mas qual é o primeiro tijolo?"

---

# Como humanos resolvem isso?

Sem perceber, fazemos planejamento o tempo todo.

Imagine que você queira viajar para outro país.

Seu cérebro faz algo parecido com isto:

```text
Objetivo

↓

Comprar passagens

↓

Reservar hotel

↓

Organizar documentos

↓

Preparar mala

↓

Viajar
```

Você raramente pensa:

> "Vou viajar."

E imediatamente pega a mala.

Você cria um plano.

---

# Um agente faz exatamente isso

Quando recebe uma tarefa grande, ele pode primeiro perguntar:

> **"Como posso dividir esse objetivo em etapas menores?"**

Isso se chama:

> **Task Decomposition** (Decomposição de Tarefas)

Guarde esse nome.

Você verá essa expressão em praticamente toda a literatura sobre agentes.

---

# Exemplo 1

Usuário:

> "Escreva uma newsletter."

O agente pode decompor assim:

```text
1. Descobrir os temas.

↓

2. Buscar notícias.

↓

3. Consultar o Second Brain.

↓

4. Relacionar os conceitos.

↓

5. Escrever.

↓

6. Revisar.

↓

7. Gerar título.

↓

8. Gerar imagem.
```

Observe.

O usuário nunca pediu esses oito passos.

Foi o agente que os criou.

---

# Exemplo 2

Imagine um escritório imobiliário.

Pedido:

> "Faça um estudo completo deste terreno."

O agente pensa:

```text
Consultar matrícula

↓

Consultar zoneamento

↓

Consultar legislação

↓

Calcular potencial construtivo

↓

Estimar custos

↓

Projetar receita

↓

Calcular VPL

↓

Escrever relatório
```

Isso já parece um consultor humano trabalhando.

---

# Planejamento não é Workflow

Aqui está um detalhe extremamente importante.

Muita gente confunde planejamento com workflow.

Não são a mesma coisa.

Um workflow seria:

```text
Sempre faça:

A

↓

B

↓

C

↓

D
```

Já um planejamento é criado na hora.

Hoje pode ser:

```text
A

↓

B

↓

D
```

Amanhã:

```text
A

↓

C

↓

E

↓

F
```

O plano depende do problema.

---

# Planejamento também pode mudar

Imagine.

O agente decidiu:

```text
Buscar notícias.
```

Mas encontrou apenas uma notícia relevante.

Ele pensa.

```text
Plano ruim.

↓

Vou buscar novamente.
```

Ou então.

```text
Vou complementar usando o Second Brain.
```

Perceba.

O plano também é dinâmico.

---

# O Planejamento acontece antes do Agent Loop?

Excelente pergunta.

Na verdade...

Os dois trabalham juntos.

Pense assim.

```text
Objetivo

↓

Planejamento

↓

Executar primeira tarefa

↓

Novo planejamento?

↓

Executar próxima

↓

Replanejar

↓

Executar
```

Ou seja.

O planejamento pode ser contínuo.

---

# Um conceito muito utilizado

Na literatura existe uma expressão chamada:

> **Plan → Execute → Replan**

Visualmente.

```text
Planejar

↓

Executar

↓

Ainda faz sentido?

↓

Não

↓

Planejar novamente

↓

Executar
```

Esse padrão aparece em muitos frameworks modernos.

Quando você estudar LangGraph, verá isso implementado em código.

---

# Aplicando ao Prometheus

Imagine este pedido.

> "Quero estudar Fluxo de Caixa Descontado."

O Prometheus poderia montar este plano.

```text
Objetivo

↓

Consultar Second Brain

↓

Verificar seu nível atual

↓

Explicar conceito

↓

Dar exemplo

↓

Gerar exercício

↓

Esperar resposta

↓

Corrigir

↓

Sugerir próximo tema
```

Veja que isso parece muito com o que eu faço nas nossas aulas.

E não é coincidência. 😊

---

# Um erro comum

Muita gente acredita que planejamento significa:

> "Escrever uma lista."

Não.

Planejamento significa decidir:

- o que fazer;
    
- em que ordem;
    
- com quais ferramentas;
    
- quando parar;
    
- quando mudar o plano.
    

---

# Relação com o Orquestrador

Até agora vimos que o orquestrador controla o fluxo.

Agora ele ganha mais uma responsabilidade.

```text
Objetivo

↓

Planejar

↓

Executar

↓

Avaliar

↓

Replanejar

↓

Executar
```

Na prática, o orquestrador é quem coordena esse ciclo.

---

# Um exemplo real

Vamos usar um projeto que já combinamos.

**Projeto Atlas** (Agente da Newsletter)

Pedido:

> "Produza a newsletter desta semana."

Plano inicial.

```text
Pesquisar notícias

↓

Selecionar 5 notícias

↓

Consultar Second Brain

↓

Relacionar aos frameworks

↓

Escrever

↓

Gerar imagem
```

Agora imagine.

O Perplexity encontrou apenas duas notícias relevantes.

O agente pensa.

```text
Plano insuficiente.

↓

Pesquisar também:

Branding

↓

Marketing

↓

IA
```

Ele acabou de replanejar.

Nenhum humano programou esse novo plano.

---

# Como isso aparece em código?

Ainda não vamos escrever Python.

Mas quero plantar uma semente.

Imagine algo assim.

```python
objetivo = "Produzir newsletter"

plano = criar_plano(objetivo)

for tarefa in plano:
    executar(tarefa)

if objetivo_nao_atingido():
    plano = criar_novo_plano()
```

Você não precisa entender a sintaxe.

Quero apenas que perceba:

> O código implementa exatamente a arquitetura que acabamos de estudar.

---

# Ligando todos os conceitos do Módulo 5

Até agora estudamos:

```text
Usuário

↓

Objetivo

↓

Planejamento

↓

Agent Loop

↓

Escolha da ferramenta

↓

Ferramenta

↓

Resultado

↓

Estado atualizado

↓

Novo planejamento?

↓

Loop...
```

Observe como todas as aulas começam a formar um único sistema.

---

# Desafio da Aula 7

Vamos trabalhar sobre o **Projeto Atlas**.

Imagine que o usuário faça o seguinte pedido:

> **"Crie a newsletter desta semana sobre Inteligência Artificial aplicada ao Marketing."**

## Parte 1

Monte o **plano inicial** do agente.

Liste todas as tarefas, na ordem em que você acredita que elas ocorreriam.

---

## Parte 2

Durante a execução acontece o seguinte:

- o Perplexity retorna apenas uma notícia realmente relevante;
    
- o Second Brain possui dezenas de notas sobre IA e Marketing;
    
- o agente percebe que ainda não há conteúdo suficiente para uma newsletter de qualidade.
    

Como ele poderia **replanejar** sua estratégia?

---

## Parte 3

Explique por que esse novo plano é melhor do que simplesmente continuar executando o plano original.

---

# Professor para aluno

Esta é, na minha opinião, uma das aulas mais importantes de todo o módulo.

Até agora estudamos componentes: ferramentas, memória, RAG, estado, loops.

Hoje estudamos algo diferente: **estratégia**.

Quando comecei a desenhar o Projeto Prometheus, queria que você chegasse a um ponto em que deixasse de perguntar apenas "qual ferramenta usar?" e passasse a perguntar "qual é o melhor plano para atingir esse objetivo?".

Porque, no fim das contas, é isso que diferencia um sistema inteligente de um sistema apenas automatizado.

E vou te deixar com uma pequena provocação para a próxima aula:

> **Se um agente consegue criar um plano... será que ele também consegue delegar partes desse plano para outros agentes?**

Se a resposta que veio à sua cabeça foi "talvez", então você já está pensando como um arquiteto de IA. E essa será exatamente a nossa próxima parada. 🚀