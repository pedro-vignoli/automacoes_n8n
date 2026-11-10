**English** | [Português](#português)

# Projeto Onboarding Hospede

n8n workflow that receives, through a webhook, the onboarding form filled in by the guest (ID document, companions, vehicle, acceptance of terms) and spreads that data to every system that needs it: control sheet, Google Calendar, WhatsApp confirmation and, in urgent cases, an automatic alert to the building front desk.

> Sanitized workflow: this was the most sensitive project. The original file had a pinned example data block (`pinData`) with **real personal ID numbers, full names of guests and companions, a phone number, an access IP and the contract acceptance hash** from real stays. That whole block was removed. Credentials, spreadsheet/calendar IDs, domain and API key were also replaced with placeholders.

## What the flow does

1. Receives the **onboarding form submit** (filled in by the guest from the link sent after the booking).
2. Validates the required fields (rejects the request if `reserva_id` or the holder's document is missing).
3. Writes the data to a dedicated onboarding sheet (document, companions, vehicle data, terms acceptance metadata: IP, user agent, timestamp and hash of the accepted contract).
4. Looks up the matching reservation in the main log and **calculates whether check-in is today**, flagging the reservation as "urgent" when it is.
5. Updates the guest's Google Calendar event with a full summary: holder, companions, documents and vehicle.
6. Sends a WhatsApp confirmation to the guest (skipped for Airbnb, which has different contact rules).
7. Triggers a "guest journey" sub-workflow to continue the next communication steps.
8. **If check-in is today and it is past a set time**, builds and sends a message to the building front desk with the list of authorized guests, documents, times and vehicle data, avoiding a last-minute phone call.

## Technical highlights

- **Payload validation at the entry**: the workflow throws an explicit error when required data is missing, instead of letting incomplete data flow silently through the next steps.
- **Date-based urgency calculation**: normalized date comparison (time set to zero) to decide, deterministically, whether the reservation is "today".
- **Dynamic front desk message**: a Code node builds a formatted message from three sources (task sheet, unit register and the filled-in onboarding), including parsing a companion list that arrives as JSON.
- **Business rule per channel**: confirmation messages are blocked for Airbnb reservations, following the platform's own contact policy.
- **Building/unit name composition** with a regex, splitting text from numbers in a combined field.

## Stack

`n8n` · `Google Sheets` · `Google Calendar` · `Evolution API (WhatsApp)` · Webhook · JavaScript (Code nodes)

## How to import

1. In n8n, choose **Import from File** and select `onboarding-hospede-submit.json`.
2. Set your Google Sheets and Google Calendar credentials.
3. Replace the placeholders (`SUA-PLANILHA-ID-AQUI`, `SUA-API-KEY-AQUI`, `SEU-CALENDAR-ID-...`, Evolution API domain and similar).
4. Point `SEU-WORKFLOW-ID-JORNADA-HOSPEDE` to the equivalent sub-workflow in your environment (or remove that step if you do not have one).
5. Adjust the message texts to your own operation.

**Privacy note:** this flow handles sensitive personal data of guests (document, name, vehicle). When reusing it, make sure you follow the LGPD (Brazil's data protection law). Avoid pinning real execution data in the saved workflow, because that leaves it permanently stored in the exported JSON file.

---

# Português

Workflow em n8n que recebe, por webhook, o formulário de onboarding preenchido pelo hóspede (documento, acompanhantes, veículo, aceite de termos) e distribui essa informação para todos os sistemas que precisam dela: planilha de controle, Google Calendar, confirmação por WhatsApp e, em casos urgentes, alerta automático para a portaria do prédio.

> Workflow sanitizado: este era o projeto mais sensível. O arquivo original trazia um bloco de dados de exemplo fixado (`pinData`) com **CPFs reais, nomes completos de hóspedes e acompanhantes, telefone, IP de acesso e o hash de aceite de contrato** de hospedagens reais. Todo esse bloco foi removido. Credenciais, IDs de planilha/calendário, domínio e API key também foram trocados por placeholders.

## O que o fluxo faz

1. Recebe o **submit do formulário de onboarding** (preenchido pelo hóspede a partir do link enviado após a reserva).
2. Valida os campos obrigatórios (recusa a requisição se faltar `reserva_id` ou documento do titular).
3. Grava os dados em uma planilha dedicada de onboarding (documento, acompanhantes, dados do veículo, metadados do aceite de termos: IP, user agent, timestamp e hash do contrato aceito).
4. Busca a reserva correspondente no log principal e **calcula automaticamente se o check-in é hoje**, marcando a reserva como "urgente" quando for o caso.
5. Atualiza o evento do Google Calendar do hóspede com um resumo completo: titular, acompanhantes, documentos e veículo.
6. Envia uma mensagem de confirmação por WhatsApp ao hóspede (pulando o Airbnb, cujas regras de contato são diferentes).
7. Dispara um sub-workflow de "jornada do hóspede" para dar sequência às próximas etapas de comunicação.
8. **Se o check-in é hoje e já passou de determinado horário**, monta e envia automaticamente uma mensagem para a portaria do prédio com a lista de hóspedes autorizados, documentos, horários e dados do veículo, evitando ligação manual em cima da hora.

## Destaques técnicos

- **Validação de payload na entrada**: o workflow lança erro explícito se faltar dado obrigatório, em vez de deixar a informação incompleta seguir silenciosamente pelas próximas etapas.
- **Cálculo de urgência baseado em data**: comparação de datas normalizada (zera horário) para decidir, de forma determinística, se a reserva é "hoje".
- **Geração dinâmica de mensagem para portaria**: um nó de Code monta uma mensagem formatada juntando dados de três fontes (planilha de tarefas, cadastro de unidades e onboarding preenchido), incluindo parsing de uma lista de acompanhantes vinda como JSON.
- **Regra de negócio por canal**: mensagens de confirmação são bloqueadas para reservas do Airbnb, respeitando a política de contato da própria plataforma.
- **Composição de nome do edifício/unidade** via regex, separando texto de números num campo combinado.

## Stack

`n8n` · `Google Sheets` · `Google Calendar` · `Evolution API (WhatsApp)` · Webhook · JavaScript (Code nodes)

## Como importar

1. No n8n, escolha **Import from File** e selecione `onboarding-hospede-submit.json`.
2. Configure suas credenciais do Google Sheets e do Google Calendar.
3. Troque os placeholders (`SUA-PLANILHA-ID-AQUI`, `SUA-API-KEY-AQUI`, `SEU-CALENDAR-ID-...`, domínio da Evolution API e parecidos).
4. Aponte o `SEU-WORKFLOW-ID-JORNADA-HOSPEDE` para o sub-workflow equivalente no seu ambiente (ou remova essa etapa se não tiver).
5. Ajuste o texto das mensagens com os dados da sua operação.

**Nota de privacidade:** esse fluxo lida com dados pessoais sensíveis de hóspedes (documento, nome, veículo). Ao reutilizar este workflow, garanta conformidade com a LGPD. Evite fixar (`pin`) dados de execução reais no workflow salvo, já que isso os deixa gravados permanentemente no arquivo JSON exportado.
