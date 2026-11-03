---
tags:
  - programação
  - IA
---
O **FastAPI** é um framework moderno de alto desempenho escrito em Python, utilizado para construir APIs (frequentemente RESTful) de maneira rápida e eficiente

Diferenças entre APIs, REST APIs e Fast APIs:
- **Natureza**: **API** é o conceito geral de interface; **REST API** é um conjunto de princípios arquiteturais (constraints) para design de serviços web; **FastAPI** é uma ferramenta de software (framework) concreta. 
    
- **Relação**: Você usa o **FastAPI** para implementar uma **REST API**. Eles não são concorrentes diretos, pois um é a ferramenta e os outros são o conceito e o estilo de design. 
    
- **Performance e Recursos**: O **FastAPI** destaca-se por ser assíncrono, oferecer validação automática de dados via Python type hints e gerar documentação interativa (OpenAPI/Swagger) automaticamente, o que reduz o tempo de desenvolvimento comparado a implementações tradicionais de REST.
    
- **Escopo**: Enquanto **REST APIs** podem ser construídas com diversas tecnologias (Node.js, Java, Python/Flask, etc.), o **FastAPI** é específico para o ecossistema Python, focado em velocidade e produtividade para desenvolvedores Python.