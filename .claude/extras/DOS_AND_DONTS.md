# Do's and Don'ts — rails_lab

Concrete rules derived from the architectural decisions already taken (`MEMORY.md` ADR-001 to ADR-003) and the chosen stack. Read this before suggesting any change to infra, stack or workflow.

---

## 🧱 Stack & Versioning

### ✅ Do
- **Do** stay on **Rails 7.2.3.1** as the working baseline (ADR-001).
- **Do** write code that is forward-compatible with **Rails 8.0** — the upgrade is planned, not optional.
- **Do** target **Ruby 3.3+**.
- **Do** document any deprecation warning the moment it appears.

### ❌ Don't
- **Don't** jump straight to Rails 8 — the lab exists to *document* the 7.2 → 8 path.
- **Don't** introduce gems that have no Rails 8 compatibility roadmap.
- **Don't** pin Ruby to a version older than 3.3.
- **Don't** use APIs already deprecated in 7.2.

---

## 🚀 Deploy & Infrastructure

### ✅ Do
- **Do** deploy exclusively through **Kamal 2** (ADR-002).
- **Do** target **Railway** as the runtime, with **PostgreSQL provisioned by Railway**.
- **Do** push images to **`ghcr.io`** (GitHub Container Registry).
- **Do** keep the `Dockerfile` **multi-stage** and based on `ruby:3.3.x-slim`.
- **Do** run the container as the non-root `rails` user.
- **Do** use `kamal config` → `kamal setup` → `kamal deploy` as the canonical flow.

### ❌ Don't
- **Don't** introduce alternative deploy tools (Capistrano, Heroku CLI, `flyctl`, raw `docker push` + `ssh`).
- **Don't** use Docker Hub or any registry other than `ghcr.io`.
- **Don't** bake secrets, master keys or tokens into the image.
- **Don't** run the application as `root` inside the container.
- **Don't** skip the `bootsnap precompile` and `assets:precompile` stages.

---

## 🔐 Secrets & Configuration

### ✅ Do
- **Do** keep secrets in `.env` locally and in **Railway Variables** in production.
- **Do** inject secrets through Kamal's `env.secret:` block (`RAILS_MASTER_KEY`, `DATABASE_URL`, `KAMAL_REGISTRY_PASSWORD`).
- **Do** rotate `KAMAL_REGISTRY_PASSWORD` (GitHub PAT) on a schedule.
- **Do** consult `DEPLOY_SECRETS.md` before changing the secret-injection flow.

### ❌ Don't
- **Don't** commit `.env`, `config/master.key`, `config/credentials/*.key` or any token.
- **Don't** put secrets under `env.clear:` in `config/deploy.yml`.
- **Don't** hardcode connection strings — read them from environment variables.

---

## 🤖 AI Workflow

### ✅ Do
- **Do** use **Claude as orchestrator** (XML/Markdown context) and **Gemini-CLI as tactical executor** (ADR-003).
- **Do** keep `GEMINI.md`, `CLAUDE.md` and `MEMORY.md` synchronized when context changes.
- **Do** record every new architectural decision as a new ADR entry in `MEMORY.md` (date in `YYYY-MM-DD`).
- **Do** write code, comments and commits in **English**.

### ❌ Don't
- **Don't** translate the historical Portuguese notes already in `MEMORY.md` — append new entries in English instead.
- **Don't** let the agent contexts (`GEMINI.md` / `CLAUDE.md`) drift apart silently.
- **Don't** make destructive moves without an ADR justifying them.

---

## 🧪 Code & Architecture

### ✅ Do
- **Do** follow "The Rails Way" — convention over configuration.
- **Do** use **Minitest + fixtures** when the application code lands.
- **Do** prefer **HAML** over ERB for views.
- **Do** use **Hotwire (Turbo + Stimulus)** as the frontend layer.
- **Do** use [Conventional Commits](https://www.conventionalcommits.org/).

### ❌ Don't
- **Don't** introduce **RSpec** or **FactoryBot**.
- **Don't** introduce a frontend framework (React, Vue, Svelte).
- **Don't** write `.erb` views.
- **Don't** put business logic in controllers or views.

---

## 📌 When in doubt

Before deviating from any rule above:

1. Check `MEMORY.md` — is there an ADR that already answers this?
2. If yes — follow it.
3. If no — open a new ADR proposal, document the rationale, and only then act.

Decisions without traceable rationale are not allowed.
