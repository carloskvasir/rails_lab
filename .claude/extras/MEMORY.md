# MEMORY - Projeto rails_lab

Este arquivo serve como a memória de longo prazo para agentes de IA (Claude/Gemini) que interagem com este repositório.

## 📝 Visão Geral
`rails_lab` é um ambiente controlado criado por Carlos Kvasir Lima para testar os limites do Ruby on Rails e validar arquiteturas de deploy modernas.

## 🛠 Decisões de Arquitetura (ADRs)

### 001: Versão do Rails e Upgrade Path
- **Data:** 2026-05-01
- **Decisão:** Iniciar no Rails 7.2.3.1 em vez de ir direto para o 8.0.
- **Racional:** Garantir que o laboratório comece em uma base 100% estável e segura, documentando o processo de migração para o Rails 8 conforme o lab evolui.

### 002: Estratégia de Deploy
- **Data:** 2026-05-01
- **Decisão:** Usar Kamal 2 apontando para o Railway.
- **Racional:** Unir a facilidade do SaaS do Railway com a flexibilidade e "ownership" de infraestrutura que o Kamal proporciona (Zero downtime, Docker-based).

### 003: Stack de IA / Workflow de Engenharia
- **Data:** 2026-05-01
- **Decisão:** Claude como orquestrador (via XML context) e Gemini-CLI como executor tático/validador de terminal.

## 🚀 Estado do Deploy
- **Target:** Railway.
- **Base:** Dockerfile padrão Rails 7.2 (Production-ready).
- **Secrets:** Gerenciados via Railway Variables e injetados via Kamal env.

## 📅 Log de Atividades
- **2026-05-01:** Inicialização do projeto, configuração do `GEMINI.md` e `MEMORY.md`. Configuração do `gemini-cli`.
