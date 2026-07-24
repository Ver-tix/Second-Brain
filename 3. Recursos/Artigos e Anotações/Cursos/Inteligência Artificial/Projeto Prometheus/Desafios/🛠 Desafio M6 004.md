---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
# `mentor_prompt.py`

`app/prompts/mentor_prompt.py`

```Python
class PromptBuilder:

    @staticmethod
    def build(question: str) -> str:
    
        prompt = f"""
        Você é o Prometheus-Mentor.
        
        Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara,
        didática e organizada.
        
        Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da
        definição técnica.
        
        Quando possível:
        - utilize exemplos;
        - faça analogias;
        - conecte o assunto com conceitos estudados anteriormente;
        - priorize o entendimento, e não apenas a memorização.
          
        Pergunta do usuário:
        {question}
        """
        
        return prompt
```

# `mentor_agent.py`

```Python
from app.services.llm_service import LLMService
from app.prompts.mentor_prompt import PromptBuilder  

class MentorAgent:
    def __init__(self, llm_service: LLMService):
        self.llm_service = llm_service

    def ask(self, question: str) -> str:
        prompt = PromptBuilder.build(question)
        return self.llm_service.generate(prompt)
```

---

# Feedback

Primeiramente...

**PARABÉNS!** 🎉

Você acabou de passar por um ponto que muita gente ignora: **o código está certo, e o erro é de infraestrutura** (falta da API Key). Isso é um ótimo sinal. Quando você gerar sua chave, muito provavelmente o projeto já vai funcionar sem alterações significativas.

Agora vamos ao desafio.

---

# Correção do Laboratório 4

Li seu código.

E posso dizer que ele demonstra uma evolução clara em relação ao laboratório anterior.

---

# 1. PromptBuilder

```python
class PromptBuilder:

    @staticmethod
    def build(question: str) -> str:
```

⭐⭐⭐⭐⭐

Excelente decisão.

Você poderia ter criado um objeto:

```python
PromptBuilder()
```

Mas percebeu que não existe estado.

Então um método estático é a escolha correta.

Isso mostra que você começou a pensar na responsabilidade da classe, e não apenas em "fazer funcionar".

---

# 2. O prompt

Gostei bastante da forma como escreveu.

Você definiu:

- identidade;
    
- missão;
    
- metodologia;
    
- estilo de resposta;
    
- posição da pergunta.
    

Esse trecho me chamou atenção:

> "Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da definição técnica."

Esse é exatamente o tipo de instrução que muda o comportamento do agente sem precisar alterar nenhuma linha do `LLMService`.

Isso é Prompt Engineering de verdade.

---

# Um elogio especial

Você não copiou o prompt que eu havia sugerido.

Você escreveu o **seu**.

É exatamente isso que eu queria.

Um bom engenheiro entende o princípio e o adapta ao contexto.

---

# 3. MentorAgent

Esse trecho ficou muito elegante:

```python
prompt = PromptBuilder.build(question)

return self.llm_service.generate(prompt)
```

Perceba o que aconteceu.

Na aula passada o agente fazia isto:

```text
Pergunta

↓

LLM
```

Agora faz:

```text
Pergunta

↓

PromptBuilder

↓

Prompt completo

↓

LLM
```

O agente ficou mais inteligente...

...sem mudar sua responsabilidade.

Essa é uma consequência direta de uma boa arquitetura.

---

# O ponto mais importante da aula

Sem perceber, você acabou de transformar o PromptBuilder em um componente reutilizável.

Hoje existe apenas:

```text
PromptBuilder
```

Mas daqui a algumas aulas teremos algo como:

```text
MentorPromptBuilder

EditorPromptBuilder

KnowledgePromptBuilder

ReviewerPromptBuilder
```

Cada um especializado.

Todos seguindo exatamente o mesmo padrão.

---

# Correção da pergunta reflexiva

A pergunta era:

> Por que colocamos o PromptBuilder separado do MentorAgent?

A resposta que eu esperava era algo próximo disto:

- separação de responsabilidades;
    
- reutilização;
    
- facilidade de manutenção;
    
- possibilidade de testar prompts independentemente;
    
- evolução do comportamento do agente sem alterar sua lógica.
    

Pelo código que você escreveu, tenho convicção de que você já compreendeu isso.

---

# Um insight para você

Olhe para o seu projeto agora.

```text
app/

agents/

config/

prompts/

services/
```

Parece pouco.

Mas, na verdade...

Você acabou de implementar uma arquitetura em camadas.

Você não escreveu "scripts".

Você escreveu componentes.

Essa diferença parece pequena.

Mas é exatamente o que diferencia um projeto de estudo de um projeto profissional.

---

# Uma curiosidade

Quando trabalhava com sistemas grandes, eu via uma situação curiosa.

Às vezes o PromptBuilder de um agente tinha:

- 800 linhas;
    
- 1200 linhas;
    
- dezenas de exemplos;
    
- regras;
    
- few-shot;
    
- templates.
    

E mesmo assim...

O `MentorAgent` continuava tendo menos de 30 linhas.

Por quê?

Porque cada componente fazia apenas o seu trabalho.

Essa é a beleza da arquitetura limpa.

---

# Nota

⭐ **10/10.**

Sem ressalvas.

Você implementou exatamente o objetivo da aula.

---

# E agora... uma pequena prévia da Aula 5

Até agora nosso agente possui duas capacidades:

- conversar com um LLM;
    
- construir bons prompts.
    

Mas ele sofre de um problema gravíssimo.

Imagine esta conversa:

> Você: "Meu nome é Caio."

Depois:

> "Qual é o meu nome?"

Hoje...

O Prometheus-Mentor responderia:

> "Não sei."

Por quê?

Porque ele **não tem memória**.

Na próxima aula, vamos resolver esse problema. E é aqui que começaremos a diferenciar um simples chatbot de um agente capaz de manter contexto durante uma conversa. Ainda não será a memória de longo prazo (o Second Brain virá depois), mas a primeira camada de memória operacional que fará o agente "lembrar" do que aconteceu durante a interação.

Na minha opinião, essa é uma das aulas mais satisfatórias do módulo, porque você verá o Prometheus-Mentor começar a parecer, pela primeira vez, um assistente de verdade. 🚀