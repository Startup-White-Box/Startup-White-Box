# 🚀 "Vibe Code" Startup-in-a-Box: Comprehensive Implementation Roadmap

**Version:** 2.0 (Zero-Cost Edition)  
**Date:** June 2026  
**Est. Duration:** 16 Weeks (4-Month Launch Cycle)  
**Team Size:** 1–3 builders (solo-friendly)  
**Monthly Burn:** **$0** (infrastructure) — you only pay for your existing LLM API subscription

---

## 📐 Roadmap Philosophy

> **"Every week ships a working slice. Every month delivers a milestone. Every quarter builds a moat — and none of it costs a cent."**

This roadmap follows **progressive disclosure** — each phase adds one major system while keeping all previous systems operational. No "big bang" deployments. No rewrite weeks. No credit card required.

---

## 🗺️ Phase Overview

| Phase | Weeks | Theme | Primary Deliverable | New Tools Added |
|---|---|---|---|---|
| **Phase 0** | W1 | Discovery & Design | Architecture Decision Record (ADR-000) | None |
| **Phase 1** | W2–W3 | Foundation | GitHub Org + Cloudflare Edge Layer | GitHub (Free), Cloudflare (Free) |
| **Phase 2** | W4–W5 | Desktop Core | Local-First AI Workspace | Cherry Studio, Obsidian PM, Shockwave |
| **Phase 3** | W6–W7 | Agent Nervous System | Background Agent Router | PilotDeck |
| **Phase 4** | W8–W9 | Communication Mesh | Multi-Channel Agent Bridge | QwenPaw, Cloudflare Queues |
| **Phase 5** | W10–W11 | Decision Engine | Structured Deliberation System | Agent Roundtable |
| **Phase 6** | W12–W13 | R&D Lab | ML Experiment Pipeline | ML-Master |
| **Phase 7** | W14–W15 | Integration & Polish | Full Stack E2E Demo | All |
| **Phase 8** | W16 | Launch & Open Source | Public Release + Community | All |

---

## Phase 0: Discovery & Design (Week 1)

### Objective
Define the startup's domain, validate the stack fit, and produce the master architecture document.

### Week 1 Milestones

| Day | Task | Deliverable | Owner |
|---|---|---|---|
| Mon | Choose startup domain (e.g., AI dev tools, content automation, data analysis) | Domain brief (1 page) | Founder |
| Tue | Evaluate stack fit against domain needs | Fit analysis matrix | Founder |
| Wed | Draft ADR-000: Master Architecture | `company/decisions/adr-000-master-architecture.md` | Founder |
| Thu | Define GitHub org structure & naming conventions | Org plan document | Founder |
| Fri | Set up local development environment | Working local env | Founder |
| Sat | Set up secret vault (`.env` files + GitHub Secrets) | Credential inventory | Founder |
| Sun | Buffer / documentation | Updated README | Founder |

### ADR-000 Template

```markdown
# ADR-000: Master Architecture — [Startup Name]

## Status
Proposed → Approved

## Context
[Why this stack? Why this domain? Why $0/month?]

## Decision
We will build [X] using the Vibe Code Startup-in-a-Box stack.

## Stack Mapping
| Capability | Tool | Rationale | Cost |
|---|---|---|---|
| Desktop UI | Cherry Studio | MCP-native, multi-provider | $0 (open source) |
| Project Mgmt | Obsidian PM | Git-synced, Markdown-native | $0 (free for personal) |
| Note IDE | Shockwave | GitHub sync, inline agents | $0 (open source) |
| Agent Router | PilotDeck | White-box memory, cost routing | $0 (open source) |
| Channels | QwenPaw | Multi-platform, security guards | $0 (open source) |
| Deliberation | Agent Roundtable | Structured ADR generation | $0 (runs on CF Workers) |
| Orchestration | Cloudflare Queues | Event-driven, auto-retry | $0 (1M ops/mo free) |
| ML/DS | ML-Master | EvoMaster, AutoML | $0 (GitHub Actions + Colab) |
| Edge Infra | Cloudflare | Workers, AI Gateway, R2, D1 | $0 (free tier) |
| Source of Truth | GitHub | Code, docs, decisions, memory | $0 (public repos) |
| LLM APIs | Your existing subscription | All AI/agent tasks | Already paid |

## Consequences
- Positive: Zero infrastructure cost, full audit trail, edge-first
- Negative: Free tier limits require monitoring; public repos mean no private code
- Risk: Cloudflare Workers 10ms CPU limit requires lean proxy design
```

### Exit Criteria
- [ ] ADR-000 merged to `main`
- [ ] GitHub org created with 4 public repos
- [ ] Local dev env verified (Node 20+, Python 3.11+, Wrangler CLI, Git)
- [ ] Cloudflare free account provisioned
- [ ] Budget confirmed: **$0/mo infrastructure**

---

## Phase 1: Foundation (Weeks 2–3)

### Objective
Build the immutable infrastructure layer: GitHub as source of truth and Cloudflare as the global edge — all free tier.

### Week 2: GitHub Org & Repo Architecture

| Day | Task | Deliverable |
|---|---|---|
| Mon | Create GitHub org + 4 **public** repos | `github.com/[org]/company`, `agents`, `memory`, `mcp-registry` |
| Tue | Configure branch protection, CODEOWNERS, PR templates | `.github/` directory in all repos |
| Wed | Set up GitHub Actions starter workflows | `.github/workflows/ci.yml` |
| Thu | Create repo directory structures | `README.md` + folder scaffolding |
| Fri | Configure GitHub Issues templates (ADR, Task, Experiment, Bug) | `.github/ISSUE_TEMPLATE/` |
| Sat | Set up GitHub Discussions for ADR deliberation | Discussions enabled |
| Sun | Document repo conventions | `CONTRIBUTING.md` |

> **Note:** All repos are public. This gives you **unlimited GitHub Actions minutes** on standard runners. The trade-off is no private code — which aligns with the open-core philosophy.

**Repo Structure at End of Week 2:**

