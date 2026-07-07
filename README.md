# News Digest Bot

A Telegram bot that collects news from selected channels once a day, summarizes them using the Gemini API, and sends subscribers a concise digest as a .docx file.

## How it works

- A Telethon user session reads public channels listed in `CHANNELS`
- The Gemini API summarizes the collected text into a short evening digest
- The resulting .docx file is sent to everyone who has messaged the bot `/start`

## Setup

1. Copy `.env.example` to `.env` and fill in your own values:
   ```bash
   cp .env.example .env
   ```
   - `API_ID` / `API_HASH` — get these at https://my.telegram.org
   - `BOT_TOKEN` — get this from @BotFather
   - `GEMINI_KEY` — your Gemini API key
   - `OWNER_CHAT_ID` — your personal Telegram chat_id

2. Build and run the container:
   ```bash
   docker build -t news-summarizer .
   docker run -d --name news-bot --restart unless-stopped \
     --env-file .env \
     -e TZ=Europe/Berlin \
     -e PYTHONUNBUFFERED=1 \
     -v $(pwd):/app \
     news-summarizer
   ```

3. On first run, the user session (Telethon) will interactively ask for your phone number. After that, the session is saved to `news_parser_session.session` and you won't be asked again.

## Important

The `.env` file, session files (`*.session`), and `subscribers.json` contain private data and are intentionally excluded from the repository via `.gitignore`. Never commit them manually.
