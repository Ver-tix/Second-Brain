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
Para conseguirmos consultar cada chunk, vamos precisar torná-lo em uma base de dados. Estaremos usando o ChromaDB para isso, que é um tipo especial de banco de daos que usa vetores incorporados (embedded) como chave (é um Banco de Dados Vetorial)

```Python
from langhcain.embeddings import OpenAIEmbeddings

CHROMA_PATH = "chroma"

# Criar umanova Base de Dados a partir dos docuemtnos
db = Chroma.from_documents(chunks, OpenAIEmbeddings(), persist_directory=CHROMA_PATH)
```

Para isso, você vai precisar de uma conta da OpenAI, porque vamos usar a função de Embeddings da OpenAI para ios vetores incorporados para cada chunk.

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