---
tipo:
  - sintese
dominio:
  - IA
Subdominio:
  - spec-driven_development
---
## 1. Visão Geral & O Paradigma do SDD
O Spec-Driven Development (SDD) é uma metodologia em que o arquivo de especificação técnica e funcional (o
"Spec") é mantido como a única fonte da verdade (Single Source of Truth) do software. Diferente da documentação tradicional, que se torna obsoleta após a escrita do código, no SDD o código-fonte torna-se um subproduto derivado e gerado diretamente a partir da especificação por meio de LLMs e agentes de execução.
>**O Papel do Desenvolvedor no SDD**
>O desenvolvedor transiciona do papel de codificador manual para o de **Arquiteto de Sistemas**, **Gestor de Regras** **de Negócio** e **Engenheiro de Validação**. O foco desloca-se da sintaxe para a corretude dos requisitos, limites operacionais e arquitetura.

## 2. Metodologias Detalhadas do SDD
### A. Especificação Comportamental e Orientada a Testes (BDD + TDD)
O SDD integra princípios de Behavior-Driven Development (BDD) e Test-Driven Development (TDD) dentro da própria escrita em Markdown. Toda funcionalidade deve ser declarada em termos de intenção comportamental e critérios de aceite inequívocos:
- **Formato BDD (Gherkin em Markdown):** Define cenários utilizando Dado que (Given), Quando (When), Então (Then) para explicitar estados anteriores, gatilhos e resultados esperados.
- **Contrato de Testes Estáticos:** A IA deve ser instruída a ler os critérios de aceite do Spec e gerar a suíte de testes automatizados (pytest, unittest ou jest) antes de implementar o código das funções.
### B. Metodologia de Modularização e Separação de Responsabilidades
Especificações monolíticas degradam a janela de contexto das IAs e geram desalinhamentos. O SDD exige a divisão da especificação em camadas semânticas distintas:
- **Architecture Spec (Arquitetura e Fluxo):** Mapeia a topologia do sistema, conexões de APIs, middlewares e
ecossistema.
- **Data Spec (Modelagem de Dados):** Mapeia estritamente Schemas de Banco de Dados (SQLite, PostgreSQL), tipos de dados, restrições e relacionamentos.
- **Behavior/Agent Spec (Lógica e Prompts):** Contém os comportamentos do agente, persona, tom de voz, regras de decisão e RAG.
- **Guardrail Spec (Segurança e Custos):** Restrições estritas de segurança, limites de orçamento de tokens e regras de recusa.

### C. Metodologia de Sincronização e Manutenção Iterativa (Spec-First Loop)
Se um bug é encontrado ou um requisito é alterado pelo cliente, é terminantemente proibido alterar o código
manualmente. O fluxo correto exige:
1. Atualização da regra no arquivo de Spec correspondente em Markdown.
2. Submissão do Spec alterado ao agente/IDE com instrução de refatoração pontual.
3. Execução da suíte de testes automatizada para garantir zero regressão.

## 3. Arquitetura de Pastas e Estrutura de Diretórios em Projetos SDD
Abaixo está a estrutura padronizada de arquivos para um projeto comercial de automação/agente de IA baseado em SDD. Esta organização garante que tanto o desenvolvedor quanto a IA saibam exatamente onde ler e escrever cada componente.

