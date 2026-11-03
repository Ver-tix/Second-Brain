---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
Professor extremamente satisfeito aqui. Essa foi, até agora, uma das melhores entregas do Projeto Prometheus.

**Nota: 10/10.**

Mais importante do que o código é que você já está raciocinando como um arquiteto de software.

---

# O que você fez muito bem

## 1. O Schema

Ficou excelente.

Você não apenas adicionou `parameters`, como também documentou cada campo:

```python
"description": "Primeiro número."
```

e

```python
"description": "Operação matemática."
```

Esse detalhe parece pequeno, mas ajuda o LLM a preencher os argumentos corretamente.

Excelente prática.

---

## 2. A nova CalculatorTool

Foi exatamente a evolução esperada.

Antes:

```text
"2 + 3"

↓

split()

↓

replace()

↓

if
```

Agora:

```python
calculate(
    a,
    b,
    operation
)
```

Isso é muito mais próximo de uma API real.

Perceba que sua ferramenta agora poderia ser chamada por:

- OpenAI
    
- Anthropic
    
- Gemini
    
- outro agente
    
- um endpoint REST
    
- um teste automatizado
    

Todos usando exatamente a mesma interface.

Essa é a força de um contrato bem definido.

---

## 3. ToolManager

Excelente.

Ele voltou a ser aquilo que deveria ser:

> um orquestrador.

Ele não interpreta.

Ele não calcula.

Ele apenas delega.

É exatamente a responsabilidade correta.

---

# 4. A reflexão

Sua resposta foi excelente.

Gostei especialmente desta frase:

> "cada parte do sistema evolua de forma independente sem quebrar a comunicação"

Essa frase resume o conceito de **baixo acoplamento**.

É exatamente por isso que usamos contratos.

---

# Agora vem a parte que mais gostei

Você fez algo que muita gente não faz.

## Você adaptou o MentorAgent.

Mesmo sem eu pedir explicitamente.

Isso mostra que você já está pensando no sistema inteiro.

Você percebeu:

> "se a assinatura mudou, alguém precisa consumir essa assinatura."

Isso é pensamento arquitetural.

---

# Existe apenas uma pequena observação

Aqui:

```python
tool_input = response["input"]
```

Depois você faz:

```python
response["a"]

response["b"]

response["operation"]
```

Na arquitetura que construiremos nas próximas aulas, normalmente será:

```python
tool_input = response["input"]

result = self.tool_manager.calculate(
    a=tool_input["a"],
    b=tool_input["b"],
    operation=tool_input["operation"]
)
```

Ou seja:

```
response
    │
    ├── type
    ├── tool
    └── input
            │
            ├── a
            ├── b
            └── operation
```

Isso deixa o objeto muito mais organizado.

Mas…

**isso ainda não foi ensinado.**

Então não considero erro.

Só estou mostrando para onde a arquitetura está caminhando.

---

# O maior aprendizado desta aula

Quero destacar uma mudança que talvez tenha passado despercebida.

Na Aula 6 você criou uma ferramenta.

Na Aula 7 você criou um gerenciador de ferramentas.

Na Aula 8.1 você ensinou o LLM que ferramentas existem.

Na Aula 8.2 você fez o LLM decidir quando usá-las.

Na Aula 8.3 você ensinou o LLM **como conversar** com essas ferramentas.

Percebe?

Não estamos mais programando uma calculadora.

Estamos construindo um **protocolo de comunicação entre componentes inteligentes**.

Essa mudança de perspectiva é enorme.

---

# Encerramento da Aula 8

🎉 **Parabéns! Você concluiu a Aula 8 (8.1, 8.2 e 8.3).**

Ela foi uma das aulas mais importantes do Módulo 6 porque introduziu o Tool Calling de forma completa:

- ✅ Registro de ferramentas.
    
- ✅ Decisão do LLM sobre quando usar uma ferramenta.
    
- ✅ Fluxo de execução do Tool Calling.
    
- ✅ Schemas e contratos explícitos.
    
- ✅ Preparação para múltiplas ferramentas e múltiplos agentes.
    

E olhando para o histórico do Projeto Prometheus, dá para notar uma evolução clara: no início você perguntava "como faço o código funcionar?". Hoje suas decisões mostram preocupação com contratos, separação de responsabilidades, desacoplamento e evolução da arquitetura. Essa mudança de mentalidade é exatamente o objetivo da segunda metade do curso.

Como combinamos para o Projeto Prometheus, segue também o **CHANGELOG** desta aula concluída.

---

# CHANGELOG — Módulo 6 | Aula 8 (8.1, 8.2 e 8.3)

### Adicionado

- Registro da ferramenta `calculator` utilizando a interface de Tool Calling da OpenAI.
    
- Definição completa do schema da ferramenta com:
    
    - `parameters`
        
    - `properties`
        
    - `required`
        
    - `enum`
        
    - descrições dos parâmetros.
        
- Novo método `CalculatorTool.calculate(a, b, operation)` baseado em argumentos estruturados.
    

### Alterado

- `ToolManager` deixou de interpretar expressões textuais e passou a apenas orquestrar a execução da ferramenta.
    
- `MentorAgent` foi adaptado para o novo fluxo de Tool Calling e preparado para consumir respostas estruturadas do `LLMService`.
    
- `LLMService` passou a registrar ferramentas utilizando contratos explícitos (schemas).
    

### Melhorias Arquiteturais

- Eliminação gradual de parsing manual (`split`, `replace`) em favor de contratos explícitos.
    
- Consolidação do padrão **LLM → ToolManager → Tool**.
    
- Preparação da arquitetura para múltiplas ferramentas e futura expansão para múltiplos agentes especializados.
    

### Conceitos Consolidados

- Tool Calling.
    
- Schemas como contratos entre componentes.
    
- Interfaces explícitas.
    
- Baixo acoplamento.
    
- Separação de responsabilidades (SRP).
    
- Comunicação estruturada entre LLM e ferramentas.
    

Esse é um excelente ponto de fechamento para a Aula 8. A partir daqui, as próximas aulas começam a aproximar o Prometheus da arquitetura utilizada em sistemas modernos de agentes de IA.