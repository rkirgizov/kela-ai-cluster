# kela-ai-cluster

Кластер AI-сервисов для развертывания LLM-инференса, генерации контента и RAG-чатбота.

## Архитектура

| Сервис | Назначение | Порт |
|--------|------------|------|
| **Ollama** | LLM-инференс (AMD GPU) | 11434 |
| **ComfyUI** | Генерация изображений/контента | 8188 |
| **Qdrant** | Vector DB для RAG | 6333 |
| **AnythingLLM** | Фронтенд-приложение для чатбота | 32001 |

## Требования

- Docker + Docker Compose
- Kubernetes (для продакшн-деплоя)
- AMD GPU с поддержкой ROCm
- `HSA_OVERRIDE_GFX_VERSION=12.0.0` (для GPU gfx1200)

## Быстрый старт (Docker Compose)

```bash
# Запуск
docker compose up -d

# Проверка статуса
docker compose ps

# Остановка
docker compose down
```

Сервисы станут доступны:
- Ollama API: `http://localhost:11434`
- ComfyUI UI: `http://localhost:8188`

## Деплой в Kubernetes

```bash
# Применить манифесты
kubectl apply -f k8s/

# Проверка
kubectl get pods
kubectl get services

# Удалить
kubectl delete -f k8s/
```

Сервисы станут доступны:
- Qdrant: `kubectl port-forward svc/kela-qdrant-service 6333:6333`
- AnythingLLM: `http://<node-ip>:32001`

## Конфигурация GPU

Для AMD GPU используется ROCm-образы и переменная `HSA_OVERRIDE_GFX_VERSION=12.0.0`.

## Структура

```
├── docker-compose.yaml    # Локальный запуск
├── k8s/
│   ├── qdrant.yaml        # Vector DB
│   └── anythingllm.yaml   # Чатбот-фронтенд
└── README.md
```
