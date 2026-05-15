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

> The official template uses a **root-level `.env`** (read by `backend/app/core/config.py` via `env_file="../.env"`). The root `package.json` is the monorepo workspace root — both are by design.

```bash
# Work from the project root
cd my-awesome-project
```

#### 4.1 Generate Secrets

Generate both `SECRET_KEY` and `FIRST_SUPERUSER_PASSWORD`:

```bash
# Generate SECRET_KEY (32 bytes)
python -c "import secrets; print(secrets.token_urlsafe(32))"

# Generate admin password (16 bytes, user-friendly)
python -c "import secrets; print(secrets.token_urlsafe(16))"
```

> 🔐 Save both outputs — you'll need them in the next step.

#### 4.2 Edit root `.env`

The root `.env` (generated by copier) is read by `backend/app/core/config.py` via `env_file="../.env"` (the template default — **no change needed**).

Replace its content with:

```env
# Domain
DOMAIN=localhost

# Frontend
FRONTEND_HOST=http://localhost:5173

# Environment: local, staging, production
ENVIRONMENT=local

# Project Identity
PROJECT_NAME=my-awesome-project
STACK_NAME=myawesomeproject

# Security (paste your generated values)
SECRET_KEY=<paste-generated-secret-key>
FIRST_SUPERUSER=admin@example.com
FIRST_SUPERUSER_PASSWORD=<paste-generated-password>

# Database Mode: sqlite | postgresql
DATABASE_TYPE=sqlite

# SQLite Settings
SQLITE_DB_NAME=app.db

# PostgreSQL Settings (uncomment if switching to postgresql)
# POSTGRES_SERVER=localhost
# POSTGRES_PORT=5432
# POSTGRES_DB=app
# POSTGRES_USER=postgres
# POSTGRES_PASSWORD=<paste-generated-password>

# Emails (optional)
SMTP_HOST=
SMTP_USER=
SMTP_PASSWORD=
EMAILS_FROM_EMAIL=info@example.com
SMTP_TLS=True
SMTP_SSL=False
SMTP_PORT=587

# Monitoring
SENTRY_DSN=

# Docker image tags
DOCKER_IMAGE_BACKEND=backend
DOCKER_IMAGE_FRONTEND=frontend
```

> 💡 The root `.env` and `package.json` are part of the official template design. `.env` is read by each container/service from the monorepo root, and `package.json` is the Bun workspace root.

#### 4.3 Modify `backend/app/core/config.py` for SQLite

Add the `DATABASE_TYPE` and `SQLITE_DB_NAME` fields, and update the URI builder:

```python
# Add these fields inside class Settings:
DATABASE_TYPE: Literal["sqlite", "postgresql"] = "sqlite"
SQLITE_DB_NAME: str = "app.db"

# Replace the SQLALCHEMY_DATABASE_URI property:
@computed_field
@property
def SQLALCHEMY_DATABASE_URI(self) -> str:
    if self.DATABASE_TYPE == "sqlite":
        return f"sqlite:///./{self.SQLITE_DB_NAME}"
    return (
        f"postgresql+psycopg://{self.POSTGRES_USER}:{self.POSTGRES_PASSWORD}"
        f"@{self.POSTGRES_SERVER}:{self.POSTGRES_PORT}/{self.POSTGRES_DB}"
    )
```

#### 4.4 Modify `backend/app/core/db.py` for SQLite

Enable direct table creation for SQLite development:

```python
def init_db(session: Session) -> None:
    # For SQLite: create tables directly
    # For PostgreSQL: comment this out and use alembic upgrade head
    from sqlmodel import SQLModel
    SQLModel.metadata.create_all(engine)

    user = session.exec(
        select(User).where(User.email == settings.FIRST_SUPERUSER)
    ).first()
    if not user:
        user_in = UserCreate(
            email=settings.FIRST_SUPERUSER,
            password=settings.FIRST_SUPERUSER_PASSWORD,
            is_superuser=True,
        )
        user = crud.create_user(session=session, user_create=user_in)
```

#### 4.5 Modify `backend/app/alembic/env.py` for SQLite

Add `render_as_batch=True` for SQLite compatibility:

```python
# In BOTH run_migrations_offline() and run_migrations_online(),
# add render_as_batch=True to context.configure():

context.configure(
    url=url,
    target_metadata=target_metadata,
    literal_binds=True,
    compare_type=True,
    render_as_batch=True,   # ← Add this for SQLite
)
```

#### 4.6 Fix `backend/app/api/deps.py` for SQLite UUID compatibility

JWT claims are always strings, but `session.get()` with SQLite needs a `uuid.UUID` object:

```python
# Add the import at the top:
from uuid import UUID

# Change this line in get_current_user():
# BEFORE:
user = session.get(User, token_data.sub)

# AFTER:
user = session.get(User, UUID(token_data.sub))
```

