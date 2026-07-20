# Buy Tickets Orchestrator

Este repositório orquestra os microsserviços do projeto Buy Tickets com Docker Compose.

## Serviços

- `buy-ticket-api` (API Spring Boot)
- `buy-ticket-consumer` (consumidor da fila)
- `buy-ticket-front` (frontend Vue + Vite)
- `db` (MySQL)

## Pré-requisitos

- Git
- Docker
- Docker Compose (comando `docker compose`)

## Clonar os repositórios

Você pode clonar tudo de duas formas.

### Opção 1 (recomendada): clonar o orquestrador com submodules

```bash
git clone --recurse-submodules https://github.com/PauloAlmeidaBrasa/buy-tickets-orchestrator.git
cd buy-tickets-orchestrator
```

Se você já clonou sem submodules:

```bash
git submodule update --init --recursive
```

### Opção 2: clonar cada microsserviço manualmente

```bash
git clone https://github.com/PauloAlmeidaBrasa/buy-tickets-orchestrator.git
cd buy-tickets-orchestrator

git clone https://github.com/PauloAlmeidaBrasa/buy-tickets.git buy-ticket-api
git clone https://github.com/PauloAlmeidaBrasa/ticket-consumer-service.git buy-ticket-consumer
git clone https://github.com/PauloAlmeidaBrasa/buy-ticket-front.git buy-ticket-front
```

## Configuração de ambiente

Crie um arquivo `.env` em `buy-ticket-api/`.

Use o exemplo abaixo como ponto de partida e troque os valores de placeholder:

```dotenv
API_VERSION=v1

SPRING_DATASOURCE_URL=jdbc:mysql://db:3306/buy_tickets
SPRING_DATASOURCE_USERNAME=buy_tickets
SPRING_DATASOURCE_PASSWORD=buy_tickets_pwd

MYSQL_DATABASE=buy_tickets
MYSQL_USER=buy_tickets
MYSQL_PASSWORD=buy_tickets_pwd
MYSQL_ROOT_PASSWORD=your_root_password

DB_NAME=buy_tickets
DB_USERNAME=buy_tickets
DB_PASSWORD=buy_tickets_pwd

AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_SQS_QUEUE_NAME=ticket-purchase.fifo
AWS_SQS_QUEUE_URL=https://sqs.us-east-1.amazonaws.com/<account-id>/ticket-purchase.fifo
AWS_SQS_MESSAGE_GROUP_ID=ticket-reservation-group

JWT_SECRET=your_jwt_secret
JWT_EXPIRATION=3600000
```

Observações:

- O arquivo `docker-compose.dev.yml` já injeta as configurações de banco para o `ticket-consumer`.
- A URL da API no frontend pode ser configurada em `buy-ticket-front/.env` com:

```dotenv
VITE_API_BASE_URL=http://localhost:8082/api/v1/
```

## Subir os microsserviços

Na raiz do orquestrador, execute:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

Para rodar em modo detached:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build -d
```

## Parar os microsserviços

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml down
```

Para remover também os volumes (incluindo dados do MySQL):

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml down -v
```

## Comandos úteis

Ver logs de todos os serviços:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml logs -f
```

Ver logs de um serviço específico:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml logs -f buy-ticket-api
```

## URLs de acesso

- Frontend: `http://localhost:5173`
- API: `http://localhost:8082/api/v1`
- Swagger UI: `http://localhost:8082/v1/swagger`
