---
tags:
  - inteligenciaartificial
---

# Seção 1: Introdução ao Curso de Engenharia de Prompts

## Como usar o ChatGPT para Melhorar seus Prompts

```markdown
Atue como um engenheiro de prompts.

Melhores o prompt a seguir para que ele sempre ofereça respostas de alta 
qualidade e relevantes.

**Pense, passo a passo, antes de responder (CoT)**

###Prompt:
Solicite ao usuário um <tópico>, um <público-alvo> e um <objetivo>. Utilize 
essas informações para criar um artigo detalhado.
```

Isso faz com que os LLMs entrem no modo reflexão ou simplesmente ele vai fazer uma análise e vai pensar um pouco, digamos que de forma mais devagar antes de responder.

Os LLMs buscam responder rápido pois milhões de pessoas usam ele. Logo, ele vai querer te responder rápido para responder outras pessoas

## O Que É Um Prompt?

### Algumas Definições Sobre Prompts

- É a forma como você interage com a IA Generativa
    
- É uma instrução ou conjunto de instruções fornecidas ao modelo de linguagem (LLM ou SLM) para gerar uma resposta específica
    
- É um estímulo para uma resposta
    
    <aside>  
    ❕
    
    O prompt atua como um gatilho que pode ajudar (ou não) o modelo a entender o tema, o contexto e o objetivo da informação que você espera obter.
    
    </aside>
    

### Um Prompt Pode Ser…

|Comando|Pergunta|Conjunto de Instruções|
|---|---|---|
|Traduza, explique, simplifique, organize, detalhe, continue, etc.|Quem, quando, como, onde, porque, quanto, quais, etc.|Uma ou mais instruções organizadas de forma a atingir um objetivo.|
|Comandos também podem ser tratados como prompts do tipo continuação.|Usam diretamente a base de conhecimento do LLM ou provocam uma busca na web.||

### Um Prompt Incompleto (ou a completar) Também Promove um Estímulo ao LLM

- Mostra a natureza semi-aleatória da construção de respostas
- Um prompt incompleto é uma ótima forma de observar como um LLM funciona ao construir uma resposta

## Elementos Que Influenciam a Resposta de um Prompt

|Elementos Internos|Elementos Externos|
|---|---|
|A estrutura do prompt, o uso de variáveis e delimitadores, etc.|Englobam o LLM, _System Prompt_, restrições, contexto, etc.|

## Elementos Internos

- Como o prompt é estruturado e organizado
    
- Contexto
    
- Personas
    
- Exemplos de resposta
    
- Formatos de resposta
    
- Uso de Marcadores e delimitadores, variáveis…
    
- Outros
    
    <aside>  
    ✅
    
    Durante o curso, abordaremos ada um dos elementos internos detalhadamente, afinal, são os elementos os quais temos controle.
    
    </aside>
    

## Elementos Externos

|CONHECIMENTO E TREINAMENTO DO MODELO|_SYSTEM PROMPT_|RESTRIÇÕES E FILTROS|CONTEXTO EXTERNO|
|---|---|---|---|
|**Base de Dados**||||
|LLMs e SLMs são treinados em conjuntos de dados massivos de texto e código. Esse conhecimento adquirido durante o treinamento é a base para gerar repostas. Modelos treinados em diferentes conjuntos terão “visões de mundo” e capacidades diferentes|**Direcionamento Inicial.**|||
|_System Prompt_ é um conjunto de instruções pré-definidas que guiam o comportamento geral do LLM ou SLM.||||

Ele define a “persona” inicial do chatbot, seu tom de voz, seus objetivos e suas restrições. | **Filtro de conteúdo.**  
São usados para evitar a geração de conteúdo inadequado, ofensivo ou prejudicial. Esses filtros podem restringir as respostas, mesmo com prompts que, em teoria, permitiriam tal conteúdo

Os filtros podem apresentar “falsos positivos”.

O uso impróprio pode causar o encerramento da conta do usuário. | **Histórico da Interação.**  
Chatbots de IA conseguem manter um histórico da conversa, o que permite que as respostas sejam contextualizadas e coerentes com as interações anteriores. Perguntas e respostas prévias influenciam a interpretação do prompt atual. |  
| **Arquitetura do Modelo**  
A arquitetura da rede neural do LLM ou SLM (_Transformer_, por exemplo) e o processo de treinamento também influenciam como o modelo processa informações e gera respostas | **Influência na Interpretação.**  
O _System Prompt_ atua como um filtro inicial na interpretação do prompt do usuário, direcionando o LLM para um tipo específico de resposta. | **Segurança e Ética.**  
Guidelines e políticas de segurança moldam o comportamento do LLM, garantindo que ele se mantenha dentro de limites éticos e legais. | **Memória.**  
Algumas ferramentas, como o ChatGPT, podem manter uma “memória”, com informações do usuário, que podem ser usadas em qualquer interação (chat) |

### Exemplo de System Prompt do Claude 3

```markdown
<system>
The assistant is Claude, created by Anthropic. The current date is Wednesday, 
March 20, 2024.

Claude's knowledge base was last updated on August 2023. It answers questions 
about events prior to and after August 2823 the way a highly informed 
individual in August 2023 would if they were talking to someone from the above
date, and can let the human know this when relevant.

It should give concise responses to very simple questions, but provide 
thorough responses to more complex and open-ended questions.

It cannot open URLs, links, or videos, so if it seems as though the 
interlocutor is expecting Claude to do so, it clarifies the situation and asks
the human to paste the relevant text or image content directly into the 
conversation.

If it is asked to assist with tasks involving the expression of views held by 
a significant number of people, Claude provides assistance with the task even 
if it personally disagrees with the views being expressed, but follows this 
with a discussion of broader perspectives. Claude doesn't engage in 
stereotyping, including the negative stereotyping of majority groups.

If asked about controversial topics, Claude tries to provide careful thoughts 
and objective information without downplaying its harmful content or implying 
that there are reasonable perspectives on both sides.

It is happy to help with writing, analysis, question answering, math, coding, 
and all sorts of other tasks. It uses markdown for coding.

It does not mention this information about itself unless the information is 
directly pertinent to the human's query.

{ "prompt": "[INSERT_PROMPT_HERE]" }
```

## Outros Fatores

- **Fatores Aleatórios e "Temperatura":** Existe um elemento de aleatoriedade na geração de texto por LLMs. Mesmo com o mesmo prompt, é possível obter respostas ligeiramente diferentes em cada interação. Estes fatores nem sempre sao acessíveis diretamente pela interface de Chat.
    
- **Capacidade de Memória:** A capacidade de memória do LLM (tamanho da janela de contexto) influencia a quantidade de contexto que ele pode considerar ao gerar uma resposta.
    
- **Atualização do Modelo:** As respostas também são influenciadas pela versão do modelo em uso. LLMs são constantemente atualizados, o que pode levar a mudanças em seu desempenho e comportamento.
    
- **Idioma:** Como o treinamento usa textos em diferentes idiomas, quanto maior a base usada para um idioma, maior é a qualidade das respostas.
    
    <aside>  
    ⚠️
    
    Há fatores que podem não ser amplamente divulgados, quando se trata de modelos Closed Source.
    
    </aside>
    

## O Que São LLMs e SLMs

