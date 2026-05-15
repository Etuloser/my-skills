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

1. **Environment verification** — check that Python, **uv** (recommended), copier, and bun are installed
2. **Project scaffolding** — run `copier copy` to generate the project from the official template
3. **Environment configuration** — generate `SECRET_KEY`, configure `.env` for backend
4. **Database setup** — run alembic migrations and seed initial data (using `uv run` for speed)
5. **Development server startup** — launch both backend (FastAPI) and frontend (React + Vite) in parallel

## When to use me

Invoke me when:
- You want to start a new full-stack project with FastAPI + React
- You need to quickly spin up the FastAPI official template
- You've forgotten the exact sequence of setup steps
- You want to verify your environment before running the template

## Project Naming Advisor

Before scaffolding the project, I help you choose a great name. Here's how it works:

### Step 1 — Describe Your Project

Tell me what your project does. For example:

> "一个面向中小企业的库存管理系统，支持多仓库、条码扫描和库存预警。"

### Step 2 — I Suggest Names

Based on your description, I propose **3 naming options** with reasoning:

| # | Suggested Name | Rationale | Best For |
|---|---------------|-----------|----------|
| 1 | `inventory-pro` | Clear, professional, implies advanced features | Teams, B2B SaaS |
| 2 | `stockguard` | Memorable, evokes "protection/alerting" | Branding-focused products |
| 3 | `warehub` | Short, modern, "hub" suggests centralization | Startups, tech-savvy users |

**Naming principles I follow:**
- ✅ **Kebab-case** (`my-project`) for repo names (GitHub standard)
- ✅ **Short & memorable** (≤ 15 chars when possible)
- ✅ **Avoid collisions** with popular packages on PyPI/npm
- ✅ **No underscores or CamelCase** in repo names (causes URL issues)
- ✅ **Semantic**: name hints at what the app does
- ⚠️ **Avoid generic words** like `app`, `system`, `platform` alone

### Step 3 — You Choose or Refine

Pick one of my suggestions, or ask me to generate more. Once confirmed, I use that name for all subsequent steps.

### Step 4 — Pre-fill Copier Context

The FastAPI template asks several interactive questions during `copier copy`. I can pre-answer them based on your project name and description:

| Copier Question | My Default Answer | Notes |
|-----------------|-------------------|-------|
| `project_name` | Your confirmed name | Human-readable title |
| `package_name` | `{{ project_name | lower | replace('-', '_') }}` | Python package import name |
| `author_name` | Git user name or prompt you | Used in LICENSE, pyproject.toml |
| `author_email` | Git user email or prompt you | Used in package metadata |
| `project_description` | Your original description | One-liner for README/PyPI |
| `domain_main` | `localhost` | For local development |
| `domain_api` | `api.localhost` | API subdomain |
| `domain_frontend` | `localhost` | Frontend origin |
| `stack_name` | `{{ project_name | lower }}` | Docker Compose stack name |
| `secret_key` | *(auto-generated)* | You don't need to set this manually |

> 💡 **Tip**: If you want to override any of these, just tell me before we run `copier copy`.

## Prerequisites

Before running this skill, ensure the following tools are installed:

| Tool | Purpose | Check Command | Priority |
|------|---------|---------------|----------|
| Python 3.10+ | Backend runtime | `python --version` | Required |
| **uv** | Python package & project manager | `uv --version` | **Recommended** |
| pip | Python package manager (fallback) | `pip --version` | Legacy |
| pipx | Install Python CLI tools globally | `pipx --version` | Legacy |
| copier | Template scaffolding engine | `copier --version` | Required |
| bun | Frontend runtime & package manager | `bun --version` | Required |
| Git | Clone templates, version control | `git --version` | Required |

> 🚀 **Why uv?** `uv` (by Astral) is a modern, extremely fast Python package manager written in Rust. It replaces `pip`, `pipx`, and `virtualenv` with a single tool. The FastAPI template officially uses `uv` for dependency management. Install it once: `curl -LsSf https://astral.sh/uv/install.sh | sh` (macOS/Linux) or `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"` (Windows).

> 💡 **Tip**: If any tool is missing, run `/env-checker` first to diagnose your environment and get install instructions.

