---
tags:
  - inteligenciaartificial
  - RAG
  - projeto
published: https://youtu.be/tcqEUSNCn8I?si=cPFArYzHdZLYn8qI
---
# Overview do Projeto:
![[Workflow do RAG.canvas]]

---

#### Carregando Arquivos em Python 
Para carregar dados markdown do seu folder para o Python, siga o seguinte:
```Python
from langchain.document_loaders import DirectoryLoader

DATA_PATH = "data/books" #substitua pelo endereço do folder

def load_documents():
	loader = DirectoryLoader(DATA_PATH, glob="*.md") # "*" signifca que vai carregar todos os arquivos que tiverem .md
	documents = loader.load()
	return documents
```

---
#### Chunking e o Problema dos Textos Muito Longos
Outro problema que encontramos é que não é suficiente carregar cada arquivo markdown em um documento. **Também precisamos dividir cada documento, caso eles sejam muito longos**. O resultado desejado é que, a cada consulta, cada chunk seja mais focado e mais relevante.

Para isso, podemos usar um divisor de texto recursivo por carateres:

```Python
from langchain.text_splitter import RecursiveCharacterTextSplitter
text_splitter = RecursiveCharacterTextSplitter(
	chunk_size=1000, #c ada chunk terá cerca de 1000 caracteres
	chunk_overlap=500, # cada chunk terá uma sobreposição de 500 caracteres
	length_function=len,
	add_start_index=True
)

chunks = text_splitter.split_documents(documents) # documentes é a variável retornada da função load_documents()
```

---
#### Transformando em uma Base de Dados
Para conseguirmos consultar cada chunk, vamos precisar torná-lo em uma base de dados. Estaremos usando o ChromaDB para isso, que é um tipo especial de banco de dados que usa vetores incorporados (embedded) como chave (é um Banco de Dados Vetorial)

```Python
from langhcain.embeddings import OpenAIEmbeddings

CHROMA_PATH = "chroma" 

# Criar umanova Base de Dados a partir dos docuemtnos
db = Chroma.from_documents(chunks, OpenAIEmbeddings(), persist_directory=CHROMA_PATH)
```

Para isso, você vai precisar de uma conta da OpenAI, porque vamos usar a função de Embeddings da OpenAI para os vetores incorporados para cada chunk.

Vamos configurar um chroma path como diretório persistente, para que quando criarmos esse db, tenhamos várias pastas no seu disco que possam usar para carregar os dados posteriormente. É útil porque podemos colocar esse banco de dado em uma função lambda ou na nuvem em algum lugar. 
(Já fazemos isso via github)

---
#### Agora, Antes de Criar o Data Base ou antes de Salvá-lo
Limpe a base de dados primeiro
```Python
if os.path.exists(CHROMA_PATH):
	shutil.rmtree(CHROMA_PATH)
```

O banco de Dados deve ser salvo automaticamente de criá-lo, mas você também pode forçá-lo a salvar usando esse método `persist` assim:
```Python
db.persist()
print(f"Saved {len(chunks)} chunks to {CHORMA_PATH}.")
```
Os dados estão prontos para uso, mas, aqui apenas fizemos as etapas de preparação de dados e de chunking. Vamos agora para a etapa de Embedding

---
# Embedding
#### Precisamos gerar um Vetor de uma Palavra
Para isso, precisaremos de um LLM, como o OpenAI
```Python
embedding_function = OpenAIEmbeddings()
vector = embedding_function.embed_query("apple") #ele vai vetorizar a palavra apple

print(vector) # por exepmlo: [0.0077788466226914, ...]
print(len(vector)) # 1536, por exemplo
```
Podemos ver que as palavras se tornam uma longa lista de números. 

---
#### Mapa semântico
A lista em sim não é interessante. O que é interessante é a distância entre dois vetores em si
```Python
from langchain.evaluation import load_evaluator

evaluator = load_evaluator("pairwise_embedding_distance")
```

Executando um exemplo:
```Python
x = evaluator.evaluate_string_pairs(prediction="apple", prediction_b="orange")
```
Executando isso, o resultado é uma pontuação de 0,13:
```Python
# "apple" vs "orange"
{'score': 0.13493062128535716}
```

Bom, não sabemos se isso é bom ou não, porque não sabemos onde 0,13 se encaixa na escala de outras palavras. Então, tentemos outras palavras apenas para testar:
```Python
# "apple" vs "beach"
{'score': 0.2016104930298288}
```
Significa que "beach" está mais distante de "orange" que "apple", suponho que porque um é uma fruta, o outro é um ambiente. Agora se eu testar "apple" consigo mesma:
```Python
# "apple" vs "apple"
{'score': 2.57259157643297e-06}
```
Tecnicamente deveria ter distância zero, já que "apple" com ela mesma estaria no mesmo lugar. Mas esse número é perto o suficiente $$\text{distância }\approx 2.5 \cdot 10^{-6}$$E agora, se compararmos "apple" com "iphone"?
```Python
# "apple" vs "iphone"
{'score':0.09673148365422168}
```

