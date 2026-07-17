---
tags:
  - inteligenciaartificial
---


Abra a Web do ADK executando:

```bash
cd ~/adk_tutorialsource .adk_env/bin/activateadk web
```

Depois de executar os comandos, você verá uma saída no terminal indicando que o servidor da Web do ADK foi iniciado, semelhante a esta:

```text
+-----------------------------------------------------------------------------+| ADK Web Server started                                                      ||                                                                             || For local testing, access at http://localhost:8000.                         |+-----------------------------------------------------------------------------+INFO:     Application startup complete.INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

👉 Em seguida, abra o navegador em [**http://localhost:8000**](http://localhost:8000/) para acessar a **interface de desenvolvimento do ADK**.

**Executando no Cloud Shell em vez de localmente?** Clique no ícone **Visualização da Web** na barra de ferramentas do Cloud Shell (geralmente no canto superior direito), selecione **Alterar porta**, defina a porta como **8000** e clique em **"Alterar e visualizar"**. O Cloud Shell vai abrir a interface de desenvolvimento do ADK em uma nova guia do navegador.

![webpreview](https://codelabs.developers.google.com/static/adkcourse/img/05-01-webpreview.png?hl=pt-br)

👉 Seu ritual de invocação está concluído e o agente está em execução. A **interface de desenvolvimento do ADK** no navegador é sua conexão direta com o Familiar.

**Escolha seu primeiro agente** No menu suspenso na parte de cima da interface, escolha **`a_single_agent`**.

Você pode selecionar **`a_single_agent`** aqui: ![traçando a imagem de agentes únicos](https://codelabs.developers.google.com/static/adkcourse/img/single_agent_result.png?hl=pt-br)

Você pode conferir o rastreamento aqui: ![traçando a imagem de um único agente](https://codelabs.developers.google.com/static/adkcourse/img/single_agent_tracing.png?hl=pt-br)

👉 **Comando de teste:**

```
Plan a trip from Sunnyvale to San Francisco this weekend, I love food and art.
```