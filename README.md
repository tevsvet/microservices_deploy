# microservices-deploy

Этот репозиторий хранит `docker compose`-сценарии для системы из нескольких отдельных репозиториев:
- https://github.com/tevsvet/eureka_server
- https://github.com/tevsvet/flow_manager
- https://github.com/tevsvet/file_conversion_service

## Что поднимается
- `compose/docker-compose.infra.yaml`
  Kafka, MinIO.
- `compose/docker-compose.platform.yaml`
  Eureka Server.
- `compose/docker-compose.apps.yaml`
  `flow-manager`, `file-conversion-service` и их отдельные Postgres.

## До запуска необходимо

Создать локальный `.env`:

macOS/Linux:
```bash
cp .env.example .env
```

Windows PowerShell:
```powerShell
Copy-Item .env.example .env
```

## Порядок запуска

1. Поднять инфраструктуру:

```bash
docker compose --env-file .env -f compose/docker-compose.infra.yaml up -d
```

2. Поднять Eureka:

```bash
docker compose --env-file .env -f compose/docker-compose.platform.yaml up -d
```

3. Поднять прикладные сервисы:

```bash
docker compose --env-file .env -f compose/docker-compose.apps.yaml up -d
```

## Остановка

```bash
docker compose --env-file .env -f compose/docker-compose.apps.yaml down
docker compose --env-file .env -f compose/docker-compose.platform.yaml down
docker compose --env-file .env -f compose/docker-compose.infra.yaml down
```

## Логи

```bash
docker compose --env-file .env -f compose/docker-compose.apps.yaml logs -f flow-manager
docker compose --env-file .env -f compose/docker-compose.apps.yaml logs -f file-conversion-service
docker compose --env-file .env -f compose/docker-compose.platform.yaml logs -f eureka-server
```
