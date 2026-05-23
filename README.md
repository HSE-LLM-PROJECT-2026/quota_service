# Quota Service

## Описание

Этот репозиторий содержит сервис квотирования потребления инференса. Сервис хранит лимиты, текущие счетчики и решает, что делать с запросом при превышении квоты.

## Основные возможности
- создание и обновление квот
- block/throttle/warn поведение при превышении лимита
- проверка квоты перед инференсом
- запись факта потребления после ответа модели
- детализация потребления по моделям
- служебные health/livez/service-info ручки

## Структура проекта

- `app/` — основной код приложения
  - `main.py` — FastAPI-приложение и HTTP-ручки
  - `config.py` — настройки сервиса

- `deploy/` — файлы и переменные для развертывания
- `.env.example` — пример переменных окружения
- `Dockerfile` — сборка Docker-образа
- `pyproject.toml` — зависимости и настройки Python-проекта
- `requirements.txt` — список зависимостей для совместимого запуска без uv

## Быстрый старт локально

1. Установите зависимости:
   ```bash
   uv sync
   ```

2. Создайте `.env` на основе `.env.example`:
   ```bash
   cp .env.example .env
   ```

3. Запустите сервис:
   ```bash
   uv run uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```

Если `uv` не используется, можно запустить через обычный virtualenv:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

## Переменные окружения
- `DATABASE_URL`
- `SECURITY_SERVICE_URL`
- `SERVICE_TOKEN`
- `DEFAULT_BASE_DELAY_MS`
- `DEFAULT_MAX_DELAY_MS`
- `LOG_LEVEL`

Пример `.env`:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/llm_platform
SERVICE_TOKEN=change-me
LOG_LEVEL=INFO
```

## Основные API-ручки
- `GET /health`
- `GET /livez`
- `GET /service-info`
- `GET /quotas`
- `POST /quotas`
- `GET /quotas/{quota_id}`
- `PATCH /quotas/{quota_id}`
- `DELETE /quotas/{quota_id}`
- `POST /quotas/check`
- `POST /quotas/consume`

## Сборка и запуск в Docker

```bash
docker build -t hse-llm-project-2026/quota_service:local .
docker run --env-file .env -p 8000:8000 hse-llm-project-2026/quota_service:local
```

## Деплой в Kubernetes

Файлы развертывания лежат в папке `deploy/`. Для сервисов, которые уже подключены к стенду, используются Helm values и deploy-скрипты из соответствующего репозитория или общего инфраструктурного пайплайна.

## Метрики и документация

- Swagger UI: `/docs`
- OpenAPI: `/openapi.json`
- Health check: `/health`
- Liveness check: `/livez`

## Автор

Igor Malysh
