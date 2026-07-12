---
tags:
  - inteligenciaartificial
  - RAG
  - projeto
published: https://youtu.be/tcqEUSNCn8I?si=cPFArYzHdZLYn8qI
---
# Overview do Projeto:
1. Pra começar, vamos precisar de uma fonte de dados. Pode ser um PDF ou uma coleção de arquivos de texto ou markdown (bom... nosso Obsidian tem pra dar e vender arquivos);
2. Uma vez com um arquivo em mãos, precisaremos fazer o upload dele, e dividi-lo em diferentes pedaços (chunks) de texto

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