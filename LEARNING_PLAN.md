# n8n AI Automation Learning Plan — v2

**10-week practical roadmap: Infrastructure / Azure Administrator → AI Automation Engineer**

Goal: use n8n to build reliable, event-driven automation across APIs, Azure, Microsoft 365,
databases and AI — and finish with a **public portfolio** that proves it.

> v2 of the original 8-week plan. What changed and why is summarized in
> [Section 12](#12-changes-from-v1).

---

## 1. Operating Principles

- You are not a generic beginner. Your infrastructure, Windows, Azure, API, monitoring and
  troubleshooting experience transfers directly. Focus on n8n workflow mechanics, API
  integration, event-driven design, AI tooling and production engineering.
- **Every week ends with a committed artifact.** A skill that isn't visible in this repo
  doesn't exist for hiring purposes. Export workflow JSON, write the README, push.
- Ratio: roughly 30% learning, 70% building.
- Progression: beginner workflow builder → automation developer → AI automation engineer →
  production n8n architect.

## 2. Week 0 — Environment & Portfolio Setup (one weekend)

Do this before Week 1. The original plan skipped it entirely.

- Self-host n8n with **Docker Compose + PostgreSQL** (not SQLite) so your instance mirrors a
  production deployment from day one. Keep the compose file in this repo (secrets in `.env`,
  which stays out of git).
- Create the portfolio structure in this repo: one folder per project under `projects/`, each
  with a README and exported workflow JSON. Use `projects/PROJECT_TEMPLATE.md`.
- Establish the export habit: **Workflow → Download → commit**. Before committing, verify the
  JSON contains no secrets — n8n exports reference credentials by name/ID, but check any
  hard-coded headers, URLs with tokens, or Code-node strings.
- Add a `.gitignore` for `.env`, local n8n data directories, and scratch files.

**Exit criteria**
- [ ] n8n running locally via Docker Compose with Postgres persistence
- [ ] This repo has the `projects/` structure and a first committed test workflow JSON
- [ ] You can explain where n8n stores credentials and why they never land in git

## 3. Weeks 1–5 — Core Automation Engineering

### Week 1 — n8n Fundamentals
- Workflows, nodes, triggers, actions, items, JSON, expressions, credentials, executions,
  active vs. manual workflows.
- Course: **N8N101 – Essentials**.
- Labs: scheduled notification; JSON transformation; conditional workflow.

**Exit criteria**
- [ ] For any workflow you build, you can explain: what triggers it, what data enters, how it
      changes, what decisions occur, what action results
- [ ] Portfolio: Project 1 (scheduled reporting automation) committed with README

### Week 2 — Data & Expressions
- JSON objects, arrays, nested data, type handling (strings, numbers, booleans, null).
- n8n expressions and dynamic references between nodes.
- Set/Edit Fields, Filter, Sort, Aggregate, Merge, Split Out, Summarize, Code.
- Lab: CSV → JSON → filter → aggregate → report; then an inactive-employee reporting workflow.

**Exit criteria**
- [ ] You can flatten/reshape nested JSON without trial-and-error
- [ ] Portfolio: inactive-employee report workflow committed, with sample (sanitized) input data

### Week 3 — APIs, HTTP & Webhooks
- REST: methods, headers, query/path parameters, request bodies, status codes, pagination.
- Auth patterns: API key, bearer token, basic auth, OAuth2.
- Polling vs. event-driven webhook automation.
- **Webhook security** (added in v2): validate a shared secret or HMAC signature on every
  inbound webhook; never expose an unauthenticated webhook that triggers actions.
- Labs: REST API reader; webhook receiver with signature validation; paginated API integration.

**Exit criteria**
- [ ] Given unfamiliar API docs, you can translate an endpoint into a working n8n workflow
- [ ] Your webhook receiver rejects unsigned/invalid requests
- [ ] Portfolio: Projects 2 and 3 (API data collector, webhook event processor) committed

### Week 4 — Advanced Workflow Logic & Error Handling
- IF, Switch, Loop Over Items, Merge, Wait, workflow sequencing.
- Error workflows with Error Trigger; explicit failure handling; retries, delays, branching,
  escalation, notification patterns.
- Project: automated IT incident workflow routing low / medium / critical events differently.

**Exit criteria**
- [ ] Every portfolio workflow from this point has an attached error workflow
- [ ] You can describe what happens when each external call in your incident workflow fails
- [ ] Portfolio: Projects 4 and 5 (Azure resource monitoring, incident notification) committed

### Week 5 — Databases & Microsoft Stack Integration
- SQL fundamentals: tables, rows, primary keys, SELECT, INSERT, UPDATE.
- PostgreSQL, SQL Server, n8n Data Tables.
- Azure, Microsoft Graph, Teams, SharePoint integration. Register your own **Entra ID app
  with least-privilege Graph scopes** — this is résumé-grade skill for your background.
- Project: employee onboarding workflow with validation, notifications, tasks and
  transaction logging.

**Exit criteria**
- [ ] You can write and debug the SQL your workflows execute
- [ ] Your Entra app registration uses only the Graph permissions the workflow needs
- [ ] Portfolio: Projects 6 and 7 (onboarding automation, database sync) committed

## 4. Weeks 6–7 — AI Automation (split from one week into two)

The original plan packed LLMs, prompts, structured output, tool calling, embeddings, RAG,
agents, memory and human approval into one week. That is 2–3 weeks of material; rushing it
produces demo-ware. Split as follows.

### Week 6 — LLM Foundations in n8n
- LLM basics: prompts, context windows, temperature, token costs.
- **Structured output** (JSON schemas from the model) — the single most important AI skill
  for automation, because downstream nodes need predictable fields.
- Classification and routing patterns; the n8n AI Agent and LLM Chain nodes.
- Tool/function calling fundamentals.
- Cost control: set per-workflow budgets, log token usage, use small models where they suffice.
- Project 1: AI IT Help Desk classifier and router (Project 8).

**Exit criteria**
- [ ] Your classifier returns validated structured JSON, and the workflow handles the case
      where the model returns garbage (retry, then fallback route)
- [ ] You can state roughly what one execution of your workflow costs
- [ ] Portfolio: Project 8 committed

### Week 7 — Agents, RAG & Evaluation
- Agents with tools and memory; when an agent is justified vs. a fixed pipeline (default to
  the fixed pipeline — agents are for genuinely open-ended tasks).
- Embeddings and RAG over your own docs (e.g., runbooks) with a vector store.
- **Evaluation and guardrails** (added in v2): build a small test set of inputs with expected
  outputs and run it after every prompt change — use n8n's evaluation features where
  available. Treat prompt changes like code changes.
- **Prompt injection awareness**: any workflow where LLM input contains external text
  (emails, tickets, alerts) must treat that text as untrusted. Consequential actions require
  human approval, always.
- Project 2: AI incident analyzer that summarizes alerts and proposes actions (Project 9).

**Exit criteria**
- [ ] Your incident analyzer cites which runbook/context chunk informed its proposal
- [ ] You have a written eval set (10+ cases) and a record of a prompt change improving it
- [ ] No AI workflow in your portfolio executes a consequential action without human approval
- [ ] Portfolio: Project 9 committed

## 5. Week 8 — Production Engineering

- Design for timeouts, auth failures, invalid data, rate limits, API outages, partial failures.
- Retries, escalation, logging, monitoring, execution review.
- Reusable sub-workflows; consistent naming and documentation conventions.
- **Added in v2:** testing with pinned/mock data; n8n backup and restore (workflows,
  credentials, database); environments (dev vs. prod instance); queue mode and scaling
  concepts — know what changes when n8n must handle real volume; secrets management.
- Target: a workflow you'd trust at 2 AM when nobody is watching.

**Exit criteria**
- [ ] You can restore your n8n instance from backup and prove it
- [ ] One earlier project is refactored to production standard (sub-workflows, error workflow,
      logging, README updated) — commit the before/after diff
- [ ] You can explain n8n queue mode and when you'd need it

## 6. Weeks 9–10 — Capstone: AI IT Operations Assistant

One week (original plan) is not enough to build *and* document *and* present this well. Two
weeks, with explicit scope tiers so it ships even if time runs short:

**MVP (must ship, Week 9)**
- Ingest alerts from monitoring + Azure APIs.
- AI analysis: classify, summarize, propose an action with cited context (RAG over runbooks).
- Post to Teams; human approval gate; execute approved action; log everything to the database.

**Stretch (Week 10, as time allows)**
- Ticketing integration; agent memory across related incidents; evaluation dashboard;
  cost/token reporting.

**Reference architecture**

```
Monitoring / Alerts ──▶  n8n  ◀── Azure APIs
                          │
                          ▼
                      AI Agent (tools: monitoring, Azure, runbook RAG)
                      /   │   \
               Database  Teams  Ticketing
                          │
                          ▼
                   Human Approval
                          │
                          ▼
                   Approved Action ──▶ Audit log
```

**Exit criteria**
- [ ] Architecture doc covering: components, credentials model, failure modes, recovery
      procedures, test cases
- [ ] 3–5 minute demo video (screen recording) linked from the project README
- [ ] Portfolio: Project 10 committed; repo README updated to feature it

## 7. Weekly Study Schedule

| Day | Time | Activity |
|---|---|---|
| Monday | 60–90 min | Theory and official course material |
| Tuesday | 60–90 min | Guided build |
| Wednesday | 60 min | Rebuild from memory without the tutorial |
| Thursday | 60–90 min | Challenge: change the requirements |
| Saturday | 2–3 hr | Build a project from scratch |
| Sunday | 30–60 min | Review, document failures, **export + commit + write README** |

Sunday's session is the portfolio session — the week isn't done until the artifact is pushed.

## 8. Technology Stack

| Area | Tools |
|---|---|
| Core | n8n (self-hosted, Docker), Git/GitHub, JSON, JavaScript, Python, REST, webhooks |
| Microsoft | Azure, Microsoft Graph, Teams, SharePoint, Entra ID, Azure Functions, Azure Monitor |
| Data | PostgreSQL, SQL Server, n8n Data Tables, a vector store for RAG, optionally Redis |
| AI | One primary provider first (Anthropic or OpenAI); then compare via OpenRouter; Google Gemini optional |

## 9. Project Portfolio

| # | Project | Difficulty | Week |
|---|---|---|---|
| 1 | Scheduled reporting automation | Beginner | 1 |
| 2 | API data collector | Beginner | 3 |
| 3 | Webhook event processor (with signature validation) | Intermediate | 3 |
| 4 | Azure resource monitoring workflow | Intermediate | 4 |
| 5 | Automated incident notification | Intermediate | 4 |
| 6 | Employee onboarding automation | Intermediate | 5 |
| 7 | Database synchronization workflow | Intermediate | 5 |
| 8 | AI document/email/ticket classifier | Advanced | 6 |
| 9 | AI IT incident analyzer (RAG + evals) | Advanced | 7 |
| 10 | AI IT Operations Assistant | Expert | 9–10 |

Every project ships with: exported workflow JSON (secret-free), README from the template,
architecture sketch, and honest notes on failure modes and lessons learned. Employers read
the failure notes more carefully than the success claims.

## 10. Publishing & Career Positioning

**Positioning:** not "n8n specialist" but
**Windows/Infrastructure + Azure + PowerShell/Python + REST APIs + n8n + AI Agents + Automation Architecture** —
enterprise IT operations automation, with n8n as the orchestration layer.

Concrete publishing actions (added in v2 — building silently earns nothing):
- Keep this repo public and current; it *is* the portfolio.
- After Weeks 5, 7 and 10, write a short post (LinkedIn or blog) walking through one project:
  problem → architecture → what broke → fix. Three posts total, tied to real artifacts.
- Publish 1–2 generic workflows as n8n community templates — external validation that costs
  little extra effort.
- Résumé bullets should be quantified from the projects, e.g. "Built an AI-assisted incident
  triage pipeline (n8n, Microsoft Graph, PostgreSQL, Claude API) that classifies and routes
  alerts with human-approval gates; documented failure modes and recovery procedures."

**Certification:** secondary to shipped projects. n8n lists a professional certification exam
as planned separately from Foundations coursework — don't delay project work waiting for it.
Given your Azure track, **AZ-104 (if not held) or AI-102** pairs credibly with this portfolio;
treat either as optional reinforcement, not a gate.

## 11. Official Learning Sequence & References

1. **N8N101 – Essentials**: interface, nodes, triggers, credentials, data structures,
   transformations, expressions, binary data → Weeks 0–2.
2. **N8N102 – Integrations**: APIs, HTTP, auth, webhooks, flow control, pagination, Code
   nodes, Data Tables, reusable workflows → Weeks 3–5 (major focus).
3. **N8N103 – In Practice**: AI agents, tools, memory, debugging, testing, modularity,
   monitoring, production practices → Weeks 6–8.

Links:
- n8n Academy: https://learn.n8n.io/
- N8N101: https://learn.n8n.io/courses/course-v1:n8n+N8N101+2026H2/about
- N8N102: https://learn.n8n.io/courses/course-v1:n8n+N8N102+2026H2/about
- N8N103: https://learn.n8n.io/courses/course-v1:n8n+N8N103+2026H2/about
- Learning resources: https://support.n8n.io/article/learning-resources
- n8n docs: https://docs.n8n.io/

Use the Academy as the structured curriculum, but keep every project
infrastructure/Azure-oriented so you finish with evidence of real-world capability.

## 12. Changes from v1

| Gap in v1 | Fix in v2 |
|---|---|
| No environment setup step | Week 0: Docker Compose + Postgres self-hosting |
| Git listed in stack but never used; no portfolio mechanics | Weekly export→commit→README ritual; Sunday is the portfolio session; publishing plan |
| All AI topics crammed into one week | Split into Week 6 (LLM foundations, structured output) and Week 7 (agents, RAG, evals) |
| Capstone build + docs in one week | Two weeks with MVP/stretch scope tiers and a demo video |
| Security only implied | Webhook signatures, least-privilege Entra scopes, secret-free exports, prompt-injection handling, mandatory human approval for consequential AI actions |
| No AI quality control | Eval sets, guardrails, cost/token tracking |
| No testing/backup/scaling coverage | Week 8: pinned-data testing, backup/restore drill, dev vs. prod, queue mode |
| Milestones vague | Checkable exit criteria every week |

---

*v2 prepared August 18, 2026. Course content and certification availability change; check the
official n8n Academy for the latest details.*
