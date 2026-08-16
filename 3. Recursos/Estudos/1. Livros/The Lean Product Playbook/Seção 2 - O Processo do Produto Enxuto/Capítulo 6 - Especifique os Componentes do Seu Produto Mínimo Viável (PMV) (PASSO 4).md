---
tags:
  - marketing
tipo:
  - fonte
dominio:
  - marketing
Subdominio:
  - marketing-tático-mix
Sub_subdominio:
  - produto
author:
  - Madhavan Ramanujam
  - Georg Tacke
---
Agora que você tem uma compreensão clara da sua proposta de valor, a próxima etapa no Processo de Produto Lean é decidir o conjunto de recursos para o seu candidato a Produto Mínimo Viável (MVP). Você não vai começar projetando um novo produto que entregue toda a sua proposta de valor, já que isso levaria muito tempo e seria muito arriscado. Para o seu MVP, você quer identificar a funcionalidade mínima necessária para validar que está indo na direção certa. Chamo isso de candidato a MVP em vez de MVP porque ele é baseado em suas hipóteses. Você ainda não validou com os clientes que eles concordam que é, de fato, um produto viável.

Para cada benefício na proposta de valor do seu produto, você deve fazer um brainstorming em equipe para criar o maior número possível de ideias de recursos sobre como seu produto poderia entregar esse benefício. Você fez todo esse ótimo trabalho no espaço do problema e agora está fazendo a transição para o espaço da solução. Neste ponto, as regras de brainstorming devem ser aplicadas. Você deve praticar o pensamento divergente, o que significa tentar gerar o maior número possível de ideias sem nenhum julgamento ou avaliação. Haverá tempo de sobra mais tarde para o pensamento convergente, onde você avalia as ideias e decide quais acha mais promissoras. À medida que sua equipe faz o brainstorming, tente construir sobre as sugestões uns dos outros e incentive-se a criar ideias ainda mais criativas e inusitadas.

Quando terminar o brainstorming, você deve capturar todas as ideias que sua equipe gerou e, em seguida, organizá-las pelo benefício que entregam. Depois, para cada benefício, você deve revisar e priorizar a lista de ideias de recursos. Você pode pontuar cada ideia com base no valor esperado para o cliente para determinar uma prioridade inicial. O objetivo é identificar os três a cinco principais recursos para cada benefício. Não há muito valor em olhar além desses principais recursos agora, porque as coisas vão mudar — e muito — depois que você mostrar seu protótipo aos clientes.

# HISTÓRIAS DE USUÁRIO: RECURSOS COM BENEFÍCIOS

As histórias de usuário (usadas no desenvolvimento Ágil) são uma ótima maneira de escrever suas ideias de recursos para garantir que o benefício correspondente para o cliente permaneça claro. Uma história de usuário é uma breve descrição do benefício que a funcionalidade específica deve fornecer, incluindo para quem é o benefício (o cliente-alvo) e por que o cliente quer o benefício. Histórias de usuário bem escritas geralmente seguem o modelo:

> Como [tipo de usuário], eu quero [fazer algo], para que eu possa [benefício desejado].

Aqui está um exemplo de uma história de usuário que segue este modelo:

> Como fotógrafo profissional, quero poder fazer upload de fotos da minha câmera para o meu site facilmente, para que eu possa mostrar rapidamente as fotos aos meus clientes.

Este modelo é um bom começo, mas escrever boas histórias de usuário é uma habilidade adquirida. O pensador Ágil Bill Wake criou um conjunto de diretrizes para escrever boas histórias de usuário; para torná-las mais fáceis de lembrar, ele usa o acrônimo INVEST:

- **Independente:** Uma boa história deve ser independente de outras histórias. As histórias não devem se sobrepor em conceito e devem ser implementáveis em qualquer ordem.
- **Negociável:** Uma boa história não é um contrato explícito para recursos. Os detalhes de como o benefício de uma história será entregue devem estar abertos à discussão.
- **Valiosa:** Uma boa história precisa ser valiosa para o cliente.
- **Estimável:** Uma boa história é aquela cujo escopo pode ser razoavelmente estimado.
- **Pequena:** Boas histórias tendem a ser pequenas em escopo. Histórias maiores terão maior incerteza, então você deve dividi-las.
- **Testável:** Uma boa história fornece informações suficientes para deixar claro como testar se a história está "pronta" (chamado de critérios de aceitação).

# DIVIDINDO OS RECURSOS

