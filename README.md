# rails_lab

> Deep experimentation lab focused on **Ruby on Rails internals** — request lifecycle, middlewares, callbacks, and the architectural transition from **Rails 7.2 to 8.0**.

[![Ruby](https://img.shields.io/badge/Ruby-3.3%2B-CC342D?logo=ruby)](https://www.ruby-lang.org/)
[![Rails](https://img.shields.io/badge/Rails-7.2.3.1-CC0000?logo=rubyonrails)](https://rubyonrails.org/)
[![Deploy](https://img.shields.io/badge/Deploy-Kamal%202-0E1116)](https://kamal-deploy.org/)
[![Target](https://img.shields.io/badge/Target-Railway-0B0D0E)](https://railway.com/)

---

## 🎯 Goal

`rails_lab` is a controlled environment, built by **Carlos Kvasir Lima**, for stress-testing the limits of Ruby on Rails and validating modern deploy architectures. The lab is used to:

- Explore the lifecycle of a Rails request (Rack → middlewares → controller → callbacks).
- Document the upgrade path from **Rails 7.2 to Rails 8.0**.
- Validate an *infrastructure-as-code* deploy workflow with **Kamal 2** targeting **Railway**.
- Serve as a practical reference for *clean code* and the "The Rails Way" philosophy.

---

## 🧱 Tech Stack

| Layer            | Technology                              |
| ---------------- | --------------------------------------- |
| Language         | Ruby 3.3+                               |
| Framework        | Ruby on Rails **7.2.3.1** (target: 8.0) |
| Database         | PostgreSQL (provisioned on Railway)     |
| Containerization | Docker (multi-stage)                    |
| Deployment       | Kamal 2                                 |
| Registry         | GitHub Container Registry (`ghcr.io`)   |
| Target           | Railway                                 |

---

## 🏛 Architectural Decisions (ADRs)

ADRs are recorded in [`.claude/extras/MEMORY.md`](./.claude/extras/MEMORY.md). Summary:

- **ADR-001 — Rails version:** start at `7.2.3.1` (stable baseline) and document the migration path to `8.0`.
- **ADR-002 — Deploy strategy:** `Kamal 2 → Railway`, combining the simplicity of the PaaS with the infrastructure ownership Kamal provides (zero-downtime, Docker-based).
- **ADR-003 — AI workflow:** Claude as orchestrator (XML context), Gemini-CLI as tactical executor.

---

## 📂 Repository Layout

```
rails_lab/
├── .claude/
│   ├── extras/
│   │   ├── CONVENTIONS.md     # Commit + authorship rules
│   │   ├── DEPLOY_SECRETS.md  # Secrets injection guide (Railway + Kamal)
│   │   ├── DOS_AND_DONTS.md   # Enforceable rule reference
│   │   └── MEMORY.md          # Long-term memory (ADRs + activity log)
│   └── skills/                # Project-scoped Claude skills
├── config/
│   └── deploy.yml             # Kamal 2 configuration
├── Dockerfile                 # Production image (multi-stage)
├── GEMINI.md                  # Technical context for Gemini-CLI
├── CLAUDE.md                  # Entry point for AI agents
└── README.md                  # This file
```

---

## 🚀 Deploy

The deploy workflow is entirely based on Kamal 2.

### Prerequisites

- A [Railway](https://railway.com/) account with a provisioned PostgreSQL service.
- A GitHub Personal Access Token with `write:packages` permission (for `ghcr.io`).
- The project's `RAILS_MASTER_KEY`.

### Steps

1. Configure local secrets (`.env`) and remote secrets (Railway) — see [`.claude/extras/DEPLOY_SECRETS.md`](./.claude/extras/DEPLOY_SECRETS.md).
2. Validate the configuration:
   ```bash
   kamal config
   ```
3. Prepare the remote server:
   ```bash
   kamal setup
   ```
4. Deploy:
   ```bash
   kamal deploy
   ```

---

## 📅 Current State

- [x] Stack defined (Rails 7.2.3.1).
- [x] Local environment and Gemini-CLI configured.
- [x] Optimized `Dockerfile` (multi-stage).
- [x] `config/deploy.yml` targeting Railway.
- [x] Secrets documentation (`.claude/extras/DEPLOY_SECRETS.md`).
- [x] Initial Rails boilerplate (`rails _7.2.3.1_ new .`).
- [ ] First successful deploy to Railway.
- [ ] Documented migration path to Rails 8.0.

---

## 🤝 Conventions

- **Code language:** English (variables, methods, classes, comments, commits).
- **Documentation language:** English. Historical Portuguese notes in `.claude/extras/MEMORY.md` are preserved as-is — new entries are added in English.
- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/) — full rules and the no-AI-attribution policy in [`.claude/extras/CONVENTIONS.md`](./.claude/extras/CONVENTIONS.md).
- **Forward compatibility:** avoid Rails 7.2 APIs already deprecated, to keep the path to 8.0 clean.

---

## 👤 Author

**Carlos Kvasir Lima** — personal lab for Rails internals and modern infrastructure.
