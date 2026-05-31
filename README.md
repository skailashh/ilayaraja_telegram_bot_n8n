# 🎵 Ilayaraja Songs Bot

A Telegram bot that answers natural language questions about Ilayaraja's Tamil film songs — powered by n8n, Google Sheets, and Groq LLM.

---

## What it does

Send a message to the Telegram bot like:

- *"Songs sung by S. Janaki in the 1980s"*
- *"Solo songs by SPB"*
- *"Top singers by number of songs"*
- *"Songs from the movie Ninaithale Inikkum"*

The bot understands the question, queries your song data, and replies with a concise answer — all within Telegram.

---

## How it works

```
Telegram message
      ↓
  n8n Trigger
      ↓
Google Sheets (fetch song data)
      ↓
JavaScript (parse + filter data based on query)
      ↓
Groq LLM — llama-3.3-70b-versatile (generate natural language answer)
      ↓
Telegram reply
```

### Nodes in the workflow

| Node | Purpose |
|------|---------|
| **Telegram Trigger** | Listens for incoming messages |
| **Get Songs** | Fetches song data from Google Sheets via API |
| **Code in JavaScript** | Parses the sheet data, filters rows based on singer/actor/year/keywords, builds a prompt |
| **HTTP Request (Groq)** | Sends the prompt to Groq's LLM API |
| **Send a text message** | Returns the LLM's answer back to the user on Telegram |

---

## Google Sheets structure

The sheet (`Ilayaraja Songs`) should have these columns in order:

| Year | Movie | Songs | Singer | Lyricist | Director | Actor |
|------|-------|-------|--------|----------|----------|-------|

- Multiple singers in a row should be comma-separated (e.g. `S. P. Balasubrahmanyam, S. Janaki`)

---

## Setup

### Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- Telegram Bot token (from [@BotFather](https://t.me/BotFather))
- Google Sheets API key (with the sheet set to public/read access)
- [Groq API key](https://console.groq.com/) (free tier works)

### Steps

1. **Import the workflow** — In n8n, go to *Workflows → Import* and upload `workflow/n8n_workflow.json`

2. **Configure credentials**
   - Add your Telegram Bot token under *Credentials → Telegram API*
   - Add your Groq API key under *Credentials → Groq API*

3. **Update the Google Sheets URL** — In the *Get Songs* node, replace the URL with your own sheet's endpoint:
   ```
   https://sheets.googleapis.com/v4/spreadsheets/YOUR_SHEET_ID/values/Sheet1!A:G?key=YOUR_API_KEY
   ```

4. **Activate the workflow** and start chatting with your bot on Telegram

### Hosting

This workflow was hosted on [Railway](https://railway.app/) using a self-hosted n8n Docker container. Any platform that supports n8n (Railway, Render, Fly.io, a VPS) will work.

---

## Example queries the bot understands

| Query type | Example |
|-----------|---------|
| By singer | `Songs by K. J. Yesudas` |
| By singer + decade | `S. Janaki songs in the 1990s` |
| Solo songs | `Solo songs by SPB` |
| By actor | `Songs featuring Rajinikanth` |
| Top singers | `Top singers by number of songs` |
| Top years | `Which year had the most songs` |
| By year range | `Songs from 1980 to 1985` |

---

## Tech stack

- **n8n** — workflow automation
- **Google Sheets API** — song database
- **Groq API** — LLM inference (llama-3.3-70b-versatile)
- **Telegram Bot API** — chat interface
- **Railway** — hosting

---
## Screenshots
n8n Workflow
<img width="1912" height="944" alt="image" src="https://github.com/user-attachments/assets/abc0c651-6c87-4d6e-8d8e-b5e221e2406f" />

Telegram Bot in action
<img width="370" height="728" alt="image" src="https://github.com/user-attachments/assets/a5e308b4-57ea-4d00-8269-c3dd06628a42" />
<img width="368" height="726" alt="image" src="https://github.com/user-attachments/assets/252b77cd-d9d8-4a43-b1f9-5ba8896bd653" />


## License

MIT