Depois de escrever histórias de usuário de alto nível para seus principais recursos, a próxima etapa é identificar maneiras de dividir cada um deles em pedaços menores de funcionalidade — o que chamo de "fatiamento" (*chunking*). O objetivo é encontrar maneiras de reduzir o escopo e construir apenas as partes mais valiosas de cada recurso. Quando alguém propõe uma ideia de recurso, muitas vezes há maneiras criativas de cortar as partes menos importantes. Uso deliberadamente o termo "pedaço de recurso" (*feature chunk*) em vez de recurso para lembrar aos leitores que vocês não devem trabalhar com itens de grande escopo, mas sim dividir tais itens em componentes menores e atômicos.

Vamos ilustrar a ideia de dividir uma história de usuário de alto nível. Digamos que você esteja trabalhando em um aplicativo de compartilhamento de fotos e comece com a história de usuário: "Como usuário, quero poder compartilhar fotos facilmente com meus amigos para que eles possam apreciá-las". Uma maneira de dividir essa história é pelos vários canais que um cliente pode usar para compartilhar fotos: Facebook, Twitter, Pinterest, e-mail, mensagem de texto, e assim por diante. Cada um desses seria um pedaço de recurso distinto ou uma história de usuário de escopo menor. Você pode não precisar construir todos esses canais de compartilhamento para o seu MVP. Mesmo que decida que precisa, ajuda dividir a história para ser mais específico na definição do seu produto, permitir um escopo mais preciso do desenvolvimento e permitir que você priorize explicitamente a ordem em que constrói os pedaços. Você também pode limitar o escopo permitindo que o usuário compartilhe apenas a foto e nada mais para o seu MVP. Você pode ter ideias para funcionalidades adicionais no futuro, como adicionar uma mensagem opcional a cada foto ou a capacidade de marcar usuários nas fotos. Cada uma dessas seria um pedaço de recurso distinto.

# TAMANHOS DE LOTE MENORES SÃO MELHORES

A tática de dividir os recursos é consistente com a melhor prática de manufatura Lean de trabalhar com tamanhos de lote pequenos. Quando um produto está sendo fabricado em uma linha de fábrica, o tamanho do lote é o número de produtos sendo trabalhados juntos ao mesmo tempo (em cada etapa do processo de fabricação). O paralelo para o desenvolvimento de software é o tamanho dos recursos ou histórias de usuário a serem codificados. Trabalhar com tamanhos de lote menores aumenta a velocidade porque permite um feedback mais rápido, o que reduz o risco e o desperdício. Se uma desenvolvedora passa um mês em seu computador desenvolvendo um recurso e depois o mostra ao gerente de produto e ao designer, há uma chance maior de que haja um desacordo e de que o feedback deles exija mudanças significativas. Se, em vez disso, a desenvolvedora mostrar seu trabalho ao gerente de produto e ao designer a cada um ou dois dias, isso evita que um grande desacordo ocorra. A magnitude do feedback e das correções de curso será muito menor e mais gerenciável, resultando em menos trabalho desperdiçado e maior produtividade.

Esse conselho também se aplica a gerentes de produto e designers mostrando seu produto de trabalho (por exemplo, histórias de usuário e wireframes) para seus colegas. O benefício de trabalhar com tamanhos de lote menores também se aplica ao feedback do cliente. Quanto mais tempo você trabalha em um produto sem obter feedback do cliente, mais você corre o risco de um grande desacordo que subsequentemente exige um retrabalho significativo.

# ESCOPO COM PONTOS DE HISTÓRIA

Leitores que têm experiência trabalhando com desenvolvimento Ágil provavelmente estão familiarizados com a ideia de dividir os recursos em pedaços menores. Em muitas formas de Ágil, depois de escrever as histórias de usuário, a equipe discute cada uma e os desenvolvedores estimam a quantidade de esforço necessária. Eles frequentemente fazem isso usando pontos de história, um tipo de moeda para estimar o tamanho relativo de diferentes histórias de usuário. Por exemplo, uma história de usuário muito pequena pode levar 1 ponto, enquanto uma história de usuário de escopo médio pode levar 3 pontos, e uma história de usuário de escopo grande pode levar 8 pontos. Discuto os pontos de história com mais detalhes no Capítulo 12.

Um bom princípio operacional é que as histórias estimadas para exigir um grande número de pontos — acima de algum valor limite máximo — precisam ser divididas em um conjunto de histórias menores que estejam abaixo do valor limite. Você pode pensar em um pedaço de recurso como correspondendo a uma história de usuário que tem um escopo aceitavelmente pequeno — um número estimado de pontos de história que está abaixo do seu limite máximo.

# USANDO O RETORNO SOBRE O INVESTIMENTO PARA PRIORIZAR

