---
tipo:
  - sintese
dominio:
  - IA
Subdominio:
  - spec-driven_development
---
## 1. Estrutura de Arquivos da Nossa Automação

Crie uma pasta no seu computador com a seguinte estrutura de arquivos `.md`:

```Plaintext
automacao-agenda-sdd/
├── .sdd/
│   └── system_instructions.md      # Instruções de compilação para a IA executora
├── .specs/
│   ├── 01_architecture.md          # Stack (FastAPI + Google API)
│   ├── 02_agent_rules.md           # Regras do calendário (Horários, Meet, Buffer)
│   ├── 03_calendar_integrations.md  # Contratos JSON dos Endpoints
│   └── 04_acceptance_tests.md      # Testes comportamentais BDD
└── src/                            # Onde a IA executora vai criar o código
```

## 2. Conteúdo dos Arquivos da Especificação (O Spec Completo)

Aqui estão os arquivos prontos. Basta copiar e colar dentro dos respectivos arquivos Markdown:

### Arquivo 1: `.sdd/system_instructions.md`
_(Este arquivo ensina a outra IA como agir ao ler este projeto)_

```Markdown
\# Diretrizes para a IA Executora (Desenvolvedor Python)

\## 1. Preparação obrigatoria do Ambiente de Desenvolvimento
Antes de gerar qualquer arquivo de código ou teste, você DEVE garantir que o ambiente virtual Python esteja configurado:
1. Verifique se a pasta `.venv/` existe na raiz do projeto.
2. Se NÃO existir, crie o ambiente virtual usando `python -m venv .venv`.
3. Crie o arquivo `requirements.txt` com todas as dependências necessárias.
4. Execute a instalação de pacotes no terminal do ambiente virtual (`.venv/Scripts/pip` no Windows ou `.venv/bin/pip` no Linux/Mac).

\## 2. Leitura e Cumprimento Estrito dos Specs
1. Leia e analise todos os arquivos contidos em `.specs/` antes de escrever qualquer linha de código.
2. Salve o código da aplicação estritamente em `src/` e a suíte de testes em `tests/`.
3. NUNCA adicione bibliotecas, rotas ou regras de negócio que não foram explicitamente definidas na especificação.
4. Se houver divergências ou ambiguidades, a regra contida em `.specs/` tem prioridade absoluta.
```

### Arquivo 2: `.specs/01_architecture.md`

```Markdown
\# Spec 01: Arquitetura da Automação do Google Calendar

\## 1. Propósito
API de alta performance em Python para gerenciamento autônomo e inteligente de compromissos no Google Calendar, garantindo prevenção de conflitos e criação automatizada de salas de reunião.

\## 2. Stack Tecnológica
- Linguagem: Python 3.11+
- Framework Web: FastAPI (pydantic para validação de esquemas)
- Servidor ASGI: Uvicorn
- SDK do Google: `google-api-python-client`, `google-auth-oauthlib`, `google-auth-httplib2`
- Autenticação: Service Account ou OAuth 2.0 com escopo `https://www.googleapis.com/auth/calendar`

\## 3. Mapeamento de Diretórios de Saída
- `src/main.py`: Ponto de entrada FastAPI e rotas de API.
- `src/services/calendar_service.py`: Camada de comunicação com a API do Google Calendar.
- `src/schemas/events.py`: Validação Pydantic dos dados de entrada e saída.
- `src/core/config.py`: Variáveis de ambiente (Timezone, Credenciais Google, etc.).

\## Dependências de Ambiente e Fuso Horário (Cross-Platform)
- Para garantir compatibilidade com ambientes Windows, a biblioteca `tzdata` DEVE estar listada no `requirements.txt`.
- Toda manipulação de data/hora deve utilizar `zoneinfo.ZoneInfo("America/Fortaleza")` garantindo que o fuso horário esteja devidamente configurado mesmo no SO Windows.
```

### Arquivo 3: `.specs/02_agent_rules.md`

```Markdown
\# Spec 02: Regras do Calendário e Guardrails Operacionais

