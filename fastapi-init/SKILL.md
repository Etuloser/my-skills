---
name: fastapi-init
description: Initialize a new project using the FastAPI full-stack template (copier-based), configure environment, set up database, and start both backend and frontend development servers.
license: MIT
compatibility: opencode
metadata:
  category: web-framework
  platform: cross-platform
  language: python
  framework: fastapi
  extensible: true
---

## What I do

I guide you through initializing a new project based on the [FastAPI full-stack template](https://github.com/fastapi/full-stack-fastapi-template). This includes:

1. **Environment verification** — check that Python, pip, pipx, copier, and bun are installed
2. **Project scaffolding** — run `copier copy` to generate the project from the official template
3. **Environment configuration** — generate `SECRET_KEY`, configure `.env` for backend
4. **Database setup** — run alembic migrations and seed initial data
5. **Development server startup** — launch both backend (FastAPI) and frontend (React + Vite) in parallel

## When to use me

Invoke me when:
- You want to start a new full-stack project with FastAPI + React
- You need to quickly spin up the FastAPI official template
- You’ve forgotten the exact sequence of setup steps
- You want to verify your environment before running the template

## Prerequisites

Before running this skill, ensure the following tools are installed:

| Tool | Purpose | Check Command |
|------|---------|---------------|
| Python 3.10+ | Backend runtime | `python --version` |
| pip | Python package manager | `pip --version` |
| pipx | Install Python CLI tools globally | `pipx --version` |
| copier | Template scaffolding engine | `copier --version` |
| bun | Frontend runtime & package manager | `bun --version` |
| Git | Clone templates, version control | `git --version` |

> 💡 **Tip**: If any tool is missing, run `/env-checker` first to diagnose your environment and get install instructions.

## Quick Start

### Step 1 — Environment Check (Optional but Recommended)

Run `/env-checker` or manually verify:

```bash
python --version    # >= 3.10
pip --version
pipx --version
copier --version
bun --version
git --version
```

If `copier` is not installed:

```bash
python -m pip install --upgrade pip
pipx install copier
```

### Step 2 — Generate Project from Template

```bash
copier copy https://github.com/fastapi/full-stack-fastapi-template my-awesome-project --trust
```

> 🔑 The `--trust` flag is required because the template uses copier tasks (Jinja2 hooks) to execute post-copy scripts.

**What happens during scaffolding:**
- Copier asks you interactive questions (project name, author, etc.)
- Generates a full project with backend (`app/`) and frontend (`frontend/`)
- Creates `.env` files, Docker configs, CI/CD workflows, tests, etc.

### Step 3 — Configure Backend Environment

```bash
cd my-awesome-project/backend
```

Generate a secure `SECRET_KEY`:

```bash
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

Edit `.env` and set at least these variables:

```env
# Required
SECRET_KEY=<paste-the-generated-key-here>

# Database — SQLite for quick local dev
DATABASE_URL=sqlite:///./app.db

# Or PostgreSQL (default in template)
# DATABASE_URL=postgresql+psycopg://postgres:changethis@localhost:5432/app

# First superuser (auto-created by initial_data.py)
FIRST_SUPERUSER=admin@example.com
FIRST_SUPERUSER_PASSWORD=changethis
```

### Step 4 — Initialize Database

```bash
# Run all pending migrations
alembic upgrade head

# Seed initial data (creates first superuser, etc.)
python .\app\initial_data.py
```

> 🛡️ On Windows use backslashes: `python .\app\initial_data.py`
> On macOS/Linux use forward slashes: `python ./app/initial_data.py`

### Step 5 — Start Development Servers

**Terminal 1 — Backend:**

```bash
cd my-awesome-project/backend
fastapi run --reload .\app\main.py
```

- API docs: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

**Terminal 2 — Frontend:**

```bash
cd my-awesome-project/frontend
bun install
bun run dev
```

- Web app: http://localhost:5173
- Hot reload enabled

## Full Command Reference

```bash
# 1. Install copier (once per machine)
python -m pip install --upgrade pip
pipx install copier

# 2. Scaffold project
copier copy https://github.com/fastapi/full-stack-fastapi-template my-awesome-project --trust

# 3. Configure env
cd my-awesome-project/backend
python -c "import secrets; print(secrets.token_urlsafe(32))"
# → edit .env with the generated SECRET_KEY

# 4. Setup database
alembic upgrade head
python .\app\initial_data.py   # Windows
# python ./app/initial_data.py  # macOS/Linux

# 5. Start backend
fastapi run --reload .\app\main.py

# 6. Start frontend (new terminal)
cd my-awesome-project/frontend
bun install
bun run dev
```

## Project Structure (After Generation)

```
my-awesome-project/
├── backend/
│   ├── app/
│   │   ├── main.py              # FastAPI entry point
│   │   ├── api/                 # API routers
│   │   ├── core/
│   │   │   └── config.py        # Settings & .env loader
│   │   ├── models/              # SQLModel/SQLAlchemy models
│   │   ├── schemas/             # Pydantic schemas
│   │   ├── crud/                # Database CRUD operations
│   │   ├── db/                  # Database session & engine
│   │   ├── initial_data.py     # Seed script
│   │   └── tests/               # Pytest test suite
│   ├── alembic/                 # Database migrations
│   ├── .env                     # Environment variables (you edit this)
│   ├── pyproject.toml           # Python dependencies
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── main.tsx             # React entry point
│   │   ├── routes/              # TanStack Router pages
│   │   ├── client/              # API client (auto-generated)
│   │   └── components/          # UI components (shadcn/ui)
│   ├── index.html
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml           # Full stack with one command
├── .github/
│   └── workflows/               # CI/CD (GitHub Actions)
└── README.md                    # Template's own documentation
```

## Environment Variables Quick Reference

| Variable | Required | Default | Purpose |
|----------|----------|---------|---------|
| `SECRET_KEY` | ✅ | — | JWT signing, cryptographic operations |
| `DATABASE_URL` | ✅ | `sqlite:///./app.db` | SQLAlchemy database URI |
| `FIRST_SUPERUSER` | ✅ | `admin@example.com` | Auto-created admin email |
| `FIRST_SUPERUSER_PASSWORD` | ✅ | `changethis` | Admin password |
| `BACKEND_CORS_ORIGINS` | ⚪ | `http://localhost:5173` | Allowed frontend origins |
| `SMTP_HOST` | ⚪ | — | Email server (for password reset) |
| `SMTP_PORT` | ⚪ | `587` | SMTP port |
| `SMTP_USER` | ⚪ | — | SMTP username |
| `SMTP_PASSWORD` | ⚪ | — | SMTP password |
| `EMAILS_FROM_EMAIL` | ⚪ | — | Sender address |
| `FRONTEND_HOST` | ⚪ | `http://localhost:5173` | Frontend URL for links |

## Troubleshooting

### `copier` command not found

```bash
pipx install copier
# Or if pipx not found:
python -m pip install --user copier
```

### `alembic` command not found

Make sure you're inside the `backend/` directory and have installed dependencies:

```bash
cd my-awesome-project/backend
pip install -e ".[dev]"
# Or with uv:
uv pip install -e ".[dev]"
```

### `fastapi` command not found

```bash
pip install fastapi[standard]
# Or:
pipx install fastapi-cli
```

### Database locked (SQLite) / connection refused (PostgreSQL)

- **SQLite**: ensure no other process is using `app.db`
- **PostgreSQL**: make sure the Docker container or local server is running:
  ```bash
  docker compose up -d db
  ```

### Frontend `bun install` fails

```bash
# If bun is not installed
curl -fsSL https://bun.sh/install | bash

# Or use npm/pnpm as fallback
npm install
# pnpm install
```

### `--trust` flag warning

The template uses copier **tasks** to run post-generation scripts (e.g., renaming files). The `--trust` flag allows these scripts to execute. Without it, copier will abort. This is safe because the template is from the official FastAPI organization.

## Alternative: Docker (One-Command Launch)

If you have Docker Desktop installed, you can skip all manual setup:

```bash
cd my-awesome-project
docker compose up -d
```

This launches:
- Backend API (`http://localhost:8000`)
- Frontend (`http://localhost:5173`)
- PostgreSQL database
- Traefik reverse proxy

Then run migrations inside the container:

```bash
docker compose exec backend alembic upgrade head
docker compose exec backend python /app/app/initial_data.py
```

## When NOT to use me

- **Modifying an existing project** — use this skill only for fresh initialization
- **Production deployment** — use the template's Docker setup and deployment docs instead
- **Adding features** — this is a setup skill, not a development guide

## Further Reading

- [FastAPI Full-Stack Template README](https://github.com/fastapi/full-stack-fastapi-template)
- [Copier Documentation](https://copier.readthedocs.io/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [TanStack Router](https://tanstack.com/router) (frontend routing)
- [shadcn/ui](https://ui.shadcn.com/) (UI components)
