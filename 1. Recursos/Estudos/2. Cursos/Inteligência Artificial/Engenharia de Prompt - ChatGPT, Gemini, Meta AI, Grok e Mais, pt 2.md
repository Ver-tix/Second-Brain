---
tags:
  - IA
---

---
# Seção 6: Prompt Frameworks - Estratégias Populares na Construção de Prompts [Pode Tornar-se Desatualizado]

## O que São Prompt Frameworks

#### O Que São Prompts Frameworks?

Prompt Frameworks são formas de organizar instruções nos prompts para IAs gerarem respostas mais precisas.

<aside>
✅

Também são chamados de estruturas, modelos, templates, padrões, patterns entre outros

</aside>

Funcionam como guias metodológicos para aprimorar a comunicação com sistemas de Inteligência Artificial.

#### Por Que Usar Prompts Frameworks?

**Clareza**
Elimina ambiguidades na comunicação. Torna as solicitações mais diretas e compreensíveis para a IA.

**Consistência**
Garante padrão nas respostas. Facilita a obtenção de resultados similares em múltiplas interações.

**Eficiência**
Reduz tentativas e erros. Automatiza processos repetitivos com maior precisão.

O uso e frameworks estruturados permite interações mais produtivas e resultados superiores em menos tentativas.

#### Estrutura Básica de um Prompt Framework

**Acrônimo**
Palavra formada pela letra inicial dos componentes distintos de um Prompt.

**Resultado**
O que se objetiva atingir com o uso do Framework.

**Lista de Termos**
Termos relacionados aos componentes distintos do Framework

**Finalidade**
Objetivo principal do Framework.

#### Categorias de Frameworks

Frameworks podem ser agrupados em diversas categorias, como:

