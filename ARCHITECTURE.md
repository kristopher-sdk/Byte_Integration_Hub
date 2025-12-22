# Hyper CEO - Web Application Architecture

## Overview

A standalone web-based multi-agent automation platform that leverages Claude Opus 4.5 to automate executive operations, integrating with users' daily productivity tools.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              WEB APPLICATION                                 │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                        FRONTEND (React/Next.js)                      │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────┐ │   │
│  │  │  Dashboard   │  │  Agent       │  │  Tool        │  │  Settings│ │   │
│  │  │  & Reports   │  │  Monitor     │  │  Connections │  │  & Auth  │ │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘  └──────────┘ │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              API LAYER (FastAPI)                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐│
│  │   REST API  │  │  WebSocket  │  │   OAuth     │  │   Webhook Handler   ││
│  │  Endpoints  │  │  (Realtime) │  │   Manager   │  │   (Tool Callbacks)  ││
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                     MULTI-AGENT ORCHESTRATION LAYER                          │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    OPUS 4.5 AGENT RUNTIME                             │  │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────────────────┐  │  │
│  │  │   Scheduler    │  │  Agent Router  │  │  Context Manager (VFS) │  │  │
│  │  │  (24hr Cadence)│  │  & Dispatcher  │  │  "Everything is a File"│  │  │
│  │  └────────────────┘  └────────────────┘  └────────────────────────┘  │  │
│  │                                                                       │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐ │  │
│  │  │                        AGENT POOL (57 Agents)                    │ │  │
│  │  │                                                                   │ │  │
│  │  │  MORNING SHIFT (06:00-07:30)    │  EVENING SHIFT (20:00-21:00)  │ │  │
│  │  │  • Revenue Priority             │  • Competitor Intelligence     │ │  │
│  │  │  • Team Unblocker               │  • Market Trends               │ │  │
│  │  │  • Operational Excellence       │  • Opportunity Scout           │ │  │
│  │  │  • Customer Pulse               │  • Strategy Synthesizer        │ │  │
│  │  │  • Strategic Alignment          │  • Technology Radar            │ │  │
│  │  │                                 │                                 │ │  │
│  │  │  IMPROVEMENT PIPELINE           │  PRODUCTIVITY AGENTS           │ │  │
│  │  │  • Review Synthesis             │  • Project Manager (Hourly)    │ │  │
│  │  │  • Implementation (Auto-76%)    │  • Email-to-Project (10min)    │ │  │
│  │  │  • Fact Validator               │  • Email Drafts (4x daily)     │ │  │
│  │  │  • Coherence Monitor            │  • Heartbeat (Hourly)          │ │  │
│  │  └─────────────────────────────────────────────────────────────────┘ │  │
│  │                                                                       │  │
│  │  ┌────────────────────────────────────────────────────────────────┐  │  │
│  │  │                       MEMORY SYSTEM (4-Tier)                    │  │  │
│  │  │  Scratchpad (24-72h) → Episodic (30-90d) → Fact → Experiential │  │  │
│  │  └────────────────────────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         TOOL INTEGRATION LAYER                               │
│                                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐│
│  │   Asana     │  │  Microsoft  │  │  Fireflies  │  │   Web Search        ││
│  │  (Tasks)    │  │  365 Suite  │  │  (Meetings) │  │   (News/Research)   ││
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────────────┘│
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐│
│  │   Slack     │  │  Google     │  │   Linear    │  │   Supermemory       ││
│  │  (Comms)    │  │  Workspace  │  │  (Projects) │  │   (Semantic Search) ││
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            DATA LAYER                                        │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────────┐ │
│  │   PostgreSQL    │  │   Redis Cache   │  │   Vector DB (Pinecone)      │ │
│  │   (Core Data)   │  │   (Sessions)    │  │   (Semantic Memory)         │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Core Components

### 1. Frontend Application
**Tech Stack**: Next.js 14, React 18, TailwindCSS, shadcn/ui

| Component | Purpose |
|-----------|---------|
| Dashboard | Morning/Evening summaries, P0 actions, metrics |
| Agent Monitor | Real-time agent status, execution logs, quality scores |
| Tool Connections | OAuth setup for integrations, connection health |
| Settings | User preferences, notification rules, approval workflows |

### 2. API Layer
**Tech Stack**: FastAPI, Python 3.12

| Endpoint Type | Purpose |
|---------------|---------|
| REST API | CRUD operations, agent triggers, reports |
| WebSocket | Real-time updates, live agent output streaming |
| OAuth Manager | Token management for tool integrations |
| Webhook Handler | Receive events from connected tools |

