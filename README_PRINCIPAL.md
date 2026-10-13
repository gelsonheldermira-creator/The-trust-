# THE TRUST — site e API (versão de lançamento técnico inicial)

Plataforma de afiliados para Portugal (EUR) e Angola (AOA/Kz), com interface responsiva, registo/login, catálogo, painel de afiliado, painel de vendedor e painel de administração.

## Começar
- **Queres publicar pelo telemóvel?** Lê primeiro `GUIA_PUBLICACAO_RENDER.md`.
- **Queres testar no teu computador com Docker?** Lê este documento e `docker-compose.yml`.
- **Queres validar o fluxo manualmente?** Segue `docs/SMOKE_TEST.md`.

## Publicação preparada
- `render.yaml`: configuração inicial de publicação num serviço Render.
- `Dockerfile.production`: executa API FastAPI e Nginx no mesmo serviço.
- `deploy/nginx.production.conf`: serve o site e encaminha `/api/` para a API.
- `deploy/supervisord.conf`: mantém os dois processos ativos.
- O modelo Render prevê armazenamento persistente em `/data` para não perder a base de dados em reinícios normais. Confirma sempre os custos atuais do plano e do disco.

## Teste local com Docker Compose
1. Copia `.env.example` para `the_trust_backend/.env`.
2. Substitui `JWT_SECRET` e `WEBHOOK_SECRET` por segredos diferentes, fortes e com pelo menos 32 caracteres.
3. Na pasta do projeto, executa `docker compose up --build -d`.
4. Abre `http://localhost:8080` e verifica `http://127.0.0.1:8000/health`.
5. Cria o primeiro administrador com `docker compose exec api python scripts/create_admin.py`.

## Funções desta versão
- Registo/login com palavras-passe protegidas por hash e tokens JWT.
- País e moeda do perfil (Portugal/EUR; Angola/AOA).
- Vendedores submetem produtos HTTPS; a conta precisa de verificação do administrador.
- Administração aprova produtos e verifica vendedores.
- Afiliados criam links; o sistema regista cliques e redireciona para a loja.
- Painéis de utilizador, vendedor e administrador; registos de auditoria.

## Limites importantes
Esta é uma **versão técnica inicial pronta para testes e preparada para alojamento**, não uma plataforma comercial totalmente validada. Não foi publicada na Internet por este pacote. Não estão ativos confirmação automática de vendas, cálculo automático de comissões a partir de vendas reais, nem pagamentos. Ainda faltam verificação de email, recuperação de palavra-passe, limites contra abuso, revisão de segurança independente, migrações/estratégia robusta de base de dados, backups testados, política final de privacidade/termos e integração aprovada com um prestador de pagamentos.

Não recolhas dados de cartões bancários no site. Só liga pagamentos depois de o prestador aprovar a entidade, o modelo comercial e os países dos vendedores/beneficiários. Usa contas e produtos fictícios nos testes.
