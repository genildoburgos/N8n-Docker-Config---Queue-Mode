# N8n Docker Config — Queue Mode

Este repositório contém uma configuração Docker Compose para executar o n8n em modo "queue" (com workers), usando PostgreSQL como banco de dados e Redis como backend de filas.

**Objetivo:**
- Fornecer uma configuração mínima e reutilizável para executar o n8n em produção/local com execução de workflows em fila (separando o serviço principal dos workers).

**O que o projeto roda:**
- **Postgres**: armazena dados do n8n.
- **Redis**: usado pelo Bull (fila) para enfileirar execuções.
- **n8n-main**: container principal (UI + API) exposto em `http://localhost:5678`.
- **n8n-worker**: worker que processa execuções offloadadas.

**Arquivo `.env`** (exemplo presente no repositório):
- `N8N_ENCRYPTION_KEY` : chave de criptografia do n8n (ex.: `jNQU76AbfBu4T46nyzeZJTcVz2WgBn4X`). Altere para uma chave forte e secreta.
- `POSTGRES_USER` : usuário do Postgres (ex.: `n8n`).
- `POSTGRES_PASSWORD` : senha do Postgres (ex.: `n8n_pass`).
- `POSTGRES_DB` : nome do banco (ex.: `n8n`).

Coloque o arquivo `.env` na raiz do projeto (mesma pasta de `docker-compose.yml`). O `docker-compose` irá carregar essas variáveis automaticamente.

Veja também (documentação oficial):
- Variáveis de ambiente para Queue Mode: https://docs.n8n.io/hosting/configuration/environment-variables/queue-mode/
- Como gerar/usar a `N8N_ENCRYPTION_KEY`: https://docs.n8n.io/hosting/configuration/configuration-examples/encryption-key/  
- Exemplo no repositório de documentação do n8n: https://github.com/n8n-io/n8n-docs/blob/main/docs/hosting/configuration/configuration-examples/encryption-key.md

**Como rodar:**
1. Abra um terminal na pasta do projet.
2. Suba os serviços em segundo plano:

```
docker-compose up -d
```

3. Verifique os containers:

```
docker-compose ps
```

4. Acompanhe logs (ex.: do serviço principal):

```
docker-compose logs -f n8n-main
```

5. Para parar e remover containers/volumes (quando necessário):

```
docker-compose down -v
```

**Acessar interface do n8n**
- URL: `http://localhost:5678`

**Persistência**
- Os volumes declarados em `docker-compose.yml` (`n8n_data`, `postgres_data`, `redis_data`) mantêm os dados entre reinícios.

**Notas de segurança / recomendações**
- Troque `POSTGRES_PASSWORD` e `N8N_ENCRYPTION_KEY` para valores fortes antes de usar em produção.
- A `N8N_ENCRYPTION_KEY` deve ser mantida secreta e faz parte da segurança de credenciais do n8n.
- Revise permissões e a opção `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS` conforme o ambiente (Windows vs Linux pode se comportar diferente).

**Problemas comuns**
- Se os serviços não quiserem subir, verifique se as portas estão livres e se o arquivo `.env` existe e tem os valores corretos.
- Use `docker-compose logs` para diagnosticar problemas de inicialização e healthchecks.