\## 1. Janela Operacional (Horário Comercial)
- Dias permitidos: Segunda a Sexta-feira.
- Horário permitido: 09:00 às 18:00.
- Fuso Horário Padrão: `America/Fortaleza` (ou `America/Sao_Paulo`).
- Bloqueio Rígido de Almoço: NENHUM evento pode ser agendado entre 12:00 e 13:30.

\## 2. Regras de Duração, Intervalos e Videochamada
- Duração padrão de reuniões: 30 minutos (caso o cliente não envie a duração).
- Buffer de descanso: Deve haver no mínimo 10 minutos de intervalo livre entre eventos consecutivos.
- Google Meet Automático: Todo evento criado DEVE conter uma sala do Google Meet gerada automaticamente (`conferenceData.createRequest`).

\## 3. Guardrails e Prevenção de Falhas
- NUNCA permitir agendamentos em finais de semana ou fora da janela das 09h às 18h, a menos que a flag `force_override: true` seja enviada no JSON.
- Em caso de conflito de horário ou falta do buffer de 10 minutos, O SISTEMA NÃO DEVE SOBRESCREVER o evento existente. Deve retornar um erro de conflito acompanhado das próximas 3 opções de horários livres.
```

### Arquivo 4: `.specs/03_calendar_integrations.md`

```Markdown
\# Spec 03: Contrato de Integração da API (FastAPI)

\## 1. Endpoint: `POST /api/v1/events/create`
Cria um novo evento no Google Calendar.

\### JSON de Entrada (Request Body):
```json
{
  "summary": "Reunião de Alinhamento de Automação",
  "description": "Discussão de escopo com o cliente",
  "start_time": "2026-08-10T10:00:00",
  "duration_minutes": 30,
  "attendees": ["cliente@empresa.com"],
  "force_override": false
}
```

### Resposta de Sucesso (HTTP 201 Created):

```JSON
{
  "status": "success",
  "event_id": "google_event_id_123",
  "html_link": "[https://calendar.google.com/calendar/event?eid=](https://calendar.google.com/calendar/event?eid=)...",
  "meet_link": "[https://meet.google.com/abc-defg-hij](https://meet.google.com/abc-defg-hij)",
  "start_time": "2026-08-10T10:00:00-03:00",
  "end_time": "2026-08-10T10:30:00-03:00"
}
```

## 2. Endpoint: `GET /api/v1/events/free-slots`

Busca os próximos horários livres no dia solicitado respeitando a janela de trabalho e o intervalo de almoço
- Query Parameter: `date` (Ex: `2026-08-10`)  

````
---

### Arquivo 5: `.specs/04_acceptance_tests.md`

```markdown
# Spec 04: Critérios de Aceite (BDD)

## Cenário 1: Agendamento bem-sucedido com Google Meet
- GIVEN que o usuário solicita uma reunião para Segunda-feira às 10:00
- WHEN a API `POST /api/v1/events/create` for acionada
- THEN o evento deve ser salvo no Google Calendar
- AND a resposta deve conter um link válido do `meet_link`
- AND o status de retorno deve ser HTTP 201.

## Cenário 2: Bloqueio do Horário de Almoço
- GIVEN que o usuário solicita um agendamento para Terça-feira às 12:30
- WHEN a requisição for processada com `force_override: false`
- THEN a API deve recusar a solicitação com HTTP 400 (Bad Request)
- AND retornar a mensagem: "Horário dentro do período reservado para almoço (12:00 às 13:30)".

## Cenário 3: Detecção de Conflito e Sugestão de Horários
- GIVEN que já existe uma reunião agendada para Quarta-feira das 14:00 às 14:30
- WHEN o usuário tenta agendar outra reunião para Quarta-feira às 14:20
- THEN a API deve retornar HTTP 409 (Conflict)
- AND retornar a lista `suggested_slots` com no mínimo 3 horários disponíveis no mesmo dia.
````

## 3. O Prompt Final para a IA Executora

Quando você abrir o seu assistente de IA em seu editor de código favorito (Cursor, Windsurf, VS Code com Claude Code, ou no ChatGPT), **basta enviar a seguinte mensagem**:
```Plaintext
Atue estritamente como o agente desenvolvedor configurado em .sdd/system_instructions.md.

