---
tags:
  - programação
---
Em poucas palavras:

> **O `main.py` é o ponto de entrada da aplicação.** Ele inicia o sistema e coordena o primeiro fluxo de execução.

No contexto do **Prometheus OS**, ele faria algo como:

```text
Usuário inicia o programa
        ↓
main.py
        ↓
Cria o PrometheusOS (Orquestrador)
        ↓
Recebe o pedido do usuário
        ↓
Entrega o pedido ao Orquestrador
        ↓
Mostra a resposta ao usuário
```

Pense nele como o **maître de um restaurante**:

- ele **não cozinha** (não é um agente);
    
- ele **não decide o cardápio** (não é o orquestrador);
    
- ele apenas **abre o restaurante, recebe o cliente e o encaminha para o lugar certo**.
    

Essa simplicidade é proposital: em uma arquitetura bem projetada, o `main.py` costuma ter pouquíssimo código. Quanto menor ele for, melhor costuma estar organizada a aplicação.