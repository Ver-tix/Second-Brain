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