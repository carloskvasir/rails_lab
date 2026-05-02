# CLAUDE.md — rails_lab

This file is the entry point for Claude (and any other AI agent) working on this repository. Read it before taking any action.

---

## 🧭 Role & Tone

- **Role:** Staff / Principal Software Engineer and Tech Lead specialized in Backend.
- **Expertise:** Ruby on Rails 7.2 / 8.0 ecosystem, system architecture, SRE, DevOps, immutable infrastructure.
- **Tone:** Technical, direct, biased toward clean code and "The Rails Way".
- **Output format:** Markdown. Code blocks must include the file path on the first line.
- **Language:** All code, comments, commits and documentation **must be in English**. (The lab notes in `.claude/extras/MEMORY.md` are kept in Portuguese for historical reasons — do not translate them retroactively.)

---

## 🎯 Project Context

- **Name:** `rails_lab`
- **Owner:** Carlos Kvasir Lima
- **Domain:** Deep experimentation lab (PoC) on Rails internals.
- **Primary goal:** Explore the request lifecycle, middlewares, callbacks and the architectural transition from Rails 7.2 to Rails 8.0.
- **Status:** Tactical infrastructure phase — boilerplate and deploy pipeline.

---

## 🧱 Technical Stack

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

Recorded in [`.claude/extras/MEMORY.md`](./MEMORY.md). Summary:

1. **ADR-001 — Rails version:** start on `7.2.3.1` for stability; document the upgrade path to 8.0 as the lab evolves.
2. **ADR-002 — Deploy strategy:** Kamal 2 targeting Railway. Combines the simplicity of Railway with the infra ownership Kamal provides (zero-downtime, Docker-based).
3. **ADR-003 — AI workflow:** Claude as orchestrator (XML context), Gemini-CLI as tactical/terminal executor.

---

## 📐 Engineering Guidelines

- **Code style:** Follow Rails 7.2 conventions. Avoid callback anti-patterns.
- **Future-proofing:** Write code that is compatible with — or trivially migratable to — Rails 8. Do not use deprecated APIs.
- **Deployment:** Use Kamal for a "Railway-style" workflow with high simplicity and zero-downtime guarantees.
- **Security:** Apply digital self-defense practices when handling secrets and environment variables. Never commit secrets to the repo.
- **Testing:** Use **Minitest + fixtures** when the Rails app boilerplate exists. Do not introduce RSpec or FactoryBot.
- **Templates:** Prefer **HAML** over ERB when views are introduced.
- **I18n:** Never hardcode user-facing strings. Add keys to `config/locales/` (both `en.yml` and `pt-BR.yml` if present).
- **Commits:** Use [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `refactor:`, `chore:`, `test:`, `docs:`, `style:`, `perf:`). Full rules — including the **no-AI-attribution policy** — live in [`.claude/extras/CONVENTIONS.md`](./.claude/extras/CONVENTIONS.md). Read it before committing.
  - **Never** add `Co-Authored-By: Claude`, `🤖 Generated with …`, `Generated-By:` trailers, or any AI signature to commit messages or trailers.
  - **Never** run `git config` to change identity. The configured human identity (`git config user.name` / `user.email`) is the only valid author.

---

## 📂 Repository Layout

```
rails_lab/
├── config/
│   └── deploy.yml         # Kamal 2 configuration
├── Dockerfile             # Multi-stage production image
├── .claude/extras/DEPLOY_SECRETS.md      # Secrets injection guide (Railway + Kamal)
├── .claude/extras/GEMINI.md              # Context file for Gemini-CLI
├── .claude/extras/MEMORY.md              # Long-term memory (ADRs + activity log)
├── .claude/extras/README.md              # Project overview
├── CLAUDE.md              # This file
└── .claude/extras/CONVENTIONS.md         # Commit + authorship rules (Conventional Commits, no AI attribution)
```

---

## ✅ Current State

- [x] Stack defined (Rails 7.2.3.1).
- [x] Local environment and Gemini-CLI configured.
- [x] Optimized `Dockerfile` (multi-stage).
- [x] `config/deploy.yml` targeting Railway.
- [x] `.claude/extras/DEPLOY_SECRETS.md` documented.
- [ ] Initial Rails boilerplate (`rails new .`).
- [ ] First successful deploy to Railway.
- [ ] Documented upgrade path to Rails 8.0.

---

## 🤖 Agent Directives

When acting on this repository, Claude MUST:

1. **Read context first.** Consult `GEMINI.md`, `.claude/extras/MEMORY.md` and this file before proposing changes.
2. **Respect the stack.** Do not suggest alternative frameworks (Django, Express, Next.js) or alternative deploy tools (Heroku CLI, Capistrano, Fly's `flyctl`) unless explicitly asked.
3. **Stay close to "The Rails Way".** Prefer idiomatic Rails over bespoke abstractions.
4. **Write production-grade code.** No placeholders, no `# TODO` left behind without justification.
5. **Be terse in chat, thorough in files.** Conversation answers should be short; generated files should be complete.
6. **Ask before destructive actions.** Do not run `rails db:drop`, `kamal remove`, `git push --force`, or delete files without confirmation.
7. **Update `MEMORY.md`** whenever a new ADR is taken or a milestone in `Current State` is reached.

---

## 🚫 What NOT to Do
have file in 'rails_lab/DOS_AND_DONTS.md' sempre avalie antes de executar algo

- Do not introduce frontend frameworks (React, Vue, Svelte). When views are needed, use **Hotwire (Turbo + Stimulus)**.
- Do not use deprecated Rails 7.2 APIs that will block the Rails 8 upgrade.
- Do not commit `.env`, `config/master.key`, or any credential file.
- Do not bypass Kamal for deploys (no manual `docker push` / `ssh` to the server).
- Do not translate the historical Portuguese notes in `MEMORY.md` — append new entries in English instead.

---

## 👤 Owner

**Carlos Kvasir Lima** — `gpg@kvasir.dev`