```
meu-projeto-sdd/
├── .sdd/                         # [SISTEMA SDD] Regras de comportamento para a IA executora
│   ├── system_instructions.md    # Diretrizes de como a IA deve ler os specs e gerar código
│   └── context_map.md            # Mapeamento de dependências entre as especificações
│
├── .specs/                       # [ESPECIFICAÇÕES] A Fonte Única da Verdade (Em Markdown)
│   ├── 01_architecture.md        # Visão geral da arquitetura, stack tecnológica e fluxos
│   ├── 02_database_schema.md     # Schemas SQL (SQLite/Postgres), tabelas, campos e chaves
│   ├── 03_agent_rules.md         # Persona, regras de negócio do agente, prompts e RAG
│   ├── 04_integrations.md        # Contratos de APIs externas (WhatsApp, CRMs, Google Calendar)
│   ├── 05_guardrails.md          # Limites de tokens, segurança, privacidade e restrições
│   └── 06_acceptance_tests.md    # Critérios de aceite em formato BDD / Checklists
│
├── src/                          # [CÓDIGO GERADO] Produzido 100% pela IA a partir de .specs/
│   ├── core/                     # Configurações gerais, variáveis de ambiente e log
│   ├── database/                 # Conexões, ORM e queries SQL (SQLite/Supabase)
│   ├── services/                 # Serviços de integração (WhatsApp API, Calendar, etc.)
│   ├── agents/                   # Lógica dos Agentes de IA e orquestração de Prompts
│   └── main.py                   # Ponto de entrada da aplicação (ex: FastAPI)
│
├── tests/                        # [SUÍTE DE TESTES] Gerados a partir de 06_acceptance_tests.md
│   ├── test_agent_rules.py       # Testes de comportamento e restrições do agente
│   └── test_integrations.py      # Testes de integração e rotas de API
│
├── data/                         # [DADOS E RAG] Persistência local e documentos de contexto
│   ├── database.sqlite           # Banco SQLite local
│   └── rag_docs/                 # Documentos Markdown/PDF consumidos pelo agente
│
├── .env.example                  # Modelo de variáveis de ambiente
└── README.md                     # Documentação executiva para humanos
```

## 4. Guia Passo a Passo: Execução e Ciclo de Vida do SDD
```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│ 1. ESPECIFICAÇÃO       │ ───► │ 2. INSTRUÇÃO DA IA     │ ───► │ 3. COMPILAÇÃO          │
│ Escrever os arquivos   │      │ Ler diretrizes de      │      │ Gerar testes e código  │
│ dentro de `.specs/`    │      │ `.sdd/` e `.specs/`    │      │ em `tests/` e `src/`   │ └────────────────────────┘      └────────────────────────┘      └────────────────────────┘
           ▲                                                                │
           │                       4. TESTES E AJUSTES                      │
           └────────────────── Alterar Spec se houver erro ─────────────────┘
```
### Passo 1 — Definição dos Specs (`.specs/`): 

- Você e o cliente definem os objetivos do projeto. Você escreve os arquivos Markdown dentro da pasta `.specs/` delimitando arquitetura, regras de banco, regras de negócio do agente e restrições de segurança.

### Passo 2 — Configuração das Instruções do Desenvolvedor IA (`.sdd/system_instructions.md`): 

- Crie as regras de execução para o seu assistente de IA/IDE. Exemplo:

```markdown
\# Instruções para o Agente Desenvolvedor
- Leia todos os arquivos na pasta `.specs/` antes de gerar qualquer código.
- O código em `src/` deve corresponder estritamente às especificações de `.specs/`.
- Se houver divergência entre o comando do usuário e o Spec, o Spec é a prioridade absoluta.
```

### Passo 3 — Geração do Código e Testes: 
- Submeta a ordem para a IA:
> _"Leia a especificação em `.specs/` e crie a suíte de testes em `tests/`. Em seguida, implemente todo o código da aplicação no diretório `src/` até que 100% dos testes passem_

### Passo 4 — Manutenção por Refatoração de Spec: 
- Caso um teste falhe ou o cliente solicite mudanças nas regras de atendimento, você edita o arquivo `03_agent_rules.md` e solicita que a IA reescreva as rotas afetadas.

## 5. Tabela Comparativa de Abordagens

| **Critério**            | **Desenvolvimento Tradicional**  | **Copilot / Prompts Soltos**  | **Spec-Driven Development (SDD)**              |
| ----------------------- | -------------------------------- | ----------------------------- | ---------------------------------------------- |
| **Fonte da Verdade**    | Código-fonte escrito à mão       | Cabeça do desenvolvedor       | **Arquivos `.specs/*.md`**                     |
| **Papel do Humano**     | Escrever linhas de código        | Aceitar/Rejeitar sugestões    | **Arquiteto de Regras e Validador**            |
| **Risco de Alucinação** | N/A                              | Alto (código sem contexto)    | **Muito Baixo (delimitado por Guardrails)**    |
| **Manutenibilidade**    | Complexa / Documentação defasada | Código caótico / Copia e Cola | **Excelente (Altera Spec -> Atualiza Código)** |