## Quick Start

### Step 1 — Environment Check (Optional but Recommended)

Run `/env-checker` or manually verify:

```bash
python --version    # >= 3.10
uv --version        # Recommended
# pipx --version    # Only if not using uv
copier --version
bun --version
git --version
```

If `copier` is not installed:

**With uv (recommended):**
```bash
uv tool install copier
```

**With pip/pipx (legacy):**
```bash
python -m pip install --upgrade pip
pipx install copier
```

### Step 2 — Confirm Project Name & Description

Before running copier, ensure you've completed the **Project Naming Advisor** section above:

1. Provide your project description
2. Review my naming suggestions
3. Confirm the final project name
4. Review the pre-filled copier context (author name, email, etc.)

**Example interaction:**

> **You**: "我要做一个博客平台，支持 Markdown 编辑和文章分类。"  
> **Me**: 建议 `markblog`（Markdown + blog 的组合，简洁）、`typeflow`（写作如流）、`scrivboard`（书写板）。  
> **You**: 选 `markblog`。  
> **Me**: 好的，copier 上下文：project_name="markblog", package_name="markblog", author="Your Name", description="A blog platform with Markdown editing and article categorization."

### Step 3 — Generate Project from Template

#### Option A: Interactive Mode (Recommended for manual use)

```bash
copier copy https://github.com/fastapi/full-stack-fastapi-template my-awesome-project --trust
```

Copier will ask you interactive questions (project name, author, email, etc.). Answer them based on the naming decisions from Step 2.

#### Option B: Non-Interactive / AI-Assisted Mode

In automation environments (like AI agents or CI/CD), copier's interactive prompts will fail. Use `--defaults` to generate with template defaults, then patch the configuration afterward:

```bash
copier copy --trust --defaults \
  https://github.com/fastapi/full-stack-fastapi-template \
  my-awesome-project
```

Then patch these files with your confirmed project name:
- `frontend/index.html` — update `<title>`
- `package.json` — update `"name"`
- `backend/pyproject.toml` — update `description` (⚠️ keep `name = "app"` — see Troubleshooting)
- `.env` — update `PROJECT_NAME`, `STACK_NAME`
- `.copier/.copier-answers.yml` — update metadata

> 🔑 The `--trust` flag is required because the template uses copier tasks (Jinja2 hooks) to execute post-copy scripts.

**What happens during scaffolding:**
- Generates a full project with backend (`app/`) and frontend (`frontend/`)
- Creates `.env` files, Docker configs, CI/CD workflows, tests, etc.
- Initializes a git repository with the template's history

### Step 4 — Configure Backend Environment

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
PROJECT_NAME=my-awesome-project
STACK_NAME=myawesomeproject

# First superuser (auto-created by initial_data.py)
FIRST_SUPERUSER=admin@example.com
FIRST_SUPERUSER_PASSWORD=changethis
```

> 📋 `.env` is read from the parent directory (`../.env`) by default. Ensure the root `.env` exists and is copied to `backend/.env` if needed.

#### Database: Choose Your Mode

**Option 1 — SQLite (No Docker Required, Recommended for Quick Start)**

If you don't have Docker installed, use SQLite for instant local development:

1. **Edit `backend/.env`:**
   ```env
   POSTGRES_SERVER=sqlite
   POSTGRES_DB=app
   ```

2. **Modify `backend/app/core/config.py`** to support SQLite:
   ```python
   # Remove PostgresDsn import, keep only: AnyUrl, BeforeValidator, EmailStr, HttpUrl
   from pydantic import (
       AnyUrl, BeforeValidator, EmailStr, HttpUrl,
       computed_field, model_validator,
   )

   # Replace the SQLALCHEMY_DATABASE_URI property:
   @computed_field
   @property
   def SQLALCHEMY_DATABASE_URI(self) -> str:
       return (
           f"sqlite:///./{self.POSTGRES_DB}.db"
           if self.POSTGRES_SERVER == "sqlite"
           else f"postgresql+psycopg://{self.POSTGRES_USER}:{self.POSTGRES_PASSWORD}@{self.POSTGRES_SERVER}:{self.POSTGRES_PORT}/{self.POSTGRES_DB}"
       )
   ```

3. **Modify `backend/app/alembic/env.py`** for SQLite batch mode:
   ```python
   # In both run_migrations_offline() and run_migrations_online(),
   # add render_as_batch=True to context.configure():
   context.configure(
       ...,
       render_as_batch=True,
   )
   ```

4. **Modify `backend/app/core/db.py`** to create tables directly (skip alembic for SQLite):
   ```python
   def init_db(session: Session) -> None:
       # SQLite quick-start: create tables directly
       from sqlmodel import SQLModel
       SQLModel.metadata.create_all(engine)
       # ... rest of the function (seed superuser)
   ```

**Option 2 — PostgreSQL (Default, Requires Docker)**

```bash
# Start PostgreSQL in Docker
docker compose up -d db

