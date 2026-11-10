**English** | [Português](#português)

# Projeto Agente Atendimento WhatsApp

n8n workflow with an AI agent that does the first contact with customers of a car repair shop over WhatsApp. It follows a guided service flow, remembers the conversation and notifies the internal team once the customer's request is complete.

> Sanitized workflow: credentials, instance names, domains and shop data (address, phone) were replaced with placeholders.

## What the flow does

1. Receives WhatsApp messages from the Evolution API through a webhook and ignores messages sent by the bot itself.
2. Normalizes each message type (plain text, extended text, ephemeral, audio, image, sticker, video) into a single text field with a `Switch`.
3. Sends the text to an AI agent (OpenAI `gpt-4o-mini`) with a window memory of 10 messages, keyed by the customer's WhatsApp ID.
4. The system prompt computes whether the shop is open (Sao Paulo time zone), sets the tone of voice and drives the service steps: greeting, problem description, follow-up questions, vehicle data and visit forecast.
5. When every item of the checklist is filled, the agent calls the `avisar_grupo_atendimento` tool exactly once, which sends the summary to the team group.
6. The reply goes back to the customer through the Evolution API, and a Redis counter of media messages is reset.

## Technical highlights

- Strict rule in the prompt and in the tool description to call the notification once per service, avoiding both missing and duplicated alerts.
- Open/closed status computed in JavaScript inside the prompt, with no external library.
- Per-customer conversation memory using the WhatsApp ID as session key.
- Handling of several WhatsApp message types before reaching the model.

## Stack

`n8n` · `OpenAI` · `Redis` · `Evolution API (WhatsApp)` · Webhook · JavaScript

## How to import

1. In n8n, choose **Import from File** and select `agente-atendimento-whatsapp.json`.
2. Set your OpenAI and Redis credentials.
3. Replace the placeholders (Evolution API domain, instance, shop data in the prompt).
4. Point the team notification tool to your own group.

---

# Português

Workflow em n8n com um agente de IA que faz o primeiro atendimento de clientes de uma oficina mecânica pelo WhatsApp. Ele segue um fluxo de atendimento guiado, lembra da conversa e avisa a equipe interna quando o pedido do cliente está completo.

> Workflow sanitizado: credenciais, nomes de instância, domínios e dados da oficina (endereço, telefone) foram trocados por placeholders.

## O que o fluxo faz

1. Recebe as mensagens do WhatsApp da Evolution API por webhook e ignora as mensagens enviadas pelo próprio bot.
2. Normaliza cada tipo de mensagem (texto simples, texto estendido, efêmera, áudio, imagem, figurinha, vídeo) em um único campo de texto com um `Switch`.
3. Envia o texto para um agente de IA (OpenAI `gpt-4o-mini`) com memória de janela de 10 mensagens, indexada pelo ID do WhatsApp do cliente.
4. O prompt de sistema calcula se a oficina está aberta (fuso de São Paulo), define o tom de voz e conduz as etapas: saudação, descrição do problema, perguntas de aprofundamento, dados do veículo e previsão de visita.
5. Quando todos os itens do checklist estão preenchidos, o agente chama a ferramenta `avisar_grupo_atendimento` exatamente uma vez, que envia o resumo ao grupo da equipe.
6. A resposta volta ao cliente pela Evolution API, e um contador de mídias no Redis é zerado.

## Destaques técnicos

- Regra rígida no prompt e na descrição da ferramenta para chamar o aviso uma única vez por atendimento, evitando alerta faltando ou duplicado.
- Status aberto/fechado calculado em JavaScript dentro do prompt, sem biblioteca externa.
- Memória de conversa por cliente, usando o ID do WhatsApp como chave de sessão.
- Tratamento de vários tipos de mensagem do WhatsApp antes de chegar ao modelo.

## Stack

`n8n` · `OpenAI` · `Redis` · `Evolution API (WhatsApp)` · Webhook · JavaScript

## Como importar

1. No n8n, escolha **Import from File** e selecione `agente-atendimento-whatsapp.json`.
2. Configure suas credenciais da OpenAI e do Redis.
3. Troque os placeholders (domínio e instância da Evolution API, dados da oficina no prompt).
4. Aponte a ferramenta de aviso para o seu próprio grupo.
