# "Vibe Code" Startup-in-a-Box: Deep-Dive Blueprint

This is a fully open-core, AI-native startup stack where **GitHub is your single source of truth**, **Cloudflare is your global edge layer**, and **every tool speaks MCP** so they're interchangeable. Below is the complete architecture, integration patterns, and a 30-day launch roadmap.

---

## 1. The Philosophy

| Principle | Implementation |
|---|---|
| **Plaintext over databases** | Every decision, memory, and task lives as Markdown in Git |
| **Edge-first compute** | Heavy AI runs where data lives; lightweight agents run on Cloudflare Workers |
| **MCP-native** | Every tool exposes/uses Model Context Protocol; no vendor lock-in |
| **White-box memory** | Every AI output is traceable to a specific Git commit |
| **Agent redundancy** | If one agent fails, another picks up from the event stream |
| **Zero-cost infrastructure** | Every service runs on a free tier designed for startup-scale workloads |

---

## 2. Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    CLOUDFLARE EDGE LAYER (FREE TIER)         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Workers    │  │  AI Gateway │  │  R2 (Object Store)  │ │
│  │  (Agents)   │  │  (Rate/Auth)│  │  (Memory/Assets)    │ │
│  │  100K/day   │  │  (free)     │  │  10GB free          │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  D1 (SQL)   │  │  KV (Cache) │  │  Pages (Frontend)   │ │
│  │  5M read/d  │  │  (State)    │  │  (Status/Docs)      │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐                           │
│  │  Queues     │  │  Cron (×5)  │                           │
│  │  1M ops/mo  │  │  (Sched.)   │                           │
│  └─────────────┘  └─────────────┘                           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      GITHUB (Source of Truth)                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Repo:      │  │  Repo:      │  │  Repo:              │ │
│  │  /company   │  │  /agents    │  │  /memory            │ │
│  │  (Code)     │  │  (MCP Svcs) │  │  (Knowledge)        │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Actions    │  │  Issues     │  │  Discussions        │ │
│  │  (CI/CD)    │  │  (Tasks)    │  │  (ADRs)             │ │
│  │  UNLIMITED  │  │             │  │                     │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│  Cherry      │    │  Shockwave   │    │  Obsidian PM │
│  Studio      │    │  (Notes IDE) │    │  (Projects)  │
│  (Desktop UI)│    │  (Free)      │    │  (Free)      │
└──────────────┘    └──────────────┘    └──────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    AGENT ORCHESTRATION LAYER                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  PilotDeck  │  │  QwenPaw    │  │  Agent Roundtable   │ │
│  │  (Router)   │  │  (Channels) │  │  (Decisions)        │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Cloudflare │  │  ML-Master  │  │  (Custom Agents)    │ │
│  │  Queues     │  │  (ML/DS)    │  │                     │ │
│  │  (Events)   │  │             │  │                     │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    FREE GPU COMPUTE                          │
│  ┌─────────────┐  ┌─────────────┐                           │
│  │  Colab      │  │  Kaggle     │                           │
│  │  T4 12h/day │  │  2×T4 30h/w │                           │
│  └─────────────┘  └─────────────┘                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Repo-by-Repo Role Mapping

### 🧠 PilotDeck → The "Nervous System"

- **Role:** Smart router, background execution, white-box memory
- **Where it lives:** Local desktop + Cloudflare Worker (lightweight router)
- **Key config:**
  - Flagship model for "CEO Agent" (strategic decisions)
  - Lightweight model for "Scribe Agent" (note-taking)
  - Background execution runs 24/7 via `nohup` or systemd
- **MCP exposure:** Exposes `pilotdeck-memory` and `pilotdeck-router` MCP servers
- **Git integration:** Every memory edit is a commit to `/memory` repo with traceable ID

### 💬 Cherry Studio → The "Front Door"

- **Role:** Universal chat UI for humans to talk to the system
- **Where it lives:** Desktop app (Mac/Windows/Linux)
- **Key config:**
  - Connects to local PilotDeck via MCP
  - Connects to Cloudflare AI Gateway for remote models
  - Custom "Startup Mode" sidebar with pinned workspaces: `#engineering`, `#marketing`, `#legal`
- **MCP exposure:** Consumes all other MCP servers; acts as the unified client

