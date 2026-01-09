---
visibility: public
title: n8n Personal Projects Guide
description: Creative n8n personal projects for developers comfortable with modern web stack
category: research
tags: [n8n, automation, projects, python, typescript, docker]
updated: 2026-01-09
status: complete
---

# Creative n8n Personal Projects Guide

**For Developers Comfortable With:** Python, FastAPI, TypeScript, Next.js, Docker, Redis, PostgreSQL

---

## Table of Contents

1. [Why n8n for Personal Projects](#why-n8n-for-personal-projects)
2. [Quick Setup](#quick-setup)
3. [Project Categories](#project-categories)
   - [AI-Powered Personal Assistants](#1-ai-powered-personal-assistants)
   - [Second Brain & Knowledge Management](#2-second-brain--knowledge-management)
   - [Smart Home & IoT](#3-smart-home--iot)
   - [Personal Finance & Crypto](#4-personal-finance--crypto)
   - [Content Creation Factory](#5-content-creation-factory)
   - [Developer Productivity](#6-developer-productivity)
   - [Voice-Enabled Automation](#7-voice-enabled-automation)
   - [MCP Integration (Claude Code)](#8-mcp-integration-with-claude-code)
4. [Advanced Patterns](#advanced-patterns)
5. [Architecture for Your Stack](#architecture-for-your-stack)

---

## Why n8n for Personal Projects

Given your stack (FastAPI, Next.js, Docker, Redis, PostgreSQL), n8n is ideal because:

1. **Self-Hosted & Free** - Run on your existing infrastructure
2. **Native PostgreSQL** - Uses the same DB you already know
3. **Docker-First** - Fits your containerized workflow
4. **Code When Needed** - JavaScript/Python code nodes for complex logic
5. **400+ Integrations** - Connect everything without building adapters
6. **AI-Native** - First-class Claude, GPT, Ollama support
7. **MCP Support** - Call n8n from Claude Code directly

---

## Quick Setup

### Docker Compose for Personal Use

```yaml
# docker-compose.personal-n8n.yml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n-personal
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=${N8N_PASSWORD}
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD}
      - EXECUTIONS_DATA_SAVE_ON_ERROR=all
      - EXECUTIONS_DATA_SAVE_ON_SUCCESS=all
      - N8N_PERSONALIZATION_ENABLED=false
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16-alpine
    container_name: n8n-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    container_name: n8n-redis
    restart: unless-stopped
    volumes:
      - redis_data:/data

  # Optional: Local AI with Ollama
  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    restart: unless-stopped
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]  # If you have GPU

  # Optional: Vector DB for RAG
  qdrant:
    image: qdrant/qdrant:latest
    container_name: qdrant
    restart: unless-stopped
    ports:
      - "6333:6333"
    volumes:
      - qdrant_data:/qdrant/storage

volumes:
  n8n_data:
  postgres_data:
  redis_data:
  ollama_data:
  qdrant_data:
```

```bash
# Start everything
docker compose -f docker-compose.personal-n8n.yml up -d

# Pull a local LLM
docker exec ollama ollama pull llama3.2
docker exec ollama ollama pull nomic-embed-text  # For embeddings
```

---

## Project Categories

### 1. AI-Powered Personal Assistants

#### Project: "Jackie" - Multi-Agent Personal Life Manager

**Inspiration:** [Personal Life Manager Template](https://n8n.io/workflows/8237-personal-life-manager-with-telegram-google-services-and-voice-enabled-ai/)

A centralized AI assistant that manages specialized sub-agents:

```
                    ┌─────────────────┐
                    │  Jackie (Main)  │
                    │   AI Orchestrator│
                    └────────┬────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
        ▼                    ▼                    ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│ Email Agent   │   │ Calendar Agent│   │ Task Agent    │
│ - Summarize   │   │ - Schedule    │   │ - Create/Done │
│ - Prioritize  │   │ - Conflicts   │   │ - Reminders   │
│ - Draft reply │   │ - Prep notes  │   │ - Dependencies│
└───────────────┘   └───────────────┘   └───────────────┘
```

**Workflow Structure:**
```yaml
trigger: Telegram message (text or voice)
nodes:
  - Whisper: Transcribe voice (if audio)
  - AI Agent: Classify intent
  - Switch: Route to specialist
    - /email → Email Agent (Gmail API + Claude)
    - /calendar → Calendar Agent (Google Calendar)
    - /task → Task Agent (Todoist/Notion)
    - /search → Knowledge Agent (RAG)
  - Merge: Combine responses
  - Telegram: Send reply (with TTS option)
```

**Code Node Example - Intent Classification:**
```javascript
// Classify user intent for routing
const message = $input.first().json.text.toLowerCase();

const intents = {
  email: ['email', 'inbox', 'unread', 'mail', 'reply', 'send'],
  calendar: ['meeting', 'schedule', 'calendar', 'event', 'tomorrow', 'today'],
  task: ['task', 'todo', 'remind', 'deadline', 'done', 'complete'],
  search: ['find', 'search', 'what is', 'how to', 'lookup']
};

let detected = 'general';
for (const [intent, keywords] of Object.entries(intents)) {
  if (keywords.some(kw => message.includes(kw))) {
    detected = intent;
    break;
  }
}

return [{ json: { intent: detected, originalMessage: message } }];
```

**Your Stack Integration:**
- Store conversation history in PostgreSQL
- Cache frequent queries in Redis
- Build a FastAPI endpoint to extend capabilities
- Create a Next.js dashboard for analytics

---

#### Project: Daily Briefing Generator

**Every morning at 7 AM:**
```
┌─────────────────────────────────────────────────────────────┐
│ Schedule Trigger (7 AM)                                      │
└────────────────────────────────────────────────────────────┬┘
                                                             │
    ┌────────────────────────────────────────────────────────┼┐
    │ Parallel Fetch                                          ││
    ├──────────────────────────────────────────────────────────┤
    │ ├─→ Gmail: Unread count + top 5 subjects                ││
    │ ├─→ Calendar: Today's events                            ││
    │ ├─→ Weather API: Forecast                               ││
    │ ├─→ News API: Top headlines                             ││
    │ ├─→ GitHub: PR reviews needed                           ││
    │ ├─→ Todoist: Due today                                  ││
    │ └─→ Finance API: Portfolio change                       ││
    └────────────────────────────────────────────────────────┬┘
                                                             │
    ┌────────────────────────────────────────────────────────┴┐
    │ Claude: Generate personalized briefing                   │
    │ "You have 3 meetings, 12 emails, partly cloudy..."      │
    └────────────────────────────────────────────────────────┬┘
                                                             │
    ┌────────────────────────────────────────────────────────┴┐
    │ Output: Telegram + Email + Audio (ElevenLabs TTS)       │
    └─────────────────────────────────────────────────────────┘
```

---

### 2. Second Brain & Knowledge Management

#### Project: Personal RAG Knowledge Base

**Architecture:**
```
┌──────────────────────────────────────────────────────────────┐
│                    Knowledge Ingestion Pipeline               │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Sources:                      Processing:        Storage:   │
│  ├─ Obsidian Notes        →   ├─ Chunk          → Qdrant    │
│  ├─ Notion Databases      →   ├─ Embed          → (vectors) │
│  ├─ Browser Bookmarks     →   ├─ Enrich         →           │
│  ├─ Saved Articles        →   └─ Index          → Postgres  │
│  ├─ YouTube Transcripts   →                       (metadata)│
│  └─ PDF Documents         →                                  │
│                                                              │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                    Query Interface                            │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Telegram/Slack: "What did I read about RAG last month?"    │
│                           │                                   │
│                           ▼                                   │
│                    ┌─────────────┐                           │
│                    │ Embed Query │                           │
│                    └──────┬──────┘                           │
│                           │                                   │
│                           ▼                                   │
│                    ┌─────────────┐                           │
│                    │ Vector Search│                          │
│                    │ (Qdrant)    │                           │
│                    └──────┬──────┘                           │
│                           │                                   │
│                           ▼                                   │
│                    ┌─────────────┐                           │
│                    │ LLM + Context│                          │
│                    │ (Claude/Llama)│                         │
│                    └──────┬──────┘                           │
│                           │                                   │
│                           ▼                                   │
│              "Based on your notes from Nov 15..."            │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

**Obsidian Integration via Webhook:**
```javascript
// n8n Code Node: Process Obsidian note
const note = $input.first().json;

// Extract frontmatter and content
const frontmatterMatch = note.content.match(/^---\n([\s\S]*?)\n---/);
const frontmatter = frontmatterMatch ?
  Object.fromEntries(
    frontmatterMatch[1].split('\n').map(line => {
      const [key, ...value] = line.split(':');
      return [key.trim(), value.join(':').trim()];
    })
  ) : {};

const content = note.content.replace(/^---\n[\s\S]*?\n---\n/, '');

// Chunk for embedding
const chunks = [];
const paragraphs = content.split('\n\n');
let currentChunk = '';

for (const para of paragraphs) {
  if ((currentChunk + para).length > 500) {
    if (currentChunk) chunks.push(currentChunk.trim());
    currentChunk = para;
  } else {
    currentChunk += '\n\n' + para;
  }
}
if (currentChunk) chunks.push(currentChunk.trim());

return chunks.map((chunk, i) => ({
  json: {
    id: `${note.path}-chunk-${i}`,
    content: chunk,
    metadata: {
      ...frontmatter,
      source: 'obsidian',
      path: note.path,
      chunk_index: i
    }
  }
}));
```

**Build with Your Stack:**
- FastAPI endpoint for n8n to call for custom processing
- PostgreSQL for metadata and search history
- Redis for caching frequent queries
- Next.js dashboard showing knowledge graph

---

#### Project: Read-It-Later AI Summarizer

```
Webhook: Receive URL from iOS Shortcut / Browser Extension
    │
    ├─→ Fetch article content (Jina Reader API or Puppeteer)
    │
    ├─→ Claude: Generate summary + key points + tags
    │
    ├─→ Store in Notion/Obsidian with metadata
    │
    ├─→ Add to RAG knowledge base
    │
    └─→ Send summary to Telegram/Slack
```

---

### 3. Smart Home & IoT

#### Project: AI-Powered Home Assistant

**Inspiration:** [Home Assistant Voice Control](https://n8n.io/workflows/8411-voice-and-text-control-for-home-assistant-using-telegram-whisper-and-gemini/)

```
┌─────────────────────────────────────────────────────────────┐
│                    Voice Command Flow                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  "Hey n8n, turn off all lights and set thermostat to 20"   │
│                           │                                  │
│                           ▼                                  │
│                    ┌─────────────┐                          │
│                    │   Whisper   │                          │
│                    │   (STT)     │                          │
│                    └──────┬──────┘                          │
│                           │                                  │
│                           ▼                                  │
│                    ┌─────────────┐                          │
│                    │   Claude    │                          │
│                    │ Parse intent│                          │
│                    └──────┬──────┘                          │
│                           │                                  │
│                           ▼                                  │
│            ┌──────────────┴──────────────┐                  │
│            │                             │                   │
│            ▼                             ▼                   │
│    ┌─────────────┐              ┌─────────────┐             │
│    │ Light Off   │              │ Thermostat  │             │
│    │ (HA API)    │              │ Set 20°C    │             │
│    └─────────────┘              └─────────────┘             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Code Node - Home Assistant Command Parser:**
```javascript
// Parse natural language to Home Assistant commands
const input = $input.first().json.transcript;

const prompt = `Parse this home automation command into JSON:
"${input}"

Return format:
{
  "actions": [
    {"domain": "light|switch|climate|cover|scene", "service": "turn_on|turn_off|set_temperature", "entity_id": "...", "data": {...}}
  ]
}

Examples:
- "turn off living room lights" → {"actions":[{"domain":"light","service":"turn_off","entity_id":"light.living_room"}]}
- "set bedroom to 22 degrees" → {"actions":[{"domain":"climate","service":"set_temperature","entity_id":"climate.bedroom","data":{"temperature":22}}]}
`;

// Send to Claude and parse response
return [{ json: { prompt, original: input } }];
```

**Raspberry Pi Integration:**
- Run n8n on Pi 4 with 4GB RAM
- Connect sensors via MQTT
- Motion detection → Telegram alerts
- Temperature monitoring → automatic HVAC adjustments

---

### 4. Personal Finance & Crypto

#### Project: Automated Expense Tracker

**Inspiration:** [Finance Automation Article](https://www.xda-developers.com/i-use-n8n-every-day-to-manage-finances-it-has-massively-improved-my-life/)

```
┌─────────────────────────────────────────────────────────────┐
│                    Expense Tracking Pipeline                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Gmail Trigger: New email from bank                         │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ Claude: Extract transaction details   │                  │
│  │ - Amount                              │                  │
│  │ - Merchant                            │                  │
│  │ - Category (auto-classify)            │                  │
│  │ - Date                                │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ Google Sheets: Append row             │                  │
│  │ Monthly budget tracker                │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ Budget Check: Over threshold?         │                  │
│  │ - Dining > $500? Alert                │                  │
│  │ - Subscriptions > $100? Review        │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  Telegram: "Spent $45 at Restaurant - 80% of dining budget" │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Project: Crypto Portfolio Monitor

```yaml
trigger: Schedule every hour
nodes:
  - HTTP: Fetch portfolio from CoinGecko/Binance API
  - Code: Calculate metrics
    - Total value
    - 24h change
    - Volatility score
    - BTC correlation
  - IF: Significant change (>5% or crossing threshold)?
    - TRUE:
      - Slack alert with chart
      - Optional: Execute trade via Binance API
    - FALSE: Log and continue
  - Postgres: Store historical data
  - Weekly: Generate PDF report with charts
```

**Code Node - Crypto Signal Generator:**
```javascript
const coins = $input.first().json.coins;

const signals = coins.map(coin => {
  const volatility = Math.abs(coin.price_change_24h / coin.current_price);
  const momentum = coin.price_change_percentage_7d > 0 ? 'bullish' : 'bearish';

  let signal = 'HOLD';
  if (volatility > 0.1 && momentum === 'bullish') signal = 'BUY';
  if (volatility > 0.15 && momentum === 'bearish') signal = 'SELL';
  if (volatility > 0.2) signal = 'HIGH_RISK';

  return {
    symbol: coin.symbol.toUpperCase(),
    price: coin.current_price,
    change_24h: coin.price_change_percentage_24h.toFixed(2) + '%',
    volatility: (volatility * 100).toFixed(1) + '%',
    signal,
    reasoning: `${momentum} momentum, ${volatility > 0.1 ? 'high' : 'normal'} volatility`
  };
});

return [{ json: { signals, generated_at: new Date().toISOString() } }];
```

---

### 5. Content Creation Factory

#### Project: YouTube Automation Pipeline

**Inspiration:** [YouTube Automation Template](https://n8n.io/workflows/3900-automated-youtube-video-scheduling-and-ai-metadata-generation/)

```
┌─────────────────────────────────────────────────────────────┐
│              Video-to-Multi-Platform Pipeline                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Trigger: New video uploaded to Google Drive                │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ Whisper: Transcribe video             │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ Claude: Generate metadata             │                  │
│  │ - SEO title (3 variations)            │                  │
│  │ - Description (keywords)              │                  │
│  │ - Chapters (from transcript)          │                  │
│  │ - Hashtags                            │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ├─→ YouTube: Upload + schedule (private → public)    │
│        │                                                     │
│        ├─→ Opus Clip API: Generate shorts                   │
│        │        │                                            │
│        │        ├─→ TikTok: Upload                          │
│        │        ├─→ Instagram Reels: Upload                 │
│        │        └─→ YouTube Shorts: Upload                  │
│        │                                                     │
│        ├─→ Claude: Generate blog post from transcript       │
│        │        └─→ Ghost/WordPress: Publish                │
│        │                                                     │
│        └─→ Claude: Generate social posts                    │
│                 ├─→ Twitter thread                          │
│                 └─→ LinkedIn post                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Project: Blog-to-Social Amplifier

```yaml
trigger: RSS feed (your blog) new item
nodes:
  - Fetch full article content
  - Claude: Generate platform-specific content
    - Twitter: Thread (5-7 tweets)
    - LinkedIn: Professional take
    - Reddit: Discussion starter
    - HackerNews: Technical summary
  - Puppeteer: Generate OG image with article title
  - Parallel post to all platforms
  - Store engagement metrics for analysis
```

---

### 6. Developer Productivity

#### Project: GitHub Automation Suite

```
┌─────────────────────────────────────────────────────────────┐
│              Developer Productivity Workflows                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. PR Review Assistant                                      │
│     Trigger: New PR opened                                   │
│     → Fetch diff                                             │
│     → Claude: Review for security, performance, style        │
│     → Post comment with suggestions                          │
│                                                             │
│  2. Issue Triage Bot                                         │
│     Trigger: New issue created                               │
│     → Claude: Classify (bug/feature/question)                │
│     → Add labels automatically                               │
│     → Assign to appropriate team member                      │
│                                                             │
│  3. Release Notes Generator                                  │
│     Trigger: Tag pushed                                      │
│     → Fetch commits since last tag                           │
│     → Claude: Generate changelog                             │
│     → Create GitHub release                                  │
│     → Post to Slack/Discord                                  │
│                                                             │
│  4. Dependency Update Digest                                 │
│     Trigger: Weekly schedule                                 │
│     → Check for outdated dependencies                        │
│     → Claude: Summarize breaking changes                     │
│     → Create consolidated PR                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Project: Codebase Q&A Bot

Use RAG to make your codebase queryable:

```
1. Index Phase (on push to main):
   - Clone/pull repo
   - Parse all files (AST for code, markdown for docs)
   - Chunk and embed
   - Store in Qdrant with file paths as metadata

2. Query Phase (Telegram/Slack):
   User: "How does authentication work in this project?"

   → Embed query
   → Vector search for relevant chunks
   → Claude with context:
     "Based on these code snippets... Authentication uses JWT tokens
      implemented in src/auth/middleware.py:45..."
```

---

### 7. Voice-Enabled Automation

#### Project: Voice-to-Task System

**Inspiration:** [Voice to Shopping List](https://medium.com/@arazuberez/from-voice-message-to-food-shopping-list-an-ai-powered-automation-using-n8n-whisper-and-todoist-f359f6ed3f93)

```
Telegram Voice Message
    │
    ├─→ Download audio file
    │
    ├─→ Whisper: Transcribe
    │
    ├─→ Claude: Parse into structured task
    │   Input: "Remind me to call mom tomorrow at 3pm
    │           and also pick up groceries on the way home"
    │   Output: [
    │     {type: "reminder", text: "Call mom", datetime: "tomorrow 15:00"},
    │     {type: "task", text: "Pick up groceries", context: "commute"}
    │   ]
    │
    ├─→ For each task:
    │   ├─→ Reminder → Google Calendar
    │   └─→ Task → Todoist with geo-fence
    │
    └─→ Telegram: "Created 2 items: Call mom (tomorrow 3pm),
                   Groceries (when near store)"
```

#### Project: Meeting Notes Automator

```
Trigger: Calendar event ends
    │
    ├─→ Fetch recording from Zoom/Meet
    │
    ├─→ Whisper: Transcribe full meeting
    │
    ├─→ Claude: Generate structured notes
    │   - Summary (3-5 sentences)
    │   - Key decisions
    │   - Action items with owners
    │   - Follow-up questions
    │
    ├─→ Notion: Create meeting notes page
    │
    ├─→ For each action item:
    │   └─→ Create task in Asana/Linear with deadline
    │
    └─→ Email: Send notes to all attendees
```

---

### 8. MCP Integration with Claude Code

**This is cutting-edge!** Connect n8n directly to Claude Code/Desktop.

**Inspiration:** [n8n MCP Server](https://github.com/czlonkowski/n8n-mcp) | [Native MCP Docs](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.mcptrigger/)

#### How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                    MCP Architecture                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Claude Code / Claude Desktop                                │
│        │                                                     │
│        │ "Hey Claude, run my morning routine workflow"       │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ MCP Client Tool (in Claude)           │                  │
│  │ Connects to n8n MCP Server            │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  ┌───────────────────────────────────────┐                  │
│  │ n8n MCP Server Trigger                │                  │
│  │ (Workflow with tools exposed)         │                  │
│  └───────────────────────────────────────┘                  │
│        │                                                     │
│        ▼                                                     │
│  Your n8n Workflows execute and return results               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Setup in Claude Desktop

```json
// ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "n8n": {
      "command": "npx",
      "args": ["-y", "mcp-n8n-server"],
      "env": {
        "N8N_API_URL": "http://localhost:5678/api/v1",
        "N8N_API_KEY": "your-api-key"
      }
    }
  }
}
```

#### Example: Ask Claude to Trigger n8n

```
You: "Claude, run my weekly report workflow"

Claude: I'll trigger that workflow for you.
        [Calls n8n MCP tool: trigger_workflow("weekly-report")]

        ✓ Workflow executed successfully
        Here's the summary from the report:
        - 15 tasks completed
        - 3 meetings attended
        - 2 blog posts published
```

---

## Advanced Patterns

### 1. Multi-Agent Architecture

Build specialized agents that collaborate:

```
┌─────────────────────────────────────────────────────────────┐
│                    Agent Team Structure                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Orchestrator Agent (Main)                                   │
│  └─ Receives user request                                    │
│  └─ Delegates to specialists                                 │
│  └─ Synthesizes final response                              │
│        │                                                     │
│        ├─→ Research Agent                                    │
│        │   - Web search                                      │
│        │   - Knowledge base lookup                           │
│        │   - Returns facts + sources                         │
│        │                                                     │
│        ├─→ Writing Agent                                     │
│        │   - Drafts content                                  │
│        │   - Edits and refines                              │
│        │   - Maintains style consistency                     │
│        │                                                     │
│        ├─→ Code Agent                                        │
│        │   - Writes/reviews code                             │
│        │   - Runs tests                                      │
│        │   - Explains technical concepts                     │
│        │                                                     │
│        └─→ Action Agent                                      │
│            - Executes external actions                       │
│            - Sends emails/messages                           │
│            - Creates calendar events                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2. State Machine Workflows

For complex multi-step processes:

```javascript
// n8n Code Node: State Machine
const state = $input.first().json;

const transitions = {
  'draft': { approve: 'review', cancel: 'cancelled' },
  'review': { approve: 'published', reject: 'draft', escalate: 'manager_review' },
  'manager_review': { approve: 'published', reject: 'cancelled' },
  'published': { archive: 'archived', unpublish: 'draft' },
  'cancelled': { reopen: 'draft' }
};

const currentState = state.status;
const action = state.action;

if (transitions[currentState]?.[action]) {
  return [{
    json: {
      ...state,
      previousStatus: currentState,
      status: transitions[currentState][action],
      transitionedAt: new Date().toISOString()
    }
  }];
} else {
  throw new Error(`Invalid transition: ${action} from ${currentState}`);
}
```

### 3. Event Sourcing with Redis Streams

Store all events for replay/audit:

```
User Action → n8n Webhook
    │
    ├─→ Redis XADD: Store event
    │   Stream: user_events
    │   Data: {type, payload, timestamp}
    │
    ├─→ Process event (side effects)
    │
    └─→ Respond to user

Recovery:
    XREAD from stream → Replay all events → Rebuild state
```

---

## Architecture for Your Stack

Given your expertise, here's how to integrate n8n with your tools:

### FastAPI + n8n Integration

```python
# Your FastAPI endpoint that n8n calls
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel
import httpx

app = FastAPI()

class N8NWebhookPayload(BaseModel):
    workflow_id: str
    data: dict
    callback_url: str  # n8n will receive response here

@app.post("/api/process")
async def process_for_n8n(
    payload: N8NWebhookPayload,
    background_tasks: BackgroundTasks
):
    # Start long-running task
    background_tasks.add_task(
        process_and_callback,
        payload.data,
        payload.callback_url
    )
    return {"status": "processing", "workflow_id": payload.workflow_id}

async def process_and_callback(data: dict, callback_url: str):
    # Your complex processing
    result = await heavy_computation(data)

    # Callback to n8n to continue workflow
    async with httpx.AsyncClient() as client:
        await client.post(callback_url, json=result)
```

### Next.js + n8n Dashboard

```typescript
// pages/api/n8n/workflows.ts
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const n8nApi = process.env.N8N_API_URL!
  const apiKey = process.env.N8N_API_KEY!

  const response = await fetch(`${n8nApi}/workflows`, {
    headers: { 'X-N8N-API-KEY': apiKey }
  })

  const workflows = await response.json()

  // Transform for dashboard
  const summary = workflows.data.map((w: any) => ({
    id: w.id,
    name: w.name,
    active: w.active,
    lastExecution: w.updatedAt,
    tags: w.tags
  }))

  res.status(200).json(summary)
}
```

### Redis for n8n State

```python
# Share state between FastAPI and n8n via Redis
import redis

r = redis.Redis(host='localhost', port=6379, db=0)

# FastAPI writes state
r.hset('n8n:user:123', mapping={
    'last_action': 'login',
    'session_count': 5,
    'preferences': json.dumps({'theme': 'dark'})
})

# n8n reads via Redis node
# Key: n8n:user:{{$json.user_id}}
```

---

## Getting Started Checklist

1. [ ] Deploy n8n with Docker Compose above
2. [ ] Pull Ollama model for local AI (`llama3.2`)
3. [ ] Set up Telegram bot (talk to @BotFather)
4. [ ] Create first workflow: Daily Briefing
5. [ ] Connect to your existing PostgreSQL
6. [ ] Build RAG pipeline with Qdrant
7. [ ] Integrate with Claude Code via MCP

---

## Resources

- [n8n Personal Productivity Templates](https://n8n.io/workflows/categories/personal-productivity/) - 506 templates
- [n8n AI Workflow Templates](https://n8n.io/workflows/categories/ai/) - 4,729 templates
- [Self-Hosted AI Starter Kit](https://github.com/n8n-io/self-hosted-ai-starter-kit)
- [n8n MCP Integration](https://github.com/czlonkowski/n8n-mcp)
- [Home Assistant + n8n](https://n8n.io/integrations/home-assistant/)
- [Building RAG Chatbots](https://blog.n8n.io/rag-chatbot/)
- [Local LLM Guide](https://blog.n8n.io/local-llm/)
- [AI Agentic Workflows](https://blog.n8n.io/ai-agentic-workflows/)

---

**The possibilities are endless. Start small, iterate fast, and build the personal automation system you've always wanted!**
