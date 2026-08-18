# AI Automation Portfolio — n8n × Azure × AI

Enterprise IT operations automation, built on an infrastructure/Azure administration
background: **Windows/Infrastructure + Azure + PowerShell/Python + REST APIs + n8n + AI
Agents + Automation Architecture**.

This repo is both the working portfolio and the roadmap that produces it.

- 📋 **[Learning plan (v2)](LEARNING_PLAN.md)** — 10-week roadmap from automation developer
  to AI automation engineer, with weekly exit criteria.
- 📁 **[projects/](projects/)** — one folder per project: exported workflow JSON
  (secret-free), README, architecture notes, failure modes and lessons learned.

## Projects

| # | Project | Difficulty | Status |
|---|---|---|---|
| 1 | Scheduled reporting automation | Beginner | 🔲 Not started |
| 2 | API data collector | Beginner | 🔲 Not started |
| 3 | Webhook event processor (signature-validated) | Intermediate | 🔲 Not started |
| 4 | Azure resource monitoring workflow | Intermediate | 🔲 Not started |
| 5 | Automated incident notification | Intermediate | 🔲 Not started |
| 6 | Employee onboarding automation | Intermediate | 🔲 Not started |
| 7 | Database synchronization workflow | Intermediate | 🔲 Not started |
| 8 | AI document/email/ticket classifier | Advanced | 🔲 Not started |
| 9 | AI IT incident analyzer (RAG + evals) | Advanced | 🔲 Not started |
| 10 | **AI IT Operations Assistant** (capstone) | Expert | 🔲 Not started |

Status legend: 🔲 Not started · 🔨 In progress · ✅ Shipped (workflow JSON + README + demo)

## Stack

n8n (self-hosted, Docker + PostgreSQL) · Microsoft Graph / Teams / SharePoint / Entra ID ·
Azure Monitor / Functions · PostgreSQL / SQL Server · LLM APIs (Anthropic / OpenAI) with
structured output, RAG and human-approval gates.

## Conventions

- Workflow exports are committed to the owning project folder; credentials never leave n8n.
- Every AI workflow that can take a consequential action includes a human approval step.
- Each project README follows [projects/PROJECT_TEMPLATE.md](projects/PROJECT_TEMPLATE.md).