### 📝 Shockwave → The "Creative Engine"

- **Role:** AI-native note IDE where agents write code, docs, and specs
- **Where it lives:** Desktop + GitHub sync
- **Key config:**
  - Git sync pushes to `/company` repo under `/docs/` and `/specs/`
  - Inline agent blocks: agents can edit their own configuration files as notes
  - "Vibe coding" mode: describe a feature in natural language, agent drafts the PR
- **MCP exposure:** Exposes `shockwave-notes` MCP server so other agents can read/write

### 📋 Obsidian PM → The "Mission Control"

- **Role:** Project management via Markdown (Gantt, Kanban, dependencies)
- **Where it lives:** Obsidian vault synced to GitHub
- **Key config:**
  - Vault path: `~/startup-vault/` → synced to `/company/projects/`
  - Each project is a folder with `README.md`, `roadmap.md`, `tasks/`
  - Agents parse task files to understand priorities and deadlines
- **MCP exposure:** Exposes `obsidian-tasks` MCP server for agents to query deadlines

### 🤖 QwenPaw → The "Embassy"

- **Role:** Multi-channel bridge (Discord, Slack, Feishu, iMessage, QQ)
- **Where it lives:** Cloudflare Worker (always-on) + local daemon
- **Key config:**
  - Each channel maps to a workspace in PilotDeck
  - Security guards enabled: tool-guard, file-access-guard, rate-limiting
  - "Heartbeat" checks via Cloudflare Cron (1 of 5 cron triggers allocated)
- **MCP exposure:** Exposes `qwenpaw-channels` MCP server; other agents can send messages

### 🗣️ Agent Roundtable → The "Senate"

- **Role:** Structured deliberation for architectural decisions
- **Where it lives:** Cloudflare Worker (stateless) + D1 (state storage)
- **Key config:**
  - ADR (Architecture Decision Record) template in Markdown
  - Required personas per decision type: `Security`, `Cost`, `UX`, `Legal`
  - Consensus threshold: 3/4 approval; dissenting opinions recorded
- **MCP exposure:** Exposes `roundtable-deliberate` MCP server; triggers from GitHub Issues

### 📨 Cloudflare Queues → The "Postal Service"

- **Role:** Event-driven orchestration between agents (replaces Solace)
- **Where it lives:** Cloudflare Queues (free tier: 1M ops/mo)
- **Key config:**
  - Topics: `agent-tasks`, `agent-results`, `system-alert`
  - Agents are stateless; Cloudflare Queues holds the workflow state
  - Retry logic: if an agent crashes, Queues redelivers to a new instance (up to 3 retries)
- **MCP exposure:** Exposed via the MCP registry Worker; agents publish/subscribe through it
- **Why this replaces Solace:** Same event-driven pattern, same retry/recovery, one less vendor, already free in your Cloudflare account

### 🔬 ML-Master → The "R&D Lab"

- **Role:** Machine learning experiments, data pipelines, AutoML
- **Where it lives:** GitHub Actions (CPU, unlimited minutes on public repos) + Google Colab/Kaggle (GPU)
- **Key config:**
  - EvoMaster framework: Draft → Debug → Improve pipeline
  - GitHub Actions runs CPU-bound experiments (6h/job max, checkpoint to resume)
  - Colab/Kaggle runs GPU-bound experiments (~12h/session, 30h/week)
  - Results auto-commit to `/company/experiments/YYYY-MM-DD/`
- **MCP exposure:** Exposes `mlmaster-experiment` MCP server to trigger runs

---

## 4. The MCP Integration Layer (The "Glue")

Since **Cherry Studio, QwenPaw, and PilotDeck** all natively support MCP, the integration strategy is to build a **unified MCP registry** hosted on Cloudflare Workers.

### MCP Registry Architecture