---

# Consultando (Querying) Dados Relevantes
Nosso objetivo é encontrar os chunks em nosso banco de dados que provavelmente contêm a resposta para a pergunta que queremos fazer. Portanto, para fazer isso, precisaremos do banco de dados que criamos mais cedo, e da mesma função de embedding.

Nossa Meta agora:
![[Workflow RAG 2.canvas]]
Nosso objetivo agora é pegar uma consulta, como do quadro à esquerda, e transformá-la num embedding usando a mesma função, e depois pesquisar em nosso banco de dados e encontrar talvez cinco chunks de informações que são mais próximas em termos de distância de embeddings da nossa consulta.

Para carregar a base de dados do Chroma que criamos, primeiro precisamos do caminho, que já temos de mais cedo, e precisaremos de uma função de embedding, que deve ser a mesma que usamos para criar o banco de dados:

```Python
# Prepare o Data Base
embedding_function = OpenAIEmbeddings() # isso deve carregar seu banco de daos a partir desse caminho. Se não, verifique se o caminho existe ou volte para o capítulo anterior e execute o script para crair o banco de daos novamente.
db = Chroma(persist_directory=CHROMA_PATH, embedding_function=embedding_function)
```

Uma vez que o banco de dados é carregado, podemos então procurar o chunk que melhor corresponda nossa consulta usando esse método. Precisamos passar o texto da nossa consulta (aqui chamado de `query_text`) como argumento e especificar o número de resultados que queremos recuperar. No exemplo a seguir, queremos recuperar as três melhores correspondências para nossa consulta.
```Python
# Search the DB.
results = db.similarity_search_with_relevance_scores(query_text, k=3)
```
Os resultados da pesquisa serão uma lista de tuplas, onde cada tupla contém um documento e sua pontuação de relevância.
```Python
# Tipo de Retorno da pesquisa 
List[Tuple[Document, float]]
```

---
#### Antes de Processar os Resultados...
Podemos adicionar algumas verificações. Por exemplo, se não houver correspondências ou se a pontuação relevante do primeiro resultado estiver abaixo de um certo limite (no exemplo abaixo, `0.7`), podemos retornar mais cedo.

Isso ajudará a garantir que realmente encontramos informações boas e relevantes primeiro antes de passar para a próxima etapa do processo
```Python
if len(results) == 0 or results [0][1] <0.7:
	print(f"Unable to find matching results.")
	return
```

---
# Crie a Resposta
Agora que encontramos os chunks relevantes para nossa consulta, podemos alimentar isso no OpenAI para criar uma resposta de alta qualidade usando esses dados como nossa fonte.

#### Prompt Template
Primeiro precisaremos de um modelo de prompt para criar um prompt
```Python
PROMPT_TEMPLATE = """
Answer the question based onl on the following context:

{context}

---

Answer the question based on the above context: {query}
"""
```
- `context`: os pedaços de informações que obtivemos do banco de dados,
- `query`: a consulta propriamente dita
#### Em Seguida...
Eis o código para usar esses dados para criar o prompt, formatando o modelo com nossas chaves.
```Python
context_text = "\n\n---\n\n".join([doc.page_content for doc, _score in results])
prompt template = ChatPromptTemplate.from_template(PROMPT_TEMPLATE)
prompt = prompt_template.format(context=context_text, question=query_text)
```

#### A Parte Mais Fácil
Simplesmente chame o modelo LLM de sua escolha com esss prompt. Aqui estamos usando o chatOpenAI, e então teremos nossa resposta.

```Python
modelo = ChatOpenAI()
response_text=modelo.predict(prompt)
```

#### Finalmente...
Se você quiser fornecer referências ao seu

```Python
sources = [doc.metadata.get("source", None) for doc, _score in results]
formatted_resopnde
```

---
```Python
pip install langchain openao faiss-cpu tiktoken
```

```Python
from operator import itemgetter

from langhcain.prompts import ChatPromptTemplate
from langhcain.chat_models import ChatOpenAI
from langhcain.embeddings import OpenAIEmbeddings
from langhcain.schema.output_parser import StrOutPutParser
from langhcain.schema.runnable import RunnablePassthrough, RunnableLambda
from langhcain.vectorstore import FAISS
```

```Python
vectorstore = FAISS.from_texts(
	["harrison worked at kensho"], embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()

template = """Answer he question based only on the following context:
{context}

Question: {question}
"""

Prompt = ChatPromptTemplate.from_template(template)

model = ChatOpenAI()
```

```Python

```

```Python

```

```Python

```

```Python

```

```Python

```