|Large Language Models (LLMs)|Small Language Models (SLMs0|
|---|---|
|Modelos massivos com mais de 100 bilhões de parâmetros|Versões compactas com menos de 100 bilhões de parâmetros, mas a maioria tem menos de 20 bilhões de parâmetros|
|Exemplos incluem Grok 3 (2,7 trilhões) e Lama 4 (Cerca de 2 trilhões)|Oferecem equilíbrio entre capacidade e eficiência computcional|
|Rodam clusters de GPUs NVidia Avançados|Modelos com menos de 13 bilhões de parâmetros (quantizados) podem ser usados com computadores desktop.|

### Capacidade 1: Domínio

da Linguagem

**Fluência Natural**  
Produzem textos que  
imitam a escrita humana  
com coerência e fluidez.

**Consistência Temática**  
Mantém o foco em tópicos  
específicos mesmo em textos  
extensos.

**Adaptação Estilística**  
Ajustam-se a diversos estilos,  
do formal ao coloquial.

### Capacidade 2: Conhecimento de Mundo

**Conhecimento Enciclopédico**  
Informações diversas sobre  
ciência, história e cultura.

**Dados de Treinamento**  
Absorvidos de bilhões de  
textos na internet.

**Limitação Temporal**  
Restrito às informações  
até a data de corte (cutoff  
date) do treinamento.

### Capacidade 3: Aprendizado Contextual

**Resposta a Prompts**  
Interpreta instruções  
diretas em linguagem  
natural.

**Compreensão Contextual**  
Considera a conversa anterior  
para maior coerência

**Few-Shot Learning**  
Aprende Padrões com apenas alguns exemplos

**Chain-of-Thought**  
Desenvolve raciocínio  
passo a passo baseado  
em contexto.

### Capacidade 4: Raciocínio Lógico

**Resolução de Problemas**  
Aborda questões complexas  
através de etapas lógicas

**Inferências**  
Deduz conclusões baseadas em informações fornecidas

**Alucinações**  
Pode gerar respostas  
plausíveis mas incorretas

**Limitações em SMLs**  
Modelos menores têm  
capacidade reduzida nesta  
área

### Capacidade 5: Alinhamento com Instruções

**Compreensão de Comandos**  
Entende instruções complexas em linguagem natura**l**

**Adaptação de Saídas**  
Ajusta respostas conforme especificações detalhadas

**Versatilidade de Formatos**  
Responde a diversos tipos  
de prompts e solicitações

### Modelos Multimodais

Capazes de lidar com diferentes tipos de entrada como imagens, vídeos, texto, documentos diversos, voz, etc.

## O Que É Engenharia de Prompt?

<aside>

Nova área crucial para interações com IA

</aside>

<aside>

Essencial para lidar com LLMs poderosos.

</aside>

<aside>

#### Termo surgiu entre 2020 e 2022 e alguns afirmam que foi "oficializado" em 2022 no artigo P_rompt Engineering: The Art of Guiding Large Language Models_, de John Hewitt, pesquisador da Google AI

Hewitt argumenta que o prompt engineering é uma habilidade critica para os desenvolvedores e engenheiros que trabalham com sistemas de linguagem natural. Ele afirma que os prompts são essenciais para moldar o comportamento desses sistemas, e que a capacidade de criar prompts eficazes pode levar a resultados mais criativos e informativos.

</aside>

<aside>

Habilidade em alta demanda por empresas

</aside>

<aside>  
⚠️

Não é uma habilidade exclusiva da computação!

</aside>

### Fundamentos da Engenharia de Prompts

**Arte e Ciência**  
A combinação de criatividade e técnicas para construir instruções eficazes para IAs.

**Comunicação Precisa**  
Resultados mais precisos e com qualidade superior.

**Resultados Superiores**  
Prompts bem elaborados melhoram o resultados em até 67%.

### Aplicações Práticas de Prompts

**Automação**

Redução de 40% no tempo de processos rotineiros. Aumento significativo na eficiência operacional.

**Análise de Dados**

Geração rápida de insights a partir de grandes volumes de informação. Tomada de decisão aprimorada

**Criação de Conteúdo**

Criação de conteúdo textual, visual ou de código, para diversas finalidades

### Desafios e Limitações

**Alucinações**  
Podem ser combatidas com prompts eficientes.

**Segurança**  
Risco de vazamento de informações sensíveis.

**Incapacidade de Lidar com Informações Recentes**  
Muitos modelos já fazem busca na internet.

**Vieses**  
Preconceitos nos modelos afetam resultados.

**Complexidade**  
Dificuldades na manutenção e versionamento de prompts, conforme os modelos evoluem.

**Dificuldades em Lidar com Grandes Contextos**  
RAG e ferramentas como NotebookLM.

<aside>  
⚠️

Afirmação errada e frequente usada por “especialistas”: **Engenharia de prompts não é mais necessário, em função da evolução das IAs.**

</aside>

### O Futuro da Engenharia de Prompts

**Multimodalidade**  
Evolução para interfaces com texto, imagem e áudio integrados.

**Ferramentas de Workflow**  
Automação simplificada no uso de prompts em cadeia.

**Integração**  
Conexão com UX, engenharia de dados e outras disciplinas.

**Profissionalização**  
Surgimento de certificações.

# Seção 3: Os Princípios Universais da Engenharia de Prompts - Design de Prompts

## Os Princípios Universais da Engenharia de Prompts - Design de Prompts

### Fundamentos: Os 3 Cs Essenciais

|Clareza (Clarity)|Contexto (Context)|Constraint (Restrições)|
|---|---|---|
|Instruções precisas e diretas|Informações de base relevantes|Limitações que guiam respostas|
|**Linguagem Precisa**.|||
|Use termos técnicos específicos em vez de palavras genéricas,|Contexto ajuda o modelo a compreender as nuances|**Limites de Formato|
|•** Estrutura específica (tabela, lista, texto)|||
|• Elementos obrigatórios e opcionais|||
|• Sequência lógica definida|||
|**Evite Ambiguidades.**|||
|Elimine frases com múltiplas interpretações possíveis.|Fornece informações para direcionar as respostas|**Limites de Conteúdo|
|•** Extensão (número de palavras/parágrafos)|||
|• Profundidade técnica (básico/avançado)|||
|• Perspectiva adotada (formal/informal)|||
|**Correlação Comprovada.**|||
|Estudos mostram que prompts claros geram respostas técnicas superiores.|Contexto rico permite conexões mais profundas e resultados relevantes|**Limites de Escopo|
|•** Períodos históricos a considerar|||
|• Geografias ou culturas específicas|||
|• Teorias ou abordagens permitidas|||
|**Estrutura Lógica.**|||
|Organize instruções em sequência natural e coerente.|Define o campo de conhecimento específico|Restrições bem definidas promovem foco e consistência nas respostas geradas.|
||Modelos respondem com maior precisão com panorama completo||

Estes pilares universais formam a base reconhecida por especialistas globais em IA. São princípios que transcendem modelos específicos e aplicações diversas.

### Uso de Exemplos

#### Zero Shot

O modelo responde sem exemplos prévios, baseando-se apenas nas instruções.

Ideal para tarefas simples ou quando não há exemplos disponíveis.

Exemplo: “Explique o conceito de prompt engineering em português.”

#### Few Shot

Fornece alguns exemplos de entrada-saída antes da tarefa principal.

O modelo aprende padrões a partir dos exemplos fornecidos.

Exemplo: “Entrada: resumir. Saída: sintetizar pontos principais. Agora: analisar.”

A escolha entre zero shot e few shot depende da complexidade da tarefa.

### Boas Práticas do Design de Prompts

#### Objetivos Claros

Defina, claramente, o que deseja alcançar com o Prompt.

#### Validação

Teste com diferentes variações e LLMs para confirmar eficácia

#### Divisão

Decomponha tarefas complexas em componentes menores

#### Refinamento

Revise e ajuste prompts com base nos resultados

O design eficaz de prompts é um processo contínuo de aperfeiçoamento.

### Estruturação de Prompts Complexos

#### Definição do Problema

Estabeleça claramente o objetivo principal do seu prompt para orientar o modelo adequadamente.

#### Decomposição em Etapas

Divida em tarefas sequenciais e progressivas para facilitar o processamento pelo modelo.

#### Instruções Específicas

Forneça diretrizes detalhadas para cada etapa, eliminando ambiguidades na interpretação

#### Critérios de Sucesso

Defina como avaliar a qualidade do resultado para garantir que o output atenda às expectativas.

Prompts complexos exigem estrutura clara para guiar o modelo com precisão.

### Iteração e Refinamento Contínuo

**Prompt Inicial**  
Crie a primeira versão baseada nos princípios fundamentais

**Avaliação da Resposta**  
Analise criticamente o resultado

**Ajuste Específico**  
Modifique elementos problemáticos ou imprecisos

**Teste e Repetição**  
Aplique o prompt revisado e avalie novamente

O processo iterativo é essencial para aperfeiçoar prompts ao longo do tempo. Cada ciclo de feedback contribui para resultados progressivamente melhores.

### Compreensão das Capacidades do Modelo

**Dados de Treinamento**  
Entenda o escopo e recência das informações que o modelo possui.  
Reconheça pontos cegos ou áreas de conhecimento limitado.

**Parâmetros Configuráveis**  
Domine ajustes como temperatura e top-p para controlar a criatividade. Adapte configurações ao tipo específico de tarefa.

**Variação entre Modelos**  
Reconheça que prompts eficazes podem variar entre diferentes LLMs.  
Teste o mesmo prompt em múltiplos modelos quando possível.

### Considerações Éticas

**Imparcialidade**  
Evite prompts que induzam vieses ou preconceitos nos resultados.  
Considere perspectivas diversas ao formular questões.

**Sensibilidade Cultural**  
Respeite diferenças culturais nas instruções e exemplos fornecidos.  
Evite generalizações ou estereótipos em seus prompts. ****Também, opte por escrever o prompt na linguagem mais proeminente em determinado assunto.

**Precisão**  
Verifique a veracidade das informações geradas pelo modelo.  
Inclua solicitações de citação de fontes quando relevante.

**Guardrails**  
Implemente regras claras para limitar conteúdo prejudicial. (exemplo: pedir para verificar sempre as fontes. Ter uma bibliografia própria e orientar a máquina a usar somente ela…)  
Estabeleça fronteiras éticas para a geração de conteúdo.

## Como Aumentar a Clareza e Especificidade

Use técnicas passadas anteriormente, como pedir para o próprio assistente IA agir como engenheiro de prompt

Peça para ele ir fazendo perguntas sobre o assunto para você, para que ele molde o output em relação a suas respostas.

Use linguagem de `Markdown` ou também a linguagem `XML`.

## Como Definir um Bom Contexto para seus Prompts

### O Que É Contexto em um Prompt?

O contexto representa todas as informações de apoio fornecidas à IA.

O contexto direciona a resposta para atender necessidades específicas.

Um bom contexto deve ser claro, objetivo e relevante, garantindo que a IA compreenda exatamente o que se espera dela para gerar respostas precisas e úteis. Além disso, incluir detalhes suficientes ajuda a evitar interpretações ambíguas e melhora a qualidade do resultado final.

### Por Que o Contexto é Essencial?

**Influência na Interpretação**  
Determina como o modelo entenderá e processará o prompt.

**Precisão e Relevância**  
Gera respostas mais precisas e alinhadas ao objetivo .

### Contexto Interno

**Detalhes Textuais**  
Informações fornecidas diretamente no prompt.

**Histórico de Conversa**  
Todas as mensagens trocadas a até o momento.

**Recurso de Memória**  
Recurso que surgiu com o ChatGPT para retenção de informações do usuário

### Contexto Externo

<aside>

Muito usado por ferramentas específicas, como o NotebookLM para coletar informações que vão apoiar a resposta. E essas informações podem ser arquivos, documentos, imagens, vídeos. Então, esse conteúdo externo ao Agente, é capturado e trazido para dentro do processamento.

</aside>

**Arquivos**  
Documentos, imagens e outros formatos anexados ao prompt.

**Links**  
Referências a conteúdos online externos.

**Bases de Dados**  
Conjuntos estruturados de informações corporativas.

**Busca Externa**  
Conexão com web e ferramentas corporativas.

### Componentes de um Contexto Detalhado

**Informações de Background**

- Fornece o cenário geral, a situação atual, o conhecimento prévio relevante ou o ambiente em que a tarefa está inserida. Ajuda o modelo a entender o “onde” e o “porquê” da solicitção.

**Domínio ou Área do Conhecimento**

- Estabelece o campo específico de atuação ou a expertise necessária para a tarefa

**Dados ou Material de Referência**

- A matéria-prima sobre a qual o modelo deve trabalhar, com textos, artigos, transcrições ou dados brutos

**Termos e Conceitos-Chave**

- Termos e conceitos relevantes ao tópico para garantir que o modelo entenda corretamente o que está sendo solicitado

**Público-Alvo**

- Identifica para quem é a resposta ou ação, adaptando o nível de detalhamento e vocabulário.

## Como Usar Pronomes Interrogativos para Criar Contexto para Prompts

Os **pronomes interrogativos** mais comuns na língua portuguesa são usados para formular perguntas diretas ou indiretas e variam conforme o que se deseja questionar (pessoa, objeto, motivo, quantidade etc.) Veja os principais:

|Pronome Interrogativo|Uso Principal|Exemplo|
|---|---|---|
|Que|Coisa, fato, definição|Que dia é hoje?|
|Quem|Pessoa(s)|Quem fez isso?|
|Qual/ Quais|Escolha ou identificação entre opções|Qual é o seu nome? / Quais são os seus livros?|
|Quanto/ Quantos|Quantidade (masculino/feminino)|Quanto custa? / Quanta água há aí?|
|Quantos/ Quantas|Quantidade (plural)|Quantos alunos vieram? / Quantas horas faltam?|
|Onde|Lugar|Onde você está?|
|Como|Modo, maneira|Como isso aconteceu?|
|Por que|Causa, motivo|Por que você chorou?|

### Formando um Contexto com Pronomes Interrogativos

1. Você pode formular um prompt assim:
    
    ```markdown
    Usando os pronomes interrogativos acima, crie perguntas sobre [assunto 
    desejado]
    ```
    
2. Após isso, você pode fazer o seguinte:
    
    ```markdown
    Responda cada uma das perguntas
    ```
    
    - Você, claro, pode oferecer mais detalhes para melhorar o contexto, como quantidade de parágrafos por resposta.
3. E então:
    
    ```markdown
    Com base nas informações acima, crie um prompt detalhado para produção
    de um relatório sobre [assunto desejado]
    ```
    
    - Você, claro, pode oferecer mais detalhes para melhorar o contexto, como:
        - É um relatório técnico?
        - É didático?
        - Linguagem do texto
        - Público-alvo
        - Quantas páginas
        - Como será estruturado?(exemplo, em seções)

## Como Aumentar a Qualidade dos Seus Prompts Definindo Restrições?

### Por Que Usar Restrições em Prompts?

**Precisão Aumentada**  
Elimina respostas vagas e genéricas que não atendem suas necessidades específicas

**Direcionamento Eficaz**  
Mantém o modelo dentro do contexto e escopo desejados

**Consistência Superior**  
Garante padrão uniforme nas respostas geradas pelo modelo

### Tipos Comuns de Restrições

**Escopo Temático**  
Limita o assunto abordado (”aborde apenas esse assunto”, “aborde apenas medicina integrativa”, “aborde apenas as leis relacionadas a imóveis”)

- Creio que seria inteligente aqui, utilizar como restrição os conteúdos planilhados de livros, com diferentes abordagens. Por exemplo: estou criando uma estratégia de marca (Brand Strategy). Para saber a melhor abordagem, debato com a IA, e, digamos que decida utilizar o Branding Cultural; pedirei à máquina para se basear em autores como Douglas B. Holt, Margaret Mark e Carol S. Pearson

**Formato Estrutural**  
Define a organização da resposta (”quero que seja estruturado como um artigo seguindo as normas da ABNT”, por exemplo)

**Tamanho do Conteúdo**  
Especifica a extensão desejada

### Exemplos Práticos de Restrições

**Limitações de Tamanho**  
”Responda em até 3 frases curtas e diretas”

**Restrição Quantitativa**  
”Liste apenas os 5 fatores mais importantes, sem explicações adicionais.”

**Foco Temático**  
”Aborde exclusivamente os impactos econômicos, ignorando aspectos sociais”

**Estilo Linguístico**  
”Use terminologia técnica adequada para profissionais da área.”

### Eliminando Ambiguidades

**Palavras-Chave Específica**  
Use termos precisos que direcionam o foco exato.

**Exclusões Explícitas**  
”Não mencione aspectos históricos ou evolutivos do tema.”

**Equilíbrio de Perspectivas**  
”Apresente apenas abordagens baseadas em evidências científicas.”

**Contextualização Clara**  
Defina o cenário específico para aplicação da resposta

<aside>  
💡

### O Contexto Ajuda Muito, Mas, As Vezes, A Restrição Ajuda Ainda Mais!

</aside>

## Categorias de Prompts

### Quais são as categorias de prompts?

Prompts são ferramentas essenciais para facilitar uma comunicação eficiente com modelos de linguagem baseados em IA.

Para criar prompts de alta qualidade, é fundamental entender como eles são classificados. Isso permite estruturá-los de forma eficaz, focando em uma resposta específica desejada.

### Principais categorias de prompts:

### 1. Prompts para busca de informação

Esses prompts são criados para coletar informações por meio de perguntas como “O que” e “Como”. São ideais para extrair detalhes ou fatos específicos do modelo de IA.

**Exemplos:**

- Quais são os benefícios para a saúde de uma dieta baseada em plantas?
- Como posso aumentar minha produtividade no trabalho?

### 2. Prompts baseados em instruções

Prompts baseados em instruções direcionam o modelo de IA a realizar uma tarefa específica. Esses prompts se assemelham à forma como interagimos com assistentes de voz como Siri, Alexa ou Google Assistente.

**Exemplos:**

- Agende uma consulta no dentista para a próxima terça-feira às 10h.
- Encontre a rota mais rápida para o aeroporto.

### 3. Prompts que fornecem contexto

Esses prompts fornecem informações contextuais ao modelo de IA, permitindo que ele compreenda melhor a resposta desejada pelo usuário. Ao oferecer contexto, é possível obter respostas mais precisas e relevantes.

**Exemplos:**

- Sou iniciante em jardinagem. Quais são algumas plantas fáceis de cultivar para iniciantes?
- Quero planejar um jantar romântico para meu parceiro. Pode sugerir receitas e ideias de ambientação?

### 4. Prompts comparativos

Prompts comparativos são usados para avaliar ou comparar diferentes opções, ajudando o usuário a tomar decisões informadas. São especialmente úteis para analisar prós e contras de alternativas.

**Exemplos:**

- Quais são os benefícios e desvantagens de alugar versus comprar um imóvel?
- Compare o desempenho de carros elétricos e carros a gasolina.

### 5. Prompts para solicitação de opinião

Esses prompts solicitam a opinião ou ponto de vista da IA sobre determinado tema. Podem ajudar na geração de ideias criativas ou em discussões instigantes.

**Exemplos:**

- Qual pode ser o impacto da inteligência artificial no mercado de trabalho?
- Como o mundo poderia mudar se a teletransporte se tornasse realidade?

### 6. Prompts reflexivos

Prompts reflexivos ajudam as pessoas a obter insights mais profundos sobre si mesmas, suas crenças e ações. Frequentemente incentivam o autoconhecimento e a introspecção com base em um tema ou experiência pessoal. Pode ser necessário fornecer algumas informações de fundo para obter uma resposta satisfatória.

**Exemplos:**

- Como posso desenvolver minha autoconfiança e superar a autocrítica?
- Quais estratégias posso aplicar para manter um equilíbrio saudável entre trabalho e vida pessoal?

# Seção 4: Elementos, Estruturas, Componentes de um Prompt

## Componentes Essenciais de um Prompt

### Instrução, Objetivo e Tarefa

Pense em uma instrução, objetivo ou tarefa como as orientações para que façam algo.

**Instruções [Descrição do Processo]**  
Orientações detalhadas que direcionam como realizar algo corretamente.

**Objetivos [Descrição do Produto]**  
Metas claras que desejamos alcançar através de nossas ações.

**Tarefas [Descrição do Desempenho]**  
Atividades específicas que, quando bem realizadas, nos aproximam dos objetivos.

Quanto mais detalhadas, precisas e completas são as **instruções**, mais bem realizadas são as **tarefas**, o que favorece alcançar os **objetivos**.

### Além disso, temos o Contexto como Componente Essencial

**Domínio específico de uma área do conhecimento:**

- **Ciências da computação:** Algoritmos, programação, sistemas operacionais, IA;
- **Saúde:** Diagnóstico médico, tratamentos, nutrição;
- **Marketing:** Estratégias de branding, publicidade, comportamento do consumidor.

**Referências ou Fontes**

- Livros, autores e artigos conhecidos (ex, Moby Dick)

**Restrição Temporal ou Geográfica**

- Período na História
- País ou Região

**Outros:**

- Imagens
- Vídeos
- Exemplos (_Few-Shot Prompting_)
- **Informação de apoio** (RAG)
    - Se você tem um grande volume de conteúdo, indico usar o NotebookLM ou produzir um algoritmo próprio

### Direcionamento de Resposta

**O Que Se Espera Que Seja Produzido**

- Formato da resposta
    - Comprimento
    - Tabelas, gráficos, diagramas e imagens
    - Listas
    - Organização (tópicos, subtópicos, etc.)

### Tom e Estilo

- Formal, Informal, Técnico, acadêmico, empático, persuasivo, didático
- Ponto de Vista
- Nível de detalhe e profundidade

### Público-Alvo

### Restrições

- Temas a não abordar

## O Que São Elementos, Estruturas, Componentes de um Prompt

### Um Prompt Pode Ser Decomposto em:

#### Componentes

- **Blocos funcionais** que formam a base do seu prompt, incluindo **contexto**, **instruções** e **exemplos** que **orientam o comportamento da IA**. Cada componente tem um propósito específico para guiar o modelo em direção à resposta.

#### Estruturas

- Definem **como os componentes são organizados**, estabelecendo a **sequência lógica** e as **relações** entre as diferentes partes do seu prompt. A estrutura define coisas como a ordem dos elementos, uso de delimitadores, etc.

#### Elementos

- Este é o **termo mais genérico**. Refere-se às **unidades mais básicas** e fundamentais que compõem um prompt. Podem ser **palavras-chave**, **frases** **curtas**, **símbolos**, **delimitadores**, ou **qualquer parte individual** do texto do prompt

A combinação eficaz destes três aspectos permite criar prompts que geram resultados consistentes e de alta qualidade.

### O Que São Componentes de um Prompt

**Contexto**  
Define o cenário e informações de base que a IA precisa entender para responder adequadamente.

**Instruções**  
Comandos claros que especifica exatamente o que você quer que a IA faça como.

**Exemplos**  
Demonstrações do formato ou estilo de resposta desejado, facilitando o alinhamento da IA.

**Restrições**  
Limitações e parâmetros que definem o que a IA deve evitar ou como deve limitar respostas.

### O Que São Estruturas de um Prompt

**Sequenciamento**  
Define a ordem em que as informações são apresentadas para melhor compreensão pela IA.

**Delimitadores**  
Símbolos ou formatação que separam claramente as diferentes seções do prompt.

**Hierarquia**  
Organiza informações por importância, criando níveis de prioridade para a IA.

**Agrupamento Lógico**  
Reúne componentes relacionados, facilitando o processamento contextual pela IA.

### O Que São os Elementos de um Prompt

As unidades fundamentais que compõem cada interação com a IA, formando a base de comunicação efetiva.

Em certos contextos, pode ser sinônimo de componentes, mas tende a ser menos técnico/funcional.

**Palavras-chave**  
Termos específicos que ativam determinados comportamentos e associações no modelo

**Frases e Símbolos**  
Expressões curtas e marcadores que direcionam o tom e formato da resposta.

**Delimitadores**  
Símbolos como aspas, colchetes ou marcadores que organizam seções distintas.

### Exemplos

#### Exemplo 1 - Prompt de Resumo de Texto

```markdown
"Leia o texto abaixo e forneça 
um resumo dos principais pontos
Abordados.
Texto:
'A inteligência Artificial tem
avançado rapidamente nos últimos
anos, impactando áreas como saúde,
educação e economia. Novas 
técnicas de aprendizado de 
máquina permitem soluções 
inovadoras, mas também levantam
questões éticas sobre privacidade
e emprego.'
Resumo:"
```

**Elementos:**

- **Palavras-Chave:** “resumo objetivo”, “principais pontos”
- **Marcador de Contexto:** “Texto:”
- **Delimitação do Texto:** uso de aspas simples para separar o texto a ser resumido
- Marcador de Resposta: “Resumo:”

**Componentes:**

- **Instrução:** “Leia o texto abaixo e forneça um resumo objetivo dos principais pontos abordados”
- **Contexto:** Texto fornecido para ser resumido
- **Sinalizador de saída:** “Resumo:” - indica o início da resposta esperada

**Estrutura**

- **Sequência Linear:**
    1. Instrução
    2. Contexto/Texto
    3. Sinalizador para a resposta (Resumo:)
- **Lógica:** O prompt primeiro explica a tarefa, apresenta o material de entrada e guia claramente onde a resposta deve começar

#### Exemplo 2 - Prompt de Geração de Exemplos Didáticos

```markdown
"Crie dois exemplos práticos de
Aplicação de funções exponenciais 
em situações cotidianas, expli-
cando brevemente cada contexto."
```

**Elementos:**

- **Número específico:** “dois exemplos práticos”
- **Tema central:** Funções exponenciais”
- **Condição Adicional:** “em situações cotidianas”
- **Orientação quanto à explicação:** “explicando brevemente cada contexto”

**Componentes:**

- **Instrução:** “Crie dois exemplos práticos de Aplicação de funções exponenciais em situações cotidianas”
- **Requisito adicional:** “explicando brevemente cada contexto”

**Estrutura:**

- **Etapas de Ação:**
    1. Solicitação de geração de exemplos
    2. Indicação do número de exemplos
    3. Solicitação de explicação sucinta para cada exemplo

#### Exemplo 3 - Prompt de Resolução de Problema Matemático

```markdown
"Resolva o seguinte problema de
álgebra mostrando todos os passos:
Se 2x + 3 = 11, qual é o valor de
x?"
```

**Elementos:**

- **Expressão Algébrica:** “2x + 3 = 11”
- **Termos de Orientação: “Mostrando todos os passos”**
- **Pergunta Direta:** “qual é o valor de x?”

**Componentes:**

- **Instrução:** “Resolva o seguinte problema de  
    álgebra mostrando todos os passos”
- **Problema/Entrada:** “Se 2x + 3 = 11, qual é o valor de x?"

**Estrutura:**

- **Etapas de Ação:**
    1. Instrução
    2. Problema matemático
- **Sequência lógica**: Apresentação da tarefa seguida imediatamente pela entrada/problema

#### Exemplo 4 - Prompt de Análise de Texto Literário

```markdown
PAPEL] Você é um crítico 
literário especializado em 
literatura brasileira.
[CONTEXTO] Analise o seguinte 
trecho da obra "Grande Sertão: 
Veredas" de Guimarães Rosa:
"O senhor... Mire veja: o mais 
importante e bonito, do mundo, é
isto: que as pessoas não estão 
sempre iguais, ainda não foram 
terminadas - mas que elas vão 
sempre mudando."
[TAREFA] Forneça uma análise 
crítica deste trecho 
considerando:
1. O uso da linguagem e estilo 
característicos do autor
2. As temáticas existenciais 
presentes
3. Como este trecho reflete a 
obra como um todo
[FORMATO] Estruture sua resposta 
em parágrafos separados para 
cada ponto de análise e conclua 
com uma síntese.
```

**Componentes:**

- Definição de papel
- Contexto
- Tarefa principal
- Subtarefas
- Formato de Resposta

**Estrutura:**

- **Organização por Seções Demarcadas:** Papel/ Persona → Contexto → Tarefa → Formato
- **Hierarquia de Tarefas:** Tarefa principal seguida de subtarefas específicas
- **Fluxo Lógico:** Identidade → Material de Análise → Instruções → Formato esperado

**Elementos:**

- **Delimitadores Entre Seções:** `[PAPEL]`, `[CONTEXTO]`, etc.
- Numeração das tarefas para clareza
- Citação direta do texto a ser analisado
- **Especificidade:** menção ao autor e obra
- **Linguagem:** técnica do domínio literário

### Exemplo 5 - Geração de Resumo com Especificações de Tom e Público

```markdown
을 Persona li
Você é um especialista em comunicação científica com habilidade para simplificar tópicos complexos para o público leigo.

### Contexto ###
o texto abaixo é um trecho de um 
artigo acadêmico sobre os efeitos 
da privação de sono na cognição.
A restrição crônica do sono (RCS) 
demonstrou consistentemente prejudicar 
uma miríade de funções neurocognitivas, 
incluindo, mas não se limitando a, atenção 
sustentada, memória de trabalho, velocidade 
de processamento psicomotor e funções 
executivas superiores. Estudos de 
neuroimagem funcional corroboram esses 
achados comportamentais, revelando 
alterações na atividade metabólica em 
regiões cerebrais críticas, como o córtex 
pré-frontal e o hipocampo, após períodos de 
sono insuficiente. Tais déficits podem ter 
implicações significativas para o 
desempenho em atividades diárias e 
profissionais.
### Tarefa ###
Resuma o texto acima em 
**exatamente 2 frases**.

### Restrições de Saída ###
- Utilize uma linguagem **clara, 
acessível e informal**, evitando
jargões técnicos.
```

**Elementos**

- Verbos de comando: "Resuma".
- Palavras-chave especificas: "especialista em comunicação cientifica", "público leigo", "artigo acadêmico*, "privação de sono", "cognição", "exatamente 2 frases", "linguagem clara, acessível e informal", "jargões técnicos", "estudantes do ensino médio", "impacto prático".  
    Delimitadores: F
- Quantificadores/Especificadores: "exatamente 2".
- Dados de entrada: O próprio trecho do artigo acadêmico.

**Componentes**

- **Persona**: Define o papel que o modelo deve assumir ("especialista em comunicação cientifica"). Isso influencia o estilo e a abordagem da resposta.
- **Contexto**: Fornece a informação de base necessária para a tarefa. Inclui uma breve descrição do texto original e o próprio texto (***texto aqui"**).
- **Tarefa**: Especifica a ação principal que o modelo deve realizar ("Resuma o texto acima em exatamente  
    2 frases*).
- **Restrições** de Saída: Detalha os requisitos para a resposta gerada (linguagem clara/acessivel/informal, público-alvo, foco no impacto prático).

**Estrutura**

- **Organização**: em seções claramente delimitadas por ### Título do Componente ###.
- **A ordem dos componentes**: primeiro estabelece quem o modelo é (Persona), depois o que ele tem para trabalhar (Contexto), o que deve fazer (Tarefa) e como deve fazer (Restrições de Saida).
- **Delimitadores**: O uso de *** *** para delimitar o texto de entrada.
- **Formatação**: O uso de negrito (**exatamente 2 frases**, **clara, acessível e informal**) para destacar elementos cruciais dentro dos componentes.

## Zero-Shot Prompting para Chatbots de LLMs

### O Que São Zero-Shot Prompts?

**Definição**  
São prompts que fornecem uma tarefa para o modelo sem fornecer exemplos de como o modelo deve responder.

**Papel Histórico**  
Antes dos modelos grandes, sempre era necessário oferecer exemplos de como a resposta deveria ser criada, para que o modelo respondesse de forma adequada. Com a enorme base conhecimento dos modelos grandes, o fenômeno zero-shot foi observado: **Os modelos passaram a ser capazes de responder sem exemplos**.

### Como Funciona os Zero-Shot Prompts?

**Entrada**  
O prompt é transformado em tokens e processado em conjunto com o _System Prompt_ e o contexto atual, se aplicável.

**Inferência Avançada**  
O LLM analisa a entrada e utiliza sua compreensão de linguagem natural e o conhecimento adquirido no treinamento para gerar respostas relevantes.

### Vantagens do Zero-Shot Prompts

**Acesso Direto ao Conhecimento do LLM**  
Permite que os chatbots realizem uma ampla gama de tarefas sem necessidade de treinamento específico.

**Simplicidade na Interção**  
Prompts zero-shot são fáceis de elaborar e usar.

### Desafios e Limitações do Zero-Shot Prompting

**Só se Aplicam para Situações Simples**  
Situações complexas demandam prompts mais robustos. O zero-Shot funciona melhor com tarefas básicas e

**Vieses e Erros**  
Sem um controle adequado, podem gerar respostas indesejadas, entretanto os sistemas atuais possuem proteções implícitas para evitar erros e vieses

**Baixa Qualidade**  
Respostas tendem a ser genéricas e com “cara de IA”, faltando originalidade e personalização.

### Tipos de Operações Zero-Shot

**Operações Generativas**  
Criam novo conteúdo a partir de uma entrada. A saída é tipicamente maior que a entrada.

**Operações Transformativas**  
Transformam a entrada em outro formato. Entrada e saída têm tamanhos e/ou significados semelhantes.

**Operações Redutivas**  
Processam grande quantidade de texto para produzir uma saída menor. A entrada é maior que a saída.

### Operações Generativas

Gerar uma grande quantidade de texto a partir de um pequeno conjunto de instruções ou dados. A entrada é menor que a saída.

**Rascunho**  
Gerar documentos iniciais como código, ficção, documentação legal, artigos científicos e narrativas diversas

**Planejamento**  
Elaboração detalhada de planos considerando ações, projetos, objetivos e restrições dentro do contexto específico

**Brainstorming**  
Exploração criativa de possibilidades para resolução de problemas e formulação de hipóteses através da ideação.

**Amplificação**  
Processo de expandir detalhar conceitos, articulando explicações mais profundas e abrangentes

### Operações de Transformação

**Reformatação**  
Alteração da apresentação mantendo o conteúdo, como converter prosa em roteiro ou XML em JSON.

**Reformulação**  
Reestruturação para maior eficiência, mantendo os mesmos resultados mas com implementação diferente.

**Mudança de Idioma**  
Tradução entre diferentes linguagens, seja de programação (C++ para Python) ou idiomas naturais.

**Reestruturação**  
Otimização da estrutura e fluxo lógico, reorganizando elementos para melhor compreensão.

**Modificação**  
Adaptação do texto para diferentes tons e níveis de formalidade, mantendo a mensagem central.

**Esclarecimento**  
Transformação de conteúdo complexo em formato mais acessível e compreensível.

### Operações Redutivas

**Resumo**  
Dizer a mesma coisa com menos palavras através de listas, notas ou resumo executivo.

**Destilação**  
Purificar os princípios ou fatos subjacentes, removendo ruído para extrair fundamentos essenciais.

**Extração**  
Recuperar tipos específicos de informações como respostas, nomes, datas e dados relevantes.

**Caracterização**  
Descrever o conteúdo do texto como um todo ou dentro do contexto específico do assunto.

**Análise**  
Encontrar padrões ou avaliar contra uma estrutura através de análise estrutural retórica.

**Avaliação e Crítica**  
Medir, classificar e fornecer feedback construtivo o contexto do material apresentado.

## One-Shot e Few-Shot Prompting para Chatbots de LLMs

Estas técnicas emergiram como resposta às limitações do treinamento supervisionado tradicional, que exigia enormes conjuntos de dados rotulados.

Sua popularização ocorreu com o advento dos modelos GPT a partir de 2020, revolucionando a interação homem-máquina

![Captura de tela 2026-06-24 194011.png](attachment:2bbbd7d6-976b-4634-89b8-875247f5dcae:Captura_de_tela_2026-06-24_194011.png)

### Definição de One-Shot Prompting

**Um Único Exemplo**  
O modelo recebe apenas um exemplo de entrada e saída antes de executar a tarefa solicitada.

**Referência Clara**  
Este exemplo único serve como referência direta para o formato e estilo da resposta desejada.

**Casos Específicos**  
Mais eficaz em tarefas com padrão específico ou poucas variações contextuais possíveis.

### Definição de Few-Shot Prompting

Recomendado para tarefas que exigem compreensão de contextos múltiplos ou respostas mais nuançadas.

**Múltiplos Exemplos**  
O modelo recebe diversos exemplos de entrada e saída que demonstram o padrão desejado.

**Reconhecimento de Padrões**  
Facilita a identificação de estruturas complexas e nuances em diferentes contextos.

**Generalização**  
Permite que o modelo extraia regras mais sofisticadas aplicáveis a situações variadas.

### Como Funciona na Prática

> **Inclusão no Prompt**  
> Os modelos são inseridos diretamente no prompt enviado ao modelo de linguagem.
> 
> > **Análise de Padrão**  
> > O modelo identifica estrutura, formato e intenção dos exemplos fornecidos.
> > 
> > > **Geração Alinhada**  
> > > A resposta gerada segue o padrão dos exemplos em estilo e qualidade.

```markdown
Exemplo One-Shot:
Pergunta: Qual a capital da França?
Respostas: A capital da França é 
Paris.

Sua pergunta: Qual a Capital do 
Brasil?
```

```markdown
Exemplo Few-Shot:
Pergunta: Qual a capital da França?
Respostas: A capital da França é 
Paris.

Pergunta: Qual o maior rio do mundo?
Respostas: O maior rio do mundo é o
Rio Nilo.

Sua pergunta: QUal a Capital do 
Brasil?
```

### Vantagens do One-Shot Prompting

**Precisão Elevada**  
Grande precisão ao seguir o exemplo fornecido, especialmente em tarefas bem definidas.

**Configuração Rápida**  
Exige apenas um exemplo bem elaborado, economizando tempo na preparação.

**Clareza de Instruções**  
Reduz ambiguidade ao demonstrar exatamente o que é esperado do modelo.

**Economia de Tokens**  
Usa menos espaço no contexto do modelo comparado ao Few-Shot Prompting.

### Vantagens do Few-Shot Prompting

**Maior Adaptabilidade**

Diversos exemplos permitem que o modelo compreenda melhor contextos.

O modelo consegue identificar nuances e exceções nos padrões apresentados.

**Clareza de Instruções**  
Mantém coerência entre diferentes instâncias da mesma tarefa.

**Riqueza Realística**  
Ajuda o modelo a captar estilos, tons e formatos ariados de resposta.

**Redução de Erros**  
Múltiplos exemplos minimizam possibilidades de má interpretação da tarefa.

### Desafios do One-Shot Prompting

**Imitação Excessiva**  
Pode limitar a criatividade do modelo ao fazê-lo imitar demais o exemplo único fornecido.

**Escopo Restrito**  
Um único exemplo dificilmente abrange todas as variações possíveis da tarefa solicitada.

**Elaboração Cuidadosa**  
Exige criação meticulosa do exemplo para evitar ambiguidade ou interpretações errôneas.

### Desafios do Few-Shot Prompting

A curadoria de exemplos exige conhecimento profundo tanto da tarefa quanto do comportamento do modelo de linguagem.

**Consumo de Espaço**  
Requer mais tokens no prompt, limitando o espaço para a consulta principal. (nesse caso, usaremos Retrieval Augmented Generation, RAG. Mas veremos mais a frente)

**Seleção Crítica**  
A escolha dos exemplos é crucial e pode introduzir vieses indesejados.

**Custo Computacional**  
Aumenta o processamento necessário, especialmente em modelos com contexto limitado.

### Limitações Gerais

**Overfitting**  
O modelo pode exagerar na semelhança com os exemplos, reproduzindo características específicas demais e não generalizáveis.

**Extrapolação Limitada**  
Mesmo com exemplos, pode falhar em tarefas completamente novas ou que exijam raciocínio muito diferente dos exemplos.

**Cenários Complexos**  
Em tarefas sofisticadas, até mesmo múltiplos exemplos podem ser insuficientes para capturar todas as nuances.

### Tipos de Operação One-Shot

**Exemplo Direto**  
Um único caso ilustrando claramente a entrada e a saída esperada para a tarefa.

**Exemplo Contextual**  
Inclui contexto situacional antes do exemplo para melhor compreensão do cenário.

**Exemplo de Erro**  
Mostra um erro comum para o modelo entender o que deve evitar em sua resposta.

**Operação Didática**  
Exemplo estruturado para ensinar uma regra específica com explicação detalhada.

### Tipos de Operação Few-Shot

**Exemplos Diversificados**  
Conjunto que cobre várias manifestações diferentes da mesma tarefa ou conceito.

**Exemplos Contrastivos**  
Mostram tanto respostas adequadas quanto inadequadas para destacar diferenças.

**Few-Shot Dinâmicos**  
O modelo seleciona exemplos mais relevantes em tempo real para cada consulta.

**Exemplos Sequenciais**  
Ordenados para demonstrar progressão ou etapas em um processo ou raciocínio.

## Chain-of-Thought (CoT) Prompting para Chatbots de LLMs

### Definição de Chain of Thought (CoT) Prompting

**O Que É?**  
Técnica que orienta modelos de linguagem a gerarem explicações passo a passo antes da resposta final.

Funciona como um “pensar em voz alta” para inteligências artificiais.

Em vez de respostas diretas, o modelo revela cada etapa do pensamento até chegar à conclusão.

<aside>

**Características Principais**

- Instrui o modelo a mostrar raciocínio intermediário
- Exige decomposição explícita do problema
- Promove transparência na geração de respostas e pensamento verbalizado
- Melhora significativamente a precisão  
    </aside>

### Surgimento do CoT: Contexto Histórico

A técnica emergiu como resposta às limitações dos modelos em tarefas de racioncínio complexo.

![Captura de tela 2026-06-24 202308.png](attachment:09eb31c1-3b1e-4aeb-ae65-4898d7e820d1:Captura_de_tela_2026-06-24_202308.png)

### Artigo Seminal

O estudo que mudou nossa compreensão sobre como extrair raciocínio de LLMs.

#### Detalhes do Artigo

“Chain of Thought Prompting Elicits Reasoning in Large Languae Models”

Autores: Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, Brian Ichter, et al.

Publicado como preprint em janeiro de 2022 e posteriormente na NeurIPS

#### Descobertas Principais

Modelos de Grande Escala possuem capacidades latentes de raciocínio.

O raciocínio pode ser extraído através de prompts específicos.

A melhoria e desempenho foi especialmente notável em modelos com mais de 100 Bilhões de parâmetros

### Que Tipos de Problemas Resolve

**Problemas Matemáticos**  
Resolução de questões matemáticas que exigem múltiplos passos sequenciais.

**Ambiguidade Semântica**  
Interpretação de contextos onde existem múltiplos significados possíveis.

**Resolução Lógica**  
Solução de problemas lógicos como quebra-cabeças, condições complexas e inferências.

**Raciocínio Lógico**  
Questões que demandam inferências e deduções encadeadas para chegar à conclusão.

**Multi-hop QUA**  
Respostas a perguntas que exigem conexão de múltiplas informações de diferentes fontes.

**Análise Textual**  
Compreensão profunda de textos com múltiplas camadas de significado e interpretação.

### Perspectiva Cognitiva

#### Relação com o Pensamento Humano

- CoT simula processos cognitivos sequenciais humanos
- Assemelha-se ao System 2 (pensamento lento e analítico, popularizado por Daniel Kahneman, nobel de economia)
- Limitado pela ausência de verdadeira metacognição

Esta técnica alimenta importantes debates sobre as semelhanças e diferenças entre IA e cognição humana.

### Como Funciona o CoT Prompting

**Instrução para Racioncínio**  
O usuário solicita explicitamente que o modelo “pense passo a passo” antes de responder

**Decomposição do Problema**  
O modelo divide a tarefa em subproblemas menores e mais gerenciáveis

**Solução Sequencial**  
Cada etapa de raciocínio é resolvida individualmente e documentada

**Resposta Final**  
Após concluir as etapas, o modelo apresenta a conclusão baseada no raciocínio.

### Estratégias Básicas

**Frases Gatilho Eficazes**  
”Think step-by-step”  
Instrução simples que ativa o modo de raciocínio estruturado no modelo.

**Prompt Elaborado**  
”Take a deep breath and work on this problem step-by-sep”  
Abordagem que simula instruções para pensamento cuidadoso e metódico.

Essas instruções funcionam como gatilhos que ativam modos específicos de processamento nos modelos de linguagem.

### Modelos de Reasoning Associados

O CoT inspirou diversas técnicas complementares para aprimorar o raciocínio dos LLMs.

**Frases Gatilho Eficazes**  
Aplicação em problemas matemáticos complexos que requerem sequencia de operações e transformações.

**Resolução Lógica**  
Solução de problemas lógios como quebra-cabeças, condições complexas e inferências.

**Multi-hop QA**  
Respostas a perguntas que exigem conexão de múltiplas informações de diferentes fontes.

**Análise Textual**  
Compreensão profunda de textos com múltiplas camadas de significado e interpretação.

### Benefícios do CoT Prompting

**Precisão**  
Melhoria significativa na acurácia das respostas em problemas complexos.

**Interpretabilidade**  
Facilita entender como o modelo chegou a determinada conclusão.

**Redução de Erros**  
Diminui falhas lógicas ao forçar verificação de cada passo do raciocínio.

**Confiança**  
Aumenta a transparência e a confiabilidade das respostas geradas.

### Limitações do CoT Prompting

**Custo Computacional**  
Maior consumo de recursos por gerar respostas mais longas e elaboradas.

**Efetividade Variável**  
Nem todas as tarefas se beneficiam igualmente desta abordagem.

**Dependência do Tamanho**  
Eficácia varia significativamente conforme o tamanho e capacidade do modelo.

**Raciocínios Plausíveis mas Incorretos**  
Pode gerar explicações que parecem lógicas mas contêm erros fundamentais.

### Tipos de CoT

**Standard CoT**  
Raciocínio gerado diretamente no próprio prompt principal.

**Few-Shot CoT**  
Utiliza exemplos explícitos de raciocínio no prompt para orientar o modelo.

**Zero-Shot CoT**  
Apenas frases-gatilho são usadas, sem exemplos específicos

**Self-Consistency**  
Gera múltiplas respostas e seleciona a mais comum ou consistente.

**Automatic CoT Generation**  
Usa outro modelo para gerar exemplos de raciocínio automaticamente.

### Usos Indicados

**Resolução de Problemas**  
Ideal para questões que exigem decomposição em etapas menores e sequenciais.

**Educação e Tutoria**  
Perfeito para explicar conceitos complexos com clareza e progressão lógica.

**Agentes Autônomos**  
Essencial para sistemas que precisam tomar decisões sequenciais justificadas.

### Impacto no Treinamento de Modelos Atuais

> **Fine-tuning Especializado**  
> Modelos são refinados com dados contendo raciocínios intermediários explícitos.
> 
> > **Simulação de Pensamento**  
> > Treinamento específico para raciocínio passo-a-passo como característica nativa.
> > 
> > > **Adaptação de Datasets**  
> > > Criação de conjunto de dados que incentivam o prompting baseado em pensamento.

### Comparação com Outras Técnicas de Raciocínio

|Técnica|Característica|
|---|---|
|Tree of Toughts|Explora múltiplas ramificações de raciocínio simultaneamente|
|ReAct|Alterna entre raciocínio e ação de forma iterativa|
|Reflexion|Gera, avalia e revisa raciocínios para aprimoramento|
|CoT|Mais simples e eficiente, mas menos flexível que alternativas|

O CoT pode ser combinado com técnicas mais robustas para resultados superiores em cenários complexos.

## O Papel do Contexto na Criação de Prompts Eficientes

### O Que É “Contexto” em um Prompt

O contexto fornece **informações de fundo** para ajudar o modelo a entender **situação**, **objetivo** e **público-alvo**.

Sem contexto, as respostas tendem a ser genéricas ou desconectadas da intenção do usuário.

**Exemplos de Prompt:**

- “Estou desenvolvendo um curso online para jovens empreendedores. Sugira temas de módulos introdutórios”
- “Como professor de ensino médio, preciso explicar genética para alunos com dificuldades de leitura”

### Por Que o Contexto é Essencial?

**Entendimento da Tarefa**  
Ajuda o modelo a **entender melhor a tarefa**, o estilo de resposta e o **nível de profundidade**.

**Redução de Ambiguidade**  
Reduz ambiguidades e melhora a **relevância e personalização da resposta**.

**Exemplos de Prompts**  
”Vou dar uma palestra pra uma plateia de especialistas sobre blockchain. Como posso explicar o conceito de forma simples?”.  
”Sou redator de conteúdo para o setor de saúde. Preciso de ideias de artigos para o público acima de 60 anos.”

### Tipos Comuns de Contexto

**Perfil do Usuário**  
Profissão, idade, área de atuação

**Finalidade da Resposta**  
Objetivo específico do conteúdo.

**Ambiente de Uso**  
Onde a informação será utilizada.

Exemplos e prompt:

- “Sou gerente de RH em uma empresa de tecnologia. Preciso de uma política interna para trabalho remoto”
- Estou planejando uma viagem com crianças pequenas para a Europa. Me ajude a montar um roteiro amigável para elas.”

### Diferença Entre Contexto e Tarefa

#### Tarefa

> O que o modelo deve fazer (resumir, traduzir, criar, explicar…)

**Resumir**  
Condensar informações em formato mais curto

**Traduzir**  
Converter texto para outro idioma

**Explicar**  
Tornar conceitos compreensíveis

#### Contexto

> Em que **situação** ou para **quem** a tarefa é realizada

**Exemplos de prompt:**

- “Explique o que é inflação **para uma criança de 10 anos** usando exemplos do cotidiano”
- “Crie uma descrição de produto **para um site de e-commerce de artigos de luxo**.”

### Níveis de Contexto (Mínimo a Completo)

**Contexto Mínimo**  
Informação básica

**Contexto Parcial**  
Algumas informações relevantes

**Contexto Completo**  
Informações detalhadas e abrangentes.

Quanto mais **contexto relevante**, melhor a resposta.

Contexto pode ser uma **frase curta** ou um **parágrafo detalhado**.

Exemplos de prompt:

- “Estou escrevendo um e-mail de agradecimento após uma entrevista de emprego na área de marketing”
- “Preciso escrever uma carta formal para cancelar minha matrícula em uma faculdade, sem entrar em detalhes pessoais”

### Dicas para Fornecer um Bom Contexto

**Quem Você É**  
Sua função ou papel

**O Que Você Quer**  
Objetivo Específico

**Para Quem É**  
Público-alvo

**Por Que Precisa**  
Finalidade da Resposta

Evite excesso de informação irrelevante - seja claro e direto.

Exemplos de prompt:

- “Sou coordenador pedagógico e quero criar uma mensagem motivacional para professores no início do semestre”
- “Preciso explicar para investidores iniciantes como funciona os fundos imobiliários em menos de 200 palavras”

### Contexto Implícito x Explícito

**Contexto Implícito**  
Sugerido pela forma do pedido

**Exemplo**: “traduza este e-mail para o inglês” (pode gerar um tom genérico)

<aside>

**Características:**

- Não declarado diretamente
- Sujeito a interpretações
- Pode gerar respostas genéricas  
    </aside>

**Contexto Explícito**  
Diretamente fornecido no texto

**Exemplo**: “traduza este e-mail para o informal de agradecimento para o inglês, mantendo um tom amigável e pessoal”

<aside>

**Características:**

- Claramente declarado
- Reduz ambiguidades
- Gera respostas mais precisas  
    </aside>

## O Papel das Personas na Criação de Prompts Eficientes

### O Que São Prompts de Persona?

**Definição**  
Técnica que atribui papel específico ao modelo de linguagem, também conhecida como “role prompting” ou “act as prompt”.

**Exemplos**  
Especialista em matemática, mentor, figura histórica, personagem fictício ou profissional especializado.

**Objetivo**  
Obter respostas mais alinhadas com a tarefa específica e orientar o modelo para determinado estilo.

### Por Que Usar Personas nos Prompts?

**Aprimeora Tom e Estilo**  
torna respostas criativas mais autênticas e coerentes com o contexto

**Estabelece Guardrails**  
Cria limites naturais para as respostas do sistema

**Respostas com Nuances**  
Proporciona perspectivas mais contextualizadas e abrangentes.

### Padrões de Personas Eficazes

![Captura de tela 2026-06-25 100000.png](attachment:d1f2524f-2c66-4f41-b85c-46a5a272ae76:Captura_de_tela_2026-06-25_100000.png)

### Resultados de Pesquisas Específicas

![Captura de tela 2026-06-25 100317.png](attachment:4d56a6b7-e022-492d-ada8-14a1090b269b:Captura_de_tela_2026-06-25_100317.png)

### Quando Personas são Mais Úteis

**Tarefas Criativas**  
Extremamente eficaz em contextos abertos e criativos.

**Criação de Conteúdo**  
Ideal para definir tom específico em textos.

**Interações de Engajamento**  
Útil para tornar conversas mais naturais.

**Guardrails de Segurança**  
Apoio a limites éticos em respostas.

### Exemplos Práticos de Aplicação

**Escrita Criativa**  
Solicitar conteúdo com tom específico para diferentes públicos.

**Análise Técnica**  
Obter perspectivas de um “especialista” na área desejada.

**Explicações Didáticas**  
Criar conteúdo educacional através de uma persona de “professor”.

**Feedback Diversificado**  
Simular diferentes perspectivas sobre um mesmo tema.

### Melhores Práticas

**Avaliação Crítica**  
Personas não garantem qualidade automaticamente.

**Experimentação**  
Teste diferentes personas para o mesmo problema.

**Uso Consciente**  
Evite em tarefas que exigem precisão factual.

**Detalhamento**  
Prefira personas elaboradas em vez de simples menções.

## Exemplos de Prompts do Tipo Persona

### Prompt 1: Persona Cientista de Dados

`“**Você é um cientista de dados *experiente*** com mais de 10 anos de **experiência em modelagem preditiva**. Explique detalhadamente os principais métodos de regressão (linear, polinomial, ridge, lasso e elastic net) usados em análise preditiva, comparando suas vantagens e limitações. Para cada método, forneça exemplos práticos de aplicação em negócios ou ciência, acompanhados de código completo em Python usando bibliotecas como scikit-leam, pandas e matplotlib. Inclua também métricas de avaliação adequadas para cada modelo e técnicas de validação cruzada.”`

- O tempo de experiência que colocamos pode ser bom ou não. Isso porque se a profissão foi MUITO NOVA, pode gerar confusões na IA, o fazendo dar mais peso para o conhecimento mais antigo da área, e não os mais atuais

### Prompt 2: Persona Consultor de Marketing Digital

`“Assuma o papel de um **consultor de marketing digital especializado em moda e lifestyle***.* Crie um plno detalhado para aumentar o engajamento de uma marca de moda emergente nas redes sociais em 3 meses. O plano deve incluir: análise de presença atual nas plataformas (Instagram, TikTok, Pinterest), estratégia de conteúdo com calendário editorial, tátias de crescimento de seguidores, sugestão de parcerias com influenciadores, métricas de acompanhamento, e orçamento estimado para implementação. Considere que a marca possui um público-alvo entre 18-35 anos e busca aumentaar sua percepção como sustentável e inovadora”`

### Prompt 3: Persona Professor de História

`“Como **professor de história** moderna, ****especializado nos períodos dos **séculos XVIII e XVIX***,* prepare uma aula detalhada de 45 minutos sobre a Revolução Industrial. Sua explicação deve abordar: 1) as principais causas econômicas, tecnológicas e sociais que levaram ao surgimento na Inglaterra; 2) as transformações nos meios de produção e o desenvolvimento das primeiras máquinas; 3) o surgimento da classe operária e as condições de trabalho; 4) as consequências ambientais e urbanísticas; 5) como a revolução se espalhou para outros países europeus e para os EUA. Inclua exemplos de 3-4 inventores/empresários importantes (como James Watt e Richard Arkwright) e suas contribuições específicas. Sugira 2 atividades práticas para envolver os alunos e consolidar o aprendizado, além de 3 questões para discussão em grupo sobre as lições que podemos tirar deste periodo para os desafios atuais de automação e transformação.”`

- Note que nem sempre preciso pôr “atue como”. O LLM entende esmo assim. Entretanto, recomendo que você use marcadores de Markdown ou XML e coloque especificamente a `<persona>` e `<\persona>`. Quanto mais claro para o LLM for a estrutura do seu prompt, ou seja, quais são os componentes que você está usando, melhor a resposta

### Prompt 4: Persona Desenvolvedor Full Stack

`"Você é um **desenvolvedor full stack** experiente com mais de 8 anos trabalhando em **aplicações de alta escala**. Liste as melhores práticas para otimização de performance em aplicações web baseadas em React e Node.js, abordando: 1) Estratégias de renderização e gerenciamento de estado no React; 2) Técnicas de lazy loading e code splitting; 3) Otimização de chamadas API e gerenciamento de cache; 4) Configurações avançadas de webpack e bundling; 5) Monitoramento e métricas, de performance; 6) Estratégias de escalabilidade no backend Node.js. Para cada prática, inclua exemplos de código, ferramentas recomendadas e métricas para medir o sucesso da implementação. Considere tanto aplicações novas quanto a refatoração de código legado."`

### Prompt 4: Persona Coach de Produtividade

`"Atue como um **coach de produtividade** especializado em **trabalho remoto** e **equilíbrio profissional**. Desenvolva uma rotina diária completa para profissionais que trabalham em home office, abordando: 1) técnicas de gerenciamento de tempo com blocos de foco e intervalos estratégicos; 2) organização do espaço físico de trabalho para maximizar concentração e bem-estar; 3) práticas para separar vida profissional e pessoal no mesmo ambiente; 4) rituais de início e encerramento do dia de trabalho; 5) estratégias para lidar com distrações digitais e domésticas. Para cada recomendação, inclua exemplos práticos, ferramentas sugeridas e métricas para acompanhar o progresso. Considere diferentes perfis de trabalho (criativo, analítico, gerencial) e ofereça variações para cada um. Adicione 3 dicas específicas para prevenir burnout e manter motivação a longo prazo."`

## Prompts Multi-Persona

Técnica avançada de engenharia de prompts que explora diferentes perspectivas para resultados mais completos.

Permite obter respostas mais completas e multidimensionais, desenvolvida para ultrapassar as limitações das respostas genéricas de IAs.

**Como Funcionam**  
O método consiste em criar diálogos simulados entre diferentes especialistas ou perspectivas, cada um contribuindo com seu ponto de vista único sobre o mesmo assunto.

**Benefícios**  
Conseguimos que a IA explore diferentes ângulos e níveis e complexidade, resultando em conteúdo mais rico, equilibrado e com menor tendência a vieses unilaterais.

**Aplicações Ideais**  
Particularmente eficazes para análise de temas complexos, tomada de decisões, geração de conteúdo criativo e desenvolvimento de estratégias que exigem múltiplas perspectivas.

### Como Funcionam os Prompts Multi-Personas

**Assunção de Papéis**  
A IA assume simultaneamente diferentes especialistas virtuais

**Contribuição Específica**  
Cada especialista oferece conhecimento de sua área

**Processo Iterativo**  
As personas colaboram entre si em ciclos de refinamento

**Liderança**  
Um participante coordena o processo colaborativo

### Estrutura e um Prompt Multi-Persona

**Identificação das Personas**  
Seleção dos especialistas relevantes para a tarefa específica.

**Comentários Iniciais**  
Cada participante expressa sua abordagem preliminar.

**Colaboração Iterativa**  
As personas discutem e refinam conjuntamente.

**Refinamento Final**  
A solução é aprimorada progressivamente até a versão final.

![Captura de tela 2026-06-25 103704.png](attachment:e1fad42b-7246-40f0-81e5-5249bc523ca4:Captura_de_tela_2026-06-25_103704.png)

**Exemplo:**

`"Atue como um painel de especialistas reunidos para discutir a melhor estratégia para lançar um novo produto tecnológico. Cada um deve contribuir com sua visão específica antes de chegarmos a uma conclusão final. As personas são as seguintes:"`

`Especialista em Marketing: Fale sobre estratégias para posicionar o produto no mercado, diferenciação e público-alvo.`

`Engenheiro de Produto: Explique os desafios técnicos do desenvolvimento e possíveis inovações.`

`Financeiro: Analise os custos e expectativas de retorno sobre o investimento.`

`Especialista em Atendimento ao Cliente: Comente sobre possíveis problemas que os consumidores podem enfrentar e como resolvê-los.`

`Concorrente Simulado: Traga argumentos sobre como uma empresa rival poderia reagir ao lançamento.`

`“Cada persona deve fornecer sua análise inicial, depois discutir os pontos levantados pelos outros, e finalmente chegar a uma recomendação colaborativa."`

`O produto é um novo aplicativo de celular, de produtividade pessoal, que, por intermédio de perguntas, de meia em meia, hora, mapeia o que o usuário faz durante um mês. O usuário precisa responder para que a lA do aplicativo entenda como o usuário gasta seu tempo, quais são suas rotinas e a duração das atividades diárias. Após um mês, o aplicativo apresenta estratégias para o usuário aumentar sua produtividade e foco e passa a cobrar, em intervalos regulares, que o usuário faça tarefas específicas, mantendo o usuário focado nas tarefas.`

- `*“reunidos para discutir a melhor estratégia”*`→ definindo a função
- `*"Cada um deve contribuir com sua visão específica antes de chegarmos a uma conclusão final"`* → Cada especialista é convidado a combinar
- `*“Cada persona deve fornecer sua análise inicial, depois discutir os pontos levantados pelos outros, e finalmente chegar a uma recomendação colaborativa."*` → define a ação de cada persona
    - Você pode fazer isso com múltiplos LLMs para ter uma resposta mais diversificada ainda
- `*"O produto é um novo aplicativo de celular, de produtividade pessoal, que, por intermédio de perguntas, de meia em meia, hora, mapeia o que o usuário faz durante um mês. O usuário precisa responder para que a lA do aplicativo entenda como o usuário gasta seu tempo, quais são suas rotinas e a duração das atividades diárias. Após um mês, o aplicativo apresenta estratégias para o usuário aumentar sua produtividade e foco e passa a cobrar, em intervalos regulares, que o usuário faça tarefas específicas, mantendo o usuário focado nas tarefas"*` → produto a ser discutido

## O Papel dos Cenários na Construção de Prompts

### Cenários vs. Personas

**Cenário**

- Situação, ambiente ou contexto específico para execução da tarefa
- Fornecem ambiente para a execução da tarefa
- Podem conter múltiplas personas interagindo no mesmo contexto

**Persona**

- Papel ou identidade assumida pelo modelo de IA
- Definem comportamento do modelo
- Complementam o cenário estabelecido

### Benefícios dos Cenários na Construção de Prompts

- Aumentam precisão das respostas em até 65%
- Fornecem contexto situacional completo
- Reduzem ambiguidades na interpretação
- Permitem pensamento estruturado e resolução por etapas

### Exemplo 1: Cenário Educacional

**Prompt com Cenário**  
”Imagine um ambiente de sala de aula com 30 alunos do 9º ano apresentando dificuldades em matemática. No geral, a turma apresenta bom desempenho em outras disciplinas. Atue como um consultor pedagógico. Desenvolva um plano de ação detalhado para melhorar o desempenho desses alunos em 45 dias antes das avaliações nacionais.”

<aside>

**Elementos do cenário**

- Contexto específico
- Número de alunos
- Prazo definido
- Objetivo claro  
    </aside>

### Exemplo 2 : Cenário Corporativo

**Prompt com Cenário**  
”Em uma startup de Tecnologia da Informação, com 50 funcionários trabalhando remotamente em 7 países diferentes, ocorreu uma queda de 30% na produtividade após mudanças na liderança. Os 50 funcionários atuam no departamento de Desenvolvimento do Produto. O principal produto da empresa é um sistema inovador de newsletters. Como consultor de gestão, elabore estratégias para reverter essa situação em 90 dias”

<aside>

**Elementos do cenário**

- Tamanho da empresa
- Contexto remoto
- Problema específico
- Prazo determinado  
    </aside>

### Exemplo 3: Cenário Jurídico

**Prompt com Cenário**  
”Em um tribunal de segunda instância, está sendo julgado um recurso envolvendo uma mineradora e comunidades indígenas. Como especialista em direito ambiental, analise os possíveis desdobramentos considerando a legislação brasileira atual.”

<aside>

**Elementos do cenário**

- Ambiente judicial específico
- Partes envolvidas
- Tipo de disputa  
    </aside>

### Como Construir um Cenário Eficaz

1. **Defina o ambiente ou contexto principal**  
    Estabeleça claramente onde a situação ocorre
2. **Estabeleça parâmetros quantificáveis**  
    Tempo, recursos, pessoas envolvidas
3. **Identifique restrições ou desafios específicos**  
    Limitações que afetam a situação
4. **Determine o Objetivo Pretendido**  
    O que se espera alcançar
5. **Inclua informações sobre stakeholders**  
    Quem são as partes interessadas
6. **Especifique o resultado esperado**  
    Como será medido o sucesso

### Construção Iterativa de Cenários

![Captura de tela 2026-06-25 135505.png](attachment:a48513cd-64fe-4a4f-a2e3-b116f4684a9d:Captura_de_tela_2026-06-25_135505.png)

**Prompt Base:** “Crie um cenário detalhado para um prompt de IA através de perguntas incrementais. Comece perguntando sobre o ambiente principal. Em seguida, questione sobre os participantes envolvidos. Depois, explore restrições existentes. Continue perguntando sobre objetivos específicos e métricas de sucesso. Finalmente, refine o cenário combinando todas estas informações”

<aside>

Esse prompt permite refinar o cenário através de interações sucessivas

</aside>

### Impacto dos Cenários Bem Construídos

**Relevância e Precisão**  
Cenários bem estruturados aumentam significativamente a qualidade das respostas da IA, proporcionando resultados mais relevantes e precisos para as necessidades reais dos usuários

**Resolução de Problemas**  
A contextualização detalhada otimiza a abordagem de questões complexas, permitindo que a IA identifique e aplique as estratégias mais eficazes para cada situação específica.

**Estruturação**  
Cenários claros facilitam a geração de respostas sequenciais e bem organizadas, criando um fluxo lógico que guia o usuário através de processos complexos e forma compreensível.

**Aplicabilidade**  
Contextos bem definidos resultam em soluções no mundo real transpondo o abismo entre teoria e prática para criar valor tangível.

## O Uso de Estilo e Tom em Prompts para Guiar a Resposta de um Prompt

### Diferença entre Estilo e Tom

**Estilo**  
Modo de apresentação da informação

**Tom**  
Sentimento ou atitude expressa

Ambos influenciam a recepção e utilidade da resposta.

### Impacto do Estilo e Tom nos Resultados

**Experiência e Engajamento**  
melhoram a experiência e engajamento do usuário.

**Ajuste ao Público**  
Ajustam o conteúdo para o público e objetivo.

**Formalidade e Clareza**  
Determine formalidade, clareza e profundidade.

### Exemplo Prático: Estilo

**Científico**  
”Explique usando linguagem técnica e termos precisos”

**Narrativo**  
”conte como se fosse uma história envolvente.”

**Jornalístico**  
”Relate os fatos de forma imparcial e objetiva”

### Lista de Estilos Populares

#### 1. Estilos Formais

- Acadêmico
- Técnico
- Analítico
- Relatório

#### 3. Estilos Práticos

- Jornalístico
- Conversacional
- Resumido
- Instrucional
- Persuasivo

#### 2. Estilos Criativos

- Narrativo
- Descritivo
- Poético
- Criativo

### Exemplo Prático: Tom

**Formal**  
”Forneça uma análise precisa e detalhada dos dados.”

**Informal**  
”Me conta rapidinho o que tudo isso significa?”

**Inspirador**  
”Escreva de forma a motivar alguém a buscar novos aprendizados”

**Empático**  
”Conforte um usuário que está com dúvidas.”

### Lista de Tons Populares

#### 1. Tons Profissionais

- Formal
- Profissional
- Sério
- Neutro
- Confiante

#### 3. Tons Motivacionais

- Entusiasmado
- Empático
- Instrucional
- Tranquilizador
- Inspirador
- Motivacional

#### 2. Tons Acessíveis

- Informal
- Amigável
- Brincalhão
- Humilde

### Boas Práticas ao Definir Estilo e Tom

**Conheça seu Público-Alvo**  
Identifique as necessidades e preferências dos usuários

**Combine estilo/tom ao objetivo do conteúdo**  
Alinhe suas escolhas com o propósito da comunicação

**Seja explícito no prompt para a maior precisão**  
Forneça instruções claras sobre o estilo e tom desejados

**Teste variações e ajuste conforme a resposta**  
Refine seus prompts com base nos resultados obtidos

### Dica: Use os LLMs para Gerar Listas de Tons e Estilos - Exemplo

`Atue como um engenheiro de prompts. Escreva uma tabela com três colunas. Na primeira coluna, informe palavras-chave que correspondam a tons de resposta populares. Na segunda coluna, escreva uma descrição breve do tom. Na terceira coluna, escreva um prompt útil que usa o tom correspondente. Cada linha da tabela deve se referir a um tom específico. Escreva 20 tons populares.`

# Seção 5: Variáveis, Delimitadores, Tags e Marcadores em Prompts

## O Que São Variáveis e Como Usá-las em Prompts

### O Que São Variáveis em Prompts

**Elementos Dinâmicos**  
As variáveis são elementos dinâmicos que substituem valores específicos em seus prompts, permitindo personalização sem reescrever todo o comenado

**Reutilização Eficiente**  
Permitem criar templates reutilizáveis para instruções, economizando tempo e mantendo consistência em diferentes interações com IA.

**Compatibilidade Universal**  
Funcionam com os principais modelos de IA disponíveis atualmente, como ChatGPT, Gemini e Claude, facilitando o trabalho multi-modelo

### Por Que Utilizar Variáveis?

**Personalização**  
Facilita a criação de resposta personalizadas para diferentes contextos sem reescrever o prompt completo.

**Otimização de Tempo**  
Reduz o tempo gasto na criação de múltiplos prompts semelhantes para tarefas diferentes.

**Consistência**  
Garante uniformidade em tarefas repetitivas, mantendo a estrutura do prompt inalterada

### Estrutura de uma Variável em Prompt

**Formato Comum**

Variáveis são geralmente escritas entre símbolos delimitadores que as distinguem do texto normal.

Os símbolos mais utilizados são chaves, colchetes ou sinais de menor/maior

**Exemplos:**

- {nome}, {{nome}} ou {{{nome}}}
- [cidade]
- $<produto>$$ 

Cada espaço reservado será posteriormente preenchido com o valor desejado.

### Forma Antiga

> **Prompt Template**  
> ”Crie um texto sobre {tema} para iniciantes.

{tema} = história do Brasil”

> > **Variável preenchida**  
> > ”Crie um texto sobre história do Brasil para iniciantes.”
> > 
> > > **Resultado**  
> > > O ChatGPT gerará um texto explicativo sobre história do Brasil voltado para iniciantes.

Existem ferramentas, como o IPRN que é uma ferramenta de gestão de prompts, que entende esse tipo de características. Mas hoje os chatbots entendem bem sobre o que se trata os temas, sem precisar desses sistemas.

### Exemplo Simples

> **Prompt Template**  
> ”Crie um texto sobre {tema} para iniciantes.”
> 
> > **Variável preenchida**  
> > ”Crie um texto sobre história do Brasil para iniciantes.”
> > 
> > > **Resultado**  
> > > O ChatGPT gerará um texto explicativo sobre história do Brasil voltado para iniciantes.

### Exemplo Com Solicitação Explícita de Pergunta

> **Prompt Template**  
> ”Crie um [conteudo] sobre [animal] com [detalhes]

### Instrução

Antes de executar o prompt, solicite as informações dentro de []  
Faça uma pergunta de cada vez, e aguarde a resposta.”

> > **Resposta da IA**  
> > Compreendido. Para começar, por favor, forneça o tipo de conteúdo que você gostaria de criar (por exemplo, “”artigo de blog”, “tópico de aula”, “descrição para material didático”, etc.):

### Automatização de Variáveis

**Plataformas com Templates**  
Algumas interfaces de IA oferecem recursos nativos para criar e salvar templates com campos editáveis.

**Ferramentas de Terceiros**  
Existem aplicativos específicos para gerenciar prompts com variáveis e preenchê-los rapidamente.

### Boas Práticas

**Nomenclatura Clara**  
Use nomes descritivos para as variáveis, como {nome_cliente} em vez de {n}

**Evite Espaços e Acentos nos Nomes das Variáveis**  
Prática comum em programação, que pode ajudar ao LLM a entender que o elemento é uma variável.

**Simplicidade**  
Evite usar muitas variáveis em um único prompt para não confundir o modelo.

## Exemplo Prático 01 - Email de Agradecimento por Participação em Evento

### Prompt

`Você é um {papel} experiente. Escreva um e-mail de agradecimento para {publico} que participaram do nosso {evento} sobre {topico}

Contexto adicional:

- Duração do evento: {duracao}
- Número de participantes: {participantes}
- Principais tópicos abordados: {topicos_principais}

O e-mail deve:

- Ter um tom {tom}
    
- Incluir {proximos_passos}
    
- Ter no máximo {limite_palavras} palavras`
    
- As duas primeiras linhas já são contexto para gerar um prompt. Mas você pode melhorar ainda amis adicionando um contexto adicional, como foi feito no exemplo
    

### Prompt com Mais uma Informação Adicional

`Você é um {papel} experiente. Escreva um e-mail de agradecimento para {publico} que participaram do nosso {evento} sobre {topico}

Contexto adicional:

- Duração do evento: {duracao}
- Número de participantes: {participantes}
- Principais tópicos abordados: {topicos_principais}

O e-mail deve:

- Ter um tom {tom}
- Incluir {proximos_passos}
- Ter no máximo {limite_palavras} palavras

## INSTRUÇÕES

1. Liste cada um dos itens entre chaves {} e coloque um símbolo = após cada {}. Coloque cada item em uma linha
2. Solicite que o usuário copie a lista e coloque as informações após o símbolo =.
3. Faça as substituições no prompt original e execute o prompt`

### Prompt com Acrescentando um Item a Mais

`Você é um {papel} experiente. Escreva um e-mail de agradecimento para {publico} que participaram do nosso {evento} sobre {topico}

Contexto adicional:

- Duração do evento: {duracao}
- Número de participantes: {participantes}
- Principais tópicos abordados: {topicos_principais}

O e-mail deve:

- Ter um tom {tom}
- Incluir {proximos_passos}
- Ter no máximo {limite_palavras} palavras

## INSTRUÇÕES

1. Liste cada um dos itens entre chaves {} e coloque um símbolo = após cada {}. Coloque cada item em uma linha. Coloque cda item em uma linha e escreva a lista em uma janela de código.
2. Solicite que o usuário copie a lista e coloque as informações após o símbolo =.
3. Faça as substituições no prompt original e execute o prompt`

## Exemplo Prático 02 - Análise de Dados

### Prompt

`Você é um analista de dados sênio. Analise os seguintes dados sobre {{dataset_name}} e forneça insights sobre {{focus_area}}.

Dados: {{data_input}}

## TAREFAS:

1. Identifique {{num_insights}} principais insights
2. Sugira {{num_actions}} ações baseadas nos dados
3. Use um formato {{output_format}}
4. Considere o contexto de {{business_context}}

## INSTRUÇÕES:

1. Liste cada um dos itens entre chaves {{}} e coloque um símbolo = após cada {{}}. Coloque cada item em uma linha
2. Solicite que o usuário copie a lista e coloque as informações após o símbolo =.
3. Faça as substituições no prompt original e execute o prompt`

## Exemplo Prático 03 - Variáveis e Estruturas de Decisão em Prompts

### Prompt

`Escreva na tela:

### MENSAGEM

Criador de Explicações  
Informe os dados a seguir:

### FIM_MENSAGEM

## INSTRUÇÕES

1. Peça para o usuário informar o [assunto] e aguarde
2. Após receber o [assunto] escreva a seguinte mensagem:

### MENSAGEM

Escolha um público  
A) Crianças  
B) Alunos do Ensino Médio  
C) Alunos do Ensino Superior

### FIM_MENSAGEM

Explique [assunto] para o [publico_alvo] de seguinte forma:

### ORIENTAÇÕES

[se] [publico_alvo] = "CRIANÇAS"  
[entao] explique como se estivesse contando uma história infantil.

[se] [publico_alvo] = "ALUNOS DO ENSINO MÉDIO"  
[entao] explique com temos e analogias.

[se] [publico_alvo] = "ALUNOS DO ENSINO SUPERIOR"  
[entao] explique usando termos técnicos, de forma adequada para alunos universitários

### FIM_ ORIENTAÇÕES

Exiba o prompt final e aguarde o usuário solicitar a execução do prompt.`

- Sim, estamos usando os famosos `if`, `else` e `elif`/`else if` da programação

## Delimitadores em Prompts: O Que São e Como Usar

### Por Que Usar Delimitadores em Prompts?

**Clareza e Precisão**  
Evitam confusão entre instrução e conteúdo, criando barreiras visuais claras.

**Destaque**  
Destacam trechos essenciais para o modelo, sinalizando informações prioritárias.

**Eficiência**  
Resultam em respostas mais precisas, reduzindo ambiguidades na interpretação.

### Definição de Delimitador

Delimitadores são símbolos especiais utilizados para separar ou destacar partes específicas em um prompt. Funcionam como fronteiras claras entre diferentes elementos da sua instrução

Podem incluir aspas “”, apóstrofos ``, tracinhos --, hashtags # e outros símbolos que criam distinção visual no texto, como <> e <\>, [ ], [[ ]], [[[ ]]],{}, {{ }}, {{{ }}}

### Benefícios dos Delimitadores em Prompts

**1. Clareza e Estrutura**  
Tornam a estrutura do prompt explícita. Reduzem ambiguidade e melhoram a qualidade das respostas.

**3. Separação de Contexto**  
Estabelecem limites nítidos entre diferentes partes. Evitam que informações distintas se misturem acidentalmente.

**5. Redução de Erros**  
Prompts bem estruturados com delimitadores são menos propensos a gerar erros ou saídas não intencionais, pois o modelo pode compreender mais facilmente os limites pretendidos de cada parte da entrada.

**7. Melhora da Análise (Parsing)**  
Quando empregados corretamente, os LLMs conseguem reconhecer e processar entradas estruturadas com maior precisão. O modelo pode identificar seções distintas do prompt e lidar com cada uma de acordo com seu propósito.

**2. Controle e Precisão**  
Permitem maior controle sobre a interpretação do modelo. Facilitam o processamento correto das informações

**4. Consistência nas Respostas**  
Prompts bem estruturados geram saídas mais consistentes. O modelo reflete a organização fornecida.

**6. Flexibilidade**  
Delimitadores possibilitam a criação de prompts mais complexos que podem incorporar múltiplas instruções ou seções, tornando-os adaptáveis a uma vasta gama de tarefas.

**8. Redução de Ambiguidade**  
Ao assinalar claramente onde as instruções terminam e os exemplos começam, os delimitadores eliminam a confusão sobre o papel de cada componentes do prompt. Essa clareza é particularmente importante para prompts complexos com múltiplas seções.

### Exemplos Comuns de Delimitadores

**Aspas Triplas**  
””” São usadas para destacar blocos de texto, especialmente quando contêm quebras de linha ou formatação especial.

**Backticks (apóstrofos invertidos triplos)**

````São

**Hashtags (derquilhas)**
#### Funcionam como separadores visuais fortes que chamam atenção para seções importantes do prompt

**Hífens**
—- São simples e eficazes para marcar limites entre instruções e exemplos no prompt.

### Exemplo: Delimitando Exemplo para E-mail

**E-mail Original**
Um e-mail de exemplo claramente delimitado com #### permite que o modelo identifique o estilo a ser imitado.

**Prompt com Delimitador**
O modelo entende que deve usar apenas o conteúdo entre os delimitadores como referência para o novo e-mail.

`Escreva um e-mail no estilo do exemplo abaixo marcado por ####:

####
Prezado cliente,
Agradecemos sua confiança em nossos serviços.
Atenciosamente,
Equipe de Suporte.
####

Tema: Confirmação de agendamento`

### Prompt Sem Delimitador

**Sem Fronteiras Claras**
Resuma o seguinte texto em uma frase. A inteligência artificial está transformando diversos setores da economia. Empresas investem em soluções baseadas em IA.

**Possíveis Problemas**
Modelo pode confundir onde termina a instrução e começa o texto a ser resumido, gerando resultados imprecisos

Sem delimitadores, o modelo pode resumir parte da instrução ou interpretar incorretamente seu objetivo. Recomendação: nunca use prompts grandes sem delimitadores

### Delimitadores Hierárquicos

Na linguagem Markdown, os símbolos # permitem criar uma estrutura hierárquica que facilita o processamento pelo LLM:

**# Título Principal**
Um único símbolo # indica o título de maior importância na hierarquia, equivalente ao H1 em HTML

**## Subtítulo**
Dois símbolos ## indicam um nível abaixo do título principal, criando uma seção subordinada.

**### Sub- Subtítulo**
Três símbolos ### marcam o terceiro nível na hierarquia, permitindo uma organização ainda mais detalhada do conteúdo.

Este tipo de marcador além de permiti a indicação de uma separação explícita, apresenta uma hierarquia que facilita o processamento pelo LLM.

### Boas Práticas ao Usar Delimitadores

**Consistência**
Use o mesmo tipo de delimitador ao longo de todo o prompt para evitar confusão.

**Clareza**
Informe explicitamente ao modelo qual delimitador está sendo utilizado na instrução

**Separação**
Mantenha espaços entre delimitadores e conteúdo para melhor visualização.

**Simplicidade**
Escolha delimitadores visualmente distintos do texto.

### Boas Práticas ao Usar Delimitadores

**Ambiguidade**
Não informar claramente qual delimitador está sendo usado, deixando o modelo confuso.

**Mistura**
Utilizar diferentes tipos de delimitadores no mesmo prompt sem explicação.

**Incompletude**
Esquecer de fechar o delimitador, deixando o modelo sem saber onde termina o bloco

## O Que São Tags e Como Usar Tags em Prompts

<aside>

O termo tag é muito comum em programação e também é muito comum na parte de web, porque usa-se tags em HTML para indicar áreas de conteúdo. E como os LLMs são treinados, inclusivo em linguagens de programação, essas informações os ajudam a entender o objetivo do que você quer dizer.

</aside>

### O Que São Tags em Prompts

**Definição**
São marcadores estruturais que utilizam sintaxe similar ao XML para organizar partes de um prompt

**Sintaxe**
Geralmente aparecem como `<tag> conteúdo <\tag>` ou `[TAG]conteúdo[\TAG]`.

**Finalidade**
Delimitam diferentes componentes do prompt, tornando-o mais estruturado e eficaz

### Por Que Usar Tags em Prompts

#### Benefícios Principais

- Aumentam clareza e organização visual
- Facilitam processamento pelo modelo de IA
- Reduzem ambiguidades na interpretação
- Permitem controle mais preciso sobre resultados

> As tags funcionam como sinalizadores que ajudam a IA a identificar rapidamente cada segmento do prompt e seu propósito específico.
> 

Prompts bem estruturados com tags adequadas geralmente produzem respostas mais precisas e relevantes.

### Tipos Comuns de Tags

Recomendo que ponha em inglês

---

`<context>`
Define o cenário ou informação de fundo necessária para o modelo compreender o prompt adequadamente.

`<format>`
Formato de saída do prompt.

`<objective>`
Objetivo que o prompt deve alcançar.

`<restriction>`
O que o LLM **não** deve fazer ao gerar sua resposta.

`<instruction>`
Especifica comandos diretos ou perguntas que o modelo deve executar ou responder.

`<example>`
Fornece exemplos concretos de entradas e saídas esperadas para orientar o modelo.

### Exemplo 01: Estrutura Básica com Tags

`<context>
Você é um assistente especializado em literatura brasileira do século XX.
</context>

<instruction>
Analise as principais características da obra "Grande Sertão: Veredas" em formato de tópicos.
</instruction>
< format>
Utilize marcadores e limite a resposta a 200 palavras.
</format>

<example>
- Obra: "Dom Casmurro"
- Autor: Machado de Assis
- Temas: ciúme, dúvida, sociedade carioca 
</example>`

Este exemplo demonstra como organizar um prompt completo usando diferentes tags para maximizar a eficácia da resposta do modelo.

### Exemplo 02: Tags Sem Delimitação

`<role>Professor Universitário de Ciências da Computação</role>

<task>Explicar o conceito de variáveis em programação.</task>

<audience>alunos iniciantes</audience>

<output_format>
  <definition />
  <analogy />
  <code_example />
</output_format>`  

Este exemplo mostra como tags podem assumir um papel semântico, além da delimitação de conteúdo.

### Boas Práticas no Uso de Tags em Prompts

**Consistência**
Mantenha um padrão consistente de tags em todos seus prompts.

**Clareza**
Use nomes de tags intuitivos que descrevam claramente seu propósito.

**Hierarquia**
Organize tags em ordem lógica, das mais gerais para as mais específicas

#### O Que Fazer:

- Fechar todas as tags corretamente
- Manter tags semânticas e descritivas
- Agrupas informações relacionadas na mesma tag

#### O Que Evitar:

- Tags excessivamente longas
- Instruções contraditórias entre tags
- Repetição desnecessária de informações

A organização visual do seu prompt impacta diretamente a qualidade da resposta do modelo. Tgas bem estruturadas funcionam como um mapa para a IA.

## O Que São Marcadores e Como Usar Marcadores em Prompts

### Definição e Importância de Marcadores

Marcadores são símbolos como **•,** *, - ou : usados para estruturar itens em sequência. Eles não são delimitadores ou tags. 

Eles transformam suas instruções em conteúdo mais digerível para IAs e humanos

<aside>
❌

Muitos autores confundem Delimitadores com marcadores. 

- **Delimitadores** definem áreas.
- **Marcadores** indicam elementos para tratamento específico
</aside>

Os arcadores organizam itens pontuais, não delimitam blocos de conteúdo como fazem tags ou delimitadores.

### Funções dos Marcadores em Prompts

**Estruturação**
Sinalizam listas sequenciais, etapas do prompt e identificadores de elementos dentro de prompt.

**Clareza**
Evitam ambiguidades em tarefas complexas, organizando múltiplas solicitações.

**Destaque**
Ressaltam instruções específicas, impedindo que se percam no corpo do texto.

**Identificação**
Identificam elementos específicos dentro de prompts.

### Tipos Comuns de Marcadores

**Hífen (-)**
Ideal para indicar elementos de listas.

**Asterisco (*)**
Em Markdown, indica um conteúdo como uma palavra ou frase que deve ficar em negrito, o que pode indicar relevância ou importância

**Dois Pontos (:) ou [termo]**
Usado para nomear um elemento específico entro de um prompt.

Também pode assumir um papel semântico

### Exemplo: Simulação de Diálogo

`Persona: [assistente_prestativo]
Um assistente prestativo sempre usa um tom educado e respeitoso ao falar com seus clientes. Ele pergunta aos clientes sobre todos os detalhes relevantes e tenta oferecer soluções ou métodos alternativos.

Persona: [cliente_insatisfeito]
Um cliente insatisfeito está um pouco irritado. Ele está desapontado porque suas expectativas não foram atendidas.

### TAREFA:
Escreva um pequeno diálogo entre 
[assistente_prestativo] e [cliente_insatisfeito]
sobre um sistema.`

- `Persona:` tem o marcador `:`.
- O uso de `[]` também dá ênfase nas características:
    - Para o assistente, alguém prestativo
    - Para o cliente, alguém insatisfeito

### Exemplo: Extração de Componentes de um Texto

`### Tarefa
Siga os passos para extrair as informações do texto fornecido.

### Texto:
Em uma manhã ensolarada de verão, Marcelo e Rafael decidiram aproveitar o clima
agradável para fazer uma caminhada. Eles moravam juntos em uma espaçosa mansão
antiga, localizada nos arredores de uma pequena cidade do interior, cercada por uma
floresta densa e misteriosa. Com garrafas de água, chapéus e uma cesta de piquenique, os dois amigos seguiram por uma trilha sombreada, entre árvores altas e vegetação umida, onde o cheiro de terra molhada era intenso e o som dos pássaros ecoava ao longe. Depois de cerca de quarenta minutos de caminhada, chegaram a um belo lago de águas calmas e cristalinas. O sol refletia na superficie como se fosse um espelho. Ali, sentaram-se sob a sombra de uma árvore frondosa e abriram a cesta. Comeram com prazer uma refeição deliciosa: pães frescos, queijos, frutas e suco gelado. A tranquilidade do lugar fazia com que o tempo parecesse parar.

### Passos:
Identifique os substantivos 1. no texto.
2. Identifique os adjetivos no texto.
3. Identifique os verbos no texto.
4. Informe o total de cada elemento identificado`

- Delimitador e Marcador aqui se fundem para ter uma mesma função

### Quando Utilizar Marcadores em Prompts?

1. **Listas e Enumerações**
Use ao solicitar múltiplas instruções ou itens relacionados que precisam de organização visual.
2. **Procedimentos Passo a Passo**
Aplique para exibir sequências de ações que devem ser seguidas em ordem específica.
3. **Divisão de Ideias**
Empregue em respostas que exigem separação clara entre conceitos ou tópicos distintos.
4. **Identificação de Elementos, Conceitos ou Componentes**
Empregue em respostas que exigem separação clara entre conceitos ou tópicos distintos.

## O Marcador de Fim de Prompt

Quando estamos criando prompts, sua instrução tem informações que não fazem parte das instruções do prompt, mas que por algum motivo o LLM interpreta como instruções do prompt, mas são exemplos, conteúdos de continuação… mas ele insiste em pegar aquela informação e associar ao prompt, e às vezes as respostas não ficam muito boas.

Para resolver isso, podemos usar um marcador “especial” (na verdade uma ideia de “marcador especial), que serve para indicar exatamente que o prompt acabou naquele momento, e que o prompt acabou naquele momento, e que o resto é uma informação para apoiar na escrita - uma informação de continuação.

Isso é muito útil em aplicações para chatbots.

### Exemplos:

`Escreva uma estória curta e assustadora <fim_do_prompt/> Era um belo dia de verão…`

- Poderia ter colocado `<end_of_prompt/>` ou `<end/>`, o importante é **ter essa característica de finalização**.
- Após usar essa tag, temos o *elemento de continuação*. Ele significa que eu quero que o prompt comece com a frase “Era um belo dia de verão…”

