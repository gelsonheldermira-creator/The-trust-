# THE TRUST — publicação na Internet (guia pelo telemóvel)

Este pacote tem uma configuração inicial para publicar o site e a API no mesmo serviço Render, com HTTPS e armazenamento persistente para a base de dados SQLite. **A publicação não acontece automaticamente ao descarregar o ZIP**: é necessário ter conta de alojamento e colocar os ficheiros num repositório Git.

## Antes de começar
- Conta GitHub e conta Render, abertas pelo navegador do telemóvel.
- Os ficheiros deste pacote extraídos para uma pasta.
- Dois segredos aleatórios diferentes, com pelo menos 32 caracteres cada, gerados num gestor de palavras-passe. Não os reutilizes e não os publiques no GitHub.
- A configuração incluída usa um serviço Render pago (`starter`) porque o banco de dados precisa de armazenamento persistente. Confirma o preço atual diretamente no Render antes de confirmar a criação. Não é necessário comprar domínio no primeiro teste.

## Passos
1. No GitHub, cria um repositório novo chamado `the-trust`. Mantém-no **privado** enquanto estiveres a preparar o projeto.
2. Carrega para o repositório o conteúdo desta pasta, incluindo `render.yaml`, `Dockerfile.production`, `deploy/`, `the_trust_site/` e `the_trust_backend/`. Não carregues ficheiros `.env` com segredos. O ficheiro `.env.example` é apenas um modelo.
3. No Render, escolhe **New → Blueprint** e liga o repositório `the-trust`.
4. O Render lê o `render.yaml`. Confirma o serviço `the-trust`, região Frankfurt, plano e disco persistente antes de aprovar. Se não quiseres assumir custos, cancela aqui e não publiques ainda.
5. Quando o Render pedir `JWT_SECRET` e `WEBHOOK_SECRET`, define dois valores longos, aleatórios e diferentes. Guarda-os num gestor de palavras-passe. Nunca os coloques no HTML, no GitHub ou em mensagens.
6. Aguarda o build e a publicação. O endereço criado pelo Render deve abrir a página inicial. O teste técnico de saúde é o endereço do site seguido de `/api/health`; deve mostrar `status: ok` e `payments_enabled: false`.
7. No painel do serviço Render, abre o Shell/Terminal se estiver disponível e executa `python scripts/create_admin.py`. Introduz o email e uma palavra-passe forte (mínimo 16 caracteres) para criar a primeira conta de administrador. Guarda-a com segurança. Se o teu plano não disponibilizar Shell, não tentes contornar o acesso: pede suporte ou executa o script num ambiente seguro com acesso à mesma base de dados.
8. Entra no site com a conta de administrador. Regista uma conta de vendedor de teste, verifica-a no painel admin, entra como vendedor e submete um produto com um URL HTTPS que controles. Aprova o produto com o admin. Depois testa uma conta de afiliado e o link gerado.

## O que está e não está ativo
- Site e API servidos pelo mesmo domínio; HTTPS é fornecido pelo alojamento.
- Registo/login, submissão e aprovação de produtos, links de afiliado e registo de cliques estão previstos nesta versão.
- A confirmação automática de vendas, cálculo automático de comissões a partir de compras reais e pagamentos **continuam desligados**. Não prometas levantamentos nem aceites dinheiro através da THE TRUST.
- Antes de convidar o público, confirma os testes, segurança, backups, termos e privacidade. O primeiro administrador é criado manualmente; não existe recuperação de palavra-passe nem verificação de email nesta versão.

## Se algo falhar
- `Build failed`: abre os logs do Render e confirma que todos os ficheiros e pastas do repositório foram enviados.
- `Application failed to respond`: confirma `JWT_SECRET` e `WEBHOOK_SECRET` com pelo menos 32 caracteres e consulta os logs do serviço.
- `status: ok` não aparece: abre os logs da API e do Nginx. Não partilhes segredos nem tokens com terceiros.
- Não apagues o disco persistente: contém a base de dados do site.

## Custos
O domínio é opcional para o primeiro teste. O plano com disco persistente pode gerar custos recorrentes; verifica o preço atual do alojamento antes de confirmar. O pacote não inclui pagamentos a fornecedores, domínio pago, email transacional ou integração comercial de pagamentos.
