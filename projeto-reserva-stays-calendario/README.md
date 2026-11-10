**English** | [Português](#português)

# Projeto Reserva Stays Calendario

n8n workflow that listens to the webhooks of **Stays** (a property management system for short-term rentals) and keeps three things in sync: the cleaning control sheet, the operation's Google Calendar and the automatic WhatsApp messages sent to the guest.

> Sanitized and condensed workflow: credentials, spreadsheet/calendar/instance IDs, domains and real financial or personal data (PIX key, bank data, company tax ID, a guest name and phone from a pinned example payload) were removed or replaced with placeholders. The internal column schema metadata that n8n generates for Google Sheets was also simplified to reduce the file size. The business logic is intact.

## What the flow does

One webhook receives every reservation event from Stays (`reservation.created`, `.modified`, `.deleted`, `.canceled`, `.requested`, `.reactivated`) and routes each one to its own path:

- **Reservation created**: identifies the unit, writes a new row in the cleaning task sheet, creates Google Calendar events (guest check-in and post-checkout cleaning) and sends the right WhatsApp message: a booking confirmation with an onboarding link (with a unique token) or a pre-booking notice with payment instructions, depending on the status.
- **Reservation modified**: updates the matching sheet row, syncs the existing calendar events and runs a **real-time critical window** check: if the check-out date changed on the same day and within business hours, a sub-workflow warns the team right away, so the provider does not show up on the wrong day.
- **Deleted or canceled**: removes the calendar events, updates the status in the sheet and sends a farewell message to the guest.
- **Schedule conflict check**: whenever a reservation changes, a separate sub-workflow checks for overlapping dates in the same unit.

## Technical highlights

- **Async event modeling**: one webhook handles 6 event types through a `Switch`, each with its own side effects (sheet, calendar, WhatsApp).
- **Idempotency and lookup by external ID**: each sheet row is indexed by the Stays `_id`, so the right record is found and updated on any later event.
- **Critical time window logic**: time calculation in the Sao Paulo time zone with no external library, to decide if a change needs an immediate alert or can wait for the daily routine.
- **Onboarding token generation**: UUID built in plain JavaScript (no dependencies) to create a unique guest data link, with an expiry date counted from check-out.
- **Sub-workflow orchestration**: reusable logic (real-time alert, conflict check) lives in separate workflows called with `Execute Workflow`.
- **Booking vs. pre-booking**: different messages and rules depending on the payment status (paid or waiting for payment).

## Stack

`n8n` · `Google Sheets` · `Google Calendar` · `Evolution API (WhatsApp)` · Webhook (Stays PMS) · JavaScript (Code nodes)

## How to import

1. In n8n, choose **Import from File** and select `reserva-stays-limpeza-calendario.json`.
2. Set your Google Sheets and Google Calendar credentials in the marked nodes.
3. Replace the placeholders (`SUA-PLANILHA-ID-AQUI`, `SUA-API-KEY-AQUI`, `SEU-CALENDAR-ID-...`, domains and similar) with your values.
4. Recreate or point the two referenced sub-workflows (`SEU-WORKFLOW-ID-ALTERACAO-CRITICA` and `SEU-WORKFLOW-ID-CONFLITO`) to the equivalent workflows in your environment.
5. Adjust the pre-booking message with your own payment details.

---

# Português

Workflow em n8n que escuta os webhooks da **Stays** (PMS de gestão de reservas de temporada) e mantém três coisas sincronizadas: a planilha de controle de limpeza, o Google Calendar da operação e as mensagens automáticas enviadas ao hóspede pelo WhatsApp.

> Workflow sanitizado e condensado: credenciais, IDs de planilha/calendário/instância, domínios e dados financeiros ou pessoais reais (chave PIX, dados bancários, CNPJ, nome de hóspede e telefone de um payload de exemplo fixado) foram removidos ou trocados por placeholders. Os metadados internos de schema das colunas do Google Sheets, gerados pelo n8n, também foram simplificados para reduzir o tamanho do arquivo. A lógica de negócio está intacta.

## O que o fluxo faz

Um único webhook recebe todos os eventos de reserva da Stays (`reservation.created`, `.modified`, `.deleted`, `.canceled`, `.requested`, `.reactivated`) e roteia cada um para uma trilha própria:

- **Criação de reserva**: identifica a unidade, grava uma nova linha na planilha de tarefas de limpeza, cria eventos no Google Calendar (check-in do hóspede e limpeza pós-checkout) e dispara a mensagem correta no WhatsApp: confirmação de reserva com link de onboarding (com um token único) ou aviso de pré-reserva com instruções de pagamento, conforme o status.
- **Modificação de reserva**: atualiza a linha correspondente na planilha, sincroniza os eventos já existentes no calendário e roda uma checagem de **janela crítica de tempo real**: se a data de checkout mudou no mesmo dia e dentro do horário comercial, um sub-workflow avisa a equipe na hora, evitando que o prestador chegue num dia errado.
- **Exclusão ou cancelamento**: remove os eventos do calendário, atualiza o status na planilha e envia uma mensagem de despedida ao hóspede.
- **Verificação de conflito de agenda**: sempre que uma reserva muda, um sub-workflow separado verifica sobreposição de datas na mesma unidade.

## Destaques técnicos

- **Modelagem de eventos assíncronos**: um único webhook trata 6 tipos de evento via `Switch`, cada um com seus próprios efeitos (planilha, calendário, WhatsApp).
- **Idempotência e busca por ID externo**: cada linha da planilha é indexada pelo `_id` da Stays, permitindo achar e atualizar o registro certo em qualquer evento seguinte.
- **Lógica de janela de tempo crítica**: cálculo de horário no fuso de São Paulo, sem biblioteca externa, para decidir se a mudança exige alerta imediato ou pode esperar a rotina diária.
- **Geração de token de onboarding**: UUID gerado em JavaScript puro (sem dependências) para criar um link único de preenchimento de dados dos hóspedes, com expiração calculada a partir do checkout.
- **Orquestração via sub-workflows**: lógicas reaproveitáveis (alerta em tempo real, verificação de conflito) ficam em workflows separados, chamados com `Execute Workflow`.
- **Reserva vs. pré-reserva**: mensagens e regras diferentes conforme o status de pagamento (paga ou aguardando pagamento).

## Stack

`n8n` · `Google Sheets` · `Google Calendar` · `Evolution API (WhatsApp)` · Webhook (Stays PMS) · JavaScript (Code nodes)

## Como importar

1. No n8n, escolha **Import from File** e selecione `reserva-stays-limpeza-calendario.json`.
2. Configure suas credenciais do Google Sheets e do Google Calendar nos nós indicados.
3. Troque os placeholders (`SUA-PLANILHA-ID-AQUI`, `SUA-API-KEY-AQUI`, `SEU-CALENDAR-ID-...`, domínios e parecidos) pelos seus valores.
4. Recrie ou aponte os dois sub-workflows referenciados (`SEU-WORKFLOW-ID-ALTERACAO-CRITICA` e `SEU-WORKFLOW-ID-CONFLITO`) para os workflows equivalentes no seu ambiente.
5. Ajuste a mensagem de pré-reserva com seus próprios dados de pagamento.