```
[org]/
├── company/
│   ├── .github/
│   │   ├── workflows/
│   │   │   ├── ci.yml
│   │   │   └── deploy-pages.yml
│   │   └── ISSUE_TEMPLATE/
│   │       ├── adr.md
│   │       ├── task.md
│   │       └── experiment.md
│   ├── docs/
│   │   └── README.md
│   ├── projects/
│   │   └── README.md
│   ├── specs/
│   │   └── README.md
│   └── experiments/
│       └── README.md
├── agents/
│   ├── .github/
│   │   └── workflows/
│   │       └── deploy.yml
│   ├── pilotdeck/
│   │   └── config.yaml
│   ├── qwenpaw/
│   │   └── personas.yaml
│   ├── roundtable/
│   │   └── rules.yaml
│   ├── queue-topology/
│   │   └── topology.yaml
│   └── mlmaster/
│       └── config.yaml
├── memory/
│   ├── pilotdeck/
│   │   └── README.md
│   ├── conversations/
│   │   └── README.md
│   └── embeddings/
│       └── README.md
└── mcp-registry/
    ├── config.yaml
    ├── schemas/
    └── README.md
```

### Week 3: Cloudflare Edge Layer (Free Tier)

| Day | Task | Deliverable | Tooling |
|---|---|---|---|
| Mon | Install Wrangler CLI, authenticate | `wrangler login` working | Wrangler |
| Tue | Deploy "Hello World" Worker to all subdomains | `health.yourstartup.workers.dev` | Workers |
| Wed | Configure AI Gateway with your existing LLM API | AI Gateway dashboard | Cloudflare AI |
| Thu | Set up R2 buckets (memory, assets, experiments) | 3 R2 buckets (within 10GB free) | R2 |
| Fri | Create D1 database + schema | `startup-db` D1 instance | D1 |
| Sat | Configure KV namespaces + Queues | `agent-state`, `feature-flags`, 3 queues | KV + Queues |
| Sun | Deploy status page to Cloudflare Pages | `status.yourstartup.pages.dev` | Pages |

**D1 Schema (v1):**

```sql
-- D1 Schema: startup-db
CREATE TABLE decisions (
    id TEXT PRIMARY KEY,
    adr_number INTEGER UNIQUE,
    title TEXT NOT NULL,
    status TEXT CHECK(status IN ('proposed','debating','approved','rejected','superseded')),
    context TEXT,
    decision TEXT,
    consequences TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    resolved_at DATETIME,
    github_issue_url TEXT,
    github_commit_sha TEXT
);

CREATE TABLE mcp_calls (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    server TEXT NOT NULL,
    tool TEXT NOT NULL,
    timestamp INTEGER NOT NULL,
    user TEXT,
    duration_ms INTEGER,
    status TEXT,
    request_hash TEXT
);

CREATE TABLE agent_tasks (
    id TEXT PRIMARY KEY,
    agent_name TEXT NOT NULL,
    task_type TEXT,
    status TEXT CHECK(status IN ('pending','running','completed','failed')),
    payload TEXT,
    result TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    completed_at DATETIME,
    queue_message_id TEXT
);

CREATE TABLE experiments (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    hypothesis TEXT,
    status TEXT CHECK(status IN ('draft','running','paused','completed','failed')),
    config TEXT,
    results TEXT,
    github_commit_sha TEXT,
    checkpoint_epoch INTEGER DEFAULT 0,
    platform TEXT CHECK(platform IN ('github-actions','colab','kaggle')),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**Cloudflare Workers Skeleton (Week 3) — Free Tier Compliant:**

> **Design constraint:** Workers free tier allows 10ms CPU per invocation. This Worker is a fast proxy — route, log to D1 (async via `ctx.waitUntil`), forward. No heavy computation.

```typescript
// workers/mcp-registry/src/index.ts
export interface Env {
  DB: D1Database;
  MCP_REGISTRY: Record<string, { endpoint: string; auth: string }>;
  JWT_SECRET: string;
  TASK_QUEUE: Queue;
  RESULT_QUEUE: Queue;
}

export default {
  async fetch(request: Request, env: Env, ctx: ExecutionContext): Promise<Response> {
    const url = new URL(request.url);

    // Health check
    if (url.pathname === '/health') {
      return Response.json({ 
        status: 'ok', 
        version: '2.0.0',
        services: Object.keys(env.MCP_REGISTRY || {}),
        queue: 'cloudflare-queues'
      });
    }

    // MCP routing
    const serverName = url.pathname.split('/')[1];
    if (!serverName) {
      return new Response('MCP Registry v2.0.0. Use /{server}/{tool}', { status: 400 });
    }

    return routeToMCP(serverName, url.pathname, request, env, ctx);
  }
};

