# Raízes do Nordeste – Back-end (API RESTful)

API RESTful multicanal para a rede de lanchonetes **Raízes do Nordeste**, desenvolvida como Atividade Prática do Projeto Multidisciplinar (trilha Back-end, UNINTER).

**Autora:** Eriselma Sena Moraes Flores – RU 4514768

> Os trechos marcados com ⚠️ **CONFIRMAR** dependem do código do projeto (porta, comandos, banco). Ajuste-os antes de publicar.

## Fluxo implementado (MVP)

**Fluxo A: Pedido → Pagamento mock → Atualização de status**

1. `POST /api/v1/pedidos` cria o pedido (valida `canalPedido`, estoque da unidade e calcula o total no servidor). O pedido nasce `AGUARDANDO_PAGAMENTO`.
2. `POST /api/v1/pagamentos/processar` envia a cobrança ao gateway simulado. Aprovado → `RECEBIDO`. Negado → `CANCELADO`, com devolução do estoque.
3. `PATCH /api/v1/pedidos/{id}/status` movimenta a fila da cozinha.
4. `GET /api/v1/pedidos?canalPedido=TOTEM` consulta pedidos por canal (paginado).

Canais: `APP`, `TOTEM`, `BALCAO`, `PICKUP`, `WEB`.

## Requisitos

- Node.js ⚠️ **CONFIRMAR versão** (ex.: 20 LTS ou superior)
- npm
- Banco de dados relacional ⚠️ **CONFIRMAR** (ex.: PostgreSQL 15 ou superior)
- Prisma ORM (migrations e acesso a dados)

## Como executar

```bash
# 1. Clonar o repositório
git clone https://github.com/selma29091979flores/Raizes-do-nordeste-Final.git
cd Raizes-do-nordeste-Final

# 2. Instalar as dependências
npm install

# 3. Configurar as variáveis de ambiente
cp .env.example .env      # no Windows: copy .env.example .env
# edite o .env com a conexão do banco e o JWT_SECRET

# 4. Criar o banco, aplicar as migrations e carregar os dados iniciais (seed)
npx prisma migrate dev
npx prisma db seed

# 5. Iniciar a API
npm run dev               # ⚠️ CONFIRMAR o script (dev/start)
```

A API sobe em `http://localhost:3000` (porta definida em `PORT`).

## Documentação (Swagger/OpenAPI)

Com a API em execução, acesse: **http://localhost:3000/api-docs** ⚠️ **CONFIRMAR a rota do Swagger**

O Swagger descreve os endpoints implementados, com exemplos de request/response e códigos de status.

## Testes (Postman)

1. Importe o arquivo `Lanchonete_Postman_Collection.json` (raiz do repositório) no Postman.
2. Execute a pasta **01. Auth** primeiro: o script captura o `jwt_token` automaticamente.
3. Execute as demais pastas na ordem: `02. Validacoes`, `03. Pedidos`, `04. Pagamento`, `05. Cozinha`.

São 11 cenários (positivos e negativos), incluindo autenticação, autorização, validações, estoque insuficiente e pagamento negado.

Usuário de teste (seed) ⚠️ **CONFIRMAR**: `atendente.centro@lanchonete.com` / `SenhaSegura123`

## Arquitetura

Quatro camadas, com dependências apontando para o domínio:

| Camada | Responsabilidade |
|---|---|
| **Domain** | Entidades, enumerados e regras de negócio (`Pedido`, `Estoque`, `CanalPedido`, `StatusPedido`) |
| **Application** | Casos de uso (criar pedido, processar pagamento, atualizar status, autenticar) |
| **Infrastructure** | Prisma, migrations, BCrypt, JWT, logs e mock do gateway de pagamento |
| **API** | Rotas, middlewares (JWT/perfil), validação e Swagger |

## Segurança e LGPD

- Senhas armazenadas com **BCrypt** (custo 12); o hash nunca é retornado nas respostas.
- Autenticação **JWT** (HMAC-SHA256, validade de 3600 s) e autorização por perfil: `CLIENTE`, `ATENDENTE`, `COZINHA`, `GERENTE`, `ADMIN`.
- Coleta mínima de dados (nome, e-mail, senha em hash) e consentimento registrado com data.
- Logs de auditoria estruturados, sem dados pessoais.
- Erros em formato JSON padronizado (inspirado na RFC 7807).

## Diagramas e modelo de dados

Na pasta [`docs/`](docs):

- `DER.png` – modelo entidade-relacionamento
- `diagrama-casos-de-uso.png`
- `diagrama-de-classes.png`
- `diagrama-de-sequencia.png`

## Evidências da entrega

| Item | Localização |
|---|---|
| Repositório | https://github.com/selma29091979flores/Raizes-do-nordeste-Final |
| Swagger/OpenAPI | http://localhost:3000/api-docs ⚠️ **CONFIRMAR** |
| Coleção Postman | `Lanchonete_Postman_Collection.json` (raiz) |
| DER | `docs/DER.png` |
