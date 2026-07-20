---
tags:
  - marketing
  - operacional
  - canais
---
### O que é taxa de rejeição?

Bounce Rate é uma métrica que mede o quão engajados os visitantes estão com uma página da web, indicando a porcentagem de usuários que saem sem realizar nenhuma ação, como clicar em um link ou interagir com o conteúdo.

![“Taxa de salto” definida](https://api.backlinko.com/app/uploads/2025/01/bounce-rate-defined.png "“Taxa de rejeição” definida")

A taxa de rejeição é importante por três motivos principais:

1. Alguém que sai do seu site (obviamente) não converteu. Então, quando você impede que um visitante salte, você também pode aumentar sua taxa de conversão.
2. Bounce Rate pode ser usado como [fator de classificação do Google](https://backlinko.com/google-ranking-factors "Fatores de classificação do Google"). De fato, um [estudo do setor](https://backlinko.com/search-engine-ranking "Classificação dos motores de busca") descobriu que a taxa de rejeição estava intimamente correlacionada com as classificações de primeira página do Google.  
    
    ![A taxa de rejeição está intimamente correlacionada com as classificações da primeira página do Google](https://api.backlinko.com/app/uploads/2025/01/bounce-rate-is-closely-correlated-to-first-page-google-rankings.png "A taxa de rejeição está intimamente correlacionada com as classificações da primeira página do Google")
    
3. Uma alta taxa de rejeição permite que você saiba que seu site (ou páginas específicas dele) tem problemas com conteúdo, experiência do usuário, layout de página ou [redação](https://backlinko.com/copywriting-guide "Guia de redação publicitária").

### Qual é a taxa de rejeição “média”?

De acordo com [um relatório no GoRocketFuel.com](https://www.gorocketfuel.com/the-rocket-blog/whats-the-average-bounce-rate-in-google-analytics/ "Relatório GoRocketFuel.com"), **a taxa média de rejeição varia entre 41 e 51%**.

![A taxa média de rejeição varia entre 41-51%](https://api.backlinko.com/app/uploads/2025/01/the-average-bounce-rate-range-is-between-41-51-percent.png "A taxa média de rejeição varia entre 41-51%")

No entanto, a taxa de rejeição de um site “normal” depende muito do seu setor e de onde vem o tráfego.

Por exemplo, o Custom Media Labs descobriu que [diferentes tipos de sites tinham taxas de rejeição completamente diferentes](https://www.customedialabs.com/blog/bounce-rates/ "Diferentes tipos de sites tinham taxas de rejeição completamente diferentes").

![A taxa média de rejeição difere entre as categorias do site](https://api.backlinko.com/app/uploads/2025/01/average-bounce-rate-differs-between-website-categories.png "A taxa média de rejeição difere entre as categorias do site")

Como você pode ver, os sites de comércio eletrônico apresentam a menor taxa média de rejeição (20-45%). Enquanto blogs e têm uma taxa de rejeição que vai até 90%.

Portanto, se você deseja descobrir o que é uma boa taxa de rejeição, certifique-se de comparar seu site com outros sites de sua categoria.

Além disso, as fontes de tráfego do seu site podem impactar drasticamente as taxas de rejeição do seu site.

[A ConversionXL descobriu que](https://conversionxl.com/guides/bounce-rate/benchmarks/ "O tráfego de e-mail e referência tem a menor taxa de rejeição") o tráfego de e-mail e referência tinha a menor taxa de rejeição.

![Diferenças na taxa de rejeição por fonte de tráfego](https://api.backlinko.com/app/uploads/2025/01/bounce-rate-differences-by-traffic-source.png "Diferenças na taxa de rejeição por fonte de tráfego")

Por outro lado, anúncios gráficos e tráfego de mídia social tendem a ter uma taxa de rejeição muito alta.

## Como funciona a taxa de rejeição no Google Analytics 4?

A principal diferença entre a taxa de rejeição no Universal Analytics (UA) e no [Google Analytics 4 (GA4)](https://backlinko.com/google-analytics-4) é como o engajamento é medido.

No UA, a taxa de rejeição é baseada exclusivamente na interação do usuário, enquanto o GA4 considera visualizações de página, duração da sessão e eventos de conversão. A abordagem do GA4’ fornece uma visão mais holística do envolvimento do usuário.  
A taxa de rejeição é calculada como a porcentagem de sessões que não foram engajadas. Uma sessão é considerada engajada se atender a algum dos seguintes critérios:

1. Duração superior a 10 segundos
2. Acionou um evento de conversão
3. Gerou pelo menos duas visualizações de página ou visualizações de tela

Se uma sessão não atender a nenhum desses critérios, ela será classificada como não engajada ou devolvida. Portanto, a taxa de rejeição no GA4 é o inverso da taxa de engajamento.

|Recurso|Análise Universal|Google Analytics 4|
|---|---|---|
|Tempo limite da sessão|Nenhum|10 segundos (padrão)|
|Eventos interativos para evitar saltos|Qualquer evento após o primeiro|Eventos de conversão ou eventos de segunda visualização|
|Cálculo da taxa de rejeição|Sessões com apenas uma visualização de página e sem interação adicional|Sessões que não estão engajadas|

### Taxa de rejeição vs. Taxa de saída

[A taxa de saída é semelhante à taxa de rejeição](https://support.google.com/analytics/answer/2525491?hl=en&ref_topic=6156780 "A taxa de saída é semelhante à taxa de rejeição"), com uma grande diferença:

Bounce Rate é a porcentagem de pessoas que chegam a uma página e saem.

Taxa de saída é a porcentagem de pessoas que saem de uma página específica (mesmo que inicialmente não tenham chegado a essa página).

Por exemplo, digamos que alguém chega à Página A do seu site. E eles apertaram o botão Voltar do navegador alguns segundos depois.

![Taxa de salto – Visual](https://api.backlinko.com/app/uploads/2025/01/bounce-rate-visual.png "Taxa de salto – Visual")

Isso é um salto.

Por outro lado, digamos que alguém chegue à Página A do seu site. Em seguida, eles clicam na página B.

Então, depois de ler a Página B, eles fecham o navegador.

![Taxa de saída](https://api.backlinko.com/app/uploads/2025/01/exit-rate.png "Taxa de saída")

Como essa pessoa clicou em algo na página A, isso não é um salto na página A. E como eles não chegaram inicialmente na Página B, também não é um salto na Página B.

Dito isso, como essa pessoa saiu do seu site na Página B, isso AUMENTARÁ a Taxa de Saída da Página B no [Google Analytics](https://analytics.google.com/analytics/web/ "Google Analytics").

E se você notar uma página no seu site com uma taxa de saída super alta, esse é um problema que vale a pena corrigir.

Com isso, aqui está uma comparação lado a lado entre Bounce Rate e Exit Rate.

![Taxa de rejeição vs. Taxa de saída](https://api.backlinko.com/app/uploads/2025/01/bouce-rate-vs-exit-rate.png "Taxa de rejeição vs. Taxa de saída")

Aqui está uma tabela resumindo as principais diferenças:

|Recurso|Taxa de rejeição|Taxa de saída|
|---|---|---|
|Escopo|Primeira página de uma sessão|Qualquer página em uma sessão|
|Propósito|Mede o engajamento inicial|Indica eficácia da página|
|Interpretação|Alta taxa na landing page sugere que é necessária melhoria|Alta taxa na página de contato pode ser positiva, indicando conclusão da tarefa|

### Por que as pessoas saltam?

Antes de entrarmos nas etapas específicas para reduzir sua taxa de rejeição, é importante entender os motivos mais comuns pelos quais as pessoas rejeitam.

**Page não atendeu às expectativas:** Por exemplo, digamos que você esteja procurando um novo liquidificador. Então você pesquisa no Google “compre liquidificadores com frete grátis”.

![Resultados da pesquisa "compre liquidificadores com frete grátis"](https://api.backlinko.com/app/uploads/2025/01/buy-blenders-free-shipping-search-results.png "“comprar liquidificadores frete grátis” resultados da pesquisa")

Você vê um anúncio que diz “Liquidificadores com frete grátis”.

!["compre liquidificadores com frete grátis" anúncio do Google](https://api.backlinko.com/app/uploads/2025/01/buy-blenders-free-shipping-google-ad.png "“compre liquidificadores com frete grátis” Anúncio do Google")

Então você clica nele.

Mas quando você clica no anúncio, em vez de uma land[ing page](https://backlinko.com/landing-page-guide) sobre diferentes liquidificadores, você está na página inicial do site.

![Exemplo de página inicial](https://api.backlinko.com/app/uploads/2025/01/homepage-example.png "Exemplo de página inicial")

O que você vai fazer? Volte ao Google para encontrar uma página 100% sobre liquidificadores.

**Design feio:** um design feio pode acabar com sua taxa de rejeição. As pessoas julg[am seu site principalmente com base no design](http://credibility.stanford.edu/guidelines/index.html "As pessoas julgam seu site em grande parte com base no design primeiro")… e no conteúdo em segundo lugar.

Então se o seu site se parece com isso…

![Site feio](https://api.backlinko.com/app/uploads/2025/01/ugly-website.png "Site feio")

…você pode esperar uma taxa de rejeição realmente alta.

**UX ruim:** Sim, seu site deve ter uma boa aparência. Mas seu site também precisa ser super fácil de usar. E quanto mais fácil for para as pessoas lerem e navegarem em seu site, menor será geralmente sua taxa de rejeição.

**A página fornece aos usuários o que eles procuram:** isso mesmo. Nem todos os saltos são “ruins”. Na verdade, um salto pode ser um sinal de que sua página deu a alguém exatamente o que ele queria.

Por exemplo, digamos que você esteja procurando uma nova receita de berinjela assada.

E você chega nesta página de receitas:

![Página de receitas](https://api.backlinko.com/app/uploads/2025/01/recipe-page.png "Página de receitas")

Esta página tem tudo o que você precisava para fazer esta receita: ingredientes, instruções detalhadas e fotos.

Então, assim que você coloca sua berinjela no forno, você fecha a página.

Embora essa sessão de página única seja tecnicamente um “salto”, não é porque o site sofria de design feio ou UX ruim. É porque você conseguiu o que precisava.

### Como melhorar sua taxa de rejeição

**1. Incorpore vídeos do YouTube em sua página**

A empresa de hospedagem de vídeos Wistia descobriu que adicionar vídeos às suas páginas [mais que dobrou o tempo médio de permanência na página](https://wistia.com/learn/marketing/video-time-on-page "Adicionar vídeos mais que dobrou o tempo médio de Wistia na página").

![Adicionar vídeos dobrou o tempo médio na página](https://api.backlinko.com/app/uploads/2025/01/adding-videos-doubled-average-time-on-page.png "Adicionar vídeos dobrou o tempo médio na página")

Também notamos que incorporar vídeos resulta em uma menor taxa de rejeição e maior tempo na página.

De fato, analisamos recentemente a diferença na taxa de rejeição para páginas com e sem vídeo incorporado.

E os dados mostram que páginas com vídeo tiveram uma taxa de rejeição significativamente menor (11%) em comparação com páginas sem vídeo.

![Vídeos incorporados podem diminuir a taxa de rejeição – Visual](https://api.backlinko.com/app/uploads/2024/10/embedded-videos-can-decrease-bounce-rate-visual.png "Vídeos incorporados podem diminuir a taxa de rejeição – Visual")

Além disso, tenha em mente que esses vídeos **não precisam necessariamente ser seus vídeos**.

Você pode incorporar QUALQUER vídeo do YouTube que faça sentido para sua página.

**2. Brigadas Sprinkle In Bucket**

Bucket Brigades são uma das melhores maneiras de melhorar sua taxa de rejeição em landing pages e postagens de blog.

Veja como funciona:

Primeiro, encontre uma seção da sua página que não seja muito atraente.

(Eu chamo essas seções de “Zonas Mortas”)

Praticamente todas as páginas da internet têm essas pequenas “zonas mortas” onde os usuários ficam entediados e clicam.

![Zonas mortas](https://api.backlinko.com/app/uploads/2025/01/dead-zones.png "Zonas mortas")

O segundo passo é adicionar uma frase da Bucket Brigade que se destaque e mantenha sua atenção.

Eis um exemplo de uma das minhas páginas:

![Frase da Brigada de Baldes](https://api.backlinko.com/app/uploads/2025/01/bucket-brigade-phrase.png "Frase da Brigada de Baldes")

Vês como isso funciona?

A frase “Na verdade:” faz com que o leitor se interesse pela próxima linha.

E quando você adiciona um punhado de Bucket Brigades ao conteúdo do seu site, você **mantém as pessoas lendo sua página**.

(O que pode reduzir significativamente sua taxa de rejeição).

Aqui estão mais alguns exemplos de Brigadas de Baldes que você pode experimentar:

- Veja isto:
- A pergunta é:
- Com isso…
- Isso me fez pensar:
- E esta estatística confirma isso:
- Uma história rápida…

**3. Velocidade de carregamento**

Uma análise do Google de 11 milhões de landing pages descobriu que a baixa velocidade de [carregamento estava correlacionada com maiores taxas de rejeição](https://www.thinkwithgoogle.com/marketing-resources/data-measurement/mobile-page-speed-new-industry-benchmarks/ "Velocidade de carregamento lenta correlaciona-se com taxas de rejeição mais altas").

![Velocidade de carregamento de página mais lenta equivale a taxas de rejeição mais altas](https://api.backlinko.com/app/uploads/2024/12/slower-page-load-speed-equals-higher-bounce-rates.png "Velocidade de carregamento de página mais lenta equivale a taxas de rejeição mais altas")

Isso não deveria ser uma surpresa. Afinal, as pessoas online são SUPER impacientes.

Com isso, aqui estão algumas maneiras de acelerar as coisas.

Seu primeiro passo é reunir referências de como você está se saindo em termos de velocidade.

Recomendo a ferramenta gratuita e útil Page[Speed Insights do Google](https://developers.google.com/speed/pagespeed/insights/ "Ferramenta PageSpeed Insights").

![Ferramenta PageSpeed Insights](https://api.backlinko.com/app/uploads/2025/01/page-speed-insights-tool.png "Ferramenta PageSpeed Insights")

Esta ferramenta dá à sua página uma pontuação de velocidade com base no código da sua página e na rapidez com que ela carrega para os usuários do Chrome.

![PageSpeed Insights – Resultados do Backlinko](https://api.backlinko.com/app/uploads/2025/01/page-speed-insights-backlinko-results.png "PageSpeed Insights – Resultados do Backlinko")

É bom saber a pontuação. Mas não é muito útil por si só.

Para aproveitar ao máximo esta ferramenta, confira as recomendações específicas (chamadas “Oportunidades”) para agilizar sua página.

![Insights do PageSpeed – Oportunidades](https://api.backlinko.com/app/uploads/2025/01/page-speed-insights-opportunities.png "Insights do PageSpeed – Oportunidades")

Por exemplo, você pode ver que muitos dos problemas de velocidade de carregamento da nossa página inicial são devidos a imagens grandes.

![PageSpeed Insights – Velocidade de carregamento da imagem](https://api.backlinko.com/app/uploads/2025/01/page-speed-insights-image-loading-speed.png "PageSpeed Insights – Velocidade de carregamento da imagem")

Agora que você tem uma pontuação de referência e dicas sobre como melhorar, siga estas práticas recomendadas para acelerar a velocidade de carregamento do seu site:

- **Compactar imagens: as** imagens [são um dos principais motivos pelos quais as páginas carregam lentamente](https://think.storage.googleapis.com/docs/mobile-page-speed-new-industry-benchmarks.pdf "As imagens são uma das principais razões pelas quais as páginas carregam lentamente"). Isso não quer dizer que você deva começar a remover imagens a torto e a direito. Eles servem a um propósito. Em vez disso, use uma ferramenta de compactação de imagem (usamos o Kra[ken Image Optimizer](https://kraken.io/ "Otimizador de imagem Kraken")) para reduzir drasticamente o tamanho da nossa imagem.
- **Use um provedor de hospedagem rápida: seu** host pode aumentar ou diminuir a velocidade de carregamento do seu site. Então, se você ainda tem um plano barato de US$ 5/mês, considere subir de nível para um anfitrião legítimo.
- **Remova plug-ins e scripts não utilizados:** use uma ferramenta como [o WebPageTest](https://www.webpagetest.org/ "Teste de página da Web") para obter uma lista de recursos que tornam sua página mais lenta.  
    
    ![Teste de página da Web – Resultados detalhados](https://api.backlinko.com/app/uploads/2025/01/web-page-test-detailed-results.png "Teste de página da Web – Resultados detalhados")
    

E exclua tudo o que você não usa ou não precisa.  

**4. Use o modelo de introdução PPT**  
Muitas pessoas decidem sair ou permanecer na sua página com base no que veem “[acima da dobra](https://en.wikipedia.org/wiki/Above_the_fold "Acima da dobra")”.

![Acima da dobra](https://api.backlinko.com/app/uploads/2025/01/above-the-fold.png "Acima da dobra")

É por isso que é SUPER importante chamar a atenção de alguém assim que ele chegar ao seu site.

E uma das melhores maneiras de fazer isso?

**Escreva uma introdução que faça alguém querer continuar lendo.**

Pessoalmente, uso cada vez mais algo chamado “The PPT Template”. Nossos dados internos mostram que é ótimo para reduzir a Bounce Rate. E é super simples de implementar.

![O modelo PPT](https://api.backlinko.com/app/uploads/2025/01/the-ppt-template.png "O modelo PPT")

Como você pode ver, o primeiro “P” em “PPT” significa “Promessa”.

É aí que você promete entregar o que essa pessoa está procurando.

![Backlinko – Postar promessa](https://api.backlinko.com/app/uploads/2025/01/backlinko-get-youtube-views.png "Backlinko – Postar promessa")

Em seguida, você dá a eles “Prova” de que você e seu conteúdo são confiáveis. Você pode citar sua experiência pessoal, resultados de um cliente ou sua educação e credenciais.

Eis um exemplo:

![Backlinko – Postar Prova](https://api.backlinko.com/app/uploads/2025/01/backlinko-post-proof.png "Backlinko – Postar Prova")

Finalmente, termine com uma “Transição”. Essa transição é como uma pequena Brigada de Baldes que os incentiva a rolar a tela para baixo.

![Backlinko – Pós Transição](https://api.backlinko.com/app/uploads/2025/01/backlinko-post-transition.png "Backlinko – Pós Transição")

**5. Torne seu conteúdo super fácil de ler**  
Ou como eu gosto de dizer:

**Difícil de ler = não lê.**

Então, se o seu conteúdo for assim, sua taxa de rejeição vai disparar.

![Parede de texto](https://api.backlinko.com/app/uploads/2024/02/wall-of-text.png "Parede de texto")

Com isso, veja como tornar seu conteúdo fácil de ler (e folhear).

- **Muito espaço em branco:** dê espaço para seu conteúdo respirar. Isso significa usar muito espaço em branco ao redor do seu texto, assim:  
    
    ![Use muito espaço em branco ao redor do conteúdo](https://api.backlinko.com/app/uploads/2025/01/use-lots-of-white-space-around-content.png "Use muito espaço em branco ao redor do conteúdo")
    
- **Parágrafos deslizáveis:** divida parágrafos grandes em pedaços de 1 a 2 frases.  
    
    ![Parágrafos desnatáveis](https://api.backlinko.com/app/uploads/2025/01/skimmable-paragraphs.png "Parágrafos desnatáveis")
    
- **Fonte 15-17px:** menor que isso e as pessoas precisam apertar e dar zoom em seus celulares.
- **Subtítulos de seção:** use subtítulos para dividir seu conteúdo em seções discretas. Isso torna mais fácil para as pessoas folhearem seu conteúdo.  
    
    ![Tamanhos e subtítulos das fontes](https://api.backlinko.com/app/uploads/2025/01/font-sizes-and-subheadings.png "Tamanhos e subtítulos das fontes")
    

**6. Satisfazer a intenção de pesquisa**

O Google é (de longe) [a principal fonte de tráfego online](https://sparktoro.com/blog/the-powerhouses-of-the-internet-are-turning-hostile-to-websites/ "O Google é a fonte de tráfego online número 1").

![O Google é de longe a fonte de tráfego número um](https://api.backlinko.com/app/uploads/2025/01/google-is-by-far-the-nnumber-one-traffic-source.png "O Google é de longe a fonte de tráfego número um")

É por isso que é super importante que todas as suas principais páginas de conteúdo e páginas de destino satisfaçam a [Intenção de Pesquisa](https://backlinko.com/hub/seo/search-intent "Intenção de pesquisa").

(Em outras palavras: sua página deve dar aos pesquisadores do Google o que eles estão procurando).

Caso contrário, os usuários do Google retornarão aos resultados da pesquisa.

![Nenhum clique na página antes de sair = "Bounce"](https://api.backlinko.com/app/uploads/2025/01/no-clicks-on-page-before-leaving-bounce.png "Nenhum clique na página antes de sair = “Bounce”")

E uma página que não atende à Intenção de Pesquisa não é ruim apenas para sua Taxa de Rejeição. É ruim para [SEO](https://backlinko.com/seo-this-year "SEO") também.

Na verdade, uma alta taxa de rejeição e um baixo [tempo de permanência](https://backlinko.com/hub/seo/dwell-time "Tempo de permanência") podem realmente prejudicar sua classificação no Google.

![Alta taxa de rejeição pode prejudicar suas classificações no Google](https://api.backlinko.com/app/uploads/2025/01/high-bounce-rate-can-hurt-your-google-rankings.png "Alta taxa de rejeição pode prejudicar suas classificações no Google")

Um bom exemplo disso é uma palavra-chave como “melhores [ferramentas de SEO](https://backlinko.com/best-free-seo-tools "Melhores ferramentas de SEO")”.

Como você pode ver nos resultados da pesquisa, praticamente todos os resultados são uma lista de ferramentas que as pessoas usam e recomendam.

![Google SERP – Melhores ferramentas de SEO](https://api.backlinko.com/app/uploads/2025/01/google-serp-best-seo-tools.png "Google SERP – Melhores ferramentas de SEO")

Por outro lado, uma palavra-chave como “verificador de SEO” traz ferramentas reais… não uma lista dos favoritos de alguém:

![Verificador de SEO – Google SERP](https://api.backlinko.com/app/uploads/2025/01/google-serp-seo-checker.png "Verificador de SEO – Google SERP")

Então, se eu criasse uma página listada “meus 15 verificadores de SEO favoritos”, eu teria 0% de chance de classificação para essa palavra-chave.

Por que?

Essa lista de ferramentas não satisfaria a Intenção de Pesquisa.

Se você quiser saber mais sobre Search Intent, recomendo ler [este estudo de caso aprofundado de SEO](https://backlinko.com/skyscraper-technique-2-0 "Estudo de caso de SEO").

![Backlinko – Técnica do Arranha-céu 2.0](https://api.backlinko.com/app/uploads/2025/01/backlinko-skyscraper-technique-2-0.png "Backlinko – Técnica do Arranha-céu 2.0")

**7. Transforme burros em unicórnios**  
Não importa o quanto você trabalhe duro em sua taxa de rejeição, você terá páginas com uma taxa de rejeição muito ruim (“burros”).

Você também terá páginas com uma taxa de rejeição muito boa (“Unicórnios”).

E transformar esses burros em unicórnios é uma das maneiras mais fáceis de melhorar sua taxa de rejeição.

Vamos decompô-lo.

Primeiro, faça login na sua conta do Google Analytics e vá para Explorar.

![GA4 – Explorar](https://api.backlinko.com/app/uploads/2024/02/ga4-explore.png "GA4 – Explorar")

Adicione o Título da Página e o Nome da Tela (ou Caminho da Página) como uma dimensão e inclua Sessões Engajadas por Usuário e Tempo Médio de Engajamento por Sessão como métricas.

![GA – Explorar – Reportar](https://api.backlinko.com/app/uploads/2025/01/ga-explore-report.png "GA – Explorar – Reportar")

Agora, crie um filtro (isso funciona como uma comparação). Na guia Configurações, procure a opção Filtro e configure-a para mostrar páginas onde as Sessões Engajadas por Usuário ou o Tempo Médio de Engajamento por Sessão estão abaixo da média do seu site.

![GA – Explorar – Aplicar filtro](https://api.backlinko.com/app/uploads/2025/01/ga-explore-apply-filter.png "GA – Explorar – Aplicar filtro")

Execute o relatório e veja as páginas com as métricas mais fracas??

![GA – Explorar – Relatório filtrado](https://api.backlinko.com/app/uploads/2025/01/ga-explore-filtered-report.png "GA – Explorar – Relatório filtrado")

Esses são burros. E quando você se concentra em melhorá-los, você pode mudar a taxa de rejeição geral do seu site em pouco tempo.

Por exemplo, posso ver que esta [lista de estatísticas de SEO](https://backlinko.com/seo-services-statistics "Estatísticas de SEO") tem uma alta taxa de rejeição.

![/seo-services-statistics – Taxa de rejeição](https://api.backlinko.com/app/uploads/2019/09/seo-services-statistics-bounce-rate.png "/seo-services-statistics – Taxa de rejeição")

E quando olho para essa página, vejo algumas maneiras de melhorar o conteúdo.

Por exemplo, listo 10 estatísticas logo após a seção de introdução.

![Backlinko – Estatísticas de serviços de SEO – Lista](https://api.backlinko.com/app/uploads/2025/01/backlinko-seo-services-statistics-list.png "Backlinko – Estatísticas de serviços de SEO – Lista")

Talvez seja melhor remover esta seção para que as pessoas possam entrar direto no cerne da página.

Além disso, tenho alguns parágrafos que são longos.

![Backlinko – Estatísticas de serviços de SEO – Principais conclusões](https://api.backlinko.com/app/uploads/2025/01/backlinko-seo-services-statistics-key-takeaway.png "Backlinko – Estatísticas de serviços de SEO – Principais conclusões")

Dito isto, tudo isso são palpites. Sem dados objetivos, é difícil saber exatamente por que a taxa de rejeição dessa página é tão alta.

Pode ser porque minha página não atende à Intenção de Pesquisa. Ou meu conteúdo é difícil de ler. Ou talvez minha página pareça estranha em tablets.

Tudo isso são palpites.

E sem **dados reais do usuário**, é impossível saber o que está acontecendo.

Então, para ter uma ideia real do motivo pelo qual tantas pessoas saltam de uma página específica, você precisa usar um mapa de calor.

Falando em mapas de calor…

**8. Use dados de mapa de calor para melhorar as principais páginas de destino**

Os mapas de calor são uma ótima maneira de ver como as pessoas usam e interagem com seu site.

(Especialmente se você quiser descobrir por que tantas pessoas estão saltando da sua página)

Existem um milhão e uma ferramentas de mapa de calor por aí.

Mas meus dois favoritos são [CrazyEgg](https://www.crazyegg.com/ "Ovo Louco") e [Hotjar](https://www.hotjar.com/ "Jarra quente").

![CrazyEgg & Hotjar – Colagem de páginas iniciais](https://api.backlinko.com/app/uploads/2025/01/crazyegg-and-hotjar-homepages-collage.png "CrazyEgg & Hotjar – Colagem de páginas iniciais")

Não importa qual ferramenta de mapa de calor você escolher, todas funcionam praticamente da mesma maneira.

Você adiciona um pequeno pedaço de javascript ao seu site. E a ferramenta começará a rastrear como as pessoas leem, clicam e rolam pela sua página.

![Mapa de calor CrazyEgg](https://api.backlinko.com/app/uploads/2025/01/crazy-egg-heatmap.png "Mapa de calor CrazyEgg")

Por exemplo, você pode ver que nesta página do nosso site, MUITAS pessoas clicam naquele link no topo da página.

![CrazyEgg – Link fortemente clicado](https://api.backlinko.com/app/uploads/2025/01/crazy-egg-heavily-clicked-link.png "CrazyEgg – Link fortemente clicado")

Obter esse tipo de engajamento do usuário no topo de uma página é uma ÓTIMA maneira de reduzir sua taxa de rejeição.

Por outro lado, muito poucas pessoas interagem com a nossa barra lateral.

![CrazyEgg – Barra lateral com clique baixo](https://api.backlinko.com/app/uploads/2025/01/crazy-egg-low-click-sidebar.png "CrazyEgg – Barra lateral com clique baixo")

Então, talvez eu queira remover a barra lateral completamente. Se ninguém clicar nela, minha barra lateral é apenas uma distração.

Então sim, como você pode ver, os dados do mapa de calor são SUPER úteis.

**9. Adicione links internos à sua página**

Você provavelmente já sabe que [links internos são ótimos para SEO](https://backlinko.com/hub/seo/internal-links "Links internos").

Mas o que você talvez não saiba é que links internos também podem ajudar a melhorar sua taxa de rejeição.

Por que?

Porque links internos **enviam pessoas para outras páginas do seu site**.

![Envie pessoas para outras páginas do seu site](https://api.backlinko.com/app/uploads/2025/01/send-people-to-other-pages-on-your-site.png "Envie pessoas para outras páginas do seu site")

Em outras palavras, aumenta naturalmente as [visualizações de página](https://backlinko.com/pageviews).

Além disso, assim que alguém visita outra página do seu site, isso não conta mais como um salto.

Por exemplo, uso vários links internos aqui:

![Exemplo de links internos](https://api.backlinko.com/app/uploads/2025/01/internal-links-example.png "Exemplo de links internos")

Como você pode ver, esses links internos não são inseridos ou forçados ali. Esses links internos são projetados para ajudar os usuários a encontrar [conteúdo útil](https://backlinko.com/helpful-content) no meu site.

O fato de que eles também ajudam minha taxa de rejeição e SEO é apenas um bônus.

![](https://api.backlinko.com/app/uploads/2024/06/icon-tip.png)

> **Dica profissional:** abra links internos (e externos) em uma nova aba, assim:
> 
> ![](https://embed-ssl.wistia.com/deliveries/602292dd0f24904a84a47436cb5d369fdf77e994.jpg)

Dessa forma, os usuários não saem da sua página quando clicam em um link.

**10. Impressione os visitantes com um design incrível**

Já mencionei que as pessoas saltam de páginas com design ruim.

Mas o que eu não mencionei é que um design INCRÍVEL pode manter as pessoas grudadas na sua página como uma supercola.

Portanto, se o design do seu site for apenas “OK”, considere investir em um design incrível.

Por exemplo, usamos um design personalizado para nosso guia [de marketing por e-mail](https://backlinko.com/email-marketing-guide "Guia de marketing por e-mail").

![Backlinko – Guia de marketing por e-mail](https://api.backlinko.com/app/uploads/2025/01/backlinko-email-marketing-guide.png "Backlinko – Guia de marketing por e-mail")

E quando você compara esta página com uma postagem ou artigo normal de blog, esse design realmente se destaca.

Esse design profissional é uma das principais razões pelas quais esta página tem uma taxa de rejeição super baixa.

![](https://api.backlinko.com/app/uploads/2025/01/email-marketing-guide-low-bouce-rate.png "guia de marketing por e-mail com baixa taxa de bouce")

**11. Use um índice (com “Jumplinks”)**

Quando se trata de obter links e compartilhamentos sociais para seu conteúdo, [nada supera o conteúdo longo](https://backlinko.com/content-study "Estudo de conteúdo").

![Conteúdo longo gera mais backlinks do que postagens curtas em blogs](https://api.backlinko.com/app/uploads/2024/10/long-form-content-generates-more-backlinks-than-short-blog-posts.png "Conteúdo longo gera mais backlinks do que postagens curtas em blogs")

Dito isto, a forma longa tem um grande problema:

É MUITO difícil encontrar uma dica, estratégia ou passo específico.

Por exemplo, esta [lista de técnicas de SEO](https://backlinko.com/seo-techniques "Técnicas de SEO") tem mais de 6.500 palavras.

![Backlinko – Técnicas de SEO](https://api.backlinko.com/app/uploads/2025/01/backlinko-seo-techniques.png "Backlinko – Técnicas de SEO")

O que significa que encontrar UMA técnica neste post será um pesadelo.

E se alguém não conseguir encontrar o que procura em cerca de 3 segundos, provavelmente vai pular.

Bem, é aí que entra um Índice.

Um índice ajuda os usuários a encontrar INSTANTANEAMENTE o principal que desejam em sua página.

![Backlinko – Técnicas de SEO – Índice](https://api.backlinko.com/app/uploads/2025/01/backlinko-seo-techniques-table-of-content.png "Backlinko – Técnicas de SEO – Índice")

E quando eles clicam em um link no seu índice, eles pulam diretamente para essa seção.

**12. Otimize sua UX móvel**

De acordo com o Search Engine Land, [57% de todo o tráfego online agora vem de dispositivos móveis](https://searchengineland.com/report-57-percent-traffic-now-smartphones-tablets-281150 "57% de todo o tráfego online agora vem de dispositivos móveis").

![57% de todo o tráfego online agora vem de dispositivos móveis](https://api.backlinko.com/app/uploads/2025/01/57-percent-of-all-online-traffic-now-comes-from-mobile-devices.png "57% de todo o tráfego online agora vem de dispositivos móveis")

Então, se você quer ter uma taxa de rejeição baixa, seu site precisa funcionar MUITO bem em celulares e tablets.

Veja como fazer isso acontecer:

Primeiro, veja como seu site fica em diferentes dispositivos móveis. Recomendo uma ferramenta gratuita chamada [Responsinator](http://www.responsinator.com/ "Responsador") para isso.

![Responsinador – Sobre](https://api.backlinko.com/app/uploads/2025/01/responsinator-about.png "Responsinador – Sobre")

Em seguida, você precisa realmente **usar** seu site usando diferentes dispositivos. Dessa forma, você pode ter certeza de que todos os links e botões funcionam.

Usamos e recomendamos [o BrowserStack](https://www.browserstack.com/ "Pilha do navegador") para testes móveis.

![BrowserStack – Página inicial](https://api.backlinko.com/app/uploads/2025/01/browserstack-homepage.png "BrowserStack – Página inicial")

Esta ferramenta permite que você use seu site com dezenas de dispositivos, sistemas operacionais e navegadores populares.

**13. Link para postagens e artigos relacionados**

Se você quiser evitar que as pessoas saiam das postagens do seu blog, considere criar links para outros conteúdos do seu site.

Isso é semelhante à vinculação interna. Mas com essa abordagem, você apresenta postagens específicas que seus visitantes podem querer ler em seguida.

Por exemplo, o blog Drift tem uma seção de postagens relacionadas no final de cada postagem:

![Link para postagens e artigos relacionados](https://api.backlinko.com/app/uploads/2025/01/link-to-related-posts-and-articles.png "Link para postagens e artigos relacionados")

Dessa forma, você dá aos usuários algo para fazer depois que eles terminarem de ler sua postagem.

**14. Use pop-ups de intenção de saída**

Você deve ter lido que pop-ups podem aumentar sua taxa de rejeição.

![As pessoas estão se tornando cada vez mais intolerantes com os pop-ups](https://api.backlinko.com/app/uploads/2025/01/people-are-growing-increasingly-intolerant-of-popups.png "As pessoas estão se tornando cada vez mais intolerantes com os pop-ups")

E isso é verdade.

(Pelo menos para pop-ups que interrompem e irritam as pessoas)

Bem, existe outra categoria de pop-ups chamada Exit-Intent Popups. E os pop-ups de intenção de saída podem realmente REDUZIR a taxa de rejeição.

Ao contrário dos pop-ups irritantes, os pop-ups de intenção de saída **só aparecem quando alguém sai da sua página**.

![](https://embed-ssl.wistia.com/deliveries/1abe769fa1ae3b5a7287ec590cbc9304378d7b76.jpg)

Essa pessoa vai embora de qualquer jeito, certo? Então você não tem nada a perder ao lançar um pop-up.

Na verdade, nossos dados internos mostram que os pop-ups de intenção de saída reduzem ligeiramente a taxa de rejeição.

O que, se você pensar bem, faz sentido.

Digamos que 50% dos seus visitantes saltam da sua página.

![50 por cento dos usuários saltam da sua página](https://api.backlinko.com/app/uploads/2025/01/50-percent-of-users-bounce-from-your-page.png "50 por cento dos usuários saltam da sua página")

E você decide testar um Popup de Intenção de Saída. E 10% das pessoas que veem esse pop-up inserem seus e-mails e convertem.

![Sair – pop-up de intenção](https://api.backlinko.com/app/uploads/2025/01/exit-intent-pop-up.png "Sair – pop-up de intenção")

Vês como isso funciona? Essa etapa simples reduziu a taxa de rejeição da página em 10%.

Além disso, como bônus, você também ganha vários assinantes de e-mail extras.

**15. Use atualizações de conteúdo**

[Atualizações de conteúdo são](https://backlinko.com/increase-conversions "Atualizações de conteúdo") ímãs de leads superespecíficos.

Então, em vez de oferecer o mesmo e-book a todos os visitantes, você apresenta algo que está 100% relacionado ao que essa pessoa está lendo.

Por exemplo, em nosso guia para SEO [on-page](https://backlinko.com/on-page-seo "SEO na página"), tivemos uma chamada à ação que oferece aos visitantes uma lista de verificação de SEO on-page.

![Download da lista de verificação de SEO na página](https://api.backlinko.com/app/uploads/2025/01/on-page-seo-checklist-download.png "Download da lista de verificação de SEO na página")

E como essa atualização de conteúdo foi SUPER específica, nossa taxa de conversão foi excelente.

Nem é preciso dizer que todos que se inscreveram para obter a atualização de conteúdo não foram mais rejeitados. Então foi uma situação vantajosa para todos.

Se você não quiser criar uma lista de verificação para acompanhar cada postagem, você pode oferecer uma versão em PDF da postagem que eles estão lendo.

Eis um exemplo:

![Ofereça uma versão em PDF da postagem](https://api.backlinko.com/app/uploads/2025/01/offer-a-pdf-version-of-the-post.png "Ofereça uma versão em PDF da postagem")

Na minha experiência, PDFs de postagens de blog como esses não convertem tão bem quanto listas de verificação. Mas ainda costuma converter melhor do que um lead magnet genérico.

Saiba mais

[O guia definitivo para otimização da taxa de conversão](https://backlinko.com/conversion-rate-optimization "O guia definitivo para otimização da taxa de conversão"): aumentar suas conversões é uma das melhores maneiras de melhorar sua taxa de rejeição. Afinal, alguém que converte automaticamente não é um salto. Este guia mostrará como usar testes A/B e outras técnicas para obter mais conversões em seu site.

[Por que a taxa de rejeição é tão importante (dicas de especialistas para entendê-la completamente)](https://www.youtube.com/watch?v=XzVSbRNDkYo "Por que a taxa de rejeição é tão importante (dicas de especialistas para entendê-la completamente)"): uma análise realmente aprofundada da taxa de rejeição, incluindo maneiras de corrigir uma alta taxa de rejeição usando o Google Analytics.

[Melhore a taxa de rejeição com um truque (simples)](https://www.youtube.com/watch?v=8Zg8aVvxFgI "Melhore a taxa de rejeição com um hack (simples)"): como reduzir sua taxa de rejeição usando conteúdo interativo (como calculadoras).