Lê o contexto fornecido no ficheiro GEMINI.md e assume o teu papel de Staff/Principal Software Engineer e Tech Lead.

Vamos iniciar a fase tática da nossa infraestrutura. O objetivo agora é garantir que o nosso ambiente de deploy com Kamal 2 para o Railway fica sólido e focado no Rails 7.2.3.1.

Por favor, executa rigorosamente os seguintes passos:

**Passo 1: Dockerfile Otimizado**
Gera o ficheiro `Dockerfile` otimizado para produção para o Rails 7.2.3.1.
- Utiliza a imagem oficial do Ruby mais leve e adequada.
- Foca na eficiência (multi-stage build, se fizer sentido para o tamanho da imagem).
- Garante que a estrutura não cria dívida técnica para a futura migração para o Rails 8.

**Passo 2: Configuração do Kamal 2 (deploy.yml)**
Gera o ficheiro `config/deploy.yml` otimizado para o Railway.
- Configura o registo de contentores (assume que vamos usar o GitHub Container Registry - ghcr.io).
- Assume que a base de dados PostgreSQL já está a correr no Railway.
- Configura a injeção de variáveis de ambiente de forma segura, sem expor os valores no código (usa a sintaxe do Kamal para ler do `.env` local ou das variáveis do Railway).

**Passo 3: Documentação de Injeção de Secrets**
Gera um pequeno ficheiro chamado `DEPLOY_SECRETS.md`.
- Explica de forma concisa como devo configurar os secrets (como a `DATABASE_URL`, `RAILS_MASTER_KEY` e o `KAMAL_REGISTRY_PASSWORD`) localmente e no painel do Railway.
- Indica o comando exato do Kamal que devo utilizar para inicializar e validar o setup no servidor remoto.

Responde apenas com os blocos de código dos ficheiros solicitados, indicando o caminho/nome de cada ficheiro na primeira linha de cada bloco.
