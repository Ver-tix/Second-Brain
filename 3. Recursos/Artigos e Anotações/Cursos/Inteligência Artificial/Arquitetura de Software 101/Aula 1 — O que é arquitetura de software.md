---
tags:
  - programação
  - inteligenciaartificial
---
# Aula 1 — O que é arquitetura de software
Esquece a definição de livro por um segundo. Pensa assim:

**Arquitetura de software é a decisão de "onde cada responsabilidade mora" dentro de um sistema.**

Não é sobre linhas de código. É sobre organização. Tipo montar uma casa: antes de escolher a cor da parede, alguém decide onde fica o cano de água, onde fica a fiação elétrica, onde fica a estrutura que aguenta o telhado. Se você não decide isso antes, a casa até fica de pé — mas reformar ela depois é um inferno.

Um sistema de software é a mesma coisa. Ele tem "canos" (dados fluindo), "fiação" (regras de negócio) e "estrutura" (o que sustenta tudo). Arquitetura é a decisão consciente de onde cada uma dessas coisas vive.

**Por que isso importa tanto?**

Porque todo sistema muda com o tempo. Regra nova aparece, dado novo precisa ser guardado, o sistema cresce. Se tudo estiver misturado num só lugar — como você viu no desafio da universidade, quando tudo tava "dentro do prompt" — qualquer mudança pequena vira um risco de quebrar tudo.

Arquitetura boa = mudança pequena, impacto pequeno.  
Arquitetura ruim = mudança pequena, sistema inteiro treme.

**Conectando com o que você já fez:**

Nos dois desafios que você resolveu, a "arquitetura" era: separar **quem decide** (aplicação) de **quem conversa** (LLM). Isso é um caso específico de um princípio bem mais geral chamado **separação de responsabilidades** (in inglês, "separation of concerns") — que é basicamente o alicerce de toda arquitetura de software. Toda aula daqui pra frente é uma variação desse mesmo princípio.
