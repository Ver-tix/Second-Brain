---
tags:
  - marketing
  - operacional
  - canais
---
### O que é Robots.txt?

Robots.txt é um arquivo que instrui os rastreadores de mecanismos de busca sobre quais URLs eles podem acessar no seu site. Ele é usado principalmente para gerenciar o tráfego de rastreadores e evitar sobrecarregar seu site com solicitações.

Embora grandes mecanismos de busca como Google, Bing e Yahoo reconheçam e respeitem as diretivas robots.txt, é importante observar que esse arquivo não é um método infalível para impedir que páginas da web apareçam nos resultados de pesquisa.

### Por que Robots.txt é importante?

A maioria dos sites não precisa de um arquivo robots.txt.[](https://www.semrush.com/blog/beginners-guide-robots-txt/ "Um guia completo para robôs.txt")

Isso ocorre porque o Google geralmente consegue encontrar e indexar todas as páginas importantes do seu site.

E eles automaticamente NÃO indexarão páginas que não sejam importantes ou duplicarão versões de outras páginas.

Dito isto, existem 3 razões principais pelas quais você gostaria de usar um arquivo robots.txt.

**Bloqueie páginas não públicas:** às vezes, você tem páginas em seu site que não deseja indexar. Por exemplo, você pode ter uma versão de preparação de uma página, uma página de login ou uma página interna de resultados de pesquisa. Essas páginas precisam existir, mas você não quer que pessoas aleatórias cheguem a elas. Nesse caso, você usaria robots.txt para bloquear essas páginas de rastreadores e bots de mecanismos de busca.

**Maximize o orçamento de rastreamento:** se você estiver tendo problemas para indexar todas as suas páginas, poderá ter um problema de [orçamento de rastreamento](https://backlinko.com/hub/seo/crawl-budget "Orçamento de rastreamento"). Ao bloquear páginas sem importância com robots.txt, o Googlebot pode gastar mais do seu orçamento de rastreamento nas páginas que realmente importam.

**Impedir a indexação de recursos em mecanismos de busca:** usar [meta diretivas](https://moz.com/learn/seo/robots-meta-directives "Meta diretivas") pode funcionar tão bem quanto Robots.txt para impedir que páginas sejam indexadas. No entanto, as meta diretivas não funcionam bem para recursos multimídia, como PDFs e imagens. É aí que o robots.txt entra em jogo.

O resultado final? Robots.txt diz aos spiders dos mecanismos de pesquisa para não rastrearem páginas específicas do seu site.

Você pode verificar quantas páginas indexou no [Google Search Console](https://backlinko.com/google-search-console "Console de pesquisa do Google").

![GSC – Indexação de páginas](https://api.backlinko.com/app/uploads/2024/06/gsc-page-indexing-1.png "GSC – Indexação de páginas")

Se o número corresponder ao número de páginas que você deseja indexar, você não precisa se preocupar com um arquivo Robots.txt.

Mas se esse número for maior do que você esperava (e você notar URLs indexadas que não deveriam ser indexadas), então é hora de criar um arquivo robots.txt para seu site.

### Melhores práticas

**Crie um arquivo Robots.txt**

Seu primeiro passo é realmente criar seu arquivo robots.txt.

Por ser um arquivo de texto, você pode criar um usando o Bloco de Notas do Windows.

E não importa como você crie seu arquivo robots.txt, o formato é exatamente o mesmo:

_Agente do usuário: X_
_Disallow: Y_

O agente do usuário é o bot específico com o qual você está falando.

E tudo o que vem depois de “proibir” são páginas ou seções que você deseja bloquear.

Eis um exemplo:

_Agente do usuário: googlebot_
_Disallow: /images_

Esta regra diria ao Googlebot para não indexar a pasta de imagens do seu site.

Você também pode usar um asterisco (*) para endereçar quaisquer bots de mecanismos de busca que passem pelo seu site.

Eis um exemplo:

Agente do usuário: *
Não permitir: /imagens

O “*” diz a todas e quaisquer aranhas para NÃO rastrearem sua pasta de imagens.

Esta é apenas uma das muitas maneiras de usar um arquivo robots.txt. Este guia útil do Google contém mais informações sobre as diferentes regras que você pode usar para bloquear ou permitir que bots rastreiem diferentes páginas do seu site.[](https://support.google.com/webmasters/answer/6062596?hl=en&ref_topic=6061961 "Guia do Google Robots.txt")

![Regras do Google Search Central – Robots.txt](https://api.backlinko.com/app/uploads/2024/08/google-search-central-robots-txt-rules.png "Regras do Google Search Central – Robots.txt")

**Torne seu arquivo Robots.txt fácil de encontrar**

Depois de ter seu arquivo robots.txt, é hora de colocá-lo no ar.

Você pode colocar seu arquivo robots.txt no diretório raiz do seu site.

Mas para aumentar as chances de seu arquivo robots.txt ser encontrado, recomendo colocá-lo em:

_https://example.com/robots.txt_

> **Observação: seu** arquivo robots.txt diferencia maiúsculas de minúsculas. Portanto, certifique-se de usar um “r” minúsculo no nome do arquivo.

**Verifique se há erros e enganos**

É MUITO importante que seu arquivo robots.txt esteja configurado corretamente. Um erro **e todo o seu site pode ser desindexado.**

Felizmente, você não precisa esperar que seu código esteja configurado corretamente. O Google tem uma [ferramenta poderosa](https://www.google.com/webmasters/tools/robots-testing-tool "Console de Pesquisa do Google – Robots.txt") para testar robôs que você pode usar:

![GSC – Configurações – Robots.txt](https://api.backlinko.com/app/uploads/2024/08/gsc-settings-robots-txt.png "GSC – Configurações – Robots.txt")

Ele mostra seu arquivo robots.txt… e quaisquer erros e avisos que encontrar.

Como você pode ver, impedimos que spiders rastreiem nossa página de administração do WP.

Também usamos robots.txt para bloquear o rastreamento de páginas de tags geradas automaticamente pelo WordPress (para limitar [conteúdo duplicado](https://backlinko.com/hub/seo/duplicate-content "Conteúdo duplicado")).

**Robôs.txt vs. Meta Diretivas**

Por que você usaria robots.txt quando pode bloquear páginas no nível da página com a meta tag “[noindex](https://support.google.com/webmasters/answer/93710?hl=en "Meta tag Noindex")”?

Como mencionei anteriormente, a tag noindex é complicada de implementar em recursos multimídia como vídeos e PDFs.

Além disso, se você tiver milhares de páginas que deseja bloquear, às vezes é mais fácil bloquear toda a seção desse site com robots.txt em vez de adicionar manualmente uma tag noindex a cada página.

Também há casos extremos em que você não quer desperdiçar nenhum orçamento de rastreamento no Google, chegando a páginas com a tag noindex.

Dito isto:

Fora desses três casos extremos, recomendo usar meta diretivas em vez de robots.txt. Elas são mais fáceis de implementar. E há menos chance de um desastre acontecer (como bloquear todo o seu site).

### Saiba mais

[Saiba mais sobre arquivos robots.txt](https://support.google.com/webmasters/answer/6062608?hl=en "Saiba mais sobre arquivos robots.txt"): Um guia útil sobre como eles usam e interpretam robots.txt.

[O que é um arquivo Robots.txt? (Uma visão geral para SEO + Key Insight)](https://www.youtube.com/watch?v=LlJy5LRkUfs "O que é um arquivo Robots.txt? (Uma visão geral para SEO + Key Insight)"): Um vídeo prático sobre diferentes casos de uso para robots.txt.