```yaml
# mcp-registry/config.yaml
servers:
  pilotdeck-memory:
    endpoint: https://mcp.yourstartup.workers.dev/pilotdeck
    auth: github-token
    tools: [memory_read, memory_write, memory_trace]
    
  shockwave-notes:
    endpoint: https://mcp.yourstartup.workers.dev/shockwave
    auth: github-token
    tools: [note_read, note_write, note_link]
    
  obsidian-tasks:
    endpoint: https://mcp.yourstartup.workers.dev/obsidian
    auth: github-token
    tools: [task_list, task_create, task_complete]
    
  qwenpaw-channels:
    endpoint: https://mcp.yourstartup.workers.dev/qwenpaw
    auth: api-key
    tools: [message_send, channel_list, heartbeat_check]
    
  roundtable-deliberate:
    endpoint: https://mcp.yourstartup.workers.dev/roundtable
    auth: github-token
    tools: [propose, debate, vote, record_adr]
    
  cloudflare-queues:
    endpoint: https://mcp.yourstartup.workers.dev/queues
    auth: cloudflare-api-token
    tools: [publish, subscribe, workflow_start]
    
  mlmaster-experiment:
    endpoint: https://mcp.yourstartup.workers.dev/mlmaster
    auth: github-token
    tools: [experiment_run, experiment_status, experiment_result]
```

### The Registry Worker (Cloudflare — Free Tier)

> **Design note:** This Worker must complete in under 10ms CPU time per invocation (Cloudflare free tier limit). It is a fast proxy — route, log to D1, forward. No heavy computation.

```typescript
// Cloudflare Worker: MCP Registry
export default {
  async fetch(request: Request, env: Env) {
    const url = new URL(request.url);
    
    // Health check
    if (url.pathname === '/health') {
      return Response.json({ 
        status: 'ok', 
        version: '1.0.0',
        services: Object.keys(env.MCP_REGISTRY || {})
      });
    }

    // Route to correct MCP server
    const serverName = url.pathname.split('/')[1];
    const target = env.MCP_REGISTRY?.[serverName];
    if (!target) return new Response('Unknown MCP server', { status: 404 });
    
    // Add auth, rate limiting, logging
    const response = await fetch(target.endpoint, {
      method: request.method,
      headers: {
        ...request.headers,
        'X-Startup-Auth': await verifyJWT(request, env.JWT_SECRET),
      },
      body: request.body,
    });
    
    // Log to D1 for audit trail (async — don't block response)
    ctx.waitUntil(
      env.DB.prepare(
        'INSERT INTO mcp_calls (server, tool, timestamp, user) VALUES (?, ?, ?, ?)'
      ).bind(serverName, url.pathname, Date.now(), getUserId(request)).run()
    );
    
    return response;
  }
};
```

---

## 5. GitHub as the Source of Truth

### Repo Structure

```
your-startup/
├── .github/
│   ├── workflows/
│   │   ├── agent-deploy.yml      # Auto-deploy agents on push
│   │   ├── roundtable-trigger.yml # Trigger deliberation on ADR issues
│   │   └── ml-pipeline.yml       # Run ML-Master experiments
│   └── ISSUE_TEMPLATE/
│       ├── adr.md                # Architecture Decision Record
│       ├── experiment.md         # ML experiment proposal
│       └── task.md               # Obsidian PM task sync
├── company/
│   ├── docs/                     # Shockwave notes (knowledge base)
│   ├── specs/                    # Feature specifications
│   ├── projects/                 # Obsidian PM vault sync
│   │   ├── 2026-q3-launch/
│   │   │   ├── roadmap.md
│   │   │   ├── tasks/
│   │   │   └── decisions/
│   └── experiments/              # ML-Master outputs
├── agents/
│   ├── pilotdeck-config/
│   ├── qwenpaw-personas/
│   ├── roundtable-rules/
│   └── queue-topology/
├── memory/
│   ├── pilotdeck/                # White-box memory entries
│   ├── conversations/            # Chat logs with trace IDs
│   └── embeddings/               # Vector index (for search)
└── mcp-registry/
    └── config.yaml
```

### GitHub Actions: The "Agent CI/CD"

> **Public repo benefit:** GitHub Actions are **entirely free and unlimited** for public repositories using standard GitHub-hosted runners. No minute caps.
>
> **Hard limits to be aware of:**
> - Single job: **6 hours max** (forcefully terminated)
> - Workflow run: **72 hours max** (auto-timed out)
> - Concurrency: **20 parallel jobs** per account
> - ToS: No cryptocurrency mining, server hosting, or stress-testing

