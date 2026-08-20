# Local n8n Stack — Runbook

Self-hosted n8n backed by PostgreSQL, running under Docker Compose. This is the
Week 0 environment from [LEARNING_PLAN.md](../LEARNING_PLAN.md).

## Layout

| File | Purpose |
|---|---|
| `docker-compose.yml` | Core stack: Postgres, n8n, task runner |
| `docker-compose.instance-ai.yml` | Optional overlay: AI Assistant sandbox + SearXNG |
| `init-data.sh` | Creates the non-root Postgres user on first boot |
| `.env.example` | Template for `.env` (never commit `.env`) |

## First run

```bash
cp .env.example .env
```

Generate a value for each secret and paste it in:

```bash
openssl rand -hex 32     # run once per secret
```

At minimum set `POSTGRES_PASSWORD`, `POSTGRES_NON_ROOT_PASSWORD`,
`RUNNERS_AUTH_TOKEN` and `N8N_ENCRYPTION_KEY`. Then:

```bash
docker compose up -d
docker compose ps          # all services should be "healthy" or "running"
```

Open http://127.0.0.1:5678 and create the owner account.

> **Back up `N8N_ENCRYPTION_KEY` somewhere outside this machine — a password
> manager is fine.** Every credential n8n stores is encrypted with it. Lose the
> key and every stored credential is unrecoverable, even with a database backup.

## Verifying it works

```bash
docker compose logs -f n8n           # watch startup
docker compose exec postgres psql -U n8n -d n8n -c '\dt'   # tables exist
```

Build a workflow with a Code node and execute it — that exercises the external
task runner, which is the piece most likely to be misconfigured.

## Optional: the AI Assistant overlay

```bash
docker compose -f docker-compose.yml -f docker-compose.instance-ai.yml up -d
```

Adds four services and needs ~4GB of free memory. **`sandbox-runner-1` runs
privileged Docker-in-Docker to execute AI-generated code — a privileged
container can escape to the host, so treat running this as granting root.** Use
it only on a machine you own and would be willing to rebuild.

This powers n8n's built-in *assistant*. It is not required for the AI Agent and
LLM Chain nodes you build workflows with — those work on the core stack alone.

## Backup

Two things need backing up, plus the encryption key above.

```bash
# 1. Database (workflows, credentials, execution history)
docker compose exec -T postgres pg_dump -U n8n n8n | gzip > backup-db-$(date +%F).sql.gz

# 2. The n8n data volume (config, binary data)
docker run --rm -v clauden8n_n8n_storage:/data -v "$PWD":/backup alpine \
  tar czf /backup/backup-n8n-$(date +%F).tar.gz -C /data .
```

Confirm the volume name first — Compose prefixes it with the project directory:

```bash
docker volume ls | grep n8n_storage
```

Keep backups out of git; `.gitignore` already excludes `.env`, and `backup-*`
files should not be committed either.

## Restore

```bash
docker compose down                    # keep volumes
gunzip -c backup-db-YYYY-MM-DD.sql.gz | \
  docker compose exec -T postgres psql -U n8n -d n8n
docker compose up -d
```

For a full rebuild from scratch, restore `.env` first (for the encryption key),
then `docker compose up -d`, then load the database dump.

**Do the restore drill at least once before you rely on it** — that is a Week 8
exit criterion in the learning plan, and an untested backup is not a backup.

## Common problems

| Symptom | Cause | Fix |
|---|---|---|
| `bad interpreter: /bin/bash^M` | `init-data.sh` checked out with CRLF | `.gitattributes` forces LF — re-clone or `git add --renormalize .` |
| n8n can't authenticate to Postgres | Changed `POSTGRES_NON_ROOT_*` after first boot | `init-data.sh` only runs on an empty volume: `docker compose down -v` (destroys data) |
| Code nodes hang or fail | Runner can't reach the broker | `docker compose logs n8n-runner`; confirm `RUNNERS_AUTH_TOKEN` matches on both services |
| Schedule triggers fire at wrong times | Timezone unset | Set both `GENERIC_TIMEZONE` and `TZ` in `.env` |
| Assistant gets 401 from the sandbox | Key mismatch | `N8N_INSTANCE_AI_SANDBOX_API_KEY` must equal a value in `SANDBOX_API_KEYS` |

## Upgrading

`N8N_VERSION` in `.env` pins both n8n and the task runner — keep them on the
same tag. Back up before upgrading, then:

```bash
docker compose pull && docker compose up -d
```