async function routeToMCP(
  server: string, 
  path: string, 
  request: Request, 
  env: Env, 
  ctx: ExecutionContext
) {
  const target = env.MCP_REGISTRY[server];
  if (!target) {
    return new Response(`Unknown MCP server: ${server}`, { status: 404 });
  }

  // Audit log to D1 — async, don't block response
  ctx.waitUntil(
    env.DB.prepare(
      'INSERT INTO mcp_calls (server, tool, timestamp, user, status) VALUES (?, ?, ?, ?, ?)'
    ).bind(server, path, Date.now(), getUserId(request), 'routed').run()
  );

  // Proxy to target — this is the hot path, must be fast
  return fetch(target.endpoint + path, {
    method: request.method,
    headers: { ...request.headers, 'Authorization': `Bearer ${target.auth}` },
    body: request.body
  });
}
```

**Cron Triggers Allocation (5 total on free tier):**

```toml
# wrangler.toml
[triggers]
crons = [
  "*/15 * * * *",   # 1. heartbeat: agent health check every 15 min
  "0 * * * *",      # 2. memory-sync: local ↔ R2 sync every hour
  "0 3 * * *",      # 3. cleanup: rotate old D1 records daily at 3 AM
  "0 9 * * 1-5",    # 4. daily-agents: background tasks weekdays 9 AM
  "0 10 * * 1"      # 5. weekly-agents: weekly analytics Monday 10 AM
]
```

### Exit Criteria (End of Phase 1)
- [ ] All 4 GitHub repos have CI/CD passing (unlimited Actions minutes)
- [ ] Cloudflare Workers respond to `health` on all subdomains
- [ ] AI Gateway routes to your existing LLM API
- [ ] D1 database created with schema applied
- [ ] R2 buckets accessible via Workers
- [ ] Queues created: `agent-tasks`, `agent-results`, `system-alerts`
- [ ] Cron triggers configured (5/5 allocated)
- [ ] Status page deployed to Pages
- [ ] ADR-001: "Cloudflare Edge Architecture (Free Tier)" merged

---

## Phase 2: Desktop Core (Weeks 4–5)

### Objective
Establish the local-first workspace where humans interact with the system.

### Week 4: Cherry Studio + Obsidian PM

| Day | Task | Deliverable |
|---|---|---|
| Mon | Install Cherry Studio, configure your LLM API | Working desktop chat |
| Tue | Create custom "Startup Mode" workspace in Cherry Studio | Sidebar with `#engineering`, `#marketing`, `#legal` |
| Wed | Install Obsidian + Obsidian PM plugin | Vault created at `~/startup-vault/` |
| Thu | Design project folder structure in Obsidian | `projects/2026-q3/` with templates |
| Fri | Configure Git sync for Obsidian vault | Auto-commit to `company/projects/` |
| Sat | Test round-trip: Cherry Studio chat → Obsidian task | E2E workflow verified |
| Sun | Document desktop setup | `company/docs/setup/desktop.md` |

**Cherry Studio Custom Workspace Config:**

```yaml
# ~/.cherry-studio/workspaces/startup-mode.yaml
name: "Startup Mode"
sidebar:
  - name: "🎯 Engineering"
    icon: "code"
    default_model: "your-llm-api"       # Your existing subscription
    pinned_topics: ["architecture", "bugs", "features"]
    mcp_servers: ["pilotdeck-memory", "obsidian-tasks", "shockwave-notes"]
  - name: "📢 Marketing"
    icon: "megaphone"
    default_model: "your-llm-api"
    pinned_topics: ["content", "launch", "analytics"]
    mcp_servers: ["qwenpaw-channels", "shockwave-notes"]
  - name: "⚖️ Legal"
    icon: "shield"
    default_model: "your-llm-api"
    pinned_topics: ["privacy", "terms", "compliance"]
    mcp_servers: ["roundtable-deliberate", "obsidian-tasks"]
```

**Obsidian PM Project Template:**

```markdown
---
project: "{{project_name}}"
status: "planning"
start_date: "{{date}}"
target_date: ""
owner: "{{owner}}"
---

# {{project_name}}

## Overview
[1-paragraph description]

## Roadmap
- [ ] Phase 1: Foundation
- [ ] Phase 2: Core
- [ ] Phase 3: Launch

## Tasks
```dataview
TABLE status, priority, due_date, owner
FROM "projects/{{project_name}}/tasks"
SORT priority DESC, due_date ASC
```

## Decisions
```dataview
TABLE status, date
FROM "projects/{{project_name}}/decisions"
SORT date DESC
```

## Links
- Spec: [[specs/{{project_name}}]]
- Experiments: [[experiments/{{project_name}}]]
```

### Week 5: Shockwave + Git Integration

| Day | Task | Deliverable |
|---|---|---|
| Mon | Install Shockwave, configure Git sync | Notes synced to `company/docs/` |
| Tue | Create "Vibe Coding" template in Shockwave | Template: `spec → code → test → PR` |
| Wed | Configure inline agent blocks | Agent can edit its own config |
| Thu | Test agent-generated PR from Shockwave | First AI-generated commit |
| Fri | Build note-to-task bridge (Shockwave → Obsidian PM) | Task auto-created from todo items |
| Sat | Create knowledge linking conventions | `[[wiki-links]]` standard |
| Sun | Document note-taking conventions | `company/docs/conventions/notes.md` |

**Shockwave Vibe Coding Template:**

```markdown
# Vibe Coding Session: {{feature_name}}

## User Request
{{natural_language_description}}

## Spec Draft
<!-- agent:spec-writer -->
[Agent drafts technical spec here]

## Code Draft
<!-- agent:code-writer -->
[Agent drafts implementation here]

## Test Plan
<!-- agent:test-writer -->
[Agent drafts tests here]

## PR Checklist
- [ ] Spec reviewed
- [ ] Code passes lint
- [ ] Tests pass
- [ ] Documentation updated

## Metadata
- agent_session: {{session_id}}
- parent_note: {{parent}}
- github_branch: {{branch}}
```

### Exit Criteria (End of Phase 2)
- [ ] Cherry Studio connects to local MCP servers
- [ ] Obsidian PM vault auto-syncs to GitHub within 5 minutes
- [ ] Shockwave generates a commit to `company/docs/` via agent
- [ ] Cross-linking works: Shockwave note → Obsidian task → GitHub issue
- [ ] ADR-002: "Desktop-First Workspace" merged

---

## Phase 3: Agent Nervous System (Weeks 6–7)

### Objective
Deploy PilotDeck as the background brain: routing, memory, and cost optimization.

### Week 6: PilotDeck Core

| Day | Task | Deliverable |
|---|---|---|
| Mon | Install PilotDeck, configure workspace isolation | 3 workspaces: `engineering`, `marketing`, `ops` |
| Tue | Set up white-box memory | Memory entries in `memory/pilotdeck/` |
| Wed | Configure smart routing (flagship vs lightweight via your LLM) | Routing rules YAML |
| Thu | Enable background execution | `systemd` service or `nohup` |
| Fri | Connect PilotDeck to Cherry Studio via MCP | Chat → PilotDeck → Model |
| Sat | Test memory traceability | Edit memory, verify downstream update |
| Sun | Document memory conventions | `company/docs/conventions/memory.md` |

**PilotDeck Configuration:**