```yaml
# .github/workflows/agent-deploy.yml
name: Deploy Agent Stack

on:
  push:
    branches: [main]
    paths: ['agents/**', 'mcp-registry/**']

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Validate MCP configs
        run: |
          npx @modelcontextprotocol/inspector mcp-registry/config.yaml
      
      - name: Test agent connections
        run: |
          curl -f https://mcp.yourstartup.workers.dev/health
          curl -f https://pilotdeck.yourstartup.workers.dev/health

  deploy:
    needs: validate
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Cloudflare
        run: |
          wrangler deploy --config agents/wrangler.toml
      
      - name: Notify QwenPaw
        run: |
          curl -X POST https://mcp.yourstartup.workers.dev/qwenpaw/message_send \
            -H "Authorization: Bearer ${{ secrets.QWENPAW_KEY }}" \
            -d '{"channel":"discord","message":"🚀 Agent stack deployed: ${{ github.sha }}"}'
```

---

## 6. Cloudflare as the Edge Layer (All Free Tier)

### Service Map

| Cloudflare Service | Role in Stack | Free Tier | Hard Limit |
|---|---|---|---|
| **Workers** | Lightweight agents, MCP registry, QwenPaw bridge, Roundtable API | 100K req/day | 10ms CPU/invocation |
| **AI Gateway** | Unified LLM proxy (your existing API). Rate limiting, caching, cost tracking | Free (pass-through) | N/A |
| **R2** | Blob storage for agent memory dumps, ML artifacts, Shockwave assets | 10GB + 1M ops/mo | 10GB storage |
| **D1** | SQL database for decision ledger, task history, MCP audit trail | 5M reads + 100K writes/day | 10GB storage |
| **KV** | Fast cache for agent state, conversation contexts, feature flags | 100K reads/day | 1GB storage |
| **Pages** | Public-facing docs, status page, internal dashboard | Unlimited | 500 builds/mo |
| **Queues** | Async job processing + event orchestration (replaces Solace) | 1M ops/mo | 1M ops/mo |
| **Cron Triggers** | Background scheduling for agents | 5 triggers total | 5 triggers total |

### AI Gateway Configuration

```yaml
# wrangler.toml AI Gateway
[ai]
binding = "AI"

[[ai.gateway]]
name = "startup-gateway"
cache = true
rate_limit = { requests = 1000, period = "1m" }
# Routes to your existing paid LLM API subscription
providers = [
  { name = "your-llm-api", endpoint = "https://api.your-provider.com", models = ["your-models"] }
]
```

### Cron Trigger Allocation (5 total on free tier)

| # | Trigger | Schedule | Purpose |
|---|---|---|---|
| 1 | `heartbeat` | Every 15 minutes | Agent health check |
| 2 | `memory-sync` | Every hour | Local ↔ R2 memory sync |
| 3 | `cleanup` | Daily at 3 AM | Rotate old D1 records, clean R2 |
| 4 | `daily-agents` | Daily at 9 AM | Daily background agent tasks |
| 5 | `weekly-agents` | Weekly Monday 10 AM | Weekly analytics, reports |

---

## 7. Event Orchestration: Cloudflare Queues (Replaces Solace)

### Why Queues Over Solace

| Factor | Cloudflare Queues | Solace PubSub+ |
|---|---|---|
| **Cost** | Free (1M ops/mo) | Free tier available but separate vendor |
| **Integration** | Native in Cloudflare ecosystem | Requires bridge Worker |
| **Complexity** | Simple publish/subscribe | Enterprise-grade (overkill) |
| **Retry** | 3 retries with backoff | Configurable |
| **Vendor count** | One less account to manage | Separate signup |

### Queue Topology

```typescript
// wrangler.toml
[[queues.producers]]
  queue = "agent-tasks"
  binding = "TASK_QUEUE"

[[queues.producers]]
  queue = "agent-results"
  binding = "RESULT_QUEUE"

[[queues.producers]]
  queue = "system-alerts"
  binding = "ALERT_QUEUE"

[[queues.consumers]]
  queue = "agent-tasks"
  max_batch_size = 10
  max_retries = 3
  max_batch_timeout = 30

[[queues.consumers]]
  queue = "agent-results"
  max_batch_size = 10
  max_retries = 3
```