> 📌 This fix is safe for PostgreSQL too — native PostgreSQL UUID accepts both str and uuid.UUID.

### Step 5 — Initialize Database

```bash
cd my-awesome-project/backend
```

#### SQLite Mode (Default — No Docker):

```bash
rm -f app.db          # clean start (optional)
uv run python app/initial_data.py
```

This creates `app.db` directly and seeds the first superuser. No alembic needed.

#### PostgreSQL Mode (Requires Docker):

First uncomment PostgreSQL variables in `backend/.env` and set `DATABASE_TYPE=postgresql`, then:

```bash
# Start database
docker compose up -d db

# Run migrations
uv run alembic upgrade head

# Seed data
uv run python app/initial_data.py
```

> 🛡️ On Windows: `python app\initial_data.py`  
> On macOS/Linux: `python app/initial_data.py`

### Step 6 — Start Development Servers

```bash
cd my-awesome-project/backend
```

**Terminal 1 — Backend:**
uv run fastapi dev app/main.py
```

- API docs: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc
- Auto-reload enabled (`--reload` is implied by `dev`)

> 💡 `fastapi dev` is the development command. It enables auto-reload, detailed tracebacks, and disables production optimizations. Use `fastapi run` (without `dev`) only for production.

**Terminal 2 — Frontend (inside `frontend/`):**

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
# - frontend/package.json: "name"
# - backend/pyproject.toml: description

# 6. Backend configuration
# 6.1 Edit root .env with generated secrets and SQLite settings
python -c "import secrets; print(secrets.token_urlsafe(32))"      # → SECRET_KEY
python -c "import secrets; print(secrets.token_urlsafe(16))"      # → FIRST_SUPERUSER_PASSWORD
# Paste into root .env (template in Step 4.2)

# 6.2 Modify backend/app/core/config.py:
#     - Add DATABASE_TYPE and SQLITE_DB_NAME fields
#     - Update SQLALCHEMY_DATABASE_URI for dual-mode

# 6.3 Modify backend/app/core/db.py:
#     - Add SQLModel.metadata.create_all(engine)

# 6.4 Modify backend/app/alembic/env.py:
#     - Add render_as_batch=True (both offline and online)

# 6.5 Modify backend/app/api/deps.py:
#     - Add from uuid import UUID
#     - Change: session.get(User, UUID(token_data.sub))

# 7. Setup database
cd backend
rm -f app.db
uv run --no-dev python app/initial_data.py

# 8. Start backend
uv run fastapi dev app/main.py

# 9. Start frontend (new terminal)
cd ../frontend
bun install
bun run dev
```

### Modern Stack with PostgreSQL + Docker (Production-Ready)

```bash
# Steps 1-4 same as above

# 5. Backend configuration
# Edit root .env: DATABASE_TYPE=postgresql, uncomment PostgreSQL variables
# Modify backend/app/core/config.py: add DATABASE_TYPE field + dual-mode URI

# 6. Generate secrets and fill root .env
python -c "import secrets; print(secrets.token_urlsafe(32))"
python -c "import secrets; print(secrets.token_urlsafe(16))"

# 7. Start PostgreSQL and setup database
docker compose up -d db
cd backend
uv run alembic upgrade head
uv run python app/initial_data.py

# 8. Start backend
uv run fastapi dev app/main.py

# 9. Start frontend (new terminal)
cd ../frontend
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

### SQLite: `POST /login/test-token` fails with `AttributeError: 'str' object has no attribute 'hex'`

**Cause**: JWT `sub` claims are always strings, but `session.get(User, pk)` with SQLite requires a `uuid.UUID` object. SQLAlchemy's SQLite UUID type calls `.hex` on the value during parameter binding — strings don't have `.hex`.

**Fix**: Convert the JWT `sub` claim to `UUID` before passing to `session.get()` in `backend/app/api/deps.py`:

```python
# BEFORE (works on PostgreSQL, fails on SQLite):
from collections.abc import Generator
...
user = session.get(User, token_data.sub)

# AFTER (works on both SQLite and PostgreSQL):
from collections.abc import Generator
from uuid import UUID      # ← add this import
...
user = session.get(User, UUID(token_data.sub))
```

> 📌 This fix is safe for PostgreSQL too — native PostgreSQL UUID type accepts both `str` and `uuid.UUID`.

### `.env` changes not reflected / "changethis" warnings

**Cause**: You're editing `backend/.env` but the template reads from the **project root** (`.env`) via `env_file="../.env"`.

**Fix**: Always edit the **root `.env`** file. The path `env_file="../.env"` is the template default — do not change it.

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
uv run fastapi dev app/main.py
```

> 💡 Use `dev` for development (auto-reload, detailed errors) and `run` for production.

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
