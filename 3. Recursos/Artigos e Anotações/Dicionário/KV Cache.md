---
tags:
  - inteligenciaartificial
  - programação
  - inovação
---
>O **Cache KV** (Key-Value Cache) é uma técnica de otimização de memória utilizada em modelos de linguagem (LLMs) que armazena os vetores de **Key** (Chave) e **Value** (Valor) calculados para tokens anteriores, evitando que o modelo recalcule toda a história da conversa a cada nova resposta.

Essa tecnologia auxilia na economia de tokens e custos de duas formas principais:
- **Eficiência no Processamento (Decode Phase):** Ao reutilizar os cálculos já feitos, o modelo reduz a complexidade computacional de quadrática para linear. Isso permite gerar tokens muito mais rápido, consumindo menos poder de processamento (e energia) por interação. 
- **Prompt Caching (Economia Direta de Dinheiro):** Em APIs de IA, como as da Anthropic, o **Prompt Caching** armazena o estado do cache KV de partes fixas da entrada (como instruções do sistema ou contexto longo).  Se a mesma entrada for enviada novamente, o sistema recupera os tensores em vez de processá-los do zero. Isso reduz drasticamente o custo: as **leituras do cache custam apenas 10%** do preço normal, enquanto as **escritas custam 25%** a mais que o preço base. 

Em resumo, o Cache KV **reduz o custo computacional e financeiro** de processá-los, permitindo que sistemas atendam a mais usuários com menos recursos e oferecendo descontos significativos para requisições que reutilizam o mesmo contexto.

---
### Ta, mas o que é um [[Cache]]