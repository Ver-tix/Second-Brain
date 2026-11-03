---
tags:
  - programação
  - IA
description: Mimo Code é um agente de programação via terminal (CLI) focado em interação com projetos, permitindo análise de arquivos, planejamento, implementação e automação de tarefas usando agentes especializados. Atua como um parceiro de desenvolvimento dentro do fluxo de trabalho do desenvolvedor.
---
# Cheat Sheet — Mimo Code (`@mimo-ai/cli`)

## 1. Comandos e controles principais

|Comando / Controle|Função|Exemplo de uso|
|---|---|---|
|`@`|Anexa arquivos ou pastas ao contexto do agente|`@src/main.py` → "Explique este código"|
|`$`|Seleciona um agente/subagente especializado|`$build Implemente autenticação JWT`|
|`/`|Executa comandos internos do Mimo|`/help`|
|`/help`|Lista comandos disponíveis|`/help`|
|`/goal`|Define um objetivo ou condição de conclusão|`/goal Criar uma API com testes`|
|`/compact`|Resume o contexto da conversa|`/compact`|
|`/dream`|Consolida conhecimento da sessão/projeto|`/dream`|
|`/distill`|Transforma padrões repetitivos em habilidades reutilizáveis|`/distill`|
|`Ctrl + C`|Interrompe execução atual|Cancelar uma tarefa|

---

# 2. Referência de arquivos (`@`)

|Sintaxe|O que faz|Exemplo|
|---|---|---|
|`@arquivo`|Adiciona um arquivo ao contexto|`@app.py`|
|`@pasta/`|Adiciona uma pasta inteira|`@src/`|
|Múltiplos `@`|Analisa arquivos relacionados|`@api.py @database.py`|
|`@projeto/`|Dá visão geral do projeto|`@src/ Explique a arquitetura`|

### Exemplos práticos

|Objetivo|Prompt|
|---|---|
|Entender código|`@main.py Explique esse arquivo linha por linha`|
|Encontrar problemas|`@src/ Procure bugs nesse projeto`|
|Refatorar módulo|`@service.py Melhore a arquitetura sem alterar comportamento`|
|Comparar arquivos|`@old.py @new.py Compare as diferenças`|

---

# 3. Agentes (`$`)

O Mimo trabalha com agentes especializados. O `$` permite escolher o modo de atuação.

|Agente|Função|Quando usar|
|---|---|---|
|`$build`|Implementação e execução|Criar código, editar arquivos, corrigir bugs|
|`$plan`|Planejamento e análise|Entender projetos e criar estratégias|
|`$compose`|Orquestração de tarefas complexas|Projetos grandes com múltiplas etapas|

---

## `$build`

|Uso|Exemplo|
|---|---|
|Criar funcionalidade|`$build Crie um sistema de login`|
|Corrigir bug|`$build Corrija o erro no endpoint`|
|Alterar código|`$build Refatore essa classe`|

---

## `$plan`

|Uso|Exemplo|
|---|---|
|Analisar arquitetura|`$plan Explique a estrutura desse projeto`|
|Criar estratégia|`$plan Como implementaria essa feature?`|
|Revisar decisões|`$plan Avalie essa arquitetura`|

---

## `$compose`

|Uso|Exemplo|
|---|---|
|Projetos grandes|`$compose Crie uma aplicação completa`|
|Dividir tarefas|`$compose Desenvolva API, testes e documentação`|

---

# 4. Comandos internos (`/`)

|Comando|Função|Exemplo|
|---|---|---|
|`/help`|Lista comandos|`/help`|
|`/goal`|Define objetivo da sessão|`/goal Criar sistema completo`|
|`/dream`|Salva aprendizado da sessão|`/dream`|
|`/distill`|Cria uma habilidade baseada em padrões|`/distill`|
|`/compact`|Reduz contexto mantendo informações importantes|`/compact`|

---

# 5. Fluxo profissional de uso

