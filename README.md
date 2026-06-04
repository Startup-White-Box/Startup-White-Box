# Startup-White-Box

> Vibe Code Startup-in-a-Box — Zero-cost AI-native infrastructure

Every decision is a commit. Every agent has an audit trail. Every feature starts with a conversation. Every experiment runs without a time limit.

**Monthly infrastructure cost: $0**

## Architecture

See [Blueprint.md](./Blueprint.md) for the full architecture and [vibe-code-startup-roadmap.md](./vibe-code-startup-roadmap.md) for the 16-week implementation plan.

## Stack

| Layer | Tool | Cost |
|---|---|---|
| Edge | Cloudflare Workers (Free) | $0 |
| Compute | Proxmox LXC + GitHub Actions (Self-Hosted Runner) | $0 |
| Storage | MinIO (self-hosted) + Cloudflare R2 (Free) | $0 |
| Queues | Redis (self-hosted) + Cloudflare Queues (Free) | $0 |
| Desktop | Cherry Studio + Obsidian PM + Shockwave | $0 |
| Agents | PilotDeck + QwenPaw + Agent Roundtable + ML-Master | $0 |
| LLM | Your existing API subscription | Already paid |

## Infrastructure

This repo uses a **self-hosted GitHub Actions runner** on Proxmox (Debian 12 LXC) with **no job time limits**.

## Quick Start

```bash
git clone https://github.com/Startup-White-Box/Startup-White-Box.git
cd Startup-White-Box
# See Blueprint.md for full setup instructions
```