```yaml
# agents/pilotdeck/config.yaml
version: "2.0"
workspaces:
  engineering:
    default_model: "your-llm-api:flagship"    # Your existing subscription
    fallback_model: "your-llm-api:lightweight"
    memory_path: "../../memory/pilotdeck/engineering/"
    background_agents:
      - name: "spec-writer"
        model: "your-llm-api:lightweight"
        schedule: "on-demand"
      - name: "code-reviewer"
        model: "your-llm-api:flagship"
        schedule: "on-file-change"
      - name: "dependency-checker"
        model: "your-llm-api:flagship"
        schedule: "hourly"
  marketing:
    default_model: "your-llm-api:flagship"
    fallback_model: "your-llm-api:lightweight"
    memory_path: "../../memory/pilotdeck/marketing/"
    background_agents:
      - name: "content-generator"
        model: "your-llm-api:lightweight"
        schedule: "daily-9am"
      - name: "trend-analyzer"
        model: "your-llm-api:flagship"
        schedule: "weekly-monday"

routing:
  rules:
    - condition: "task.complexity > 0.8"
      model: "flagship"
    - condition: "task.type == 'summarize'"
      model: "lightweight"
    - condition: "task.type == 'code-review'"
      model: "flagship"
      max_tokens: 4000

memory:
  backend: "git"
  path: "../../memory/pilotdeck/"
  traceability: true
  auto_commit: true
  commit_message_template: "[memory] {agent_name}: {action} {entry_id}"
```

### Week 7: PilotDeck + Cloudflare Integration

| Day | Task | Deliverable |
|---|---|---|
| Mon | Deploy PilotDeck lightweight router to Cloudflare Worker | `pilotdeck.yourstartup.workers.dev` |
| Tue | Connect PilotDeck to AI Gateway | Cost tracking per workspace |
| Wed | Set up background agent scheduling via Cron | Cron trigger allocated (1 of 5) |
| Thu | Build memory sync: local ↔ Cloudflare R2 | Bidirectional sync |
| Fri | Implement usage alerts (per workspace) | Slack/Discord notification on threshold |
| Sat | Stress test: 50 concurrent agent tasks | Performance baseline |
| Sun | Document agent operations | `company/docs/ops/agents.md` |

**Cloudflare Worker: PilotDeck Router (Free Tier Compliant)**

```typescript
// workers/pilotdeck-router/src/index.ts
export interface Env {
  AI_GATEWAY: string;
  R2_MEMORY: R2Bucket;
  DB: D1Database;
  PILOTDECK_SECRET: string;
}

export default {
  async fetch(request: Request, env: Env, ctx: ExecutionContext): Promise<Response> {
    const url = new URL(request.url);

    if (url.pathname === '/route') {
      const body = await request.json();
      const { task, workspace } = body;

      // Smart routing logic
      const model = selectModel(task);

      // Log to D1 — async
      ctx.waitUntil(
        env.DB.prepare(
          'INSERT INTO agent_tasks (id, agent_name, task_type, status, payload) VALUES (?, ?, ?, ?, ?)'
        ).bind(crypto.randomUUID(), 'pilotdeck-router', task.type, 'routing', JSON.stringify(task)).run()
      );

      // Route to AI Gateway → your LLM API
      const response = await fetch(env.AI_GATEWAY, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ model, messages: task.messages })
      });

      return response;
    }

    return new Response('PilotDeck Router v2.0.0', { status: 200 });
  },

  // Cron: heartbeat + memory sync
  async scheduled(event: ScheduledEvent, env: Env, ctx: ExecutionContext): Promise<void> {
    if (event.cron === '*/15 * * * *') {
      // Heartbeat check
      ctx.waitUntil(checkAgentHealth(env));
    }
    if (event.cron === '0 * * * *') {
      // Memory sync
      ctx.waitUntil(syncMemory(env));
    }
  }
};

function selectModel(task: any): string {
  if (task.complexity > 0.8 || task.type === 'code-review') return 'flagship';
  if (task.type === 'summarize') return 'lightweight';
  return 'flagship';
}
```

### Exit Criteria (End of Phase 3)
- [ ] PilotDeck routes tasks to correct models based on complexity
- [ ] Background agents run on schedule via Cron triggers
- [ ] Memory edits traceable to specific Git commits
- [ ] Cost per workspace visible in dashboard
- [ ] ADR-003: "Agent Routing & Memory Architecture" merged

---

## Phase 4: Communication Mesh (Weeks 8–9)

### Objective
Connect the system to the outside world via QwenPaw and enable event-driven orchestration via Cloudflare Queues (replaces Solace).

### Week 8: QwenPaw Multi-Channel Bridge

| Day | Task | Deliverable |
|---|---|---|
| Mon | Deploy QwenPaw core to Cloudflare Worker | `qwenpaw.yourstartup.workers.dev` |
| Tue | Configure Discord bot integration | Bot online in test server |
| Wed | Configure Slack app integration | App installed in workspace |
| Thu | Set up security guards (tool-guard, file-guard) | Guard rules active |
| Fri | Map channels to PilotDeck workspaces | `#engineering` → `engineering` workspace |
| Sat | Test E2E: Discord message → Agent → GitHub commit | Full pipeline working |
| Sun | Document channel conventions | `company/docs/conventions/channels.md` |

**QwenPaw Configuration:**

```yaml
# agents/qwenpaw/config.yaml
version: "2.0"
channels:
  discord:
    enabled: true
    bot_token: "${DISCORD_BOT_TOKEN}"
    guild_id: "${DISCORD_GUILD_ID}"
    command_prefix: "!"
    allowed_channels: ["engineering", "marketing", "general"]
  slack:
    enabled: true
    app_token: "${SLACK_APP_TOKEN}"
    bot_token: "${SLACK_BOT_TOKEN}"
    allowed_channels: ["#engineering", "#marketing"]

security:
  tool_guard: true
  file_access_guard: true
  rate_limit: 100  # requests per hour per user
  allowed_tools:
    - "obsidian-tasks"
    - "shockwave-notes"
    - "pilotdeck-memory"

workspaces:
  engineering:
    channels: ["discord:engineering", "slack:#engineering"]
    default_agent: "tech-lead"
  marketing:
    channels: ["discord:marketing", "slack:#marketing"]
    default_agent: "content-strategist"

heartbeat:
  enabled: true
  via: "cloudflare-cron"    # Uses 1 of 5 cron triggers
  interval: 900  # 15 minutes
  tasks:
    - name: "daily-standup-reminder"
      schedule: "0 9 * * 1-5"
      channel: "slack:#engineering"
    - name: "weekly-analytics"
      schedule: "0 10 * * 1"
      channel: "discord:marketing"
```