|Etapa|Ação|Exemplo|
|---|---|---|
|1. Explorar|Adicionar contexto|`@src/ Analise este projeto`|
|2. Entender|Usar planejamento|`$plan`|
|3. Definir objetivo|Criar meta clara|`/goal Criar API REST`|
|4. Implementar|Executar mudanças|`$build`|
|5. Revisar|Avaliar resultado|`$plan Revise a implementação`|
|6. Consolidar|Registrar aprendizado|`/dream`|

---

# 6. Fluxos recomendados

## Projeto desconhecido

|Passo|Comando|
|---|---|
|Adicionar projeto|`@src/`|
|Analisar|`$plan Explique a arquitetura`|
|Criar plano|`/goal`|
|Implementar|`$build`|

---

## Criando uma funcionalidade

|Passo|Comando|
|---|---|
|Contexto|`@arquivos-relacionados`|
|Planejamento|`$plan`|
|Objetivo|`/goal Implementar funcionalidade X`|
|Código|`$build`|
|Revisão|`$plan Revise o código`|

---

## Debugging

|Passo|Comando|
|---|---|
|Adicionar código|`@arquivo-com-erro`|
|Explicar problema|`$plan Encontre a causa raiz`|
|Corrigir|`$build Corrija o problema`|
|Validar|`$build Execute testes`|

---

# 7. Boas práticas de prompting

|Evite|Prefira|
|---|---|
|"Faça um sistema"|"Crie uma API REST em FastAPI com autenticação JWT e testes"|
|"Corrija isso"|"Identifique a causa raiz e proponha soluções"|
|"Melhore o código"|"Refatore mantendo comportamento e explique mudanças"|
|Dar pouco contexto|Usar `@arquivos` relevantes|

---

# 8. Agentes personalizados (conceito)

|Tipo|Função|
|---|---|
|Agente de arquitetura|Define estrutura de sistemas|
|Agente de programação|Implementa código|
|Agente de revisão|Procura problemas|
|Agente de testes|Cria testes|
|Agente de documentação|Produz documentação|

Exemplos de especialistas que você poderia criar:

|Nome|Papel|
|---|---|
|`python-teacher`|Professor de Python|
|`code-reviewer`|Auditor de código|
|`architect`|Arquiteto de software|
|`security-reviewer`|Analista de segurança|
|`documentation-writer`|Documentador técnico|

---

# 9. Atalhos e controles do terminal

|Controle|Função|
|---|---|
|`↑`|Recuperar comando anterior|
|`↓`|Navegar no histórico|
|`Tab`|Autocompletar|
|`Ctrl + C`|Cancelar execução|
|`Ctrl + L`|Limpar terminal|

---

# 10. Fluxo ideal para o Projeto Prometheus

|Objetivo|Uso|
|---|---|
|Aprender código|`@arquivo → $plan → explique antes de alterar`|
|Construir projeto|`$compose → $build`|
|Criar arquitetura|`$plan`|
|Implementar feature|`$build`|
|Revisar qualidade|`$plan`|
|Documentar|`/dream` + documentação|

---

## Fluxo completo recomendado

```text
@projeto/

↓

$plan
"Analise a arquitetura e explique como tudo funciona"

↓

/goal
"Implementar nova funcionalidade X"

↓

$build
"Execute o plano aprovado"

↓

$plan
"Faça uma revisão crítica do código"

↓

/dream
"Registre os aprendizados do projeto"
```

---

### Regra de ouro do Mimo Code

|Situação|Use|
|---|---|
|Não conhece o projeto|`@ + $plan`|
|Precisa criar algo|`$build`|
|Projeto grande|`$compose`|
|Quer evitar erros|Planeje antes de executar|
|Quer evoluir como dev|Peça explicação antes da alteração|

Esse fluxo é especialmente interessante para o seu momento no **Projeto Prometheus**, porque transforma o Mimo em algo mais próximo de um **mentor técnico + desenvolvedor + revisor**, e não apenas um gerador de código.