### Queue Consumer Worker

```typescript
// workers/agent-orchestrator/src/index.ts
export default {
  // Producer: enqueue agent tasks
  async fetch(request: Request, env: Env): Promise<Response> {
    const { agent, task, payload } = await request.json();
    
    await env.TASK_QUEUE.send({
      agent,
      task,
      payload,
      id: crypto.randomUUID(),
      timestamp: Date.now()
    });

    return Response.json({ status: 'queued' });
  },

  // Consumer: process agent tasks with automatic retry
  async queue(batch: MessageBatch, env: Env): Promise<void> {
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
          original_task: msg.body.id,
          agent: msg.body.agent,
          result
        });
        
        msg.ack();
      } catch (err) {
        // Queues will retry up to max_retries (3) with backoff
        msg.retry();
      }
    }
  }
};
```

### Event Topic Structure

```
agent-tasks queue:
  agent/{agent_name}/task/assigned
  agent/{agent_name}/task/started
  agent/{agent_name}/task/completed
  agent/{agent_name}/task/failed

agent-results queue:
  workflow/{workflow_id}/step/{step_number}
  memory/update/{workspace}

system-alerts queue:
  system/alert/critical
  system/alert/warning
  system/audit/mcp_call
```

---

## 8. ML-Master: Free Compute Strategy

### Compute Allocation

| Task Type | Platform | Specs | Time Limit | Cost |
|---|---|---|---|---|
| **CI/CD + deploy** | GitHub Actions | 2-core CPU, 7GB RAM | Unlimited minutes | $0 |
| **CPU experiments** | GitHub Actions | 2-core CPU, 7GB RAM | 6h/job (checkpoint to resume) | $0 |
| **GPU experiments** | Google Colab | T4 16GB VRAM | ~12h/session, reconnect daily | $0 |
| **GPU experiments (alt)** | Kaggle | 2× T4 | 30h/week | $0 |

### Checkpoint + Resume Pattern for GitHub Actions

Every ML experiment must be designed to **complete or checkpoint within 6 hours**.

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
    timeout-minutes: 350  # 5h50m — buffer before 6h kill
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Restore checkpoint
        uses: actions/cache@v4
        with:
          path: experiments/checkpoint/
          key: exp-${{ github.event.inputs.experiment_id || github.event.issue.number }}
          restore-keys: |
            exp-

      - name: Run experiment (checkpoint-aware)
        run: |
          pip install -r experiments/requirements.txt
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

### Checkpoint-Aware Experiment Runner

```python
# experiments/run.py
import time, json, os, sys

CHECKPOINT_DIR = os.environ.get("CHECKPOINT_DIR", "experiments/checkpoint/")
MAX_RUNTIME = int(os.environ.get("MAX_RUNTIME_MINUTES", "330")) * 60
CHECKPOINT_INTERVAL = 600  # Save every 10 minutes

def run_experiment(exp_id):
    checkpoint = load_checkpoint(exp_id)
    start_epoch = checkpoint.get("epoch", 0)
    last_checkpoint = time.time()
    start_time = time.time()

    for epoch in range(start_epoch, TOTAL_EPOCHS):
        elapsed = time.time() - start_time

        # Check time budget — stop 10 minutes early
        if elapsed > MAX_RUNTIME - 600:
            save_checkpoint(exp_id, {"epoch": epoch, "status": "paused"})
            print(f"⏸️ Paused at epoch {epoch}. Re-triggering to resume.")
            retrigger_workflow(exp_id)
            return

        # Periodic checkpoint
        if time.time() - last_checkpoint > CHECKPOINT_INTERVAL:
            save_checkpoint(exp_id, {"epoch": epoch, "status": "running"})
            last_checkpoint = time.time()

        # Run one epoch
        metrics = train_one_epoch(epoch)
        log_metrics(exp_id, epoch, metrics)

    # Completed
    save_checkpoint(exp_id, {"epoch": TOTAL_EPOCHS, "status": "completed"})
    save_results(exp_id)
    print(f"✅ Experiment {exp_id} completed.")

def retrigger_workflow(exp_id):
    """Re-dispatch this workflow to resume from checkpoint."""
    import requests
    requests.post(
        f"https://api.github.com/repos/{os.environ['GITHUB_REPOSITORY']}/actions/workflows/ml-pipeline.yml/dispatches",
        headers={"Authorization": f"token {os.environ['GITHUB_TOKEN']}"},
        json={"ref": "main", "inputs": {"experiment_id": str(exp_id)}}
    )
```