### Week 9: Cloudflare Queues (Event Orchestration)

> **Why Cloudflare Queues instead of Solace:** Same event-driven pattern with automatic retry, but already free in your Cloudflare account. One less vendor, one less account, same functionality.

| Day | Task | Deliverable |
|---|---|---|
| Mon | Create 3 Cloudflare Queues: `agent-tasks`, `agent-results`, `system-alerts` | Queues active |
| Tue | Define message schemas for each queue | Schema docs |
| Wed | Implement agent-to-agent messaging via Queues | `agent-tasks` publish/subscribe working |
| Thu | Build workflow state management in D1 | Long-running workflow survives crash |
| Fri | Implement consumer Worker with retry logic | Auto-retry up to 3 times on failure |
| Sat | Test failure recovery: kill agent mid-task | Queue redelivers to new instance |
| Sun | Document event architecture | `company/docs/architecture/events.md` |

**Queue Message Schemas:**

```typescript
// Queue message types
interface AgentTaskMessage {
  id: string;
  agent: string;
  task: string;
  payload: Record<string, unknown>;
  workflow_id?: string;
  step_number?: number;
  timestamp: number;
  retry_count: number;
}

interface AgentResultMessage {
  original_task_id: string;
  agent: string;
  status: 'completed' | 'partial' | 'failed';
  result: Record<string, unknown>;
  timestamp: number;
}

interface SystemAlertMessage {
  level: 'critical' | 'warning' | 'info';
  source: string;
  message: string;
  context?: Record<string, unknown>;
  timestamp: number;
}
```

**Queue Consumer Worker:**

```typescript
// workers/agent-orchestrator/src/index.ts
export interface Env {
  TASK_QUEUE: Queue;
  RESULT_QUEUE: Queue;
  ALERT_QUEUE: Queue;
  DB: D1Database;
}

export default {
  // HTTP: enqueue tasks
  async fetch(request: Request, env: Env): Promise<Response> {
    const { agent, task, payload, workflow_id } = await request.json();
    
    const message: AgentTaskMessage = {
      id: crypto.randomUUID(),
      agent,
      task,
      payload,
      workflow_id,
      timestamp: Date.now(),
      retry_count: 0
    };

    await env.TASK_QUEUE.send(message);
    return Response.json({ status: 'queued', task_id: message.id });
  },

  // Queue consumer: process agent tasks
  async queue(batch: MessageBatch<AgentTaskMessage>, env: Env, ctx: ExecutionContext): Promise<void> {
    for (const msg of batch.messages) {
      try {
        const result = await executeAgentTask(msg.body, env);
        
        // Store result in D1 (async)
        ctx.waitUntil(
          env.DB.prepare(
            'UPDATE agent_tasks SET status = ?, result = ?, completed_at = ? WHERE id = ?'
          ).bind('completed', JSON.stringify(result), Date.now(), msg.body.id).run()
        );

        // Publish result event
        await env.RESULT_QUEUE.send({
          original_task_id: msg.body.id,
          agent: msg.body.agent,
          status: 'completed',
          result,
          timestamp: Date.now()
        });
        
        msg.ack();
      } catch (err) {
        // Queue will retry up to max_retries (3) with exponential backoff
        ctx.waitUntil(
          env.DB.prepare(
            'UPDATE agent_tasks SET status = ? WHERE id = ?'
          ).bind('failed', msg.body.id).run()
        );
        msg.retry();
      }
    }
  }
};
```

### Exit Criteria (End of Phase 4)
- [ ] QwenPaw responds in Discord and Slack
- [ ] Channel messages trigger PilotDeck agents
- [ ] 3 Cloudflare Queues defined with message schemas
- [ ] Workflow survives agent crash and resumes (auto-retry)
- [ ] ADR-004: "Event-Driven Communication via Cloudflare Queues" merged

---

## Phase 5: Decision Engine (Weeks 10–11)

### Objective
Implement structured deliberation for high-stakes decisions using Agent Roundtable.

### Week 10: Agent Roundtable Core

| Day | Task | Deliverable |
|---|---|---|
| Mon | Deploy Roundtable API to Cloudflare Worker | `roundtable.yourstartup.workers.dev` |
| Tue | Define persona templates (Security, Cost, UX, Legal) | 4 persona YAML files |
| Wed | Implement debate rounds (opening, rebuttal, closing) | Round logic working |
| Thu | Build consensus scoring algorithm | Vote tallying system |
| Fri | Create ADR generation from roundtable output | Markdown ADR auto-generated |
| Sat | Test: mock architectural decision | Full roundtable completes in <10 min |
| Sun | Document deliberation rules | `company/docs/conventions/decisions.md` |

**Agent Roundtable Persona Template:**

```yaml
# agents/roundtable/personas/security.yaml
name: "Security Agent"
role: "security_reviewer"
model: "your-llm-api:flagship"
prompt_template: |
  You are a pragmatic security engineer. Your job is to evaluate proposals
  from a security and privacy perspective. You care about:
  - Data encryption (at rest and in transit)
  - Access control and least privilege
  - Compliance (GDPR, SOC2)
  - Supply chain security

  When evaluating a proposal:
  1. Identify 2-3 specific security risks
  2. Suggest concrete mitigations
  3. Rate the proposal: SAFE / CONDITIONAL / UNSAFE

  Be specific. Reference standards when possible.

  Current proposal:
  {proposal}

  Your evaluation:

# agents/roundtable/personas/cost.yaml
name: "Cost Agent"
role: "cost_analyst"
model: "your-llm-api:lightweight"
prompt_template: |
  You are a cost-conscious infrastructure analyst. We run on $0/mo infrastructure.
  Evaluate proposals based on:
  - Will this push us beyond Cloudflare free tier limits?
  - Will this require paid services we currently don't use?
  - Operational complexity (maintenance hours)
  - Risk of hitting free tier quotas

  Keep us at $0/mo. If a proposal threatens that, say so.

  Current proposal:
  {proposal}

  Your evaluation:
```