Esta é uma boa hora para introduzir o conceito de retorno sobre o investimento (ROI). Até agora, você priorizou apenas com base em quanto valor para o cliente acredita que cada recurso criará. Você ainda não levou em consideração a quantidade de recursos necessária para construir cada recurso. Depois de terminar de fatiar suas ideias de recursos, você deve realizar uma segunda passagem de priorização que leve em consideração tanto o valor quanto o esforço.

Uma maneira simples de ilustrar o ROI é imaginar que invisto US$ 100 em uma ação. Vários meses depois, ela vale US$ 200 e eu a vendo. Tenho um retorno — ou lucro líquido — de US$ 100, já que US$ 200 - US$ 100 = US$ 100. Meu investimento foi de US$ 100. Portanto, meu ROI é US$ 100 ÷ US$ 100 = 1, ou 100%. A fórmula para o ROI é:

$\text{ROI} = \frac{\text{Valor Final} - \text{Investimento}}{\text{Investimento}} = \frac{\text{Retorno}}{\text{Investimento}}$

No contexto de investimentos, ambos os números que você insere na fórmula são valores monetários (por exemplo, dólares). No entanto, esse geralmente não é o caso do ROI no contexto de desenvolvimento de produtos. Quando você está construindo um produto ou recurso, o investimento é geralmente o tempo que seus recursos de desenvolvimento gastam trabalhando nele, que você geralmente mede em unidades como semanas-desenvolvedor (um desenvolvedor trabalhando por uma semana). É verdade que você provavelmente poderia calcular um valor equivalente em dólares, mas as pessoas usam unidades como semanas-desenvolvedor porque são mais simples e claras.

Da mesma forma, no contexto de desenvolvimento de um novo produto, o "retorno" geralmente não é um valor em dólares. Em vez disso, geralmente é alguma medida relativa da quantidade de valor para o cliente que você espera que um determinado recurso crie. Desde que você use uma escala numérica apropriada para estimar o valor para o cliente, os cálculos de ROI funcionarão bem. Você precisa usar uma "escala de razão", o que apenas significa que as pontuações que você usa são proporcionais ao seu valor. Por exemplo, digamos que você use uma escala de 0 a 10 para o valor do cliente e estime pontuações para todos os seus pedaços de recursos. Usando uma escala de razão, se um pedaço de recurso tem uma pontuação de 10 e um segundo pedaço de recurso tem uma pontuação de 5, isso deve significar que o primeiro recurso criaria o dobro da quantidade de valor para o cliente em relação ao segundo.

## Visualizando o ROI

A Figura 6.1 — que mostra o retorno, ou valor para o cliente criado, no eixo vertical e o investimento, ou esforço de desenvolvimento, no eixo horizontal — ilustra o conceito de ROI. Vamos começar com as ideias de recursos A e B, ambas estimadas para criar 6 unidades de valor para o cliente. No entanto, a ideia B requer 4 semanas-desenvolvedor para ser implementada, enquanto a ideia A requer apenas 2 semanas-desenvolvedor. O ROI para a ideia A é 6 ÷ 2 = 3, enquanto o ROI para a ideia B é 6 ÷ 4 = 1,5. Você deve priorizar o recurso A acima do recurso B.

![[ROI.png]]

Às vezes, dois recursos oferecem aproximadamente o mesmo ROI. Observe as ideias de recursos C e D. A ideia C oferece 4 unidades de valor para o cliente por 4 semanas-desenvolvedor, para um ROI de 4 ÷ 4 = 1. A ideia D oferece 8 unidades de valor para o cliente por 8 semanas-desenvolvedor, para um ROI de 8 ÷ 8 = 1. Quando você tem duas ideias de recursos com o mesmo ROI, é melhor priorizar a ideia de escopo menor, pois leva menos tempo para ser implementada. Você entregará o valor aos clientes mais rapidamente — e ao ter o recurso ativo mais cedo, você também obterá um feedback valioso dos clientes sobre ele mais cedo.

Também existem ideias ruins por aí — como a ideia F, que oferece 2 unidades de valor para o cliente por 8 semanas-desenvolvedor, um ROI de 2 ÷ 8 = 0,25. O grande esforço de uma ideia de baixo ROI é frequentemente reconhecido no início, enquanto a equipe trabalha em sua implementação; no entanto, eles geralmente não percebem o baixo valor para o cliente até depois do lançamento. O Google Buzz e o Google Wave são exemplos de projetos de baixo ROI que levaram um grande número de horas de desenvolvedor para construir, mas foram descontinuados pouco depois do lançamento, quando a reação dos clientes indicou que não haviam criado valor suficiente.