### 3. Multi-Agent Orchestration
**Tech Stack**: Claude Opus 4.5 via Anthropic API

| Component | Purpose |
|-----------|---------|
| Scheduler | 24-hour operational cadence execution |
| Agent Router | Routes tasks to appropriate agents |
| Context Manager (VFS) | Unified interface to all data sources |
| Memory System | 4-tier memory for continuity |

### 4. Tool Integration Layer
Unified adapter pattern for external services:

```python
# VFS Path Structure
/context/memory        → Internal Fact Store
/context/pad           → Agent Scratchpad
/context/tools/asana   → Task Management
/context/tools/ms365   → Email & Calendar
/context/tools/slack   → Team Communications
/context/tools/web     → Web Search
```

---

## Data Flow

```
User Request → Frontend → API → Agent Router → Opus 4.5 Agent
                                      ↓
                              Context Manager (VFS)
                                      ↓
                    ┌─────────────────┼─────────────────┐
                    ↓                 ↓                 ↓
               Memory            Tool APIs         Web Search
                    ↓                 ↓                 ↓
                    └─────────────────┼─────────────────┘
                                      ↓
                              Agent Output
                                      ↓
                    ┌─────────────────┼─────────────────┐
                    ↓                 ↓                 ↓
               Dashboard        Notifications      Outbox (Actions)
```

---

## Key Design Patterns

### 1. Heartbeat + Outbox Pattern
Agents propose actions without side effects, then a gated outbox executes approved actions.

```
Agent 99 (Heartbeat) → Propose Intents → Approval Gate → Execute via Outbox
                                              ↓
                              AUTO / POLICY / CEO APPROVAL
```

### 2. Common Sense Protocol (76% Automation)
```
AUTO_APPROVE           → Low-risk changes, formatting, bug fixes
COMMON_SENSE_APPROVAL  → Framework improvements, optimizations  
CEO_DECISION           → Business logic, behavior changes
```

### 3. 5-Dimension Quality Framework
All agent outputs scored on:
- **Completeness** (20%) - All sections present
- **Accuracy** (20%) - Correct data, cited sources
- **Relevance** (20%) - Aligned with CEO priorities
- **Actionability** (25%) - Clear, executable recommendations
- **Efficiency** (15%) - Time-to-value ratio

---

## Deployment Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLOUD PROVIDER                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │   Vercel    │  │   Railway   │  │   Managed Services      │ │
│  │  (Frontend) │  │  (Backend)  │  │  (DB, Redis, Vector)    │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │               ANTHROPIC API (Opus 4.5)                   │   │
│  │   • Multi-agent conversations                            │   │
│  │   • Tool use for integrations                            │   │
│  │   • Structured outputs                                   │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Security Considerations

| Layer | Security Measure |
|-------|------------------|
| Frontend | JWT auth, HTTPS only, CSP headers |
| API | Rate limiting, input validation, CORS |
| Tool Integrations | OAuth 2.0, encrypted token storage |
| Data | Encryption at rest, VPC isolation |
| Agent Actions | Outbox approval gate, audit logging |

---

## Implementation Phases

### Phase 1: Foundation (Weeks 1-4)
- [ ] Frontend scaffold with auth
- [ ] API layer with WebSocket
- [ ] Single agent proof-of-concept with Opus 4.5
- [ ] Memory system (Scratchpad + PostgreSQL)

### Phase 2: Core Agents (Weeks 5-8)
- [ ] Morning shift agents (1-5, 12, 14)
- [ ] Evening shift agents (6-11)
- [ ] VFS and tool adapters (2-3 integrations)
- [ ] Dashboard with real-time updates

### Phase 3: Intelligence Loop (Weeks 9-12)
- [ ] Review agents (101-119)
- [ ] Improvement pipeline (agents 16-20)
- [ ] Common Sense Protocol implementation
- [ ] Quality scoring framework

### Phase 4: Scale & Polish (Weeks 13-16)
- [ ] Remaining tool integrations
- [ ] Advanced scheduling & dependencies
- [ ] User customization & preferences
- [ ] Production hardening & monitoring

---

## Tech Stack Summary

| Layer | Technology |
|-------|------------|
| Frontend | Next.js 14, React 18, TailwindCSS, shadcn/ui |
| Backend | FastAPI, Python 3.12, Celery (task queue) |
| AI Engine | Claude Opus 4.5 (Anthropic API) |
| Database | PostgreSQL 16, Redis 7 |
| Vector Store | Pinecone (semantic memory) |
| Hosting | Vercel (FE), Railway (BE), AWS RDS |
| Auth | Clerk or Auth.js |
| Monitoring | Sentry, Datadog |
