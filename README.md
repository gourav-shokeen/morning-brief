# 🌅 MorningBrief

> A self-hosted n8n workflow that sends a personalized morning briefing to Telegram with weather, calendar events, Notion tasks, AI updates, tech news, and a configurable daily summary.

[Workflow screenshot placeholder](workflow-screenshot.png)

## What this project does

MorningBrief runs on a schedule every morning, collects information from multiple sources in parallel, formats everything into a Telegram-friendly briefing, and sends it to your chat.

It is designed as an open-source portfolio project:

- importable `workflow.json`
- self-hosted friendly
- minimal setup friction
- easy to customize from one config node
- documented enough for someone else to clone and run

## What's inside the briefing?

| Section | Source | Required setup |
|---------|--------|----------------|
| 🌤 Weather | OpenWeatherMap | API key |
| 📅 Events | Google Calendar iCal feed | Secret iCal URL |
| 📋 Tasks | Notion database | Integration token + database ID |
| 💻 News Highlights | Hacker News | None |
| 🤖 AI & ML | ArXiv + OpenAI RSS + optional extra feed | None for RSS |
| 🗓 Timetable | Built into the workflow | Optional edit |
| 💡 Quote | Built into the workflow | None |

## Quick Start

### Option 1: Use the included Docker Compose setup

```bash
git clone https://github.com/<your-username>/morning-brief-n8n.git
cd morning-brief-n8n
cp .env.example .env
docker compose up -d
```

Open `http://localhost:5678`.

### Option 2: Use your existing n8n instance

If you already run n8n locally, on a VPS, or through a service manager, you can skip Docker entirely and just import the workflow file.

## Setup

### 1. Import the workflow

In n8n:

1. Open `Workflows`
2. Click `Import from File`
3. Select [`workflow.public.json`](workflow.public.json)

### 2. Create credentials

Add these credentials in n8n:

- Telegram API
- Notion API

Setup guides:

- [Telegram setup](docs/setup-telegram.md)
- [Notion setup](docs/setup-notion.md)
- [Google Calendar iCal setup](docs/setup-gcal.md)

### 3. Configure node values

After import, open these nodes and set your own values:

- `📨 Send to Telegram`: select your Telegram credential and enter your chat ID
- `📋 Fetch Notion Tasks`: select your Notion credential and enter your database ID
- `🌤 Fetch Weather`: enter your OpenWeather API key if you are not using environment variables
- `📅 Fetch Calendar`: enter your private Google Calendar iCal URL if you are not using environment variables

### 4. Edit the config node

Open `⚙️ Config` and update:

- `firstName`
- `city`
- `timezone`
- `profile`
- each `enable*` toggle

### 5. Set your schedule

Open `⏰ Morning Schedule` and edit the cron expression.

Examples:

- `30 7 * * *` = 7:30 AM every day
- `0 8 * * *` = 8:00 AM every day
- `15 6 * * 1-5` = 6:15 AM on weekdays

### 6. Test and activate

Run the workflow once manually, confirm the message looks right, then toggle it to `Active`.

## Configuration Reference

The `⚙️ Config` node is the main control panel for the workflow.

| Field | Example | Description |
|------|---------|-------------|
| `profile` | `student` | Use `student` or `professional` to control profile-specific sections |
| `city` | `New Delhi` | City used by the weather request |
| `timezone` | `Asia/Kolkata` | Used for date formatting and scheduling context |
| `firstName` | `Gourav` | Personal greeting in the briefing |
| `enableNotion` | `true` | Include Notion tasks |
| `enableCalendar` | `true` | Include calendar events |
| `enableTimetable` | `true` | Include timetable block |
| `enableWeather` | `true` | Include weather block |
| `enableAINews` | `true` | Master switch for the AI section |
| `enableTechNews` | `true` | Include Hacker News items |
| `enableArxiv` | `true` | Include ArXiv research items |
| `enableOpenAI` | `true` | Include latest OpenAI RSS item |
| `enableAnthropic` | `true` | Reserved for an optional Anthropic or alternate AI source |

## Workflow Architecture

The workflow follows a simple fan-out / fan-in structure:

1. `⏰ Morning Schedule` triggers once per day
2. `⚙️ Config` provides the user-controlled settings
3. Data sources fetch in parallel
4. Each branch formats its own output block
5. `🔀 Merge All Sections` waits for all branches
6. `✍️ Assemble Briefing` builds the final Telegram message
7. `📨 Send to Telegram` sends one or more messages if the output is long

This keeps the workflow resilient. One failed source should not break the entire morning briefing.

## Running without Docker

This project does not require Docker. If you already run n8n another way, you can:

1. import [`workflow.json`](workflow.json)
2. set credentials in the n8n UI
3. place API keys or URLs directly in nodes
4. activate the workflow

For n8n Community Edition, direct node values or environment variables are usually simpler than relying on platform-level variables.

## Notes and Limitations

- Workflow exports should not include real credentials
- Telegram requires a valid numeric chat ID and an existing chat with your bot
- Notion databases must be shared with the integration
- Google Calendar uses the private iCal feed
- RSS feeds can change over time; if a source starts returning `404`, replace that feed or disable the section

## Project Structure

```text
morning-brief-n8n/
├── workflow.json
├── docker-compose.yml
├── .env.example
├── .gitignore
├── LICENSE
├── README.md
├── CONTRIBUTING.md
└── docs/
    ├── workflow-screenshot.md
    ├── setup-notion.md
    ├── setup-gcal.md
    ├── setup-telegram.md
    └── publish-checklist.md
```

## Roadmap

- [ ] Weekly summary edition
- [ ] Custom RSS feeds from config
- [ ] Telegram commands for reminders
- [ ] Discord output
- [ ] WhatsApp output
- [ ] Better AI source switching

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Fork it, adapt it, and run it however you want.