Boas equipes de produto se esforçam para criar ideias como a ideia G na Figura 6.1 — aquelas que criam alto valor para o cliente com baixo esforço. Grandes equipes de produto são capazes de pegar ideias como essa, dividi-las em pedaços, cortar as partes menos valiosas e identificar maneiras criativas de entregar o valor para o cliente com menos esforço do que o escopo inicial — indicado na figura movendo a ideia G para a esquerda.

Algumas pessoas têm dificuldade em criar estimativas numéricas de valor para o cliente que consideram precisas. No entanto, isso não é algo com que se preocupar muito, já que não se trata de alcançar uma precisão de ponto decimal. Mesmo as estimativas de esforço provavelmente não serão muito precisas, porque você ainda não projetou totalmente os recursos. Você não pode esperar que os desenvolvedores lhe deem estimativas precisas com base apenas em uma descrição de alto nível de um recurso. A precisão das estimativas deve ser proporcional à fidelidade da definição do produto. O principal objetivo desses cálculos é menos sobre descobrir os valores reais de ROI e mais sobre como eles se comparam uns aos outros. Você quer se concentrar primeiro nos recursos de maior ROI e evitar os recursos de menor ROI.

Você pode classificar sua lista de pedaços de recursos pelo ROI estimado para criar uma lista ordenada por classificação — que é um bom ponto de partida para ajudar a decidir quais pedaços de recursos devem fazer parte do candidato a MVP. No entanto, às vezes você não pode apenas seguir a ordem de classificação estrita para criar um MVP "completo"; você pode precisar pular para incluir recursos importantes.

O retorno no cálculo de ROI pode ser uma medida de valor para o seu negócio em vez de valor para o cliente. Nesses casos, você geralmente tem um valor em dólares estimado que pode usar para o retorno. Isso será um ganho esperado de receita ou uma diminuição esperada de custo. Digamos, por exemplo, que você tenha um produto ativo e esteja tentando melhorar sua taxa de conversão de usuários gratuitos para usuários pagos. Para uma determinada melhoria na taxa de conversão, você deve ser capaz de estimar a melhoria esperada na receita. Portanto, você deve ser capaz de associar um valor em dólares estimado a cada ideia de melhoria que tiver. Os Capítulos 13 e 14 discutem como maximizar seu ROI à medida que melhora as métricas do seu negócio e produto.

## Aproximando o ROI

Expliquei como pensar sobre o ROI de forma rigorosa, mas você também pode usar essa ferramenta de priorização de uma maneira menos rigorosa. Se estiver com dificuldades para criar estimativas numéricas de valor para o cliente ou esforço de desenvolvimento, você pode pontuar cada ideia de recurso como alta, média ou baixa em valor para o cliente e em esforço. Isso criará uma grade de três por três, como mostrado na Figura 6.2. Todas as suas ideias de recursos cairão em um dos nove quadrantes. Embora você não esteja calculando ROIs para cada recurso numericamente, você pode ordenar os nove quadrantes com base no ROI, como mostrado na figura. Portanto, todos os recursos no quadrado número 1, que tem o maior valor e o menor esforço, teriam prioridade mais alta do que os recursos no quadrado número 2, que teriam prioridade mais alta do que os recursos no quadrado número 3, e assim por diante.

![[aproximando ROI.png]]

Se você se sentir travado porque não tem certeza sobre as estimativas de valor para o cliente e esforço, apenas use seu melhor palpite para colocar cada recurso em uma das nove células. Estas são apenas suas hipóteses iniciais; você pode — e provavelmente irá — alterá-las à medida que aprende e itera.

# DECIDINDO SOBRE O SEU CANDIDATO A MVP

Depois de terminar o fatiamento, o escopo e a priorização, você pode criar uma grade simples que lista os benefícios da sua proposta de valor e que lista, para cada benefício, as principais ideias de recursos divididas em pedaços. Veja a Figura 6.3.

Na Figura 6.3, listei os principais pedaços de recursos para cada benefício em ordem de prioridade, com maior prioridade à esquerda. Em vez de nomear benefícios ou pedaços de recursos específicos, dei intencionalmente nomes genéricos para que você possa visualizar mais facilmente a substituição deles pelo que seria relevante para o seu produto. "M1A" significa pedaço de recurso A para a necessidade básica 1. "P2B" significa pedaço de recurso B para o benefício de desempenho 2, e "D2C" significa pedaço de recurso para o benefício encantador 2. Ao preencher uma grade semelhante para o seu produto, você usaria os rótulos específicos para seus benefícios e pedaços de recursos.

