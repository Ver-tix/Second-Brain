---
tags:
  - inteligenciaartificial
  - RAG
---
# RAG Explicado em 1 Minuto
## 1. Preparação de Dados - Fase do Processo de Indexação
Pegamos nossos documentos (Raw Data), o colocamos no processo de Chunking (divisão do conteúdo por assuntos), e depois passam pelo processo de Embedding para pormos num Banco de Dados Vetorial (BDV) ou num Knowledge Graph

## 2. Retrieval Augmented Generation - Processo de Query
### Retrieval
Aqui pegamos uma pergunta de um usuário ("quais os itens de ação da reunião do dia 31/10?"), passamos ela pelo embedding, e a procuramos na nossa Base de Dados Vetorial por Chunks similares.

### Augmentation
Então, passamos para o LLM, assim ele consegue aproveitar esse matching de pergunta do usuário com chunks, como um contexto adicional para a fase de Augmentation

### Generation
E com esse contexto adicional, ele gera uma resposta muito mais contextualizada

## Mas, Há Tantas Formas Diferentes de 
- Realizar o Processo de Preparação de Dados
- Estratégias de Chunking
- Diferentes formas de pesquisarmos nas Bases de Dados Vetoriais,incluindo formatar os dados como um knowledge graph...

Veremos as formas aqui

# Ferramentas Complementares
[Material complementar no GitHub](https://github.com/coleam00/ottomator-agents/tree/main/all-rag-strategies)
# #1 -  Estratégia de Reclassificação
Nessa estratégia, temos um Retrieval em dois passos:
1. Puxamos um grande número de chunks do nosso banco de dados vetorial
2. Então, usamos um modelo de reclassificação especializado, geralmente um codificador, para encontrar aqueles que são realmente relevantes para nosso Query, E então, retornar apenas alguns dados

Então, o LLM só recebe alguns dos chunks, mas somente o mais relevante. E isso é importante, especialmente em casos em que o BDV retorna muitos chunks, podendo sobrecarregar o modelo de IA. Ou seja, esse modelo se foca em filtrar o que for mais relevante, afim de reduzir ainda mais a carga inferencial.