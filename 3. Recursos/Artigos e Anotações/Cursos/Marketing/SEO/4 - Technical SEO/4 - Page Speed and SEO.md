---
tags:
  - marketing
  - operacional
  - canais
---
### O que é velocidade de página?

Velocidade da página é o tempo que uma página da web leva para carregar. A velocidade de carregamento de uma página é determinada por vários fatores diferentes, incluindo o servidor do site, o tamanho do arquivo da página e a compactação da imagem.

Dito isto:

“Velocidade da página” não é tão simples quanto parece.

Isso ocorre porque existem muitas maneiras diferentes de medir a velocidade da página. Aqui estão três dos mais comuns:

Página totalmente carregada: é quanto tempo leva para carregar 100% dos recursos de uma página. Esta é a maneira mais direta de determinar a rapidez com que uma página carrega.

Tempo até o primeiro byte: mede quanto tempo leva para uma página iniciar o processo de carregamento.

![Hora do primeiro byte](https://api.backlinko.com/app/uploads/2019/02/time-to-first-byte.png "Hora do primeiro byte")

Se você já acessou uma página e olhou para uma tela branca por alguns segundos, isso é o TTFB em ação.

Primeira pintura significativa/primeira pintura contextual: tempo que uma página leva para carregar recursos suficientes para que um usuário consiga ler o conteúdo daquela página.

Por exemplo, digamos que você tenha uma postagem de blog que leva 10 segundos para carregar completamente.

![Tempo de página totalmente carregado](https://api.backlinko.com/app/uploads/2019/02/fully-loaded-page-time.png "Tempo de página totalmente carregado")

Isso é muito tempo… se você APENAS observar quanto tempo leva para a página inteira carregar.

Por outro lado, prestar atenção ao First Meaningful Paint às vezes representa melhor como os usuários realmente interagem com sua página à medida que ela carrega.

Por exemplo, vamos olhar novamente para a página que leva 10 segundos para carregar todos os recursos da página.

Mesmo que demore um pouco para a página inteira carregar, quando um usuário acessa a página pela primeira vez, ele recebe uma “Primeira Pintura Significativa” após 1,5 segundos.

![Primeira tinta significativa](https://api.backlinko.com/app/uploads/2019/02/first-meaningful-paint.png "Primeira tinta significativa")

O que significa que eles podem começar a interagir com sua página praticamente instantaneamente. Então, para um usuário, sua página é rápida.

### Conclusão?

Existem muitas maneiras diferentes de medir a velocidade da página. E não há nenhuma métrica “certa” que supere todas as outras. Todos eles têm prós e contras.

Em vez disso, concentre-se em melhorar a velocidade de carregamento da sua página para TODAS as métricas encontradas.

### Por que a velocidade da página é importante para SEO?

O Google usa a velocidade da página como fator de classificação [desde 2010](https://webmasters.googleblog.com/2010/04/using-site-speed-in-web-search-ranking.html "O Google usa a velocidade da página como fator de classificação desde 2010").

![Velocidade do site é um fator - Desde 2010](https://api.backlinko.com/app/uploads/2019/02/site-speed-since-2010.png "Velocidade do site um fator – Desde 2010")

E em 2018 o Google aumentou a importância da velocidade da página com a [atualização “Speed”](https://webmasters.googleblog.com/2018/01/using-page-speed-in-mobile-search.html "A atualização de velocidade").

![Atualização de velocidade do site, 2018](https://api.backlinko.com/app/uploads/2019/02/site-speed-update-2018.png "Atualização de velocidade do site, 2018")

Em resumo:

Um site de carregamento lento pode prejudicar sua classificação no Google.

A questão é: como o Google determina a velocidade de carregamento do seu site? Eles observam quanto tempo leva para 100% da página carregar? Ou TTFB?

Eles não fizeram nenhuma declaração oficial sobre isso. Mas considerando que eles relatam todas essas métricas em sua ferramenta Page[Speed Insights,](https://developers.google.com/speed/pagespeed/insights/ "Ferramenta Google PageSpeed Insights") isso me diz que eles provavelmente usam uma combinação de diferentes medições de velocidade de página:

![Insights sobre velocidade de página](https://api.backlinko.com/app/uploads/2019/02/page-speed-insights.png "Insights sobre velocidade de página")

Com isso, veja como melhorar a velocidade de carregamento do seu site.

### Melhores práticas

Comprimir Imagens

Coloco isso em primeiro lugar porque geralmente é a maior vitória.

Afinal, as imagens geralmente ocupam de 50 a 90% do tamanho de uma página.

![Melhore a velocidade da página compactando imagens](https://api.backlinko.com/app/uploads/2019/02/improve-page-speed-by-compressing-images.png "Melhore a velocidade da página compactando imagens")

(E velocidade de carregamento)

Por exemplo, veja este relatório de velocidade de página de uma página do meu site:

![Teste de página da Web](https://api.backlinko.com/app/uploads/2019/02/web-page-test.png "Teste de página da Web")

Como você pode ver, 86,2% do tamanho da página se deve às imagens:

![Tamanho do arquivo de imagem](https://api.backlinko.com/app/uploads/2019/02/image-file-size.png "Tamanho do arquivo de imagem")

Portanto, quanto mais você puder compactar suas imagens, mais rápido sua página será carregada.

Como?

Se o seu site roda em WordPress, então EU recomendo fortemente um plugin chamado [WP Smush](https://premium.wpmudev.org/project/wp-smush-pro/ "WP Esmagar"):

  
![WP Esmagar](https://api.backlinko.com/app/uploads/2019/02/wp-smush.png "WP Esmagar")

Ele compacta automaticamente qualquer imagem que você carrega na biblioteca de mídia do WordPress. E pelo menos de acordo com os criadores do plugin, isso pode reduzir o tamanho do seu arquivo de imagem em 14,2%.

Não usa WordPress? Ainda existem muitas opções de compactação de imagem por aí, como [o Cesium](https://saerasoft.com/caesium/ "Césio") e o [Mass Image Compressor](https://sourceforge.net/projects/icompress/ "Compressor de imagem em massa").

Ao contrário de antigamente, a maioria das ferramentas de compactação agora usa compactação sem perdas ou reduz a qualidade da imagem apenas a um ponto quase imperceptível.

Por exemplo, compactamos 100% das imagens aqui no Backlinko. E ainda parecem bonitos e afiados:

![Backlinko – Imagens Otimizadas](https://api.backlinko.com/app/uploads/2020/06/backlinko-optimized-images.png "Backlinko – Imagens Otimizadas")

Limpe e compacte seu código

Ou seja: [minifique os recursos encontrados na sua página](https://developers.google.com/speed/docs/insights/MinifyResources "Minimizar recursos").

Isso inclui:

- HTML
- CSS
- JavaScript
- E qualquer outro código encontrado na sua página

Seu primeiro passo deve ser limpar qualquer código inchado que você tenha em sua página. Esse código extra pode ser de recursos que você não tem mais em seu site. Ou de um trabalho de desenvolvedor de má qualidade.

De qualquer forma: quanto mais limpo for o seu código, mais rápido as coisas serão carregadas.

(Sim, isso rima 🙂 )

Em seguida, compacte seu código usando um programa como o [GZip](https://www.gnu.org/software/gzip/ "GZip").

Hospedagem de atualização

Essa é uma dica sobre a qual não vejo muita gente falando.

Você pode limpar seu código e compactar a imagem o dia todo. Mas se você gastar US$ 4,99/mês em hospedagem, seu site não carregará rapidamente.

Isso ocorre porque você está compartilhando um servidor com um milhão de outros sites.

Existem um milhão de hosts web por aí. Então não posso recomendar nenhum em particular.

Mas posso dizer isso como regra geral: quando se trata de hospedagem, você recebe aquilo pelo qual paga.

Então, se você realmente quer melhorar a velocidade de carregamento do seu site, talvez seja hora de atualizar para um host premium ou para um servidor dedicado.

Ativar cache do navegador

Isso permite que os usuários armazenem partes da sua página no cache do navegador.

![Ativar cache do navegador](https://api.backlinko.com/app/uploads/2019/02/activate-browser-caching.png "Ativar cache do navegador")

Então, na próxima vez que eles visitarem seu site, ele carregará MUITO mais rápido.

![O cache torna o tempo de carregamento muito mais rápido para segundas visitas](https://api.backlinko.com/app/uploads/2019/02/caching-makes-load-time-much-quicker-for-second-visits.png "O cache torna o tempo de carregamento muito mais rápido para segundas visitas")

Infelizmente, isso não ajudará sua página a carregar mais rápido para visitantes de primeira viagem. Mas é ótimo para melhorar sua velocidade de carregamento para pessoas que já visitaram seu site antes.

Você pode configurar o cache [do navegador](https://gtmetrix.com/leverage-browser-caching.html) no seu arquivo .htaccess. Ou com um plugin WordPress.[](https://backlinko.com/htaccess-redirect)

Implementar um CDN

Uma Rede de Distribuição de Conteúdo (CD[N](https://en.wikipedia.org/wiki/Content_delivery_network "CDN")) é uma das maneiras mais fáceis de aumentar a velocidade de carregamento do seu site.

As CDNs funcionam descobrindo onde seu visitante está fisicamente localizado… e então disponibilizando os recursos do seu site a partir de um servidor próximo a ele.

![Implementar um CDN para aumentar a velocidade](https://api.backlinko.com/app/uploads/2019/02/implement-a-cdn-to-boost-speed.png "Implementar um CDN para aumentar a velocidade")

Teste com várias ferramentas de teste de velocidade de página

Agora que você implementou essas etapas, é hora de ver como você está.

E recomendo testar a velocidade da sua página usando duas ferramentas diferentes.

O primeiro é o [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/ "Insights do Google PageSpeed").

![Ferramenta Google PageSpeed Insights](https://api.backlinko.com/app/uploads/2019/02/google-page-speed-insights-tool.png "Ferramenta Google PageSpeed Insights")

A ferramenta do Google verifica o código da sua página em busca de problemas…

![Insights – problemas do PageSpeed](https://api.backlinko.com/app/uploads/2019/02/site-speed-insights-problems.png "Insights – problemas do PageSpeed")

… e oportunidades.

![Insights do PageSpeed – Oportunidades](https://api.backlinko.com/app/uploads/2019/02/page-speed-insights-opportunities.png "Insights do PageSpeed – Oportunidades")

E eles adicionaram recentemente um recurso que informa quanto tempo leva para seu site carregar para usuários humanos reais (usando dados do navegador Google Chrome).

![Insights sobre a velocidade do site – Chrome](https://api.backlinko.com/app/uploads/2019/02/site-speed-insights-chrome.png "Insights sobre a velocidade do site – Chrome")

O que é SUPER útil.

Um aviso: às vezes você descobrirá que as recomendações da ferramenta não fazem sentido para seu site.

Por exemplo, a ferramenta do Google recomendou que I “Servisse imagens em formatos de próxima geração”.

![Formatos de próxima geração](https://api.backlinko.com/app/uploads/2019/02/next-gen-formats.png "Formatos de próxima geração")

No entanto, esses formatos “de última geração” não são suportados pela maioria dos navegadores (incluindo Safari e Firefox). Então, se você mudar para esses formatos de última geração, a experiência do usuário do seu site irá por água abaixo.

Dito isto: há muitos insights úteis nesta ferramenta. E recomendo implementar o máximo que puder.

A seguir, temos [WebPageTest.org](https://www.webpagetest.org/ "Teste de página da Web").

![WebPageTest – Pesquisa](https://api.backlinko.com/app/uploads/2019/02/web-page-test-search.png "WebPageTest – Pesquisa")

O que é legal sobre o WebPageTest é que ele carrega sua página em um navegador real. E permite que você saiba sobre partes específicas da sua página que demoram muito para carregar.

![WebPageTest – Resultados](https://api.backlinko.com/app/uploads/2019/02/web-page-test-results.png "WebPageTest – Resultados")

Saiba mais

[Estudos de caso de velocidade móvel – Impulsione carregamentos de páginas mais rápidos](https://searchengineland.com/mobile-speed-case-studies-push-for-faster-page-loads-295331 "Estudos de caso de velocidade móvel - Impulsione carregamentos de página mais rápidos"): 3 estudos de caso aprofundados que mostram como melhorar a velocidade pode se traduzir em classificações mais altas e tráfego mais orgânico.