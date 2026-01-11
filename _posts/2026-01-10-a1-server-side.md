---
layout: post
title: "Server-Side Python Setup for Agent A1"
hidden: true
date: 2026-01-10
categories: [infrastructure, python, agents]
tags: [python, venv, fastapi, uvicorn, linux]
---

## 🚀  Server-Side Python Setup for Agent A1 

This document describes the **server-side Python setup** used to run Agent A1.

The goal of the setup is to remain:
- simple
- explicit
- reproducible
- easy to reason about

No containers or orchestration frameworks are used at this stage.

---

## Directory Structure

The agent lives in a dedicated directory on the Linux server.

Example structure:

```text
agent-demo/
├── agent_file_mover.py      # Core agent runtime (decision + execution)
├── agent_api.py             # FastAPI wrapper exposing the agent
├── agent_cli.py             # Optional CLI entrypoint
├── requirements.txt         # Python dependencies
├── .env                     # Environment variables (not committed)
├── .venv/                   # Python virtual environment
│
├── inbox/                   # Source folder (files to be processed)
├── processed/               # Target folder
│
├── logs/
│   └── agent.log            # Structured run history (JSON lines)
```

The structure is intentionally flat and explicit.

---

## Python Virtual Environment (venv)

The agent runs inside a **local Python virtual environment**.

This avoids dependency conflicts and keeps the system self-contained.

### Create the virtual environment

```bash
python3 -m venv .venv
```

---

### Activate the virtual environment

```bash
source .venv/bin/activate
```

Once activated, your shell prompt typically changes, for example:

```text
(.venv) user@server:~/agent-demo$
```

---

### Install dependencies

Dependencies are installed from `requirements.txt`:

```bash
pip install -r requirements.txt
```

Typical dependencies include:

- fastapi
- uvicorn
- openai
- python-dotenv (optional)

---

## Environment Variables

Sensitive configuration is passed via environment variables.

Example (OpenAI API key):

```bash
export OPENAI_API_KEY="sk-..."
```

This can be:
- exported manually
- placed in `.env`
- or injected via systemd later

The `.env` file is **not committed to Git**.

---

## Running the Agent Manually

### Run the agent directly (CLI-style)

```bash
python agent_file_mover.py
```

Useful for:
- testing
- debugging
- cron execution

---

## Running the API Service

Agent A1 is exposed via a small REST API using **FastAPI** and **Uvicorn**.

### Start the API manually

```bash
uvicorn agent_api:app --host 127.0.0.1 --port 8000
```

Expected output:

```text
Uvicorn running on http://127.0.0.1:8000
```

The API is bound to `127.0.0.1` and is typically accessed through **Nginx as a reverse proxy**.

---

## API Endpoints (Summary)

The service exposes a minimal API surface:

- `POST /api/a1/run`  
  Triggers one agent run

- `GET /api/a1/status`  
  Returns the last run object

- `GET /api/a1/history`  
  Returns historical run data

All endpoints are protected at the Nginx layer.

---

## Logs and Run History

Each agent execution produces **one structured log entry**.

Logs are written as **JSON lines**:

```text
logs/agent.log
```

Each line represents one complete agent run, including:

- run ID
- agent name
- model
- input files
- actions
- timestamps
- duration

This makes the system auditable and easy to extend.

---

## Manual Operation Model

At this stage, the system is intentionally operated manually:

- venv is activated explicitly
- uvicorn is started by hand
- the agent can be triggered via UI, API, CLI, or cron

This keeps failure modes visible and avoids hidden complexity.

Automation (systemd, containers, orchestration) can be added later if needed.

Cheat sheet for starting new service manually
```bash
ssh tom@<cantaloop-IP>
cd ~/agent-demo
source .venv/bin/activate
export OPENAI_API_KEY="sk-..."
mv processed/test1.txt inbox
uvicorn agent_api:app --host 127.0.0.1 --port 8000
Ctrl-C to stop service
```


---

## Summary

This setup favors:

- clarity over abstraction
- observability over automation
- boring infrastructure over clever tooling

It is designed to support experimentation with agent behavior while keeping the runtime predictable and debuggable.

---

_Last updated: January 2026_  
_Source: Cantaloop Aps._
