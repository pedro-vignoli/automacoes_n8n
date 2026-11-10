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

[English](README.md) | **Português**

---

## Sobre

Projetos meus de automação em n8n, rodando em produção. Usam agentes de IA, WhatsApp (Evolution API), Google Sheets e Calendar, e webhooks de PMS.

Todos os workflows foram sanitizados antes de entrar aqui: credenciais, chaves de API, IDs internos, domínios e qualquer dado pessoal ou financeiro real foram removidos e trocados por placeholders. A lógica de negócio e a arquitetura são as usadas em produção.

---

## Projetos

| Projeto | Descrição | Stack |
|---|---|---|
| [Projeto Agente Atendimento WhatsApp](projeto-agente-atendimento-whatsapp) | Agente de IA que faz o primeiro atendimento de clientes de uma oficina mecânica pelo WhatsApp, com memória de conversa e aviso para a equipe | n8n, OpenAI, Redis, Evolution API |
| [Projeto Agenda Limpeza WhatsApp](projeto-agenda-limpeza-whatsapp) | Workflow agendado que avisa cada equipe de limpeza, pelo WhatsApp, sobre as tarefas do dia seguinte | n8n, Google Sheets, Evolution API |
| [Projeto Reserva Stays Calendario](projeto-reserva-stays-calendario) | Escuta os webhooks do PMS (Stays) e mantém a planilha de controle, o Google Calendar e as mensagens ao hóspede sincronizados | n8n, Google Sheets, Google Calendar, Evolution API |
| [Projeto Onboarding Hospede](projeto-onboarding-hospede) | Recebe o formulário de onboarding do hóspede, grava os dados, atualiza o calendário e avisa a portaria em check-ins no mesmo dia | n8n, Google Sheets, Google Calendar, Evolution API |

---

## Como usar

Cada pasta de projeto tem o workflow exportado (`.json`) e um README próprio. Para testar um:

1. No n8n, escolha **Import from File** e selecione o `.json` do projeto.
2. Configure suas credenciais nos nós indicados.
3. Troque os placeholders (`SUA-PLANILHA-ID-AQUI`, `seu-dominio-evolution.com` e parecidos) pelos seus valores.

---

## Estrutura

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

## Contato

Pedro Vignoli, estudante de Tecnologia em Inteligência Artificial. Interesse principal em IA, automação e dados.

- LinkedIn: [linkedin.com/in/pedro-vignoli-977999427](https://linkedin.com/in/pedro-vignoli-977999427)
- E-mail: pedro.code@proton.me
