# N8n Docker Config — Queue Mode

Este repositório contém uma configuração Docker Compose para executar o n8n em modo "queue" (com workers), usando PostgreSQL como banco de dados e Redis como backend de filas.

**Documentação oficial:**
- Guia de Queue Mode: https://docs.n8n.io/hosting/scaling/queue-mode/
- Variáveis de ambiente (Queue Mode): https://docs.n8n.io/hosting/configuration/environment-variables/queue-mode/
- Chave de criptografia (exemplos): https://docs.n8n.io/hosting/configuration/configuration-examples/encryption-key/
- Exemplo no repositório de docs do n8n: https://github.com/n8n-io/n8n-docs/blob/main/docs/hosting/configuration/configuration-examples/encryption-key.md

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

**Como rodar (PowerShell no Windows):**
1. Abra um terminal PowerShell na pasta do projeto (`c:\Users\...\n8n-devops`).
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

Se quiser, eu posso subir os containers agora ou validar a configuração localmente — quer que eu execute os comandos para você? 

**Imagens / Capturas**

Você pode adicionar diagramas e capturas de tela ao `README` criando uma pasta `images/` na raiz do projeto e colocando os arquivos lá. Recomendações de boas práticas:
- Use nomes simples, minúsculos e com hífen: `diagrama-arquitetura.png`, `screenshot-workflow-1.png`.
- Adicione texto alternativo (`alt`) para acessibilidade.

Exemplos de como referenciar as imagens no Markdown:

```
![Diagrama de arquitetura](images/diagrama-arquitetura.png)
```

Se quiser controlar o tamanho diretamente no `README` (ou para visualização no GitHub), use HTML:

```
<img src="images/screenshot-workflow-1.png" alt="Captura do workflow" width="700" />
```

Sugestão de seções para organizar imagens:
- **Diagrama de arquitetura** — coloque aqui um diagrama explicando componentes (Postgres, Redis, n8n-main, n8n-worker).
- **Capturas de tela** — coloque screenshots da UI do n8n, exemplos de workflows ou logs.

Após adicionar as imagens, faça commit normalmente:

```
git add images/* README.md
git commit -m "docs: adicionar placeholders e instruções para imagens"
git push
```

Se quiser, crio a pasta `images/` vazia e adiciono um arquivo `.gitkeep` para você começar — quer que eu faça isso?