**English** | [Português](#português)

# Projeto Agenda Limpeza WhatsApp

Scheduled n8n workflow that reads the cleaning task register in Google Sheets and tells each team, over WhatsApp, about the next day's tasks.

> Sanitized workflow: spreadsheet IDs, phone numbers, domains and instance names were replaced with placeholders.

## What the flow does

1. Runs every day at 20:55 (`55 20 * * *`).
2. Reads two sheets: the unit register (`CAD`, with the team of each unit) and the task log (`LOG`).
3. Keeps only the tasks with check-out date equal to tomorrow and status `reservado`.
4. Groups the tasks by team and sorts them in time order.
5. Builds one message per recipient: the provider, two managers and, when configured, a copy recipient for that provider. The header uses the provider's name taken from the unit register.
6. Sends each message through the Evolution API, one by one, with a configurable delay between them.
7. Stops without sending anything when there are no tasks for tomorrow.

## Technical highlights

- Routing of recipients driven by one config node: manager phones and a JSON map of provider to copy recipient.
- Date window handled in plain JavaScript (tomorrow only).
- Loop with delay between messages to avoid blocking by the messaging service.
- Guard filter so an empty day does not trigger any send.

## Stack

`n8n` · `Google Sheets` · `Evolution API (WhatsApp)` · Schedule trigger · JavaScript (Code nodes)

## How to import

1. In n8n, choose **Import from File** and select `agenda-limpeza-whatsapp.json`.
2. Set your Google Sheets credentials.
3. Fill the `Set: Config` node (manager phones, copy routes, delay) and replace the spreadsheet ID, Evolution API domain and instance.

---

# Português

Workflow agendado em n8n que lê o cadastro de tarefas de limpeza no Google Sheets e avisa cada equipe, pelo WhatsApp, sobre as tarefas do dia seguinte.

> Workflow sanitizado: IDs de planilha, telefones, domínios e nomes de instância foram trocados por placeholders.

## O que o fluxo faz

1. Roda todo dia às 20:55 (`55 20 * * *`).
2. Lê duas planilhas: o cadastro de unidades (`CAD`, com a equipe de cada unidade) e o log de tarefas (`LOG`).
3. Mantém só as tarefas com data de checkout igual a amanhã e status `reservado`.
4. Agrupa as tarefas por equipe e ordena em ordem de horário.
5. Monta uma mensagem por destinatário: o prestador, dois gerentes e, quando configurado, um destinatário de cópia daquele prestador. O cabeçalho usa o nome do prestador vindo do cadastro de unidades.
6. Envia cada mensagem pela Evolution API, uma a uma, com atraso configurável entre elas.
7. Encerra sem enviar nada quando não há tarefas para amanhã.

## Destaques técnicos

- Roteamento de destinatários controlado por um único nó de configuração: telefones dos gerentes e um mapa JSON de prestador para destinatário de cópia.
- Janela de datas tratada em JavaScript puro (somente amanhã).
- Loop com atraso entre mensagens para evitar bloqueio pelo serviço de mensagens.
- Filtro de segurança para que um dia sem tarefas não dispare nenhum envio.

## Stack

`n8n` · `Google Sheets` · `Evolution API (WhatsApp)` · Schedule trigger · JavaScript (Code nodes)

## Como importar

1. No n8n, escolha **Import from File** e selecione `agenda-limpeza-whatsapp.json`.
2. Configure suas credenciais do Google Sheets.
3. Preencha o nó `Set: Config` (telefones dos gerentes, rotas de cópia, atraso) e troque o ID da planilha, o domínio e a instância da Evolution API.
