---
tags:
  - inteligenciaartificial
  - RAG
---
# RAG Explicado em 1 Minuto
## 1. Processo de Indexação
### Preparação de dados: 
Anotação Contextual -> Chunking -> Embedding -> Vector Data Base (ou para um knoledge graph)

## 2. Processo de Query
### Retrieval Augmented Generation
Aqui se divide em uma bifurcação:
--- start-multi-column: ID_r3p4
```column-settings
Number of Columns: 2
Largest Column: standard
```

Embedding -> "Quais são os itens de ação da reunião da data 31/10?"

--- column-break ---

Dados Relevantes -> LLM -> Resposta -> "os itens de ação da reunião são..."

--- end-multi-column

# Recursos para Cada Estratégia