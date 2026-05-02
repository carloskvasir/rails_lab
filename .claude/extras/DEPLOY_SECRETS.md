# Gestão de Secrets e Deploy (Kamal 2 + Railway)

Este guia descreve como configurar os segredos necessários para o deploy do `rails_lab`.

## 1. Configuração Local (.env)

O Kamal 2 lê variáveis de ambiente do teu ficheiro `.env` local. Cria um ficheiro `.env` com o seguinte:

```env
GITHUB_USER=teu-utilizador-github
KAMAL_REGISTRY_PASSWORD=teu-token-github
DEPLOY_IP=ip-do-teu-servidor
DOMAIN=teu-dominio.com
RAILS_MASTER_KEY=valor-da-tua-master-key
DATABASE_URL=postgres://utilizador:password@host-do-railway:5432/db-name
```

## 2. Configuração no Railway

Garante que no painel do Railway:
- O acesso externo está ativo.
- Tens a `DATABASE_URL` correta.

## 3. Comandos de Inicialização e Deploy

```bash
# Valida a configuração
kamal config

# Prepara o servidor
kamal setup

# Deploy
kamal deploy
```

## 4. Verificação

```bash
# Logs
kamal app logs

# Detalhes
kamal details
```
