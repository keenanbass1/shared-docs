---
visibility: public
title: Claude Code Advanced Usage
description: Comprehensive guide to leveraging Claude Code features, best practices, and advanced patterns
category: research
tags: [claude-code, ai, development, best-practices, documentation]
updated: 2026-01-09
status: complete
---

# Claude Code Advanced Usage Guide

> A comprehensive guide to leveraging Claude Code like a senior developer, covering all major features, best practices, and advanced patterns based on official documentation.

---

## Table of Contents

1. [Core Architecture Overview](#core-architecture-overview)
2. [Memory System (CLAUDE.md)](#memory-system-claudemd)
3. [Configuration System](#configuration-system)
4. [Slash Commands](#slash-commands)
5. [Hooks System](#hooks-system)
6. [MCP Servers](#mcp-servers)
7. [Subagents](#subagents)
8. [Agent Skills](#agent-skills)
9. [Plugins](#plugins)
10. [SDK & Programmatic Usage](#sdk--programmatic-usage)
11. [Headless Mode & CI/CD Integration](#headless-mode--cicd-integration)
12. [Model Selection & Optimization](#model-selection--optimization)
13. [Prompt Caching](#prompt-caching)
14. [Extended Thinking](#extended-thinking)
15. [Project Structure Best Practices](#project-structure-best-practices)
16. [Real-World Workflow Examples](#real-world-workflow-examples)

---

## Core Architecture Overview

Claude Code follows Unix philosophy principles: composable, scriptable, and designed for integration into existing development workflows. Key characteristics:

- **Terminal-native**: Works directly in your terminal, not a separate IDE
- **MCP Integration**: Extends capabilities via Model Context Protocol servers
- **Hierarchical configuration**: Enterprise > Project > User > Local settings
- **Agentic capabilities**: Can edit files, run commands, create commits, and orchestrate subagents

---

## Memory System (CLAUDE.md)

The memory system provides persistent context that loads automatically when Claude Code starts.

### Memory Hierarchy (Precedence Order)

| Level | Location | Purpose |
|-------|----------|---------|
| Enterprise | `/Library/Application Support/ClaudeCode/CLAUDE.md` (macOS) | Organization-wide standards |
| | `/etc/claude-code/CLAUDE.md` (Linux/WSL) | |
| | `C:\ProgramData\ClaudeCode\CLAUDE.md` (Windows) | |
| Project | `./CLAUDE.md` or `./.claude/CLAUDE.md` | Team-shared project context |
| User | `~/.claude/CLAUDE.md` | Personal preferences (all projects) |
| Local | `./.claude/CLAUDE.local.md` | Personal project-specific (deprecated) |

### Best Practices for CLAUDE.md

```markdown
# Project: My App

## Build Commands
- `npm run dev` - Start development server
- `npm run test` - Run test suite
- `npm run lint` - Check code style

## Code Style
- Use 2-space indentation
- Prefer async/await over callbacks
- Use TypeScript strict mode

## Architecture
- `/src/components` - React components
- `/src/hooks` - Custom React hooks
- `/src/api` - API client functions

## Naming Conventions
- Components: PascalCase (e.g., `UserProfile.tsx`)
- Hooks: camelCase with `use` prefix (e.g., `useAuth.ts`)
- Utils: camelCase (e.g., `formatDate.ts`)
```

### Memory Discovery

Claude reads memories recursively starting from the current working directory up to (but not including) root. It also discovers CLAUDE.md files in subdirectories when accessing files in those locations.

### Bootstrap Command

```bash
# Initialize a CLAUDE.md for your project
/init
```

---

## Configuration System

### Settings File Locations

| Scope | File | Purpose |
|-------|------|---------|
| User | `~/.claude/settings.json` | Personal global settings |
| Project | `.claude/settings.json` | Team-shared project settings |
| Local | `.claude/settings.local.json` | Per-machine overrides (gitignored) |

### Example settings.json

```json
{
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(git:*)",
      "Read",
      "Write",
      "Edit"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)"
    ]
  },
  "env": {
    "NODE_ENV": "development",
    "DEBUG": "true"
  },
  "hooks": {},
  "enabledPlugins": {
    "my-plugin@my-marketplace": true
  },
  "extraKnownMarketplaces": {
    "team-marketplace": {
      "source": "github:myorg/claude-plugins"
    }
  }
}
```

---

## Slash Commands

Custom commands that can be invoked with `/command-name`. They're markdown files with optional YAML frontmatter.

### Creating Slash Commands

**Location**: `.claude/commands/` (project) or `~/.claude/commands/` (user)

**Example**: `.claude/commands/commit.md`

```markdown
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit with conventional format
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## Your Task

Based on the changes above, create a single git commit following conventional commit format:
- feat: new feature
- fix: bug fix
- docs: documentation
- refactor: code refactoring
- test: adding tests
```

### Command Arguments

```markdown
# .claude/commands/review-pr.md
---
description: Review a specific PR
---

Review PR #$1 with priority $2 and assign to $3
```

Usage: `/review-pr 456 high alice`

### Using All Arguments

```markdown
# Uses all arguments passed to the command
Analyze and explain: $ARGUMENTS
```

### Dynamic Context with Bash

Prefix commands with `!` to execute before the prompt:

```markdown
Current environment: !`printenv | grep NODE`
Package versions: !`npm list --depth=0`
```

---

## Hooks System

Hooks execute shell commands or LLM prompts at specific lifecycle events.

### Hook Events

| Event | Trigger | Use Cases |
|-------|---------|-----------|
| `PreToolUse` | Before tool execution | Validation, logging, blocking |
| `PostToolUse` | After tool execution | Cleanup, notifications |
| `Notification` | On assistant notifications | Alerts, logging |
| `Stop` | Before conversation ends | Cleanup, summaries |
| `SubagentStop` | Before subagent stops | Validation, continuation logic |

### Configuration Example

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '\"\(.tool_input.command)\"' >> ~/.claude/bash-log.txt"
          }
        ]
      },
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Modifying: $CLAUDE_TOOL_FILE_PATH' >> ~/.claude/file-changes.log"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/notify-on-error.sh"
          }
        ]
      }
    ]
  }
}
```

### MCP Tool Hooks

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Memory operation' >> ~/mcp-ops.log"
          }
        ]
      },
      {
        "matcher": "mcp__.*__write.*",
        "hooks": [
          {
            "type": "command",
            "command": "/scripts/validate-mcp-write.py"
          }
        ]
      }
    ]
  }
}
```

### LLM-Based Hooks (Prompt Type)

```json
{
  "hooks": {
    "SubagentStop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if this subagent completed its task. Input: $ARGUMENTS\n\nReturn: {\"decision\": \"approve\" or \"block\", \"reason\": \"explanation\"}"
          }
        ]
      }
    ]
  }
}
```

---

## MCP Servers

Model Context Protocol servers extend Claude Code's capabilities with external tools and data sources.

### Adding MCP Servers

```bash
# stdio transport (local process)
claude mcp add --transport stdio my-server -- node ./my-mcp-server.js

# With environment variables
claude mcp add --transport stdio airtable --env AIRTABLE_API_KEY=YOUR_KEY \
  -- npx -y airtable-mcp-server

# HTTP transport
claude mcp add --transport http github https://api.example.com/mcp

# SSE transport
claude mcp add --transport sse events https://events.example.com/stream
```

### Managing MCP Servers

```bash
# List all servers
claude mcp list

# Get server details
claude mcp get github

# Remove a server
claude mcp remove github

# Within Claude Code
/mcp
```

### .mcp.json Configuration

```json
{
  "mcpServers": {
    "my-tool": {
      "command": "node",
      "args": ["./my-mcp-server.js"],
      "env": {
        "DEBUG": "${DEBUG:-false}"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem"],
      "env": {
        "ALLOWED_PATHS": "/Users/me/projects"
      }
    },
    "secure-api": {
      "type": "sse",
      "url": "https://api.example.com/mcp",
      "headers": {
        "Authorization": "Bearer ${API_TOKEN}",
        "X-API-Key": "${API_KEY:-default-key}"
      }
    }
  }
}
```

### Using MCP Prompts as Slash Commands

```bash
# MCP servers can expose prompts as slash commands
/mcp__github__list_prs
/mcp__github__pr_review 456
/mcp__jira__create_issue "Bug in login flow" high
```

---

## Subagents

Specialized AI assistants with custom system prompts and tool configurations for specific task types.

### Built-in Subagents

| Agent | Description | Tools |
|-------|-------------|-------|
| `Explore` | Codebase exploration and search | Read, Glob, Grep |
| `Plan` | Architecture planning and design | Read, Glob, Grep, Bash |

### Creating Custom Subagents

**Project level**: `.claude/agents/code-reviewer.md`

```markdown
---
name: code-reviewer
description: Expert code review specialist. Use proactively after code changes.
tools: Read, Grep, Glob, Bash
model: inherit
---

You are a senior code reviewer ensuring high standards of code quality.

When invoked:
1. Run git diff to see recent changes
2. Focus on modified files
3. Begin review immediately

Review checklist:
- Code is simple and readable
- Proper error handling
- No exposed secrets
- Good test coverage

Provide feedback organized by priority:
- Critical issues (must fix)
- Warnings (should fix)
- Suggestions (consider improving)
```

### CLI-Defined Subagents

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "Debugging specialist for errors and test failures.",
    "prompt": "You are an expert debugger. Analyze errors, identify root causes, and provide fixes."
  }
}'
```

### Model Selection for Subagents

- `sonnet` - Default for most subagents
- `opus` - Complex reasoning tasks
- `haiku` - Fast, simple operations
- `inherit` - Use same model as main conversation

### Managing Subagents

```bash
# Interactive management
/agents

# Explicitly invoke
> Use the code-reviewer subagent to review my changes
> Have the test-runner subagent fix failing tests
```

### Resuming Subagent Conversations

```typescript
{
  "description": "Continue analysis",
  "prompt": "Now examine the error handling patterns",
  "subagent_type": "code-analyzer",
  "resume": "previous-agent-id-abc123"
}
```

---

## Agent Skills

Skills are model-invoked capabilities that Claude autonomously uses based on task context.

### Skill Locations

| Type | Location | Scope |
|------|----------|-------|
| Personal | `~/.claude/skills/` | All your projects |
| Project | `.claude/skills/` | Team-shared |
| Plugin | Via installed plugins | Automatic |

### Creating a Skill

**Directory structure**: `.claude/skills/my-skill/`

```
my-skill/
├── SKILL.md          # Required: Main skill definition
├── reference/        # Optional: Additional documentation
│   ├── examples.md
│   └── api.md
└── scripts/          # Optional: Helper scripts
    └── helper.sh
```

**SKILL.md example**:

```markdown
# Database Migration Skill

## Overview
Create and manage database migrations safely.

## Process

### Phase 1: Analysis
1. Examine current schema
2. Identify required changes
3. Check for data dependencies

### Phase 2: Implementation
1. Generate migration file
2. Add rollback logic
3. Test migration locally

## Reference Files
- [Examples](./reference/examples.md) - Common migration patterns
- [API Reference](./reference/api.md) - Migration tool API

## Quality Checklist
- [ ] Migration is reversible
- [ ] Data integrity preserved
- [ ] Indexes updated appropriately
```

### Viewing Available Skills

```bash
# Ask Claude
> What Skills are available?
> List all available Skills
```

---

## Plugins

Plugins bundle commands, agents, hooks, MCP servers, and skills into distributable packages.

### Plugin Structure

```
my-plugin/
├── plugin.json           # Plugin manifest
├── .mcp.json             # Optional: MCP server config
├── commands/             # Slash commands
│   └── my-command.md
├── agents/               # Subagents
│   └── my-agent.md
├── skills/               # Agent skills
│   └── my-skill/
│       └── SKILL.md
└── README.md
```

### plugin.json

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom plugin",
  "mcpServers": {
    "plugin-api": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/api-server",
      "args": ["--port", "8080"]
    }
  }
}
```

### Managing Plugins

```bash
# Interactive plugin management
/plugin

# Enable in settings.json
{
  "enabledPlugins": {
    "my-plugin@marketplace-name": true
  },
  "extraKnownMarketplaces": {
    "my-marketplace": {
      "source": "github:myorg/plugins"
    }
  }
}
```

### Team Plugin Distribution

In `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "team-plugins": {
      "source": "github:myorg/claude-plugins"
    }
  },
  "enabledPlugins": {
    "code-quality@team-plugins": true,
    "deployment-tools@team-plugins": true
  }
}
```

---

## SDK & Programmatic Usage

### Python SDK Installation

```bash
pip install claude-agent-sdk

# Prerequisites
# - Python 3.10+
# - Node.js
# - Claude Code: npm install -g @anthropic-ai/claude-code
```

### Basic Query

```python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, AssistantMessage, TextBlock

async def main():
    async for message in query(prompt="What is 2 + 2?"):
        if isinstance(message, AssistantMessage):
            for block in message.content:
                if isinstance(block, TextBlock):
                    print(block.text)

anyio.run(main)
```

### With Options

```python
from claude_agent_sdk import query, ClaudeAgentOptions

options = ClaudeAgentOptions(
    system_prompt="You are a helpful assistant",
    max_turns=1,
    allowed_tools=["Read", "Write", "Bash"],
    permission_mode='acceptEdits'
)

async for message in query(prompt="Create a hello.py file", options=options):
    print(message)
```

### Custom MCP Servers (In-Process)

```python
from claude_agent_sdk import tool, create_sdk_mcp_server, ClaudeAgentOptions, ClaudeSDKClient

@tool("greet", "Greet a user", {"name": str})
async def greet_user(args):
    return {
        "content": [
            {"type": "text", "text": f"Hello, {args['name']}!"}
        ]
    }

server = create_sdk_mcp_server(
    name="my-tools",
    version="1.0.0",
    tools=[greet_user]
)

options = ClaudeAgentOptions(
    mcp_servers={"tools": server},
    allowed_tools=["mcp__tools__greet"]
)

async with ClaudeSDKClient(options=options) as client:
    await client.query("Greet Alice")
    async for msg in client.receive_response():
        print(msg)
```

### PreToolUse Hook in SDK

```python
from claude_agent_sdk import ClaudeAgentOptions, ClaudeSDKClient, HookMatcher

async def check_bash_command(input_data, tool_use_id, context):
    tool_name = input_data["tool_name"]
    tool_input = input_data["tool_input"]

    if tool_name != "Bash":
        return {}

    command = tool_input.get("command", "")
    forbidden = ["rm -rf", "sudo", "dd if="]

    for pattern in forbidden:
        if pattern in command:
            return {
                "hookSpecificOutput": {
                    "hookEventName": "PreToolUse",
                    "permissionDecision": "deny",
                    "permissionDecisionReason": f"Blocked: {pattern}",
                }
            }
    return {}

options = ClaudeAgentOptions(
    allowed_tools=["Bash"],
    hooks={
        "PreToolUse": [
            HookMatcher(matcher="Bash", hooks=[check_bash_command]),
        ],
    }
)
```

### Programmatic Subagents

```typescript
import { query } from '@anthropic-ai/claude-agent-sdk';

const result = query({
  prompt: "Review the authentication module",
  options: {
    agents: {
      'code-reviewer': {
        description: 'Expert code review specialist',
        prompt: `You are a code review specialist...`,
        tools: ['Read', 'Grep', 'Glob'],
        model: 'sonnet'
      },
      'test-runner': {
        description: 'Runs and analyzes test suites',
        prompt: `You are a test execution specialist...`,
        tools: ['Bash', 'Read', 'Grep'],
      }
    }
  }
});

for await (const message of result) {
  console.log(message);
}
```

---

## Headless Mode & CI/CD Integration

### Basic Headless Execution

```bash
# Simple prompt with print mode
claude -p "Explain this codebase structure"

# With allowed tools
claude -p "Stage changes and create a commit" \
  --allowedTools "Bash,Read" \
  --permission-mode acceptEdits

# JSON output for pipeline integration
claude -p "Analyze code quality" --json | jq '.result'
```

### Tool Restrictions

```bash
# Allow specific tools
claude --allowedTools mcp__slack mcp__filesystem

# Allow tools with patterns
claude --allowedTools "Bash(npm install),mcp__filesystem"

# Disallow specific tools
claude --disallowedTools "Bash(git commit),mcp__github"
```

### Continuing Conversations

```bash
# Continue most recent session
claude --continue "Now refactor for better performance"

# Resume specific session by ID
claude --resume 550e8400-e29b-41d4-a716-446655440000 "Update tests"

# Resume in non-interactive mode
claude --resume SESSION_ID "Fix linting issues" --no-interactive
```

### CI Integration Examples

**GitHub Actions Translation Workflow**:

```bash
claude -p "Translate new strings to French and raise a PR" \
  --allowedTools "Read,Write,Bash(git:*)" \
  --permission-mode acceptEdits
```

**Security Audit Function**:

```bash
audit_pr() {
    local pr_number="$1"
    gh pr diff "$pr_number" | claude -p \
      --append-system-prompt "You are a security engineer. Review for vulnerabilities." \
      --output-format json \
      --allowedTools "Read,Grep,WebSearch"
}

# Usage
audit_pr 123 > security-report.json
```

**Incident Response Bot**:

```bash
investigate_incident() {
    local description="$1"
    local severity="${2:-medium}"

    claude -p "Incident: $description (Severity: $severity)" \
      --append-system-prompt "You are an SRE expert. Diagnose and provide action items." \
      --output-format json \
      --allowedTools "Bash,Read,WebSearch,mcp__datadog" \
      --mcp-config monitoring-tools.json
}

investigate_incident "Payment API 500 errors" "high"
```

**Fanning Out Tasks**:

```bash
# Process multiple files
for file in src/*.py; do
  claude -p "Migrate $file from Flask to FastAPI. Return OK or FAIL." \
    --allowedTools "Edit,Bash(git commit:*)" \
    --permission-mode acceptEdits
done
```

---

## Model Selection & Optimization

### Available Model Aliases

| Alias | Use Case |
|-------|----------|
| `default` | Recommended model (usually Sonnet) |
| `sonnet` | Daily coding tasks, balanced speed/capability |
| `opus` | Complex reasoning, architectural decisions |
| `haiku` | Fast, simple operations |
| `sonnet[1m]` | 1 million token context window |
| `opusplan` | Opus for planning, Sonnet for execution |

### Changing Models

```bash
# Interactive
/model

# Via environment variable
export CLAUDE_MODEL=opus

# Per-session
claude --model opus
```

### Cost Optimization Strategies

1. **Use appropriate models**: Haiku for simple tasks, Sonnet for complex reasoning
2. **Implement prompt caching**: Reduce costs for repeated context
3. **Batch operations**: Use Batch API for non-time-sensitive tasks
4. **Monitor usage patterns**: Track token consumption

### Model-Specific Cache Limits

| Model | Minimum Cacheable Tokens |
|-------|-------------------------|
| Claude Opus 4.x, Sonnet 4.x/3.7 | 1024 |
| Claude Haiku 4.5 | 4096 |
| Claude Haiku 3.5/3 | 2048 |

---

## Prompt Caching

Prompt caching reduces costs and latency for repeated context.

### Basic Caching (API)

```python
from anthropic import Anthropic

client = Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=300,
    system=[
        {
            "type": "text",
            "text": large_context,
            "cache_control": {"type": "ephemeral"}
        },
    ],
    messages=[{"role": "user", "content": "Analyze this..."}],
)
```

### Speculative Cache Warming

Pre-warm the cache while user is typing:

```python
async def speculative_caching():
    # Start cache warming in background
    cache_task = asyncio.create_task(sample_one_token(client, [context]))

    # Wait for user input
    user_question = await get_user_input()

    # Ensure cache is warm
    await cache_task

    # Send actual query (cache hit)
    response = await client.messages.stream(
        messages=[{...context, "content": user_question}],
        model=MODEL,
    )
```

### Cache Benefits

- **Cost reduction**: Cached tokens cost 90% less
- **Latency improvement**: Faster time to first token
- **Efficiency**: Reuse context across conversation turns

---

## Extended Thinking

For complex tasks requiring deep reasoning.

### Enabling Extended Thinking

```bash
# Toggle in interactive mode
# Press Tab to toggle "Thinking" on

# Via environment variable
export MAX_THINKING_TOKENS=10000

# In settings.json
{
  "env": {
    "MAX_THINKING_TOKENS": "10000"
  }
}
```

### Triggering Depth Levels

| Prompt | Thinking Depth |
|--------|----------------|
| "think" | Basic |
| "think hard" | Moderate |
| "think more" | Deeper |
| "think a lot" | Extended |
| "think longer" | Maximum |

### Example Usage

```
> I need to implement OAuth2 authentication. Think deeply about the best approach.

> Think hard about potential security vulnerabilities in this implementation.

> Think about edge cases in the payment processing flow.
```

---

## Project Structure Best Practices

### Recommended .claude/ Directory

```
.claude/
├── settings.json          # Project settings (tracked in git)
├── settings.local.json    # Local overrides (gitignored)
├── CLAUDE.md              # Project memory
├── commands/              # Custom slash commands
│   ├── commit.md
│   ├── review.md
│   └── deploy.md
├── agents/                # Custom subagents
│   ├── code-reviewer.md
│   ├── test-runner.md
│   └── debugger.md
├── skills/                # Agent skills
│   └── database-migrations/
│       └── SKILL.md
└── .mcp.json              # MCP server configuration
```

### .gitignore Additions

```gitignore
# Claude Code local files
.claude/settings.local.json
.claude/*.local.md
```

### Team Onboarding Configuration

In `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "team-tools": {
      "source": "github:myorg/claude-plugins"
    }
  },
  "enabledPlugins": {
    "our-standards@team-tools": true
  },
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(yarn:*)",
      "Bash(pnpm:*)",
      "Bash(git:*)",
      "Read",
      "Write",
      "Edit"
    ]
  }
}
```

---

## Real-World Workflow Examples

### Feature Development Workflow

```bash
# 1. Plan the feature (uses opus in plan mode)
/model opusplan
> Help me implement user authentication with OAuth2

# 2. Claude explores codebase and creates plan
# 3. Review and approve plan

# 4. Implementation (switches to sonnet)
# Claude implements with code-reviewer subagent checking work

# 5. Commit changes
/commit
```

### Code Review Workflow

```bash
# Define in .claude/agents/reviewer.md
# Then use:
> Use the code-reviewer agent to review my changes

# Or with PR review toolkit plugin
> Review this PR for security issues
```

### Automated PR Checks (CI)

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Review PR
        run: |
          gh pr diff ${{ github.event.pull_request.number }} | \
          claude -p "Review this PR for issues" \
            --output-format json \
            --allowedTools "Read,Grep" > review.json
```

### Multi-Agent Orchestration

```python
# Parallel agent execution
from claude_agent_sdk import query, ClaudeAgentOptions

options = ClaudeAgentOptions(
    agents={
        'security-reviewer': {...},
        'performance-checker': {...},
        'test-analyzer': {...}
    }
)

# All agents can run concurrently on different aspects
result = query(
    prompt="Analyze this PR comprehensively",
    options=options
)
```

---

## Quick Reference

### Essential Commands

| Command | Description |
|---------|-------------|
| `/init` | Bootstrap CLAUDE.md |
| `/model` | Change model |
| `/mcp` | View MCP servers |
| `/agents` | Manage subagents |
| `/plugin` | Manage plugins |
| `/help` | Get help |

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `CLAUDE_MODEL` | Default model |
| `MAX_THINKING_TOKENS` | Extended thinking budget |
| `ANTHROPIC_API_KEY` | API authentication |
| `CLAUDE_CONFIG_DIR` | Custom config location |

### CLI Flags

| Flag | Purpose |
|------|---------|
| `-p` / `--print` | Headless/print mode |
| `--json` | JSON output |
| `--continue` | Continue last session |
| `--resume ID` | Resume specific session |
| `--allowedTools` | Restrict to specific tools |
| `--disallowedTools` | Block specific tools |
| `--agents` | Define subagents inline |
| `--mcp-config` | Load MCP config file |

---

## Resources

- [Official Documentation](https://docs.claude.com/en/docs/claude-code/overview)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)
- [Claude Code Templates](https://github.com/davila7/claude-code-templates)
- [ClaudeLog Knowledge Base](https://claudelog.com)

---

*Last updated: December 2025*
