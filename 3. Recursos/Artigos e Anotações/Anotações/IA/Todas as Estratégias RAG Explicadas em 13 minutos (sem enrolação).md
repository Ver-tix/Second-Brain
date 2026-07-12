---
tags:
  - inteligenciaartificial
  - RAG
published: https://youtu.be/tLMViADvSNE?si=kCGjgtmZqKmtFZj6
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

Pode ser mais caro, devido a termos um segundo modelo, especializado em filtragem de dados.

Exemplo de Código:
```Python
# Linhas 194-256 em rag_agent_advanced.py
async def search_with_reranking(cts: RunContext[None], query: str, limit: int=5) -> str:
	"""Two-stage with cross-encoder re-ranking"""
	initialize_reranker() #carrega um codificador cruzado/ms-marco-MiniLM-L-6-v2
	
	# Estágio 1: Retrieval rápido (extrai 20 cnadidatos)
	candidate_limit = min(limit * 4, 20)
	results = await vector_search(query, candidate_limit)
	
	# Estágio 2: reclassificação com codigicador cruzado
	pairs = [[query, row['content']] for row in results]
	scores = reranker.predict(pairs)
	
	# Filtrar por novos scores e retornar o top N
	reranked= sorted(zip(results, scores), key=lambda x:x[1], reverse=True)[:limit]
	return format_results(reranked)
```

✅ Significativamente mais preciso, mais conhecimento considerado sem sobrecarregar o LLM
❌ Levemente mais devagar que uma pesquisa pura, maior uso computacional

---
# 2. RAG Agêntico
Agente autonomicamente escolhe entre múltiplas ferramentas de retrieval, por exemplo:
- `search_knoledge_base()` - pesquisa semântica em chunks (pode incluir pesquisa híbrida: vetor denso + keywords esparças/BM25)
- `retrieve_full_document()` - puxadocumentos inteiros quando os chunks não são o suficiente



Exemplo de Código:
```Python
# Ferramenta 1: Pesquisa Semântica (Linhas 263-305)
@agent.tool
async def search_knowledge_base(query: str, limit: int = 5) -> str:
    """Standard semantic search over document chunks."""
    query_embedding = await embedder.embed_query(query)
    results = await db.match_chunks(query_embedding, limit)
    return format_results(results)

# Ferramenta 2: Retireval de documento inteiro (Linhas 308-354)
@agent.tool
async def retrieve_full_document(document_title: str) -> str:
    """Retrieve complete document when chunks lack context."""
    result = await db.query(
        "SELECT title, content FROM documents WHERE title ILIKE %s",
        f"%{document_title}%"
    )
    return f"**{result['title']}**\n\n{result['content']}"
```
✅ Flexível, adapta-se às necessidades do query automaticamente
❌ Mais complexo, comportamento menos previsível
# 3. Knowledge Graphs
Combina pesquisa vetorial com base de dados de grafos (como Neo4j/FalkorDB) para capturar relações entre entidades.


✅ Captura faltas em relações vetoriais. ótimo para dados inte
❌ Mais complexo, comportamento menos previsível