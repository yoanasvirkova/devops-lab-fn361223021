# 2. Използваме PostgreSQL за съхранение на бележките

## Status

Accepted

## Context

Приложението пазеше бележките в паметта и ги губеше при рестарт.
Нужна е постоянна база, която тръгва заедно с приложението в Docker Compose.

## Decision

Използваме PostgreSQL 17 (образ `postgres:17-alpine`) като услуга `db` в `compose.yaml`.
Данните са в именуван volume `pgdata`. Приложението се свързва чрез `DATABASE_URL`,
а тайните са в `.env`, който не се commit-ва.

## Consequences

- Бележките оцеляват при `docker compose down` и рестарт.
- `app` чака базата да е healthy (`depends_on` с `service_healthy`).
- Нужен е още един контейнер, а `docker compose down -v` трие всички данни.