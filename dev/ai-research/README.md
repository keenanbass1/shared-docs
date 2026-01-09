---
visibility: public
title: AI Research Index
description: Centralized research for building a personal automation ecosystem as a solo developer
category: index
tags: [automation, ai, development, personal-research, n8n]
updated: 2026-01-09
status: in-progress
---

# Personal Research & Automation Knowledge Base

**Purpose:** Centralized research for building a personal automation ecosystem as a solo developer.

---

## Document Structure

```
personal-research/
├── README.md                          # This file - index and strategy
├── 00-MASTER-REFERENCE.md            # Quick reference cheat sheet (TBD)
│
├── 01-AI-AND-DEV-TOOLS.md            # AI tools, Claude API, coding assistants
├── 02-AUTOMATION-PLATFORMS.md        # n8n, automation patterns, workflows
├── 03-SELF-HOSTED-STACK.md           # Open source SaaS alternatives
├── 04-HARDWARE-AND-IOT.md            # Server specs, IoT sensors, smart home
├── 05-IMPLEMENTATION-ROADMAP.md      # Prioritized action plan
│
└── archive/                          # Superseded documents
    ├── N8N_AUTOMATION_AUDIT_REPORT.md    # (Work-specific, move to main docs/)
    ├── N8N_PERSONAL_PROJECTS_GUIDE.md    # → Merged into 02
    ├── PERSONAL_AUTOMATION_ECOSYSTEM.md  # → Split into 03, 04
    └── OPEN_SOURCE_SAAS_ALTERNATIVES.md  # → Merged into 03
```

## Document Principles

1. **Single Source of Truth** - Each topic lives in ONE place
2. **No Duplication** - Cross-reference, don't copy
3. **Living Documents** - Update existing docs, don't create new ones
4. **Action-Oriented** - End with concrete next steps

---

## Hardware Profile

| Asset | Specs | Role |
|-------|-------|------|
| **Dev Laptop** | MSI Vector, i9, RTX 4080 12GB VRAM | GPU inference, development |
| **Home Server** (planned) | 32-64GB RAM, capable CPU | Always-on services |
| **Claude Code Max** | High token allowance | AI coding, API access |

---

## Quick Links

### By Topic
- [AI Tools & Claude API](./01-AI-AND-DEV-TOOLS.md) - What's possible in Nov 2025
- [Automation Platforms](./02-AUTOMATION-PLATFORMS.md) - n8n, workflows, integrations
- [Self-Hosted Stack](./03-SELF-HOSTED-STACK.md) - Open source alternatives
- [Hardware & IoT](./04-HARDWARE-AND-IOT.md) - Server, sensors, smart home
- [Implementation Roadmap](./05-IMPLEMENTATION-ROADMAP.md) - What to do first

### By Use Case
- Personal AI Assistant → [01](./01-AI-AND-DEV-TOOLS.md#personal-ai-assistant)
- Content Automation → [02](./02-AUTOMATION-PLATFORMS.md#content-automation)
- Smart Home → [04](./04-HARDWARE-AND-IOT.md#smart-home)
- Cost Savings → [03](./03-SELF-HOSTED-STACK.md#cost-comparison)

---

## Research Status

| Document | Status | Last Updated |
|----------|--------|--------------|
| 01-AI-AND-DEV-TOOLS.md | 🟢 Complete | Nov 2025 |
| 02-AUTOMATION-PLATFORMS.md | ⚪ Not Started | - |
| 03-SELF-HOSTED-STACK.md | ⚪ Not Started | - |
| 04-HARDWARE-AND-IOT.md | ⚪ Not Started | - |
| 05-IMPLEMENTATION-ROADMAP.md | ⚪ Not Started | - |

---

## Migration Plan

The existing documents contain valuable content but have overlap. Here's the consolidation plan:

1. **N8N_AUTOMATION_AUDIT_REPORT.md** → Move to `docs/` (work-specific, not personal)
2. **N8N_PERSONAL_PROJECTS_GUIDE.md** → Extract to 02-AUTOMATION-PLATFORMS.md
3. **PERSONAL_AUTOMATION_ECOSYSTEM.md** → Split between 03, 04, 05
4. **OPEN_SOURCE_SAAS_ALTERNATIVES.md** → Merge into 03-SELF-HOSTED-STACK.md

Will consolidate after completing new research to avoid losing context.
