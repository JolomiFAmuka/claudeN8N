# Project N — <Name>

> One-sentence summary: what real problem this automates and for whom.

**Status:** 🔨 In progress · **Difficulty:** Beginner/Intermediate/Advanced · **Week:** N

## Problem

What was manual, slow, or error-prone before this workflow existed.

## Architecture

```
Trigger ──▶ ... ──▶ Action
```

Nodes/services used and why. Note any sub-workflows.

## Workflow files

- `workflow.json` — exported from n8n (credentials referenced by name only; verified
  secret-free before commit)
- Sample input/output data (sanitized) if useful

## Error handling

What happens when each external dependency fails: timeouts, auth failure, rate limits,
invalid data. Link the error workflow if separate.

## Security notes

Auth model, scopes/permissions used and why they're minimal, webhook validation, and (for AI
workflows) how untrusted text is handled and where the human approval gate sits.

## What broke / lessons learned

Honest notes. This section is the most valuable part of the portfolio.

## Demo

Screenshot or short recording of a successful execution (and, ideally, a handled failure).