Sua prioridade imediata é:
1. Configurar o ambiente virtual Python criando a pasta `.venv` na raiz.
2. Gerar o `requirements.txt` com base na arquitetura.
3. Instalar os pacotes do `requirements.txt` diretamente dentro do `.venv` recém-criado.

Em seguida, leia todos os arquivos de `.specs/` e:
4. Implemente todo o código da aplicação em `src/`.
5. Crie a suíte de testes com Pytest em `tests/` cobrindo 100% das regras BDD de `.specs/04_acceptance_tests.md`.
6. Execute o comando de teste usando o ambiente virtual (`.venv/Scripts/pytest` no Windows) e garanta que todos os testes passem.

Pode iniciar o processo de setup e codificação.
```

## Evoluindo:
### Como testar sua API na prática agora

Para ver e interagir com os endpoints criados a partir dos specs:

1. **Abra a documentação interativa (Swagger UI):** Acesse no navegador: **`[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)`**
    
2. **Faça um teste de agendamento:**
    
    - Na página do Swagger, clique em `POST /api/v1/events/create`.
        
    - Clique em **Try it out**.
        
    - Cole um JSON de teste com um horário de almoço (ex: 12:30) para ver o erro `400` programado no Spec, ou um horário comercial válido (ex: 10:00) para ver o evento criado com o link do Google Meet!
        

Você acabou de passar por todo o ciclo de engenharia com SDD: **Especificou -> Deixou a IA configurar o ambiente e criar o código -> Executou testes automatizados BDD -> Subiu a API sem erros.**

Faça um teste para ver a **regra do almoço** em ação:
```JSON
{
  "summary": "Reunião de Teste de Almoço",
  "description": "Validando o bloqueio de horário comercial",
  "start_time": "2026-08-10T12:30:00",
  "duration_minutes": 30,
  "attendees": ["teste@empresa.com"],
  "force_override": false
}
```
- **Validação de Final de Semana:** Sábado/Domingo ❌ _(Barrado aqui)_
- **Validação da Janela de Trabalho:** 09:00 às 18:00
- **Validação de Almoço:** 12:00 às 13:30
- **Validação de Conflito de Horário e Buffer**
## Próximos passos:
### Opção 1: Evoluir as Especificações no SDD (Recomendado)

Como você está praticando SDD, a melhor forma de adicionar novas funcionalidades é **escrever um novo spec primeiro** e pedir para o Composer implementar.

Algumas ideias de novos specs:

1. **Spec 05 — Notificações e Webhooks:** Enviar e-mail de confirmação ou disparar uma mensagem no WhatsApp/Telegram assim que a reunião for agendada.
    
2. **Spec 06 — Reagendamento e Cancelamento:** Criar os endpoints `PUT /api/v1/events/{id}` e `DELETE /api/v1/events/{id}` com os devidos guardrails (ex: só permitir cancelamentos com mais de 2 horas de antecedência).
    
3. **Spec 07 — Multi-calendários:** Permitir agendar em agendas diferentes dependendo do tipo de assunto (ex: suporte vs. vendas).
    

### Opção 2: Conectar essa API a uma IA de Atendimento (n8n, Make ou LangChain/Dify)

Sua API FastAPI agora está pronta para servir de **Ferramenta (Tool)** para agentes de IA de atendimento (WhatsApp, Instagram, Webchat).

- Você pode expor seu servidor local na internet usando o **ngrok** (`ngrok http 8000`).
    
- Configurar o n8n ou Make para chamar o endpoint `GET /api/v1/events/free-slots` e `POST /api/v1/events/create`.
    
- O agente de IA conversará com o cliente, consultará os horários vagos e fechará o agendamento sozinho.
    

### Opção 3: Organizar o Repositório e Documentação do Projeto

Para deixar seu projeto profissional no GitHub:

1. Garanta que o arquivo `.gitignore` esteja configurado para **NUNCA** subir o `.venv`, `credentials.json` e `token.json`.
    
2. Peça ao Composer para gerar um arquivo `README.md` bem estruturado na raiz, explicando o que é o projeto, a arquitetura SDD e como subir a aplicação.
    

Qual desses caminhos você gostaria de explorar agora? Se quiser, podemos escrever o Spec da próxima funcionalidade juntos!