### Colab GPU Strategy (Phase 6+)

For experiments requiring GPU, ML-Master generates self-contained Colab notebooks:

```python
# Auto-generated by ML-Master for GPU experiments
# Experiment: {experiment_name}
# This notebook runs on Google Colab Free (T4 GPU)

# 1. Setup
!pip install -q {dependencies}
from google.colab import drive
drive.mount('/content/drive')

# 2. Clone results repo
!git clone https://github.com/yourorg/company.git /content/company

# 3. Run GPU experiment
import torch
assert torch.cuda.is_available(), "No GPU available"
results = run_gpu_experiment(
    dataset="{dataset}",
    metrics={metrics},
    max_runtime_minutes=600  # Well within 12h session
)

# 4. Save results + push
import json
with open("/content/company/experiments/{date}-{name}/results.json", "w") as f:
    json.dump(results, f, indent=2)

%cd /content/company
!git add experiments/
!git commit -m "[ml-master] GPU experiment: {name}"
!git push

print("✅ Results pushed. Safe to close this notebook.")
```

**Total free GPU budget:**
- Colab: ~12h/day × 7 = 84 GPU-hours/week
- Kaggle: 30 GPU-hours/week
- **Total: ~114 GPU-hours/week** — sufficient for startup ML

---

## 9. Day-in-the-Life Workflow

### Scenario: You want to launch a new feature ("AI Memory Search")

**09:00 AM** — You open Cherry Studio and type:
> "We need AI-powered memory search. Draft a spec."

**09:01 AM** — PilotDeck routes this to the "Spec Writer" agent (lightweight model). It:

1. Reads existing memory entries from `/memory/pilotdeck/`
2. Checks Obsidian PM for conflicting tasks
3. Drafts a Markdown spec in Shockwave under `/company/specs/ai-memory-search.md`

**09:05 AM** — Shockwave auto-commits to GitHub. A GitHub Action triggers (free, unlimited minutes on public repo):

- Linting the spec
- Creating an Obsidian PM task in `/company/projects/2026-q3-launch/tasks/`
- Posting a notification via QwenPaw to your Slack: `#engineering`

**09:30 AM** — You review the spec in Shockwave, add a comment:
> "Need to consider privacy implications."

**09:31 AM** — PilotDeck detects the comment and spawns an Agent Roundtable:

- **Security Agent:** "We need encryption at rest for memory vectors"
- **Cost Agent:** "Vector search will add usage to R2 + Workers"
- **UX Agent:** "Search should be instant (<200ms)"

**09:45 AM** — Roundtable reaches consensus. An ADR is auto-committed to `/company/projects/2026-q3-launch/decisions/adr-007-memory-search.md`

**10:00 AM** — ML-Master agent picks up the ADR and starts an experiment:

- Drafts vector embedding pipeline
- Runs on GitHub Actions (CPU, checkpoint-aware)
- If GPU needed, generates Colab notebook and runs on T4
- Stores results in `/company/experiments/2026-06-04-memory-search/`

**02:00 PM** — Experiment succeeds. Cloudflare Queues orchestrates:

1. **Deploy Agent** updates Cloudflare Worker code
2. **Test Agent** runs integration tests on GitHub Actions
3. **Docs Agent** updates user documentation in Shockwave

**02:30 PM** — QwenPaw announces in Slack:
> "🎉 AI Memory Search is live. ADR-007 approved. Experiment logs: [link]. Docs: [link]."

**All of this is traceable** — every output links back to a specific Git commit, memory entry, and agent decision.

---

## 10. 30-Day Launch Roadmap

### Week 1: Foundation

- [ ] Set up GitHub org with the 4-repo structure (all public, free)
- [ ] Deploy Cloudflare Workers: MCP registry, health checks (free tier)
- [ ] Configure AI Gateway with your existing LLM API subscription
- [ ] Install Cherry Studio + PilotDeck locally
- [ ] Sync Obsidian PM vault to GitHub