# Then use the template defaults (no code changes needed)
alembic upgrade head
python app/initial_data.py
```

### Step 5 — Initialize Database

#### For SQLite (Quick Start):

```bash
cd my-awesome-project/backend
rm -f app.db          # clean start
uv run python app/initial_data.py
```

This creates `app.db` and seeds the first superuser.

#### For PostgreSQL (Docker):

```bash
cd my-awesome-project/backend
uv run alembic upgrade head
uv run python app/initial_data.py
```

> 🛡️ On Windows use backslashes: `python app\initial_data.py`
> On macOS/Linux use forward slashes: `python app/initial_data.py`

### Step 6 — Start Development Servers

**Terminal 1 — Backend:**

```bash
cd my-awesome-project/backend
```

**On macOS/Linux:**
```bash
uv run fastapi run --reload ./app/main.py
```

**On Windows (recommended, avoids Unicode rendering issues):**
```bash
uv run python -m uvicorn app.main:app --reload --port 8000
```

- API docs: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

> ⚠️ **Windows Note**: The `fastapi run` CLI may throw `NoConsoleScreenBufferError` on some Windows terminals due to `rich_toolkit` Unicode rendering. Use `uvicorn` directly as shown above.

**Terminal 2 — Frontend:**

```bash
cd my-awesome-project/frontend
bun install
bun run dev
```

- Web app: http://localhost:5173
- Hot reload enabled

## Full Command Reference

### Modern Stack with SQLite (No Docker — Quick Start)

```bash
# 1. Install uv (once per machine)
# macOS/Linux:
curl -LsSf https://astral.sh/uv/install.sh | sh
# Windows:
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# 2. Install copier via uv
uv tool install copier

# 3. Describe your project & confirm name with the AI
# (see "Project Naming Advisor" section above)

# 4. Scaffold project (use --defaults if non-interactive, then patch config)
copier copy --trust --defaults \
  https://github.com/fastapi/full-stack-fastapi-template \
  my-awesome-project

# 5. Patch project metadata (only needed with --defaults)
# - frontend/index.html: <title>
# - package.json: "name"
# - backend/pyproject.toml: description
# - .env: PROJECT_NAME, STACK_NAME

# 6. Configure SQLite mode
# Edit backend/.env:
#   POSTGRES_SERVER=sqlite
#   POSTGRES_DB=app
#   PROJECT_NAME=my-awesome-project
# Modify backend/app/core/config.py for SQLite URI (see Step 4)
# Modify backend/app/core/db.py for SQLModel.metadata.create_all(engine)
# Modify backend/app/alembic/env.py for render_as_batch=True

# 7. Generate SECRET_KEY and configure .env
python -c "import secrets; print(secrets.token_urlsafe(32))"

# 8. Setup database
rm -f backend/app.db
uv run --no-dev python backend/app/initial_data.py

# 9. Start backend
# macOS/Linux:
uv run fastapi run --reload backend/app/main.py
# Windows (recommended):
uv run python -m uvicorn app.main:app --reload --port 8000

# 10. Start frontend (new terminal)
cd my-awesome-project/frontend
bun install
bun run dev
```

### Modern Stack with PostgreSQL + Docker (Production-Ready)

```bash
# Steps 1-4 same as above

# 5. Start PostgreSQL
docker compose up -d db

# 6. Configure .env (keep PostgreSQL defaults)
python -c "import secrets; print(secrets.token_urlsafe(32))"

