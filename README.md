# rails_lab

> Laboratório de experimentação profunda sobre **Ruby on Rails internals**, com foco no ciclo de vida da requisição, middlewares, callbacks e na transição arquitetural **Rails 7.2 → 8.0**.

[![Ruby](https://img.shields.io/badge/Ruby-3.3%2B-CC342D?logo=ruby)](https://www.ruby-lang.org/)
[![Rails](https://img.shields.io/badge/Rails-7.2.3.1-CC0000?logo=rubyonrails)](https://rubyonrails.org/)
[![Deploy](https://img.shields.io/badge/Deploy-Kamal%202-0E1116)](https://kamal-deploy.org/)
[![Target](https://img.shields.io/badge/Target-Railway-0B0D0E)](https://railway.com/)

---

## 🎯 Objetivo

`rails_lab` é um ambiente controlado, criado por **Carlos Kvasir Lima**, para testar os limites do Ruby on Rails e validar arquiteturas de deploy modernas. O laboratório serve para:

- Explorar o ciclo de vida de uma requisição Rails (Rack → middlewares → controller → callbacks).
- Documentar o caminho de upgrade **Rails 7.2 → Rails 8.0**.
- Validar um workflow de deploy *infra-as-code* com **Kamal 2** apontando para o **Railway**.
- Servir como referência prática de *clean code* e da filosofia "The Rails Way".

---

## 🧱 Stack Técnica

| Camada | Tecnologia |
|---|---|
| Linguagem | Ruby 3.3+ |
| Framework | Ruby on Rails **7.2.3.1** (alvo: 8.0) |
| Base de Dados | PostgreSQL (provisionado pelo Railway) |
| Containerização | Docker (multi-stage) |
| Deploy | Kamal 2 |
| Registry | GitHub Container Registry (`ghcr.io`) |
| Target | Railway |

---

## 🏛 Decisões de Arquitetura (ADRs)

As decisões estão documentadas em [`MEMORY.md`](./MEMORY.md). Resumo:

- **ADR-001 — Versão Rails:** começar em `7.2.3.1` (base estável) e documentar a migração para `8.0`.
- **ADR-002 — Estratégia de Deploy:** `Kamal 2 → Railway`, unindo a simplicidade do SaaS à *ownership* de infra que o Kamal entrega (zero-downtime, Docker-based).
- **ADR-003 — Workflow de IA:** Claude como orquestrador (contexto XML), Gemini-CLI como executor tático.

---

## 📂 Estrutura do Repositório

```
rails_lab/
├── config/
│   └── deploy.yml         # Configuração do Kamal 2
├── Dockerfile             # Imagem de produção (multi-stage)
├── DEPLOY_SECRETS.md      # Guia de injeção de secrets (Railway + Kamal)
├── GEMINI.md              # Contexto técnico para o Gemini-CLI
├── MEMORY.md              # Memória de longo prazo (ADRs + log)
└── README.md              # Este ficheiro
```

---

## 🚀 Deploy

O fluxo de deploy é totalmente baseado em Kamal 2.

### Pré-requisitos

- Conta no [Railway](https://railway.com/) com um serviço PostgreSQL provisionado.
- Personal Access Token do GitHub com permissão `write:packages` (para o `ghcr.io`).
- `RAILS_MASTER_KEY` do projeto.

### Passos

1. Configura os secrets locais (`.env`) e remotos (Railway) — ver [`DEPLOY_SECRETS.md`](./DEPLOY_SECRETS.md).
2. Valida a configuração:
   ```bash
   kamal config
   ```
3. Prepara o servidor remoto:
   ```bash
   kamal setup
   ```
4. Deploy:
   ```bash
   kamal deploy
   ```

---

## 📅 Estado Atual

- [x] Definição de stack (Rails 7.2.3.1).
- [x] Setup do ambiente local e Gemini-CLI.
- [x] `Dockerfile` otimizado (multi-stage).
- [x] `config/deploy.yml` para Railway.
- [x] Documentação de secrets (`DEPLOY_SECRETS.md`).
- [ ] Boilerplate Rails inicial (`rails new .`).
- [ ] Primeiro deploy bem-sucedido no Railway.
- [ ] Migração documentada para Rails 8.0.

---

## 🤝 Convenções

- **Idioma do código:** inglês (variáveis, métodos, classes, commits).
- **Idioma da documentação de laboratório:** português (este README, ADRs, notas).
- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/).
- **Compatibilidade:** evitar APIs depreciadas no Rails 7.2 que dificultem o upgrade para 8.0.

---

## 👤 Autor

**Carlos Kvasir Lima** — laboratório pessoal de Rails internals e infra moderna.
