```
 _   _   ___   _   _
| \ | | ( _ ) | \ | |
|  \| | / _ \ |  \| |
| |\  || (_) || |\  |
|_| \_| \___/ |_| \_|

    _     _   _  _____   ___   __  __     _     _____  ___   ___   _   _  ____
   / \   | | | ||_   _| / _ \ |  \/  |   / \   |_   _||_ _| / _ \ | \ | |/ ___|
  / _ \  | | | |  | |  | | | || |\/| |  / _ \    | |   | | | | | ||  \| |\___ \
 / ___ \ | |_| |  | |  | |_| || |  | | / ___ \   | |   | | | |_| || |\  | ___) |
/_/   \_\ \___/   |_|   \___/ |_|  |_|/_/   \_\  |_|  |___| \___/ |_| \_||____/
```

**English** | [Português](README.pt-BR.md)

---

## About

n8n automation projects of mine, running in production. They use AI agents, WhatsApp (Evolution API), Google Sheets and Calendar, and PMS webhooks.

Every workflow was sanitized before being stored here: credentials, API keys, internal IDs, domains and any real personal or financial data were removed and replaced with placeholders. The business logic and architecture are the ones used in production.

---

## Projects

| Project | Description | Stack |
|---|---|---|
| [Projeto Agente Atendimento WhatsApp](projeto-agente-atendimento-whatsapp) | AI agent that handles the first contact with customers of a car repair shop over WhatsApp, with conversation memory and a team notification | n8n, OpenAI, Redis, Evolution API |
| [Projeto Agenda Limpeza WhatsApp](projeto-agenda-limpeza-whatsapp) | Scheduled workflow that tells each cleaning team, over WhatsApp, about the next day's tasks | n8n, Google Sheets, Evolution API |
| [Projeto Reserva Stays Calendario](projeto-reserva-stays-calendario) | Listens to PMS (Stays) webhooks and keeps the control sheet, Google Calendar and guest messages in sync | n8n, Google Sheets, Google Calendar, Evolution API |
| [Projeto Onboarding Hospede](projeto-onboarding-hospede) | Receives the guest onboarding form, stores it, updates the calendar and warns the building front desk on same-day check-ins | n8n, Google Sheets, Google Calendar, Evolution API |

---

## How to use

Each project folder has the exported workflow (`.json`) and its own README. To try one:

1. In n8n, choose **Import from File** and select the `.json` of the project.
2. Set up your own credentials in the marked nodes.
3. Replace the placeholders (`SUA-PLANILHA-ID-AQUI`, `seu-dominio-evolution.com` and similar) with your values.

---

## Structure

```
automacoes_n8n/
|-- projeto-agente-atendimento-whatsapp/
|-- projeto-agenda-limpeza-whatsapp/
|-- projeto-reserva-stays-calendario/
|-- projeto-onboarding-hospede/
|-- README.md
`-- README.pt-BR.md
```

---

## Contact

Pedro Vignoli, student of Technology in Artificial Intelligence. Main interests: AI, automation and data.

- LinkedIn: [linkedin.com/in/pedro-vignoli-977999427](https://linkedin.com/in/pedro-vignoli-977999427)
- E-mail: pedro.code@outlook.pt