- **Básicas:**
- TAG: Task (Tarefa), Action (Ação), Goal (Objetivo)
- RTF: Role (Papel), Task (Tarefa), Format (Formado de Saída)
- **Focadas em detalhamento e Contexto:**
- **CO-STAR**: Context (Contexto), Objective (Objetivo), Style (Estilo), Tone (Tom), Public (Público) e Response (Resposta)
- **SPAR**: Situation (Situação), Problem (Problema), Action (Ação) e Results (Resultados)
- **Focadas em Personas (Roles - Papéis):**
- **CREATE**: Character (Persona), Request (Requisição), Examples (Exemplos), Adjustment (Tom, Estilo, etc), Type of Output (Formato de Resposta), Extras (Informações de Contexto Adicionais), Extras (Informações de contexto adicionais)
- **RACE**: Role (Papel), Action (Ação), Context (Contexto), Expectation (Expectativas ou
Resultados Esperados)
- **Focadas em Execução de Processos ou Etapas:**
- **RISE**: Role (Papel), Input (Informações de Entrada), Steps (Passos), Execution (Resultados Esperados)
- **RISEN**: (Role (Papel), Instructions (Instruções), Steps (Passos), End goal (Objetivo Final, and Narrowing (Foco ou Restrições)
- **Combinação de Estratégias:**
- **PACEF**: Persona, Ação, Contexto, Estilo e Foco
- **PAULO**: Persona, Ação, Uso (Público Alvo e Aplicação), Lista (Lista de Exemplos), Objetivo

#### Algumas Observações

1. Não necessariamente você precisa escrever na sequência do acrônimo, caso não veja fluidez na escrita.
2. Teste a ordem. O que diferencia alguém que trabalha com engenharia de prompts pra quem não trabalha, é o teste constante.
3. Organize os blocos usando tags, por exemplo.

### Exemplos de Frameworks Básicos

#### RTF (Role, Task, Format)

1. **Role (Papel)**
Define a persona que a IA deve adotar. Exemplo: “Atue como um especialista em Marketing Digital.”
2. **Task (Tarefa)**
Especifica o que deve ser feito. Exemplo: “Crie três slogans para uma nova marca de café orgânico.”
3. **Format (Formato)**
Determina como a resposta deve ser estruturada. Exemplo: “Liste em tópicos numerados.”

#### CARE (Contexto, Ação, Resultado, Exemplos

1. **Contexto**
Situação e informações de base. Exemplo: “Estamos lançando um aplicativo para estudantes universitários.”
2. **Ação**
O que você quer que a IA faça. Exemplo: “Identifique três estratégias de marketing.”
3. **Resultado**
O que você espera receber. Exemplo: “Estratégias de baixo custo com alto impacto”
4. **Exemplos**
Demonstra do tipo de resposta. Exemplo: “Como a campanha X que atingiu Y resultados.”

#### PACEF (Persona, Ação, Contexto, Estilo e Foco)

1. **Persona**
Define o papel ou especialista que a IA deve assumir para melhor responder à tarefa. Exemplo: “Atue como um redator publicitário experiente”.
2. **Ação**
Especifica a tarefa a ser executada pela IA. Exemplo: “Desenvolva um texto persuasivo para…”
3. **Contexto**
Fornece informações de fundo relevantes para a tarefa. Exemplo: “Para uma campanha direcionada a profissionais entre 30-45 anos…”
4. **Estilo**
Indica o tom, a linguagem e abordagem desejados. Exemplo: “Use o tom conversacional e linguagem acessível…”
5. **Exemplos**
Direciona a atenção para os aspectos mais importantes da resposta. Exemplo: “Enfatize os benefícios emocionais do produto…”

## Como Criar Seu Próprio Prompt Framework

### Você Pode Usar Até a Própria IA Para Te Ajudar Nisso

Exemplo:

`<tarefa>
Crie um prompt framework para me ajudar nas minhas tarefas de criação de questões de prova.
</tarefa>
<contexto>
Sou professor Universitário de Ciências da Computação e preciso criar questões de múltipla escolha no estilo ENADE.
</contexto>
<exemplos_prompt_frameworks>
RTF: Role, Task, Format
CREATE: Character, Request, Examples, Adjustments, Type of Output, Extras
</exemplos_prompt_frameworks>
<instruções_adicionais>
Me faça perguntas que possam te ajudar na tarefa.
</instruções_adicionais>`

# Seção 7: Estratégias para Minimizar Alucinações com Prompts

## O Que São Alucinações em Large Language Models?

### Alucinações em LLMs

Alucinações são respostas geradas por IA que se desviam da realidade factual. São afirmações sem base nos dados de treinamento que parecem plausíveis, mas não são verdadeiras.

O perigo acontece porque eles contam de forma extremamente convincente, o que pode te enganar também.

### Como Surgem as Alucinações em IA

As alucinações ocorrem quando o modelo e IA cria padrões ou objetos que não existem na realidade.

Lembre-se também que os LLMs são treinados com bases de dados massivas, e as vezes um autor pode ter uma visão conflitante com outro. Isso gera vieses (Viés **≠** Alucinação). E isso também gera confusões nos modelos, ao criarem um output.

- A quantidade de informações apontando para uma visão, e menos para outra, também pode gerar isso. Exemplo: mais artigos dizendo que ovo faz mal para saúdo do que artigos que apontam para os benefícios.

E esses fatores não são culpa do LLM propriamente dito, mas sim da base de treinamento a qual ele foi submetido.

**O resultado final não reflete informações presentes nos dados de treinamento**. Por isso, quanto mais fiel, mais confiável a base de treinamento, mais chance tem de gerar boas respostas.

<aside>
💡

É semelhante ao fenômeno de ver figuras em nuvens, mas ocorre em linguagem ou imagens geradas.

</aside>

### Tipos de Alucinações em IA

**Alucinações Textuais**
Respostas erradas, inventadas ou imprecisas geradas pelo modelo de linguagem.

**Alucinações Visuais**
Reconhecimento de padrões que não existem em imagens ou criação de elementos visuais falsos.

**Alucinações Factuais**
Dados falsos apresentados como fatos corretos e verificados.

### Exemplos Concretos de Alucinações

**Citações Fantasma**
IA afirma existirem artigos, estudos científicos ou pessoas que nunca existiram na realidade.

**Detalhes Inventados**
Adição de informações fictícias em resumos ou criação de citações que parecem reais.

**Objetos Irreais**
Modelos de visão computacional inserem ou identificam objetos inexistentes em imagens.

### Principais Causas das Alucinações

**Dados Incompletos**
Treinamento com informações parciais ou enviesadas leva a generalizações incorretas.

**Overfitting**
Modelo aprende ruídos nos dados em vez de padrões significativos reais.

**Complexidade**
Sistemas muito complexos dificultam o controle dos resultados.

### Impactos das Alucinações em Respostas

**Informação Errada**
Dados incorretos podem afetar decisões importantes ou comprometer a credibilidade.

**Confiança Falsa**
Usuários tendem a acreditar em respostas falsas devido ao tom confiante da IA.

**Difícil Verificação**
Identificar alucinações pode ser complexo sem ferramentas de checagem externa.

### Técnicas para Reduzir Alucinações

**Prompts Claros**
Projete instruções ricas em contexto e bem estruturadas.

**Exemplos Específicos**
Forneça exemplos concretos e informações detalhadas no prompt.

**Busca por Referências**
Oriente a IA a buscar referências que validem as informações criadas. (Perplexity, Grok e Gemini são bons, pois são ferramentas que primeiro fazem buscas para depois responder)

**Retrieval Augmented Generation (RAG)**
Uso explícito de fontes para geração de respostas. (nesse sentido, o NotebookLM é o melhor, pois ele se baseia em dados  que você inseriu previamente)

## Pesquise Por Informações Atuais, Use Fontes Confiáveis e Cite as Fontes

### Exemplos da Aula:

`Crie uma mini-biografia de 200 palavras sobre Rod Stewart com o seguinte formato
<formato>
## NOME
## DATA DE NASCIMENTO
## LOCAL DE NASCIMENTO
## DATA DE FALECIMENTO
## LOCAL DE FALECIMENTO
## REALIZAÇÕES
## PRINCIPAIS OBRAS
</formato>`

- Lembrando: até a data atual (28/06/2026), Rod Stewart está vivo.

`Crie uma mini-biografia de 200 palavras sobre Sebastião Salgado com o seguinte formato
<formato>
## NOME
## DATA DE NASCIMENTO
## LOCAL DE NASCIMENTO
## DATA DE FALECIMENTO
## LOCAL DE FALECIMENTO
## REALIZAÇÕES
## PRINCIPAIS OBRAS
</formato>
<importante>
Para responder pesquise por informações atuais e use fontes confiáveis.
</importante>`

- O `<importante>` foi usado para destacar uma instrução a ser levada em alta consideração.

`Crie uma mini-biografia de 200 palavras sobre Sebastião Salgado com o seguinte formato
<formato>
## NOME
## DATA DE NASCIMENTO
## LOCAL DE NASCIMENTO
## DATA DE FALECIMENTO
## LOCAL DE FALECIMENTO
## REALIZAÇÕES
## PRINCIPAIS OBRAS
</formato>
<importante>
Para responder pesquise por informações atuais e use fontes confiáveis.

Cite as fontes usadas para gerar a resposta.
</importante>`

- Quer dar mais ênfase ainda? peça para citar as fontes

## Adicione as Fontes e Exija que o Modelo Consulte as Fontes Antes de Responder

Quando você sabe de uma fonte que pode te ajudar na pesquisa, é melhor você indicar a fonte do que simplesmente pedir para o LLM buscar por respostas, pois ele pode buscar em locais que contém informações erradas. Isso ajuda a definir os limites para o modelod e IA.

<aside>
💡

Indico buscar primeiro sites em inglês. Normalmente as informações saem primeiro neles, e depois para o português.

</aside>

Usaremos aqui o mesmo exemplo da aula passada

`Crie uma mini-biografia de 200 palavras sobre Sebastião Salgado com o seguinte formato
<formato>
## NOME
## DATA DE NASCIMENTO
## LOCAL DE NASCIMENTO
## DATA DE FALECIMENTO
## LOCAL DE FALECIMENTO
## REALIZAÇÕES
## PRINCIPAIS OBRAS
</formato>
<importante>
Consulte os sites globo.com e forbes.com.br antes de responder.
</importante>`

## Usando Recursos de Verificação do Perplexity, Google Gemini e ChatGPT

O **Perplexity** foi feito para primeiro buscar informações e depois escrever. O **Google Gemini** tem poder de buscar informações para te trazer os dados mais precisos e atuais. 

Nessa aula, foram comparados esses dois recursos para analisar qual entregava a melhor resposta, usando o prompt da aula passada:

`Crie uma mini-biografia de 200 palavras sobre Sebastião Salgado com o seguinte formato
<formato>
## NOME
## DATA DE NASCIMENTO
## LOCAL DE NASCIMENTO
## DATA DE FALECIMENTO
## LOCAL DE FALECIMENTO
## REALIZAÇÕES
## PRINCIPAIS OBRAS
</formato>`

# Seção 8: Como Avaliar seus Prompts para Melhorias

## Um Pequeno Checklist para Avaliar seu Prompt

```markdown
Prompt para avaliar um prompt e gerar melhorias

### ROLE
Você é um engenheiro de prompts especializado em modelos de linguagem como ChatGPT, Claude, Gemini e DeepSeek. Sua função é avaliar e melhorar prompts fornecidos, com base em boas práticas de Engenharia de Prompts.

### TASK
Avalie criticamente o prompt abaixo usando o checklist estruturado. Em seguida, apresente:

1. **Análise do que está funcionando bem**
2. **Pontos que podem ser melhorados**
3. **Uma nova versão do prompt, aprimorada**

### CHECKLIST DE AVALIAÇÃO

- [ ] O prompt possui instruções claras?
- [ ] Fornece detalhes específicos sobre a tarefa?
- [ ] Apresenta informações de contexto relevantes?
- [ ] Está estruturado de forma organizada (com divisões, listas, formatação)?
- [ ] Utiliza linguagem simples, direta e sem ambiguidade?
- [ ] Inclui exemplos ou modelos de resposta esperados?
- [ ] Está livre de erros gramaticais e ortográficos?
- [ ] Define claramente o papel que o chatbot deve assumir?
- [ ] Explicita o objetivo final da tarefa?
- [ ] Estabelece critérios de sucesso ou restrições (ex: limite de palavras)?
- [ ] Indica o formato desejado da resposta (ex: tabela, markdown, lista)?
- [ ] Está formulado de forma que minimize interpretações ambíguas?
- [ ] É compatível com diferentes modelos (ChatGPT, Claude, etc.)?

### INPUT
"""[Cole aqui o prompt que deseja avaliar e melhroar]"""

### OUTPUT
Forneça sua resposta com as seguintes seções:

#### Análise Positiva 
Liste os elementos que estão bem definidos no prompt.

####  Pontos de Melhoria
Liste aspectos que podem ser ajustados para melhorar clareza, foco e estrutura.

####  Versão Aprimorada do Prompt
Escreva uma nova versão do prompt original, aplicando as boas práticas listadas no checklist.
````

## Como Melhorar Seus Prompts Usando o GPT-5 Prompt Optimizer

[OpenAI Platform](https://platform.openai.com/chat/edit?models=gpt-5.4-mini&optimize=true)

## Como Melhorar Seus Prompts Usando o Claude Improve Prompt

[Create Organization | Claude Platform](https://platform.claude.com/create/credits?from=resume)

(Por enquanto está pago)

# Seção 9: Comparando Modelos de Graça Usando o LMArena. AI

[Arena AI: The Official AI Ranking & LLM Leaderboard](https://arena.ai/)