### Week 2: Agent Integration

- [ ] Connect all tools via MCP registry
- [ ] Configure QwenPaw for 1 channel (start with Discord or Slack)
- [ ] Set up Cloudflare Queues with 3 topic queues
- [ ] Write first ADR template for Agent Roundtable
- [ ] Test end-to-end: Cherry Studio → PilotDeck → GitHub commit

### Week 3: Workflows

- [ ] Build GitHub Actions for agent CI/CD (unlimited minutes)
- [ ] Configure ML-Master with checkpoint-aware experiment runner
- [ ] Create "vibe coding" template in Shockwave
- [ ] Set up D1 tables for decision ledger
- [ ] Stress-test: trigger 10 parallel agent tasks (max 20 concurrent)

### Week 4: Polish & Launch

- [ ] Deploy status page on Cloudflare Pages (free)
- [ ] Add cost-tracking dashboard (AI Gateway + D1)
- [ ] Document the stack in `/company/docs/startup-in-a-box.md`
- [ ] Open-source the MCP registry config
- [ ] Write launch post: "We built a startup with $0/month infrastructure"

---

## 11. Cost Estimate (Monthly)

| Service | Cost |
|---|---|
| Cloudflare Workers (100K req/day free tier) | **$0** |
| Cloudflare AI Gateway (pass-through) | **$0** |
| Cloudflare D1 + R2 + KV + Queues (free tier) | **$0** |
| Cloudflare Pages (free) | **$0** |
| GitHub Free (public repos, unlimited Actions) | **$0** |
| Google Colab (T4 GPU, free) | **$0** |
| Kaggle Notebooks (2× T4, free) | **$0** |
| LLM APIs (your existing subscription) | **Already paid** |
| **Infrastructure Total** | **$0/mo** |

Compare to a traditional startup stack: $500+/mo for Vercel + Supabase + Notion + Slack + Linear + OpenAI.

---

## 12. Free Tier Limits Summary

| Service | Limit | Startup Impact | Mitigation |
|---|---|---|---|
| Cloudflare Workers | 10ms CPU/invocation | Low — MCP router is a fast proxy | Heavy work → GitHub Actions |
| Cloudflare Workers | 100K req/day | None at startup scale | Monitor; upgrade when revenue justifies |
| Cloudflare D1 | 5M reads/day | None | Monitor usage |
| Cloudflare R2 | 10GB storage | None for months | Auto-cleanup via Cron trigger |
| Cloudflare KV | 100K reads/day | None | Use D1 for hot data |
| Cloudflare Pages | 500 builds/mo | None | Optimize build triggers |
| Cloudflare Queues | 1M ops/mo | None at startup scale | Monitor; upgrade when needed |
| Cloudflare Cron | 5 triggers total | Low — prioritize wisely | Combine schedules where possible |
| GitHub Actions | 6h/job max | Low — checkpoint ML experiments | Checkpoint + resume pattern |
| GitHub Actions | 72h/workflow max | None — no workflow chains that long | N/A |
| GitHub Actions | 20 concurrent jobs | None at startup scale | Stagger long-running experiments |
| Google Colab | ~12h/session | Low — reconnect daily | Design experiments to complete in one session |
| Kaggle | 30 GPU-h/week | Low — supplementary | Use GitHub Actions for CPU work |

---

## 13. The "Vibe Code" Manifesto (for your README)

```markdown
# Startup-in-a-Box

We don't use databases. We use Git.
We don't use SaaS PM tools. We use Markdown.
We don't have black-box AI. We have traceable agents.
We don't pay for infrastructure. We use free tiers.
We don't have 10 subscriptions. We have one LLM API.

Every decision is a commit.
Every agent has an audit trail.
Every feature starts with a conversation.
Every experiment checkpoints before the 6-hour bell.

This is "vibe coding" at the organizational level.
This is startup infrastructure at $0/month.
```

---

**Want me to generate the actual starter code?** I can produce:

- The Cloudflare Worker MCP registry code
- The GitHub Actions workflow files
- The Obsidian PM vault template
- The PilotDeck workspace configuration
- The Cloudflare Queues topology + consumer Worker
- The ML-Master checkpoint-aware experiment runner

Just say which piece you want first.