**Roundtable API Contract:**

```typescript
// POST /roundtable/deliberate
interface DeliberateRequest {
  proposal: string;
  proposal_id: string;
  required_personas: string[];  // ["security", "cost", "ux"]
  consensus_threshold: number;    // 0.75 = 3/4 approval
  max_rounds: number;             // 3
}

interface DeliberateResponse {
  roundtable_id: string;
  status: "debating" | "consensus" | "deadlock";
  rounds: Round[];
  final_score: number;
  adr_draft: string;
  dissenting_opinions: Opinion[];
}

interface Round {
  round_number: number;
  persona_responses: Record<string, string>;
  consensus_score: number;
}
```

### Week 11: Roundtable Integration

| Day | Task | Deliverable |
|---|---|---|
| Mon | Trigger roundtable from GitHub Issues | Issue label `needs-adr` → roundtable |
| Tue | Post roundtable results to QwenPaw channels | Discord/Slack notification |
| Wed | Store all deliberations in D1 | Queryable decision history |
| Thu | Build "ADR Explorer" UI on Cloudflare Pages | Browse past decisions |
| Fri | Implement superseded ADR workflow | ADR-005 supersedes ADR-003 |
| Sat | Test deadlock resolution (tie-breaker human) | Human-in-the-loop fallback |
| Sun | Document decision lifecycle | `company/docs/ops/decisions.md` |

**GitHub Issue → Roundtable Trigger (Unlimited Actions Minutes):**

```yaml
# .github/workflows/roundtable-trigger.yml
name: Trigger Agent Roundtable

on:
  issues:
    types: [labeled]

jobs:
  roundtable:
    if: github.event.label.name == 'needs-adr'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Extract proposal from issue
        id: extract
        run: |
          echo "proposal=$(jq -r '.issue.body' $GITHUB_EVENT_PATH)" >> $GITHUB_OUTPUT
          echo "proposal_id=ADR-$(jq -r '.issue.number' $GITHUB_EVENT_PATH)" >> $GITHUB_OUTPUT

      - name: Call Roundtable API
        run: |
          curl -X POST https://roundtable.yourstartup.workers.dev/deliberate \
            -H "Authorization: Bearer ${{ secrets.ROUNDTABLE_KEY }}" \
            -H "Content-Type: application/json" \
            -d '{
              "proposal": ${{ steps.extract.outputs.proposal }},
              "proposal_id": "${{ steps.extract.outputs.proposal_id }}",
              "required_personas": ["security", "cost", "ux"],
              "consensus_threshold": 0.75,
              "max_rounds": 3
            }'

      - name: Comment on issue with roundtable link
        run: |
          gh issue comment ${{ github.event.issue.number }} \
            --body "🤖 Agent Roundtable initiated. Track progress: https://roundtable.yourstartup.workers.dev/ADR-${{ github.event.issue.number }}"
```

### Exit Criteria (End of Phase 5)
- [ ] Roundtable completes 3-persona debate in <10 minutes
- [ ] ADR auto-generated and committed to GitHub
- [ ] GitHub Issue label `needs-adr` triggers roundtable
- [ ] Results posted to Discord/Slack
- [ ] ADR Explorer UI browsable on Pages
- [ ] ADR-005: "Structured Deliberation Process" merged

---

## Phase 6: R&D Lab (Weeks 12–13)

### Objective
Deploy ML-Master as the experimentation engine — running on free compute (GitHub Actions for CPU, Colab/Kaggle for GPU).

### Week 12: ML-Master Core

| Day | Task | Deliverable |
|---|---|---|
| Mon | Deploy ML-Master EvoMaster framework | Checkpoint-aware experiment runner |
| Tue | Configure GitHub Actions experiment pipeline | `ml-pipeline.yml` with 6h checkpoint |
| Wed | Set up Colab GPU integration (notebook generator) | Auto-generated Colab notebooks |
| Thu | Implement experiment result storage | Results → R2 + GitHub commit |
| Fri | Create experiment proposal template | `company/docs/templates/experiment.md` |
| Sat | Run first CPU experiment on GitHub Actions | First experiment completed |
| Sun | Document ML ops | `company/docs/ops/ml.md` |

**ML-Master Configuration:**

```yaml
# agents/mlmaster/config.yaml
version: "2.0"
framework: "evomaster"

compute:
  draft:
    provider: "github-actions"        # Free, unlimited minutes on public repo
    model: "your-llm-api:lightweight"
    max_runtime_minutes: 350          # 5h50m (buffer before 6h kill)
    checkpoint: true
    auto_resume: true
  debug:
    provider: "github-actions"
    model: "your-llm-api:flagship"
    max_runtime_minutes: 350
    checkpoint: true
    auto_resume: true
  improve:
    provider: "colab-free"             # Google Colab T4 (free)
    instance: "colab-t4-16gb"
    max_runtime_minutes: 600           # Well within 12h session
    checkpoint: true

storage:
  results: "r2://experiments/"         # Within 10GB free tier
  logs: "r2://logs/"
  git:
    repo: "company"
    path: "experiments/"
    auto_commit: true

experiments:
  default_metrics:
    - "accuracy"
    - "latency"
    - "cost"                           # Track against $0 budget
  default_budget: 0.00                 # $0 — we don't pay for compute
```

**GitHub Actions ML Pipeline (Unlimited Minutes, 6h Job Limit):**

