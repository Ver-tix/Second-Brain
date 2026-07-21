---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# Questão 1

## Vamos reler a questão

![[Aula 12 - Da Arquitetura ao Código - Como um Sistema Inteligente Nasce (Transição para Implementação)#^0aacc6]]


---

## Minha primeira crítica ao exercício

Eu mudaria uma coisa na própria questão.

Ela diz:

> "Escolha um módulo."

Mas, para um iniciante, isso gera ansiedade.

Porque parece que existe uma resposta correta.

Na verdade, **não existe**.

Arquitetura raramente possui uma única solução.

Ela possui soluções melhores ou piores para determinado contexto.

Então podemos escolher qualquer um.

---

## Qual módulo eu escolheria?

Eu escolheria o **Prometheus-Mentor**.

Por quê?

Porque é o mais fácil de visualizar.

Imagine um professor.

Ele faz várias coisas diferentes.

Às vezes ensina.

Às vezes corrige.

Às vezes recomenda livros.

Às vezes sintetiza ideias.

Ou seja...

Naturalmente ele pode ser dividido.

---

## Agora vem a mudança de mentalidade

Muita gente começaria desenhando pastas.

Eu não.

Primeiro responderia:

> **O que esse módulo faz?**

Resposta:

Ele é responsável por ajudar o aluno a aprender.

Essa é sua missão.

Agora pergunto:

> "Que atividades são necessárias para cumprir essa missão?"

E aí surgem naturalmente os agentes.

---

## Vamos descobrir os agentes

Em vez de decorar os nomes da aula, vamos pensar juntos.

Imagine que você me faz uma pergunta.

Por exemplo:

> "Explique o Attention Mechanism."

O módulo Mentor precisa...

### Primeiro

Entender sua dúvida.

Quem faz isso?

Talvez:

```text
TutorAgent
```

---

Depois...

Talvez consultar seu nível de conhecimento.

Quem faz isso?

```text
EvaluatorAgent
```

---

Depois...

Buscar materiais.

Quem faz isso?

```text
CuratorAgent
```

---

Depois...

Produzir uma explicação integrada.

Quem faz isso?

```text
SynthesizerAgent
```

Percebe?

Os agentes surgiram da missão.

Não da linguagem Python.

---

## Só agora pensamos nas pastas

Veja como ficou natural.

```text
mentor/

│

├── tutor.py

├── evaluator.py

├── curator.py

├── synthesizer.py
```

Nem precisei decorar.

A arquitetura praticamente se desenhou sozinha.

---

## Agora vem a pergunta difícil

Por que dividir assim?

Essa é justamente a parte que faz o aluno travar.

Mas existe uma resposta baseada em engenharia.

Eu escreveria algo assim:

> O módulo foi dividido de acordo com as responsabilidades de cada agente. Cada componente possui uma única função bem definida, facilitando manutenção, testes e evolução independente. Caso seja necessário melhorar apenas a curadoria de conteúdos, por exemplo, basta modificar o CuratorAgent sem alterar o restante do sistema.

Percebe?

Não falei de Python.

Falei de responsabilidades.

---

## Responsabilidades

Agora fica fácil.

|Arquivo|Responsabilidade|
|---|---|
|tutor.py|Interagir com o usuário e conduzir o ensino|
|evaluator.py|Avaliar o conhecimento do aluno|
|curator.py|Selecionar materiais e referências|
|synthesizer.py|Integrar as informações em uma resposta coerente|

---

## Comunicação

Outra parte que intimida.

Mas pense na aula.

Quem conversa com o Mentor?

O Orquestrador.

Então:

```text
Usuário

↓

PrometheusOS

↓

Mentor
```

Dentro do Mentor:

```text
Tutor

↓

Evaluator

↓

Curator

↓

Synthesizer
```

Depois...

Se precisar de conhecimento:

```text
Curator

↓

Knowledge Service

↓

RAG
```

E então volta.

---

## O que a questão realmente queria avaliar?

Essa é a parte que acho que faltou ficar explícita na aula.

Na minha opinião, o professor **não quer saber se você sabe organizar pastas**.

Ele quer verificar se você compreendeu três ideias fundamentais:

1. **Um módulo existe para cumprir uma missão.**
    
2. **Uma missão pode ser dividida em responsabilidades menores.**
    
3. **Cada responsabilidade pode ser representada por um componente (classe, arquivo ou serviço).**
    

Se você demonstrar esse raciocínio, a estrutura de diretórios quase se torna um detalhe de implementação.

---

## Um elogio e uma pequena crítica à aula

Acho que o exercício é excelente, mas eu acrescentaria uma etapa intermediária para alunos iniciantes.

Em vez de pedir diretamente a estrutura de pastas, eu proporia este roteiro:

1. Qual é a missão do módulo?
    
2. Quais tarefas ele precisa executar para cumprir essa missão?
    
3. Quem deveria ser responsável por cada tarefa?
    
4. Só então desenhe os arquivos e pastas.
    

Esse roteiro ensina um padrão de raciocínio que você poderá reutilizar em qualquer projeto, não apenas no Prometheus.

E eu gostaria que, nas próximas questões do desafio, resolvêssemos sempre dessa forma: **primeiro arquitetamos no papel; depois organizamos os componentes; só por último pensamos em código**. Isso está totalmente alinhado com a filosofia que o próprio Módulo 5 procurou ensinar.