# 7. Setup database
uv run alembic upgrade head
uv run python app/initial_data.py

# 8-9. Start servers same as above
```

### Legacy Stack (with pip/pipx — still supported)

```bash
# 1. Install copier
python -m pip install --upgrade pip
pipx install copier

# 2-4. Same as modern stack above

# 5. Setup database with legacy Python
alembic upgrade head
python .\app\initial_data.py   # Windows
# python ./app/initial_data.py  # macOS/Linux

# 6. Start backend
fastapi run --reload .\app\main.py
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

### `uv` is not installed

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Verify
uv --version
```

> 💡 **Why switch?** `uv` is 10-100x faster than pip, has a global cache, and replaces pip + pipx + virtualenv + pyenv with one tool.

### Copier crashes with `NoConsoleScreenBufferError` or interactive prompt fails

**Cause**: Copier requires an interactive TTY to ask questions. In automation environments (AI agents, CI/CD, some IDEs), this fails.

**Fix**: Use `--defaults` flag, then manually patch the generated files:

```bash
copier copy --trust --defaults \
  https://github.com/fastapi/full-stack-fastapi-template \
  my-awesome-project
```

Then patch:
- `frontend/index.html` → `<title>Your Project</title>`
- `frontend/package.json` → `"name": "your-project"`
- `backend/pyproject.toml` → `description = "Your description"` (⚠️ **keep `name = "app"`**)
- `.env` → `PROJECT_NAME`, `STACK_NAME`
- `.copier/.copier-answers.yml` → metadata for future updates

### Hatchling build fails: "No module named 'app'" or `default_only_include` error

**Cause**: You changed `name = "app"` in `backend/pyproject.toml` to your project name.

**Fix**: The FastAPI template's internal Python package **must** remain named `app`. Only change the `description` field, not `name`:

```toml
[project]
name = "app"          # ✅ Keep this exactly as "app"
version = "0.1.0"
description = "Your actual project description"   # ✅ Change only this
```

The `name` field here is for PyPI package metadata; the actual importable package is determined by the directory structure (`backend/app/`).

### SQLite: `alembic upgrade head` fails with "near ALTER: syntax error"

**Cause**: The template's alembic migrations use `ALTER COLUMN` which SQLite does not support.

**Fix**: Add `render_as_batch=True` to both `context.configure()` calls in `backend/app/alembic/env.py`:

```python
context.configure(
    ...,
    render_as_batch=True,   # ✅ Add this for SQLite compatibility
)
```

Even better: for SQLite development, skip alembic entirely and create tables directly in `backend/app/core/db.py`:

```python
def init_db(session: Session) -> None:
    from sqlmodel import SQLModel
    SQLModel.metadata.create_all(engine)   # ✅ Direct table creation
    # ... seed superuser
```

### Windows: `fastapi run` throws `NoConsoleScreenBufferError`

**Cause**: The `fastapi` CLI uses `rich_toolkit` for fancy output, which crashes on some Windows terminals due to console screen buffer API issues.

**Fix**: Use `uvicorn` directly instead:

```bash
# Instead of:
fastapi run --reload app/main.py

# Use:
uv run python -m uvicorn app.main:app --reload --port 8000
```

This bypasses the CLI wrapper and works reliably on all Windows terminals.

### `.env` changes not reflected / "changethis" warnings

**Cause**: The backend reads `.env` from the **parent directory** (`../.env`) by default, not `backend/.env`.

**Fix**: Ensure `.env` exists at the project root, or copy it:

```bash
cp .env backend/.env
```

If you've modified `backend/.env` but not the root `.env`, the app will still read the old values from the root.

### `alembic` command not found

Make sure you're inside the `backend/` directory and have installed dependencies:

**With uv (recommended):**
```bash
cd my-awesome-project/backend
uv pip install -e ".[dev]"
uv run alembic upgrade head
```

**Legacy:**
```bash
cd my-awesome-project/backend
pip install -e ".[dev]"
alembic upgrade head
```

### `fastapi` command not found

**With uv (recommended):**
```bash
uv run fastapi run --reload ./app/main.py
```

**Legacy:**
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