```yaml
# .github/workflows/ml-pipeline.yml
name: ML Experiment Pipeline

on:
  workflow_dispatch:
    inputs:
      experiment_id:
        description: 'Experiment ID'
        required: true
  issues:
    types: [labeled]

jobs:
  experiment:
    if: github.event.label.name == 'experiment' || github.event_name == 'workflow_dispatch'
    runs-on: ubuntu-latest
    timeout-minutes: 350  # 5h50m — buffer before GitHub's 6h kill
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Cache dependencies
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('experiments/requirements.txt') }}

      - name: Restore checkpoint
        uses: actions/cache@v4
        with:
          path: experiments/checkpoint/
          key: exp-${{ github.event.inputs.experiment_id || github.event.issue.number }}

      - name: Install dependencies
        run: pip install -r experiments/requirements.txt

      - name: Run experiment (checkpoint-aware)
        run: |
          python experiments/run.py \
            --id "${{ github.event.inputs.experiment_id || github.event.issue.number }}" \
            --checkpoint-dir experiments/checkpoint/ \
            --max-runtime-minutes 330 \
            --auto-resume

      - name: Commit results
        run: |
          git config user.name "ml-master[bot]"
          git config user.email "ml-master[bot]@users.noreply.github.com"
          git add experiments/
          git commit -m "[ml-master] Experiment results" || true
          git push
```

### Week 13: ML-Master Integration

| Day | Task | Deliverable |
|---|---|---|
| Mon | Connect ML-Master to Cloudflare Queues | Experiment events published to `agent-results` |
| Tue | Trigger experiments from Obsidian PM tasks | Task label `experiment` → ML-Master |
| Wed | Display experiment results in Cherry Studio | Custom panel showing live experiments |
| Thu | Build experiment comparison dashboard | Compare 2+ experiments on Pages |
| Fri | Implement free-tier usage alerts | Notify when approaching R2/Queues limits |
| Sat | Test parallel experiments | 3 experiments on Actions (max 20 concurrent) |
| Sun | Document experiment lifecycle | `company/docs/conventions/experiments.md` |

**Free GPU Budget:**

| Source | GPU | Hours Available | Use For |
|---|---|---|---|
| Google Colab Free | T4 16GB | ~12h/day = 84h/week | GPU-bound experiments |
| Kaggle Notebooks | 2× T4 | 30h/week | Supplementary GPU |
| GitHub Actions | CPU only | Unlimited | CPU-bound experiments |
| **Total** | | **114 GPU-hrs + unlimited CPU** | |

### Exit Criteria (End of Phase 6)
- [ ] ML-Master runs 3-stage experiment pipeline on GitHub Actions
- [ ] Checkpoint + resume works within 6h job limit
- [ ] Colab notebooks auto-generated for GPU experiments
- [ ] Results auto-commit to `company/experiments/`
- [ ] ADR-006: "ML Experimentation on Free Compute" merged

---

## Phase 7: Integration & Polish (Weeks 14–15)

### Objective
Wire all systems together and build the unified dashboard.

### Week 14: Full Stack E2E

| Day | Task | Deliverable |
|---|---|---|
| Mon | Build unified MCP health dashboard | All 7 MCP servers green/red status |
| Tue | Implement cross-system alerting via Queues | One failure → `system-alerts` → notify all channels |
| Wed | Create "Day in the Life" demo script | 5-minute demo video script |
| Thu | Record demo: Feature request → Launch | Working E2E recording |
| Fri | Stress test: 100 concurrent agent tasks | Performance report (max 20 on Actions, rest on Workers) |
| Sat | Security audit: review all guards | Security checklist signed off |
| Sun | Bug bash: fix integration edge cases | Zero critical bugs |

**Unified Dashboard (Cloudflare Pages — Free):**

```html
<!-- pages/dashboard/index.html -->
<!DOCTYPE html>
<html>
<head><title>Startup-in-a-Box Dashboard</title></head>
<body>
  <h1>🚀 System Health</h1>
  <h2>💰 Monthly Cost: $0</h2>
  <div id="mcp-health">
    <!-- Fetches from /health on all MCP servers -->
  </div>

  <h2>📊 Free Tier Usage</h2>
  <div id="usage">
    <div>Workers: <span id="workers-usage">—</span> / 100K req/day</div>
    <div>D1: <span id="d1-usage">—</span> / 5M reads/day</div>
    <div>R2: <span id="r2-usage">—</span> / 10GB</div>
    <div>Queues: <span id="queue-usage">—</span> / 1M ops/mo</div>
  </div>

  <h2>📋 Active Decisions</h2>
  <div id="active-adrs">
    <!-- Fetches from D1 decisions table -->
  </div>

  <h2>🔬 Running Experiments</h2>
  <div id="experiments">
    <!-- Fetches from D1 experiments table -->
  </div>

  <script>
    async function loadHealth() {
      const servers = ['pilotdeck', 'qwenpaw', 'roundtable', 'queues', 'mlmaster', 'shockwave', 'obsidian'];
      for (const server of servers) {
        try {
          const res = await fetch(`https://mcp.yourstartup.workers.dev/${server}/health`);
          const status = res.ok ? '🟢' : '🔴';
          document.getElementById('mcp-health').innerHTML += `<div>${status} ${server}</div>`;
        } catch {
          document.getElementById('mcp-health').innerHTML += `<div>🔴 ${server} (unreachable)</div>`;
        }
      }
    }
    loadHealth();
  </script>
