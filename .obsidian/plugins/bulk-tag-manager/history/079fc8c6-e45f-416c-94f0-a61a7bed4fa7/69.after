---
tags:
  - IA
---

```
<question_set>Antes de escrever qualquer código, quero que você responda a uma pergunta arquitetural.

Imagine que, daqui a seis meses, você precise transformar esse programa simples em um assistente corporativo com:

- memória;
- consulta a documentos;
- múltiplos provedores;
- autenticação de usuários;
- logs.

<b>Quais arquivos ou módulos da estrutura acima você acredita que permanecerão praticamente inalterados e quais deverão crescer ou ser subdivididos?</b>

Justifique sua resposta pensando em:
- responsabilidade única;
- escalabilidade;
- manutenção.
</question_set>

<answer_set>
`main.py` não será alterado. Sua responsabilidade única é apenas ser o ponto de entrada e coordenar tudo. A estrutura do input, no caso a linguagem humana, é mantida.

De resto, sinto que seriam sim alterados: `config.py` seria alterado porque a API key e configurações mudam de Provider para Provider; `providers/` mudariam (é o pressuposto da questão).
</asnswer_set>
```
---
# Feedback
Caio,

Antes de corrigir, preciso dizer uma coisa que me deixou bastante satisfeito.

Você **não respondeu pensando no presente**.

Você respondeu pensando em **como o sistema evoluiria**.

Essa mudança de perspectiva é exatamente o objetivo deste módulo.

Agora vamos à correção.

---

# Avaliação geral

**Nota: 8,8/10**

O raciocínio central está correto, mas há um detalhe importante que quero lhe ensinar.

---

# O acerto

Você escreveu:

> "`main.py` não será alterado. Sua responsabilidade única é apenas ser o ponto de entrada e coordenar tudo."

Excelente.

Isso mostra que você entendeu o princípio da **Responsabilidade Única (SRP - Single Responsibility Principle)**.

O `main.py` não deve conhecer OpenAI.

Não deve conhecer banco de dados.

Não deve conhecer memória.

Ele apenas inicializa a aplicação.

Na prática, um `main.py` profissional pode ficar praticamente igual durante anos.

---

# O ponto que merece refinamento

Você escreveu:

> "`config.py` seria alterado porque a API key e configurações mudam de Provider para Provider."

Essa conclusão parece intuitiva, mas há uma sutileza arquitetural.

Na verdade, **o arquivo `config.py` idealmente muda muito pouco**.

Por quê?

Porque sua responsabilidade não é conhecer a OpenAI.

Sua responsabilidade é:

> **Ler configurações.**

Por exemplo:

Hoje:

```text
OPENAI_API_KEY
MODEL
```

Amanhã:

```text
ANTHROPIC_API_KEY
MODEL
```

Depois:

```text
GOOGLE_API_KEY
MODEL
```

Quem mudou?

Os valores.

Não necessariamente a lógica do `config.py`.

Ele continua fazendo a mesma coisa:

```text
Ler variáveis

↓

Entregar configurações
```

Ou seja:

**A responsabilidade permanece a mesma.**

---

# Sobre `providers/`

Aqui você acertou completamente.

Esse diretório provavelmente será o que mais crescerá.

Hoje:

```text
providers/

└── openai_provider.py
```

Depois:

```text
providers/

├── openai_provider.py

├── anthropic_provider.py

├── google_provider.py

├── local_provider.py

└── provider_factory.py
```

Perceba que o diretório cresce, mas **cada arquivo continua pequeno**.

Isso é arquitetura modular.

---

# O que faltou perceber

A pergunta mencionava:

- memória;
    
- documentos;
    
- autenticação;
    
- logs.
    

Esses itens, idealmente, **não seriam colocados dentro dos arquivos existentes**.

Você criaria **novos módulos**.

Por exemplo:

```text
hello_llm/

main.py

config.py

providers/

memory/

documents/

auth/

logging/

models/

services/

utils/
```

Veja o padrão.

Em vez de aumentar arquivos antigos...

...criamos novos componentes.

Isso é muito importante.

---

# Escalabilidade

Aqui existe um princípio muito bonito.

Quando um sistema cresce...

...os arquivos não deveriam crescer muito.

Quem deveria crescer é a quantidade de módulos especializados.

É quase uma analogia biológica.

Uma célula enorme é pouco eficiente.

Um organismo cresce criando novas células especializadas.

---

# Manutenção

Imagine dois cenários.

## Cenário A

Um arquivo.

```text
main.py

4200 linhas
```

Boa sorte.

Agora outro.

```text
memory.py

120 linhas

documents.py

180 linhas

provider.py

150 linhas

auth.py

90 linhas
```

Muito melhor.

Você sabe exatamente onde procurar.

---

# Um insight importante

Você escreveu:

> "A estrutura do input, no caso a linguagem humana, é mantida."

Gostei da observação, mas ela pode ser refinada.

O motivo pelo qual o `main.py` tende a permanecer estável **não é porque a entrada continua sendo linguagem humana**.

É porque sua **responsabilidade arquitetural** continua sendo a mesma:

```text
Inicializar

↓

Receber entrada

↓

Chamar o sistema

↓

Mostrar saída
```

Mesmo que amanhã a entrada deixe de ser texto e passe a ser voz, imagem ou vídeo, o papel do `main.py` ainda será coordenar o fluxo.

---

# O exercício escondido

Quero revelar algo.

Essa pergunta nunca foi sobre Python.

Ela era sobre arquitetura.

Você percebe?

Se eu trocasse Python por Java, C#, Go ou Rust...

...a resposta seria praticamente igual.

Porque estamos estudando **Engenharia de Software**, não apenas uma linguagem.

---

# Um desafio extra

Vou deixar uma reflexão para você.

Imagine que daqui a um ano esse assistente corporativo tenha **100 mil usuários**.

Você acha que `main.py` continuará sendo um único arquivo?

Ou surgirão novos pontos de entrada?

Por exemplo:

- interface web;
    
- aplicativo mobile;
    
- integração com Slack;
    
- integração com WhatsApp;
    
- [[API REST]].
    

Todos usando o mesmo núcleo da aplicação.

Se você conseguir responder essa pergunta sozinho nas próximas aulas, perceberá que começou a enxergar sistemas como um arquiteto, e não apenas como um programador. Isso será um excelente sinal da sua evolução.