![[lista-de-pedaços-de-componentes-para-cada-benefício.png]]

Depois de organizar sua lista de pedaços de recursos por benefício e priorizá-los, é hora de começar a tomar algumas decisões difíceis. Você deve decidir o conjunto mínimo de funcionalidades que ressoará com seus clientes-alvo. Você vai olhar para a coluna mais à esquerda de pedaços de recursos e determinar quais acha que precisam estar no seu candidato a MVP. Ao fazer isso, você deve se referir à proposta de valor do seu produto. Para começar, seu candidato a MVP precisa ter todas as necessidades básicas que você identificou.

Depois disso, você deve se concentrar no principal benefício de desempenho que planeja usar para vencer a concorrência. Você deve selecionar o conjunto de pedaços de recursos para esse benefício que acredita que fornecerá o suficiente para que os clientes vejam a diferença no seu produto.

Os encantadores também fazem parte da sua diferenciação. Você deve incluir seu principal encantador no seu candidato a MVP. Isso pode não ser necessário se você tiver uma vantagem muito grande em um benefício de desempenho. O objetivo é garantir que seu candidato a MVP inclua algo que os clientes considerem superior a outros produtos e, idealmente, único.

Os pedaços de recursos que você acredita que precisam estar no seu candidato a MVP permanecerão na coluna mais à esquerda, que você pode rotular como "v1", como vê na Figura 6.4, enquanto os outros são empurrados para a direita. Você pode criar um roteiro preliminar do produto continuando este processo e criando colunas para cada versão futura, com cada coluna contendo os pedaços de recursos que planeja adicionar.

Como planeja ser o melhor no benefício de desempenho 3, você está incluindo o pedaço de recurso de maior prioridade, P3A, no seu candidato a MVP. Você também planeja se diferenciar com o diferenciador 2, então está incluindo o pedaço de recurso D2A no seu candidato a MVP. Seu candidato a MVP também tem as duas necessidades básicas.

Olhando para a sua próxima versão, v1.1, você planeja investir ainda mais no benefício de desempenho 3 e no encantador 2 com os pedaços de recursos P3B e D2B, respectivamente. Na versão depois dessa, v1.2, você planeja começar a abordar o benefício de desempenho 1 com o pedaço de recurso de maior prioridade P1A.

Não recomendo que você planeje mais de uma ou duas versões menores à frente no início — já que muitas coisas provavelmente mudarão quando você mostrar seu candidato a MVP aos clientes pela primeira vez. Você aprenderá que algumas de suas hipóteses não estavam totalmente corretas e criará novas. Você pode acabar mudando de ideia sobre qual benefício é mais importante ou criar ideias para novos recursos para abordar os mesmos benefícios. Portanto, se você fez planos provisórios além do seu MVP, deve estar preparado para jogá-los pela janela e criar novos planos com base no que aprender com os clientes.

![[decidindo-quais-pedaços-de-componente_estão-em-seu-candidato.png]]

Da maneira como desenhei a Figura 6.4, há, no máximo, apenas um pedaço de recurso para um determinado benefício no seu candidato a MVP. No entanto, pode ser o caso de você precisar de dois ou três pedaços de recursos para um determinado benefício, dependendo da sua situação e do quão pequenos são os seus pedaços. A ideia ainda é a mesma: escolher quais pedaços de recursos precisam estar naquela coluna mais à esquerda, que corresponde ao seu candidato a MVP.

Vamos dar um passo atrás e refletir. Neste ponto do Processo de Produto Lean, você fez um bom trabalho. Você:

- Formou hipóteses sobre seus clientes-alvo
- Formou hipóteses sobre suas necessidades não atendidas
- Articulou a proposta de valor que planeja perseguir para que seu produto seja melhor e diferente
- Identificou as principais ideias de recursos que acredita que abordarão essas necessidades e as dividiu em pedaços menores
- Priorizou esses pedaços de recursos com base no ROI
- Selecionou um conjunto desses pedaços de recursos para o seu candidato a MVP, que você hipotetiza que os clientes acharão valioso

Você fez muito pensamento rigoroso para chegar a este ponto, mas seu MVP ainda é apenas um candidato, um pacote de hipóteses inter-relacionadas. Você precisa obter o feedback dos clientes sobre o seu candidato a MVP para testar essas hipóteses. Mas antes de poder testar, você precisa criar uma representação no espaço da solução do seu candidato a MVP que possa mostrar aos clientes, que é a próxima etapa no Processo de Produto Lean.