</body>
</html>
```

### Week 15: Polish & Documentation

| Day | Task | Deliverable |
|---|---|---|
| Mon | Write public README for GitHub org | `README.md` with architecture diagram |
| Tue | Create setup script for new contributors | `scripts/setup.sh` (one-command install) |
| Wed | Write API documentation for all MCP servers | `mcp-registry/docs/` |
| Thu | Build free-tier usage monitoring | Dashboard shows % of free tier consumed |
| Fri | Create troubleshooting runbook | `company/docs/ops/runbook.md` |
| Sat | Final security review | All secrets in GitHub Secrets + `.env` |
| Sun | Prepare launch checklist | Launch readiness document |

### Exit Criteria (End of Phase 7)
- [ ] All 7 MCP servers report healthy
- [ ] E2E demo completes in <5 minutes
- [ ] Dashboard shows real-time free-tier usage
- [ ] Setup script works on fresh machine (Mac + Linux)
- [ ] Zero critical bugs
- [ ] ADR-007: "Production Readiness at $0/mo" merged

---

## Phase 8: Launch & Open Source (Week 16)

### Objective
Public release, community onboarding, and continuous improvement.

### Week 16: Launch

| Day | Task | Deliverable |
|---|---|---|
| Mon | Publish blog post: "The $0/Month Startup Stack" | Blog post live |
| Tue | Open-source MCP registry config | `mcp-registry` repo public (already is) |
| Wed | Publish setup tutorial video | YouTube/Twitter video |
| Thu | Launch on Hacker News / Product Hunt | Launch post live |
| Fri | Monitor community feedback | Feedback log in GitHub Discussions |
| Sat | First community PR review | Merge external contribution |
| Sun | Retrospective: what worked, what didn't | Retrospective ADR |

**Launch Blog Post Outline:**

```markdown
# We Built a Startup on $0/Month Infrastructure

## The Stack
[Diagram showing all tools — all free]

## The Cost
$0/mo infrastructure vs $2,000/mo traditional stack

## The Philosophy
- Every decision is a commit
- Every agent has an audit trail
- Every feature starts with a conversation
- Every experiment checkpoints before the 6-hour bell

## The Compute
- GitHub Actions: unlimited minutes (public repos)
- Cloudflare Workers: 100K req/day (free)
- Google Colab: T4 GPU 12h/day (free)
- Kaggle: 2× T4 30h/week (free)

## The Demo
[5-minute video showing: Discord message → Agent deliberation → GitHub PR → Deploy]

## Get Started
```bash
git clone https://github.com/yourorg/startup-in-a-box
cd startup-in-a-box
./scripts/setup.sh
```

## What's Next
We're opening our MCP registry and experiment logs. 
Build your own vibe-coded startup — for free.
```

### Exit Criteria (End of Phase 8)
- [ ] Blog post published
- [ ] MCP registry open-sourced (already public)
- [ ] Tutorial video published
- [ ] 100+ GitHub stars on main repo
- [ ] First community contribution merged
- [ ] ADR-008: "Launch Retrospective" merged

---

## 📊 Cost Tracking by Phase

| Phase | Cloudflare | GitHub | GPU | LLM API | **Total Infra** |
|---|---|---|---|---|---|
| 0–1 (Foundation) | $0 | $0 | $0 | Already paid | **$0** |
| 2–3 (Desktop + Agents) | $0 | $0 | $0 | Already paid | **$0** |
| 4–5 (Comms + Decisions) | $0 | $0 | $0 | Already paid | **$0** |
| 6–7 (ML + Integration) | $0 | $0 | $0 | Already paid | **$0** |
| 8 (Launch) | $0 | $0 | $0 | Already paid | **$0** |
| **Steady State** | **$0** | **$0** | **$0** | Already paid | **$0/mo** |

---

## 🎯 Key Performance Indicators (KPIs)

| Metric | Target | Measurement |
|---|---|---|
| **Time to Decision** | < 24 hours | GitHub Issue created → ADR merged |
| **Agent Uptime** | > 99% | Cron heartbeat monitoring |
| **Cost per Task** | $0 (infra only) | Cloudflare dashboard |
| **Memory Traceability** | 100% | Every memory entry has Git commit SHA |
| **E2E Feature Delivery** | < 48 hours | Spec approved → Production deploy |
| **Community Contributions** | 5/month | GitHub PRs from external users |
| **Free Tier Headroom** | > 50% remaining | Dashboard monitoring |

---

## 🛡️ Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Cloudflare free tier exceeded | Medium | Medium | Dashboard alerts at 50%, 75%, 90% |
| GitHub Actions 6h job kill | Low | Low | Checkpoint + resume pattern |
| GitHub Actions 20 concurrent limit | Low | Low | Stagger experiments; prioritize |
| R2 10GB storage limit | Low | Medium | Auto-cleanup via Cron; compress old data |
| Agent hallucination | High | Medium | Roundtable review + human gate |
| Cloudflare Workers 10ms CPU limit | Low | Medium | Keep Workers as fast proxies; heavy work on Actions |
| Colab session timeout | Medium | Low | Design experiments to complete in <12h |
| Public repo exposure | Low | Low | This is open-core — no secrets in code |

---

## 📋 Free Tier Quick Reference

| Service | Free Allowance | Hard Limit |
|---|---|---|
| **Cloudflare Workers** | 100K req/day | 10ms CPU/invocation |
| **Cloudflare D1** | 5M reads + 100K writes/day | 10GB storage |
| **Cloudflare R2** | 10GB + 1M ops/mo | 10GB storage |
| **Cloudflare KV** | 100K reads/day | 1GB storage |
| **Cloudflare Pages** | Unlimited | 500 builds/mo |
| **Cloudflare Queues** | 1M ops/mo | 1M ops/mo |
| **Cloudflare Cron** | 5 triggers | 5 triggers total |
| **GitHub Actions** | Unlimited minutes (public) | 6h/job, 72h/workflow, 20 concurrent |
| **Google Colab** | T4 16GB | ~12h session |
| **Kaggle** | 2× T4 | 30h/week |
| **Your LLM API** | Per your plan | Per your plan |

---

## 🚀 Quick Start: Today

If you want to start **right now**, do these 5 things in the next 2 hours:

1. **Create GitHub org** → 4 public repos with READMEs (unlimited Actions minutes)
2. **Sign up Cloudflare Free** → Deploy hello Worker + create D1 + R2
3. **Install Cherry Studio** → Point to your existing LLM API
4. **Install Obsidian + PM plugin** → Create vault + Git init
5. **Write ADR-000** → 1-page architecture decision

That's it. You're in Phase 1. **Total spent: $0.**

---

*This roadmap is a living document. Update ADRs as the stack evolves. The goal is not perfection — it's shipping a working slice every week, for free.*

**Next step:** Pick Phase 0 or Phase 1, and generate the exact starter code for that phase.
