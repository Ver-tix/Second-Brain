---
tags:
  - business
  - marketing
tipo:
  - fonte
dominio:
  - business e administração
  - marketing
Subdominio:
  - business-idea
  - marketing-tático-mix
Sub_subdominio:
  - produto
autor: Steph France
---
# O que é o Gold Mine framework
é um framework de cinco passos que ajuda a desenvolver business ideas

# Passo 1 - Encontre um Mercado para Explorar
A ideia é começar com o que você possui vantagem ou o que você tem interesse,, dentro dos três mercados centrais: ![[Segredo 3 - Os Três Mercados Centrais ou Desejos#OS TRÊS MERCADOS/DESEJOS CENTRAIS]]
- Você escolhe eu nicho/subnicho. Então, use a IA para ajudá-lo a explorar esse mercado.
- A meta aqui é ter uma lista de subnichos

Veja o prompt:

```markdown
## Your mission:
You are a business strategy and market segmentation expert tasked with generating a list of markets, categories, niches or subniches across three core markets: Health, Wealth, and Relationships. For each core market, you will identify relevant subcategories and break them down into detailed sub-niches.
### How to respond based on the user's prompt
* If the user asks for **random ideas**, generate  potential categories, subcategories, niches and sub-niches across all three markets (Health, Wealth, and Relationships).

* If the user asks you to **focus on a specific subcategory**, ONLY generate the submarkets under that subcategory within its corresponding core market.

For example:
* If the user asks you to focus on "alternative medicine", Start with "alternative medicine" (subcategory within the Health market) as the first step in the hierarchy of your ouput and only provide subcategories underneath this one. Do not mention Wealth or Relationships in this case.

## Output format
Your output will contain only the answer, nothing before, nothing after. 
The output should follow this structure:

{

- [Core Market]

- [Category] (as many as you can)

  - [Subcategory] (as many as you can)

    - [Niche] (as many as you can)

		- [Sub-Niche] (as many as you can)

}

## Important rules
* The categories must be based on the core markets Health, Wealth, and Relationships.

* If a specific area of focus is requested by the user (e.g., alternative medicine), ONLY provide subcategories and niches underneath it

* Always provide as many potential categories, subcategories, niches and sub-niches as you can

* Avoid overlap between categories, subcategories, niches and sub-niches; each should be unique to its sub-niche.


## Next steps
Ask the user to provide the market segment they want to explore and wait for the answer
```

# Passo 2 - Valide seu Mercado
1. Nós usamos o buscador Google junto a uma extensão chamada "Keywords Everywhere" para rapidamente encontrar o volume de pesquisa e toda as palavras-chave relacionadas que possam nos orientar até encontrarmos que seja interessante de nos aprofundarmos. Então, comparamos à lista que nós possuímos
2. Você pode pesquisar pelo nicho no Google, usar o "Keywords Everywhere". Após isso, use uma ferramenta como o Google Trends para descobrir se esse nicho está estável (flat, estático, etc.) ou em tendência. A meta é sabermos se está numa tendência.
# Passo 3 - Colete Dados
Agora precisamos encontrar onde as pessoas falam sobre suas:
- Frustrações
- Dores
Em relação ao tema (nicho, subnicho que você quer entrar).

O melhor local para fazer essa pesquisa é o **Reddit**, pois é uma plataforma que mixa: muitas pessoas comentando sobre tudo + conforto da anonimidade. Essa é a combinação perfeita para que as pessoas deem suas opiniões e "abram seus corações" numa rede social.
1. Você pode usar uma query especial com a pesquisa avançada do google para explorar as threads do Reddit onde as pessoas estão falando sobre o problema e expressando suas dores. Eis a estrutura do Query avançado:
	```
	“{Market to explore}" (

   site:reddit.com 

   inurl:comments|inurl:thread 

   | intext:"I think"|"I feel"|"I was"|"I have been"|"I experienced"|"my experience"|"in my opinion"|"IMO"|

   "my biggest struggle"|"my biggest fear"|"I found that"|"I learned"|"I realized"|"my advice"|

   "struggles"|"problems"|"issues"|"challenge"|"difficulties"|"hardships"|"pain point"|

   "barriers"|"obstacles"|"concerns"|"frustrations"|"worries"|"hesitations"|"what I wish I knew"|"what I regret"

)
	```
2. Agora, esse é o momento de você filtrar e escolher sabiamente: veja se os comentários e threads são atuais, por exemplo. 
3. Copie todas as threads (completas) e as ponha em algum documento

# Passo 4 - Processe os Dados
1. Copie o Prompt Pain Point Extractor, e cole na ferramenta de IA que você está usando (use num chat novo).
```
Context
I'm analyzing Reddit conversations to identify common pain points and problems within a specific market. By extracting authentic user language from Reddit threads, I aim to understand the exact problems potential customers are experiencing in their own words. This analysis will help me identify market gaps and opportunities for creating solutions that address real user needs. The extracted insights will serve as the foundation for product development and marketing messages that speak directly to the target audience using language that resonates with them.
Your Role
You are an expert Market Research Analyst specializing in analyzing conversational data to identify pain points, frustrations, and unmet needs expressed by real users. Your expertise is in distilling lengthy Reddit threads into clear, actionable insights while preserving the authentic language users employ to describe their problems.
Your Mission
	1	Carefully analyze provided Reddit conversations and comments
	2	Identify distinct pain points, problems, and frustrations mentioned by users
	3	Extract and organize these pain points into clear categories
	4	For each pain point, include all direct quotes from users that best illustrate this specific problem
	5	Extract EVERY valuable pain point - thoroughness is crucial
Analysis Criteria
INCLUDE:
	•	Specific problems users are experiencing (e.g., "I've tried 5 different migraine medications and none of them work for more than a few hours")
	•	Frustrations with existing solutions (e.g., "Every budgeting app I've tried forces me to categorize transactions manually which takes hours")
	•	Unmet needs and desires (e.g., "I wish there was a way to automatically track my water intake without having to log it every time")
	•	Workarounds users have created (e.g., "I ended up creating my own spreadsheet because none of the existing tools track both expenses and time")
	•	Specific usage scenarios where problems occur (e.g., "The pain is worst when I've been sitting at my desk for more than 2 hours")
	•	Emotional impact of problems (e.g., "The constant back pain has made it impossible to play with my kids, which is devastating")
DO NOT INCLUDE:
	•	General discussion not related to problems or pain points
	•	Simple questions asking for advice without describing a problem
	•	Generic complaints without specific details
	•	Positive experiences or success stories (unless they contrast with a problem)
	•	Discussions about news, politics, or other topics unrelated to personal experiences
Output Format
	1	Pain Point Analysis Summary: Begin with a brief overview of the major pain points identified across the data
	2	Categorized Pain Points: Organize findings into clear thematic categories (e.g., "Problems with Existing Solutions", "Physical Symptoms", "Emotional Challenges")
	3	For each pain point:
	◦	Create a clear, descriptive heading that captures the essence of the pain point
	◦	Provide a brief 1-2 sentence summary of the pain point
	◦	List 3-5 direct user quotes that best illustrate this pain point
	◦	Include a note on the apparent frequency/intensity of this pain point across the data
	4	Priority Ranking: Conclude with a ranked list of pain points based on:
	◦	Frequency (how often mentioned)
	◦	Intensity (emotional language, urgency)
	◦	Specificity (detailed vs. vague)
	◦	Potential solvability (could a product or service address this?)
Examples
Good Pain Point Extraction:

{
Users struggle to find ergonomic desk setups that fit in apartments or small rooms while remaining affordable.
	•	"I've measured every corner of my 450 sq ft apartment and can't find a standing desk that would fit without blocking my only window."
	•	"Spent $300 on a 'compact' desk that still takes up half my bedroom and wobbles whenever I type."
	•	"Living in a tiny NYC apartment means choosing between a proper desk setup or having space to walk around. Currently using my kitchen counter which is killing my back."
	•	"Every ergonomic chair I've found is massive and designed for spacious offices, not tiny home workspaces."
Frequency/Intensity: High frequency (mentioned in ~40% of comments), with intense frustration expressed through language like "impossible," "nightmare," and "giving up."
Output Instructions
	•	First, scan the entire Reddit data to identify recurring themes and pain points
	•	Create relevant category headers based on these pain points
	•	Extract ONLY specific problems, frustrations, and unmet needs
	•	For each pain point, include the most illustrative direct quotes from users
	•	Extract EVERY SINGLE valuable pain point that matches the criteria
	•	Preserve the EXACT original language - no modifications to user text
	•	Rank the pain points based on apparent importance to users
	•	If a potential solution is frequently mentioned or requested, note this in your analysis
Paste your Reddit data below:

```
1. Pegue todos os dados da