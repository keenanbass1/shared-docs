---
visibility: public
title: Open Source SaaS Alternatives
description: Ultimate guide to replacing commercial SaaS with self-hosted open source alternatives
category: research
tags: [open-source, self-hosted, saas, alternatives, infrastructure]
updated: 2026-01-09
status: complete
---

# Comprehensive Open Source Self-Hosted SaaS Alternatives

**The Ultimate Guide to Replacing Commercial SaaS with Self-Hosted Open Source**

---

## Table of Contents

1. [Why Self-Host?](#why-self-host)
2. [Marketing & Email](#marketing--email)
3. [CRM & Sales](#crm--sales)
4. [Analytics & Tracking](#analytics--tracking)
5. [Project Management](#project-management)
6. [Customer Support](#customer-support)
7. [Communication & Collaboration](#communication--collaboration)
8. [Documentation & Knowledge Base](#documentation--knowledge-base)
9. [Scheduling & Booking](#scheduling--booking)
10. [Invoicing & Billing](#invoicing--billing)
11. [Media & Photos](#media--photos)
12. [Forms & Surveys](#forms--surveys)
13. [Security & Passwords](#security--passwords)
14. [Monitoring & DevOps](#monitoring--devops)
15. [Link Management](#link-management)
16. [Cloud Storage](#cloud-storage)
17. [Additional Tools](#additional-tools)
18. [Integration Matrix](#integration-matrix)
19. [Recommended Personal Stack](#recommended-personal-stack)

---

## Why Self-Host?

According to a 2024 survey by The Linux Foundation, **82% of enterprises** now integrate open-source tools into their IT stack.

| Benefit | Description |
|---------|-------------|
| **Data Sovereignty** | Your data stays on your servers, not third-party clouds |
| **Cost Savings** | One-time hardware vs recurring subscriptions |
| **Privacy & Compliance** | GDPR, CCPA compliance without trusting vendors |
| **Customization** | Modify source code to fit your exact needs |
| **No Vendor Lock-in** | Switch or modify anytime |
| **Unlimited Usage** | No per-seat pricing or usage caps |

---

## Marketing & Email

### Salesforce Marketing Cloud Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Mautic](https://mautic.org)** | Full marketing automation | 8.8K+ | GPL-3.0 |
| **[Listmonk](https://listmonk.app)** | High-volume newsletters | 18K+ | AGPL-3.0 |
| **[Erxes](https://erxes.io)** | All-in-one growth platform | 3.5K+ | MIT |

### Mautic (Full Marketing Automation)

**Replaces:** Salesforce Marketing Cloud, HubSpot, Marketo, ActiveCampaign

**Features:**
- Multi-channel campaigns (email, SMS, social, web)
- Lead scoring and nurturing
- Landing page builder
- A/B testing
- CRM integration
- Marketing analytics
- Dynamic content personalization

```yaml
# docker-compose.mautic.yml
services:
  mautic:
    image: mautic/mautic:latest
    ports:
      - "8080:80"
    environment:
      - MAUTIC_DB_HOST=mysql
      - MAUTIC_DB_USER=mautic
      - MAUTIC_DB_PASSWORD=${DB_PASSWORD}
      - MAUTIC_DB_NAME=mautic
    volumes:
      - mautic_data:/var/www/html
    depends_on:
      - mysql

  mysql:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=mautic
      - MYSQL_USER=mautic
      - MYSQL_PASSWORD=${DB_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
```

### Listmonk (High-Performance Newsletters)

**Replaces:** Mailchimp, ConvertKit, Substack (email portion)

**Features:**
- Blazing fast (Go backend, handles millions of subscribers)
- Multi-list management
- Template editor
- Campaign analytics
- Subscriber import/export
- API-first design
- Tiny footprint (~15MB binary)

**Best for:** Developers, high-volume senders, privacy-focused newsletters

```yaml
# docker-compose.listmonk.yml
services:
  listmonk:
    image: listmonk/listmonk:latest
    ports:
      - "9000:9000"
    environment:
      - TZ=Australia/Sydney
    volumes:
      - ./config.toml:/listmonk/config.toml
    depends_on:
      - postgres

  postgres:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=listmonk
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=listmonk
    volumes:
      - listmonk_db:/var/lib/postgresql/data
```

### Comparison: Mautic vs Listmonk

| Feature | Mautic | Listmonk |
|---------|--------|----------|
| Email campaigns | Yes | Yes |
| Marketing automation | Yes | No |
| Lead scoring | Yes | No |
| Landing pages | Yes | No |
| SMS marketing | Yes | No |
| Performance | Heavy | Ultra-fast |
| Resources needed | High | Low |
| Best for | Full marketing suite | Simple newsletters |

---

## CRM & Sales

### Salesforce Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Twenty](https://twenty.com)** | Modern CRM, Salesforce replacement | 23K+ | AGPL-3.0 |
| **[Krayin](https://krayincrm.com)** | Laravel-based CRM | 11K+ | MIT |
| **[SuiteCRM](https://suitecrm.com)** | Enterprise-grade CRM | 4K+ | AGPL-3.0 |
| **[EspoCRM](https://espocrm.com)** | Lightweight CRM | 1.8K+ | GPL-3.0 |
| **[Erxes](https://erxes.io)** | CRM + Marketing + Support | 3.5K+ | MIT |

### Twenty CRM

**Replaces:** Salesforce, Pipedrive, HubSpot CRM

**Features:**
- Modern UI (looks like a 2024 app, not 2005)
- People, Companies, Opportunities, Tasks
- Email sync and timeline
- REST + GraphQL APIs
- Webhooks for automation
- Custom fields and views (Kanban, Table)
- Workflow automation

**Why Twenty:**
- GPL-licensed (you own it)
- Built with modern stack (React, Node, PostgreSQL)
- Active development (23K+ GitHub stars)
- Zapier integration

### Erxes (All-in-One)

**Replaces:** Salesforce + Zendesk + Mailchimp

**Features:**
- CRM with pipeline management
- Marketing automation
- Customer support inbox
- Forms and popups
- Knowledge base
- Task management

**Best for:** Teams wanting one platform for everything

---

## Analytics & Tracking

### Google Analytics Alternatives

| Tool | Best For | Script Size | License |
|------|----------|-------------|---------|
| **[Umami](https://umami.is)** | Simple, fast, free tier | ~1KB | MIT |
| **[Plausible](https://plausible.io)** | Privacy-first, beautiful UI | ~1KB | AGPL-3.0 |
| **[Matomo](https://matomo.org)** | Feature-rich, enterprise | ~23KB | GPL-3.0 |
| **[PostHog](https://posthog.com)** | Product analytics + session replay | Varies | MIT |

### Comparison

| Feature | Umami | Plausible | Matomo | PostHog |
|---------|-------|-----------|--------|---------|
| Privacy-focused | Yes | Yes | Configurable | Yes |
| Script size | ~1KB | ~1KB | ~23KB | ~45KB |
| Session recording | No | No | Plugin | Yes |
| Heatmaps | No | No | Plugin | Yes |
| Self-hosted cost | Free | Free | Free | Free tier |
| Best for | Lightweight | Beautiful simplicity | Enterprise features | Product teams |

### Umami (Recommended for Personal Use)

```yaml
# docker-compose.umami.yml
services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgresql://umami:${DB_PASSWORD}@postgres:5432/umami
      DATABASE_TYPE: postgresql
      HASH_SALT: ${HASH_SALT}
    depends_on:
      - postgres

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: umami
      POSTGRES_USER: umami
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - umami_db:/var/lib/postgresql/data
```

---

## Project Management

### Jira/Linear/Asana Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Plane](https://plane.so)** | Linear/Jira alternative | 41K+ | AGPL-3.0 |
| **[OpenProject](https://openproject.org)** | Enterprise project management | 10K+ | GPL-3.0 |
| **[Taiga](https://taiga.io)** | Agile teams | 13K+ | MPL-2.0 |
| **[Focalboard](https://focalboard.com)** | Notion/Trello alternative | 22K+ | Various |

### Plane

**Replaces:** Jira, Linear, Monday, Asana

**Features:**
- Issues with rich text, file uploads, sub-properties
- Cycles (sprints) with burn-down charts
- Modules for complex projects
- Custom views and filters
- AI-powered Pages
- API-first design

```yaml
# docker-compose.plane.yml
services:
  plane-app:
    image: makeplane/plane-frontend:latest
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
    depends_on:
      - plane-api

  plane-api:
    image: makeplane/plane-backend:latest
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://plane:${DB_PASSWORD}@postgres:5432/plane
      - REDIS_URL=redis://redis:6379
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: plane
      POSTGRES_USER: plane
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - plane_db:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
```

---

## Customer Support

### Zendesk/Intercom Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Chatwoot](https://chatwoot.com)** | Omnichannel support | 22K+ | MIT |
| **[Zammad](https://zammad.com)** | Helpdesk & ticketing | 4.5K+ | AGPL-3.0 |
| **[FreeScout](https://freescout.net)** | HelpScout clone | 3K+ | AGPL-3.0 |
| **[Peppermint](https://peppermint.sh)** | Lightweight ticketing | 1.5K+ | AGPL-3.0 |

### Chatwoot

**Replaces:** Intercom, Zendesk, Freshdesk, Drift

**Features:**
- Omnichannel: website chat, email, WhatsApp, Facebook, Twitter, Instagram
- AI-powered Captain agent
- Canned responses and macros
- Team inbox with assignments
- Customer portal
- Webhooks and API
- 100+ integrations

```yaml
# docker-compose.chatwoot.yml
services:
  chatwoot:
    image: chatwoot/chatwoot:latest
    ports:
      - "3000:3000"
    environment:
      - SECRET_KEY_BASE=${SECRET_KEY}
      - RAILS_ENV=production
      - DATABASE_URL=postgresql://chatwoot:${DB_PASSWORD}@postgres:5432/chatwoot
      - REDIS_URL=redis://redis:6379
    depends_on:
      - postgres
      - redis
```

---

## Communication & Collaboration

### Slack/Zoom Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Mattermost](https://mattermost.com)** | Slack alternative | 31K+ | Various |
| **[Rocket.Chat](https://rocket.chat)** | Team chat + video | 41K+ | MIT |
| **[Jitsi Meet](https://jitsi.org)** | Video conferencing | 24K+ | Apache-2.0 |
| **[BigBlueButton](https://bigbluebutton.org)** | Online learning | 8.5K+ | LGPL |
| **[Element](https://element.io)** | Matrix-based, decentralized | 11K+ | Apache-2.0 |

### Jitsi Meet

**Replaces:** Zoom, Google Meet, Microsoft Teams (video)

**Features:**
- No account required
- End-to-end encryption
- Screen sharing
- Recording (local or cloud)
- Custom URLs
- Mobile apps
- Integrates with Mattermost, Slack

```yaml
# docker-compose.jitsi.yml
services:
  jitsi:
    image: jitsi/web:latest
    ports:
      - "8080:80"
      - "8443:443"
    environment:
      - PUBLIC_URL=https://meet.yourdomain.com
      - ENABLE_AUTH=1
      - ENABLE_GUESTS=1
```

---

## Documentation & Knowledge Base

### Notion/Confluence Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Outline](https://getoutline.com)** | Team wiki, beautiful | 30K+ | BSL |
| **[Docmost](https://docmost.com)** | Confluence alternative | 8K+ | AGPL-3.0 |
| **[BookStack](https://bookstackapp.com)** | Simple knowledge base | 16K+ | MIT |
| **[AFFiNE](https://affine.pro)** | Notion + Miro hybrid | 45K+ | MIT |
| **[AppFlowy](https://appflowy.io)** | Offline Notion | 60K+ | AGPL-3.0 |

### Outline

**Replaces:** Notion (team docs), Confluence, Slite

**Features:**
- Beautiful, fast interface
- Real-time collaboration
- Markdown support
- Slack, Figma, Loom integrations
- SSO support
- Full-text search
- API access

### AppFlowy (Recommended for Personal Use)

**Replaces:** Notion (personal)

**Features:**
- Works offline
- Data stored locally
- Kanban, calendar, table views
- AI integration
- Cross-platform (desktop)

---

## Scheduling & Booking

### Calendly Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Cal.com](https://cal.com)** | Full Calendly replacement | 34K+ | Various |
| **[Easy!Appointments](https://easyappointments.org)** | Simple booking | 3.2K+ | GPL-3.0 |

### Cal.com

**Replaces:** Calendly, Doodle, SavvyCal

**Features:**
- Unlimited event types
- Team scheduling
- Paid bookings
- Custom domains (white-label)
- API-driven
- 100+ integrations
- Recurring events
- Round-robin assignment

```yaml
# docker-compose.calcom.yml
services:
  calcom:
    image: calcom/cal.com:latest
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://calcom:${DB_PASSWORD}@postgres:5432/calcom
      - NEXT_PUBLIC_WEBAPP_URL=https://cal.yourdomain.com
      - CALENDSO_ENCRYPTION_KEY=${ENCRYPTION_KEY}
    depends_on:
      - postgres
```

---

## Invoicing & Billing

### Stripe Billing/FreshBooks Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Invoice Ninja](https://invoiceninja.com)** | Full invoicing + 40 gateways | 8.5K+ | Various |
| **[Crater](https://craterapp.com)** | Freelancers, simple | 8K+ | AGPL-3.0 |
| **[Lago](https://getlago.com)** | Usage-based billing API | 8K+ | AGPL-3.0 |
| **[Akaunting](https://akaunting.com)** | Full accounting | 8K+ | GPL-3.0 |
| **[Frappe Books](https://frappebooks.com)** | Desktop accounting | 3K+ | AGPL-3.0 |

### Invoice Ninja

**Replaces:** FreshBooks, QuickBooks, Xero

**Features:**
- Professional invoice design
- 40+ payment gateways (Stripe, PayPal, etc.)
- Recurring invoices
- Auto-billing
- Expense tracking
- Time tracking
- Client portal
- Mobile apps (Flutter)

### Lago (Usage-Based Billing)

**Replaces:** Stripe Billing, Chargebee, Recurly

**Features:**
- Usage metering
- Hybrid pricing (subscription + consumption)
- Real-time billing
- Coupons and credits
- Webhooks
- Stripe integration

---

## Media & Photos

### Google Photos/iCloud Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Immich](https://immich.app)** | Google Photos clone | 58K+ | AGPL-3.0 |
| **[PhotoPrism](https://photoprism.app)** | AI-powered photo management | 36K+ | AGPL-3.0 |
| **[Nextcloud Photos](https://nextcloud.com)** | Part of Nextcloud | N/A | AGPL-3.0 |

### Immich

**Replaces:** Google Photos, iCloud Photos

**Features:**
- Identical UI to Google Photos
- Mobile auto-backup (iOS & Android)
- Face recognition
- Object detection
- Location-based organization
- Timeline view
- Shared albums
- Machine learning tagging

```yaml
# docker-compose.immich.yml
services:
  immich-server:
    image: ghcr.io/immich-app/immich-server:release
    volumes:
      - ${UPLOAD_LOCATION}:/usr/src/app/upload
    environment:
      - DB_DATABASE_NAME=immich
      - DB_USERNAME=immich
      - DB_PASSWORD=${DB_PASSWORD}
      - REDIS_HOSTNAME=redis
    depends_on:
      - redis
      - postgres

  immich-machine-learning:
    image: ghcr.io/immich-app/immich-machine-learning:release
    volumes:
      - model-cache:/cache
```

---

## Forms & Surveys

### Typeform/Google Forms Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Formbricks](https://formbricks.com)** | Experience management | 9K+ | AGPL-3.0 |
| **[HeyForm](https://heyform.net)** | Conversational forms | 7K+ | AGPL-3.0 |
| **[LimeSurvey](https://limesurvey.org)** | Academic surveys | 2.8K+ | GPL-2.0 |

### Formbricks

**Replaces:** Typeform, SurveyMonkey, Hotjar

**Features:**
- In-app surveys
- Website surveys
- Link surveys
- Multi-language support
- Advanced targeting
- GDPR/CCPA compliant
- API access

### HeyForm

**Replaces:** Typeform

**Features:**
- Conversational UI
- Conditional logic
- Custom branding
- Webhooks
- Zapier/Make integration
- File uploads

---

## Security & Passwords

### LastPass/1Password Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Vaultwarden](https://github.com/dani-garcia/vaultwarden)** | Lightweight Bitwarden | 41K+ | AGPL-3.0 |
| **[Bitwarden](https://bitwarden.com)** | Official self-host | 26K+ | Various |
| **[Passbolt](https://passbolt.com)** | Team password sharing | 5K+ | AGPL-3.0 |

### Vaultwarden

**Replaces:** LastPass, 1Password, Dashlane

**Why Vaultwarden over Official Bitwarden:**
- Single Docker container (vs 12 containers)
- Runs on Raspberry Pi
- All premium features free
- Compatible with all Bitwarden clients

```yaml
# docker-compose.vaultwarden.yml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    ports:
      - "8080:80"
    environment:
      - ADMIN_TOKEN=${ADMIN_TOKEN}
      - SIGNUPS_ALLOWED=false
    volumes:
      - vaultwarden_data:/data
```

---

## Monitoring & DevOps

### Datadog/Pingdom Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Uptime Kuma](https://uptime.kuma.pet)** | Uptime monitoring | 63K+ | MIT |
| **[Grafana](https://grafana.com)** | Dashboards | 66K+ | AGPL-3.0 |
| **[Prometheus](https://prometheus.io)** | Metrics collection | 57K+ | Apache-2.0 |
| **[Netdata](https://netdata.cloud)** | Real-time monitoring | 73K+ | GPL-3.0 |
| **[Gatus](https://gatus.io)** | Health checks | 6K+ | Apache-2.0 |

### Uptime Kuma

**Replaces:** Pingdom, UptimeRobot, StatusCake

**Features:**
- 90+ notification services
- Beautiful status pages
- Multi-location monitoring
- TCP, HTTP, DNS, Docker monitoring
- 20-second check intervals
- Maintenance windows

```yaml
# docker-compose.uptime-kuma.yml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    ports:
      - "3001:3001"
    volumes:
      - uptime_data:/app/data
```

---

## Link Management

### Bit.ly Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Dub.co](https://dub.co)** | Modern link management | 20K+ | AGPL-3.0 |
| **[Shlink](https://shlink.io)** | PHP-based shortener | 3.2K+ | MIT |
| **[YOURLS](https://yourls.org)** | Simple & powerful | 10K+ | MIT |
| **[Kutt](https://kutt.it)** | Modern shortener | 8.5K+ | MIT |

### Dub.co

**Replaces:** Bit.ly, Rebrandly, TinyURL

**Features:**
- Custom domains
- Link analytics
- QR codes
- Link expiration
- Password protection
- API access
- Team collaboration

---

## Cloud Storage

### Dropbox/Google Drive Alternatives

| Tool | Best For | GitHub Stars | License |
|------|----------|--------------|---------|
| **[Nextcloud](https://nextcloud.com)** | Full suite | 28K+ | AGPL-3.0 |
| **[Seafile](https://seafile.com)** | High performance | 12K+ | Various |
| **[FileBrowser](https://filebrowser.org)** | Simple file access | 27K+ | Apache-2.0 |
| **[ownCloud](https://owncloud.com)** | Enterprise file sync | 8K+ | Various |

### Nextcloud

**Replaces:** Google Workspace, Dropbox, OneDrive

**Features:**
- File sync and share
- Collaborative editing (OnlyOffice/Collabora)
- Calendar, Contacts
- Talk (video calls)
- Photos app
- Mail client
- 200+ apps

---

## Additional Tools

### Social Media

| Tool | Replaces | Description |
|------|----------|-------------|
| **[Postiz](https://postiz.com)** | Buffer, Hootsuite | Social media scheduling |
| **[Mastodon](https://joinmastodon.org)** | Twitter | Decentralized social |
| **[Pixelfed](https://pixelfed.org)** | Instagram | Photo sharing |
| **[PeerTube](https://joinpeertube.org)** | YouTube | Video hosting |
| **[Lemmy](https://join-lemmy.org)** | Reddit | Link aggregator |

### Media

| Tool | Replaces | Description |
|------|----------|-------------|
| **[Jellyfin](https://jellyfin.org)** | Plex, Netflix | Media server |
| **[Navidrome](https://navidrome.org)** | Spotify | Music streaming |
| **[Audiobookshelf](https://audiobookshelf.org)** | Audible | Audiobook server |
| **[Kavita](https://kavitareader.com)** | Kindle | eBook reader |

### Productivity

| Tool | Replaces | Description |
|------|----------|-------------|
| **[Vikunja](https://vikunja.io)** | Todoist | Task management |
| **[Habitica](https://habitica.com)** | Habitify | Habit tracking |
| **[Linkwarden](https://linkwarden.app)** | Pocket, Instapaper | Bookmark manager |
| **[Paperless-ngx](https://docs.paperless-ngx.com)** | Evernote (docs) | Document management |
| **[Stirling-PDF](https://github.com/Frooodle/Stirling-PDF)** | Adobe Acrobat | PDF tools |

### Development

| Tool | Replaces | Description |
|------|----------|-------------|
| **[Gitea](https://gitea.io)** | GitHub | Git hosting |
| **[GitLab](https://gitlab.com)** | GitHub + CI/CD | Full DevOps platform |
| **[Drone](https://drone.io)** | GitHub Actions | CI/CD |
| **[Sentry](https://sentry.io)** | Itself (self-host) | Error tracking |
| **[Plausible](https://plausible.io)** | Google Analytics | Web analytics |

### AI/ML

| Tool | Replaces | Description |
|------|----------|-------------|
| **[Ollama](https://ollama.ai)** | OpenAI API | Local LLMs |
| **[Open WebUI](https://openwebui.com)** | ChatGPT | Chat interface |
| **[Whisper](https://github.com/openai/whisper)** | Rev.com | Transcription |
| **[Stable Diffusion](https://stability.ai)** | Midjourney, DALL-E | Image generation |

---

## Integration Matrix

How these tools work together with n8n:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           n8n INTEGRATION HUB                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  MARKETING                    CRM                      SUPPORT              │
│  ┌─────────────┐             ┌─────────────┐          ┌─────────────┐       │
│  │ Listmonk    │◄───────────►│ Twenty CRM  │◄────────►│ Chatwoot    │       │
│  │ Mautic      │             │ Erxes       │          │ Zammad      │       │
│  └──────┬──────┘             └──────┬──────┘          └──────┬──────┘       │
│         │                           │                        │              │
│         └───────────────────────────┼────────────────────────┘              │
│                                     │                                        │
│                              ┌──────▼──────┐                                │
│                              │    n8n      │                                │
│                              │  Workflows  │                                │
│                              └──────┬──────┘                                │
│                                     │                                        │
│         ┌───────────────────────────┼────────────────────────┐              │
│         │                           │                        │              │
│  ┌──────▼──────┐             ┌──────▼──────┐          ┌──────▼──────┐       │
│  │ Cal.com     │             │ Invoice     │          │ Postiz      │       │
│  │ Scheduling  │             │ Ninja       │          │ Social      │       │
│  └─────────────┘             └─────────────┘          └─────────────┘       │
│                                                                             │
│  ANALYTICS                   STORAGE                   MONITORING           │
│  ┌─────────────┐             ┌─────────────┐          ┌─────────────┐       │
│  │ Umami       │◄───────────►│ Nextcloud   │◄────────►│ Uptime Kuma │       │
│  │ Plausible   │             │ Immich      │          │ Grafana     │       │
│  └─────────────┘             └─────────────┘          └─────────────┘       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Recommended Personal Stack

Based on your hardware (RTX 4080 laptop + planned home server):

### Tier 1: Essential (Start Here)

| Category | Tool | Why |
|----------|------|-----|
| **Automation** | n8n | Central hub for everything |
| **Passwords** | Vaultwarden | Security foundation |
| **Monitoring** | Uptime Kuma | Know when things break |
| **Storage** | Nextcloud | Files, calendar, contacts |
| **AI** | Ollama + Open WebUI | Local LLM on your GPU |

### Tier 2: Productivity

| Category | Tool | Why |
|----------|------|-----|
| **Photos** | Immich | Google Photos replacement |
| **Docs** | Paperless-ngx | Go paperless |
| **Tasks** | Vikunja | Task management |
| **Bookmarks** | Linkwarden | Save anything |
| **Notes** | AppFlowy | Offline Notion |

### Tier 3: Professional

| Category | Tool | Why |
|----------|------|-----|
| **CRM** | Twenty | Contact management |
| **Scheduling** | Cal.com | Booking links |
| **Invoicing** | Invoice Ninja | Get paid |
| **Email** | Listmonk | Newsletters |
| **Analytics** | Umami | Website stats |

### Tier 4: Advanced

| Category | Tool | Why |
|----------|------|-----|
| **Marketing** | Mautic | Full automation |
| **Support** | Chatwoot | Customer comms |
| **Social** | Postiz | Multi-platform posting |
| **Media** | Jellyfin | Stream your media |
| **Forms** | Formbricks | Surveys & feedback |

### Full Docker Compose

See `PERSONAL_AUTOMATION_ECOSYSTEM.md` for the complete Docker Compose file that runs this entire stack.

---

## Cost Comparison

| SaaS Stack (Monthly) | Self-Hosted (One-Time) |
|---------------------|------------------------|
| Mailchimp: $50 | Listmonk: $0 |
| Salesforce: $75 | Twenty: $0 |
| Calendly: $12 | Cal.com: $0 |
| Typeform: $35 | Formbricks: $0 |
| Zendesk: $55 | Chatwoot: $0 |
| Google Photos: $10 | Immich: $0 |
| Zoom: $16 | Jitsi: $0 |
| 1Password: $8 | Vaultwarden: $0 |
| **Total: ~$261/month** | **Hardware: ~$700 once** |
| **Year 1: $3,132** | **Year 1: $700** |
| **Year 5: $15,660** | **Year 5: $700** |

---

## Resources

### Discovery

- **[Awesome-Selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)** - The definitive list (200K+ stars)
- **[OpenAlternative.co](https://openalternative.co)** - Find open source alternatives
- **[AlternativeTo](https://alternativeto.net)** - Compare software options
- **[It's FOSS](https://itsfoss.com)** - Linux & open source guides

### Communities

- **[r/selfhosted](https://reddit.com/r/selfhosted)** - Reddit community
- **[selfh.st](https://selfh.st)** - Newsletter and resources
- **[Perfect Media Server](https://perfectmediaserver.com)** - Complete guides

### Learning

- **[Noted.lol](https://noted.lol)** - Self-hosting tutorials
- **[LinuxServer.io](https://linuxserver.io)** - Docker images
- **[TechnoTim](https://youtube.com/@TechnoTim)** - YouTube tutorials

---

**Sources:**
- [n8n Open Source Marketing Tools](https://blog.n8n.io/open-source-marketing-automation-tools/)
- [Mautic](https://mautic.org)
- [Listmonk vs Mautic Comparison](https://openalternative.co/compare/listmonk/vs/mautic)
- [Twenty CRM](https://twenty.com/)
- [Plane - Open Source Jira Alternative](https://github.com/makeplane/plane)
- [Chatwoot](https://chatwoot.com/)
- [Umami vs Plausible vs Matomo](https://aaronjbecker.com/posts/umami-vs-plausible-vs-matomo-self-hosted-analytics/)
- [Immich - Google Photos Alternative](https://immich.app/)
- [Formbricks - Open Source Form Builder](https://formbricks.com/)
- [Vaultwarden](https://github.com/dani-garcia/vaultwarden)
- [Cal.com](https://cal.com/)
- [Invoice Ninja](https://invoiceninja.com/)
- [Uptime Kuma](https://github.com/louislam/uptime-kuma)
- [Awesome-Selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)
