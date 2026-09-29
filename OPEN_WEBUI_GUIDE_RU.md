# Итоговая инструкция по использованию Open WebUI

## 📋 Оглавление

1. [Что такое Open WebUI](#что-такое-open-webui)
2. [Основные возможности](#основные-возможности)
3. [Установка](#установка)
4. [Настройка окружения](#настройка-окружения)
5. [Первые шаги после установки](#первые-шаги-после-установки)
6. [Работа с моделями](#работа-с-моделями)
7. [Основные функции интерфейса](#основные-функции-интерфейса)
8. [Продвинутые возможности](#продвинутые-возможности)
9. [Администрирование](#администрирование)
10. [Безопасность](#безопасность)
11. [Решение проблем](#решение-проблем)

---

## Что такое Open WebUI

**Open WebUI** — это расширяемая, функциональная и удобная самодостаточная AI-платформа, предназначенная для работы полностью офлайн. Она поддерживает различные LLM-раннеры, включая **Ollama** и **OpenAI-совместимые API**, со встроенным движком вывода для RAG (Retrieval Augmented Generation).

### Ключевые преимущества:
- 🚀 Простая установка через pip, Docker или Kubernetes
- 🤝 Поддержка широкого спектра моделей и API
- 🔐 Детальный контроль доступа (RBAC)
- 🧩 Система плагинов для расширения функциональности
- 📱 Адаптивный дизайн с поддержкой PWA

---

## Основные возможности

### 🎨 Интерфейс и взаимодействие
- **Дизайн**: Полностью переработанный интерфейс с узкой колонкой диалогов
- **Markdown и LaTeX**: Полная поддержка форматирования
- **Мультиязычность**: Поддержка i18n с возможностью добавления языков
- **PWA**: Progressive Web App для установки как нативного приложения

### 🤖 Работа с моделями
- **Поддержка провайдеров**: Ollama, OpenAI, LMStudio, GroqCloud, Mistral, OpenRouter, vLLM
- **Мульти-модельные разговоры**: Одновременное использование нескольких моделей
- **Агенты**: Создание специализированных агентов с инструментами и знаниями
- **Память**: Постоянная память о фактах между разговорами
- **Суб-агенты**: Делегирование задач фоновым помощникам

### 📁 Управление контентом
- **Чаты**: Организация в папки с сортировкой и предпросмотром
- **Заметки**: Отдельное пространство для контента с AI-редактированием
- **Каналы**: Общие пространства для командной работы с AI
- **Календарь**: Планирование встреч и событий с AI-ассистентом
- **Автоматизации**: Расписание повторяющихся задач

### 🔍 Поиск и RAG
- **Локальный RAG**: 9 векторных баз данных (ChromaDB, PGVector, Qdrant, Milvus и др.)
- **Веб-поиск**: Интеграция с SearXNG, Google PSE, Brave Search, DuckDuckGo и др.
- **Просмотр веб-страниц**: Загрузка сайтов прямо в чат
- **Извлечение контента**: Поддержка Tika, Docling, Mistral OCR, PaddleOCR-vl

### 🎨 Генерация изображений
- **Движки**: DALL·E, Gemini, ComfyUI, AUTOMATIC1111
- **Редактирование**: Изменение изображений по текстовому описанию

### 🎤 Голосовое управление
- **Распознавание речи**: Local Whisper, OpenAI, Deepgram, Azure
- **Синтез речи**: Azure, ElevenLabs, OpenAI, Transformers, WebAPI
- **Видеозвонки**: Интегрированные голосовые и видео вызовы

### 👥 Совместная работа
- **RBAC**: Гранулярные роли и группы пользователей
- **Общий доступ**: К чатам, папкам, заметкам с контролем прав
- **LDAP/AD**: Интеграция с корпоративными каталогами
- **SSO**: Единый вход через OAuth провайдеров
- **SCIM 2.0**: Автоматическая синхронизация пользователей

---

## Установка

### Способ 1: Установка через pip (Python)

**Требования**: Python 3.11

```bash
# Установка
pip install open-webui

# Запуск сервера
open-webui serve
```

Сервер будет доступен по адресу: http://localhost:8080

### Способ 2: Docker (рекомендуется)

#### Базовая установка с Ollama на том же хосте:

```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

#### С подключением к удалённому Ollama:

```bash
docker run -d \
  -p 3000:8080 \
  -e OLLAMA_BASE_URL=https://example.com \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

#### С поддержкой NVIDIA GPU:

```bash
docker run -d \
  -p 3000:8080 \
  --gpus all \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:cuda
```

#### Только для OpenAI API:

```bash
docker run -d \
  -p 3000:8080 \
  -e OPENAI_API_KEY=your_secret_key \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

#### С встроенным Ollama (всё в одном контейнере):

**С GPU:**
```bash
docker run -d \
  -p 3000:8080 \
  --gpus=all \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama
```

**Без GPU:**
```bash
docker run -d \
  -p 3000:8080 \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama
```

### Способ 3: Docker Compose

Создайте файл `docker-compose.yaml`:

```yaml
services:
  ollama:
    volumes:
      - ollama:/root/.ollama
    container_name: ollama
    pull_policy: always
    tty: true
    restart: unless-stopped
    image: ollama/ollama:latest

  open-webui:
    build:
      context: .
      dockerfile: Dockerfile
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    volumes:
      - open-webui:/app/backend/data
    depends_on:
      - ollama
    ports:
      - 3000:8080
    environment:
      - 'OLLAMA_BASE_URL=http://ollama:11434'
      - 'WEBUI_SECRET_KEY='
    extra_hosts:
      - host.docker.internal:host-gateway
    restart: unless-stopped

volumes:
  ollama: {}
  open-webui: {}
```

Запуск:
```bash
docker-compose up -d
```

#### С поддержкой GPU (дополнительно):

Создайте `docker-compose.gpu.yaml`:
```yaml
services:
  ollama:
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities:
                - gpu
```

Запуск с GPU:
```bash
docker-compose -f docker-compose.yaml -f docker-compose.gpu.yaml up -d
```

### Способ 4: Нативная установка (для разработки)

```bash
# Клонируйте репозиторий
git clone https://github.com/open-webui/open-webui.git
cd open-webui

# Установите зависимости
pip install -r backend/requirements.txt

# Запустите в режиме разработки
cd backend
python -m open_webui.dev
```

---

## Настройка окружения

### Основные переменные окружения

#### Подключение к моделям

```bash
# Ollama
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_BASE_URLS=http://localhost:11434;http://server2:11434  # Несколько серверов

# OpenAI и совместимые API
OPENAI_API_KEY=sk-your-key-here
OPENAI_API_KEYS=sk-key1;sk-key2  # Несколько ключей
OPENAI_API_BASE_URLS=https://api.openai.com/v1;https://other-provider.com/v1

# RAG OpenAI (для эмбеддингов)
RAG_OPENAI_API_KEY=your-key
RAG_OLLAMA_BASE_URL=http://localhost:11434
```

#### База данных

```bash
# SQLite (по умолчанию)
DATABASE_URL=sqlite:///./webui.db

# PostgreSQL
DATABASE_TYPE=postgresql
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=openwebui
DATABASE_USER=postgres
DATABASE_PASSWORD=yourpassword

# SQLCipher (шифрованная SQLite)
DATABASE_URL=sqlite+sqlcipher:///./webui.db
```

#### Безопасность

```bash
# Секретный ключ (обязательно для production!)
WEBUI_SECRET_KEY=your-very-long-random-secret-key-here

# Включение/выключение аутентификации
WEBUI_AUTH=true

# Данные администратора (для первого входа)
WEBUI_ADMIN_EMAIL=admin@example.com
WEBUI_ADMIN_PASSWORD=securepassword
WEBUI_ADMIN_NAME=Admin
```

#### Хранилище файлов

```bash
# Локальное хранилище (по умолчанию)
STORAGE_PROVIDER=local

# Amazon S3
STORAGE_PROVIDER=s3
S3_ACCESS_KEY_ID=your-access-key
S3_SECRET_ACCESS_KEY=your-secret-key
S3_REGION_NAME=us-east-1
S3_BUCKET_NAME=your-bucket
S3_ENDPOINT_URL=https://s3.amazonaws.com

# Google Cloud Storage
STORAGE_PROVIDER=gcs
GCS_BUCKET_NAME=your-bucket
GOOGLE_APPLICATION_CREDENTIALS_JSON={"type":"service_account",...}

# Azure Blob Storage
STORAGE_PROVIDER=azure
AZURE_STORAGE_ENDPOINT=https://account.blob.core.windows.net
AZURE_STORAGE_CONTAINER_NAME=container
AZURE_STORAGE_KEY=your-key
```

#### Векторные базы данных

```bash
# ChromaDB (по умолчанию)
RAG_EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2

# Qdrant
VECTOR_DB=qdrant
QDRANT_HOST=localhost
QDRANT_PORT=6333

# PGVector
VECTOR_DB=pgvector
PGVECTOR_HOST=localhost
PGVECTOR_PORT=5432

# Milvus
VECTOR_DB=milvus
MILVUS_HOST=localhost
MILVUS_PORT=19530
```

#### Веб-поиск

```bash
# SearXNG
ENABLE_RAG_WEB_SEARCH=true
RAG_WEB_SEARCH_ENGINE=searxng
SEARXNG_QUERY_URL=http://localhost:8080/search?q=<query>

# Google Programmable Search Engine
RAG_WEB_SEARCH_ENGINE=google_pse
GOOGLE_PSE_API_KEY=your-api-key
GOOGLE_PSE_ENGINE_ID=your-engine-id

# Другие поддерживаемые движки:
# brave_search, kagi, tavily, perplexity, firecrawl, duckduckgo
```

#### Генерация изображений

```bash
# ComfyUI
ENABLE_IMAGES_GENERATION=true
IMAGES_GENERATION_ENGINE=comfyui
COMFYUI_BASE_URL=http://localhost:8188

# AUTOMATIC1111
IMAGES_GENERATION_ENGINE=automatic1111
AUTOMATIC1111_BASE_URL=http://localhost:7860

# OpenAI DALL-E
IMAGES_OPENAI_API_KEY=your-openai-key
```

#### Логирование

```bash
LOG_FORMAT=json  # или text (по умолчанию)
GLOBAL_LOG_LEVEL=INFO  # DEBUG, INFO, WARNING, ERROR, CRITICAL
```

#### Производительность

```bash
# Таймауты
AIOHTTP_CLIENT_TIMEOUT=300  # секунд (по умолчанию 5 минут)
AIOHTTP_CLIENT_STREAM_IDLE_TIMEOUT=60  # секунд

# Workers uvicorn
UVICORN_WORKERS=4

# Redis (для горизонтального масштабирования)
REDIS_URL=redis://localhost:6379
ENABLE_REDIS_SESSION=true
```

#### Observability

```bash
# OpenTelemetry
ENABLE_OTEL=true
ENABLE_OTEL_METRICS=true
OTEL_EXPORTER_OTLP_ENDPOINT=http://grafana:4317
OTEL_SERVICE_NAME=open-webui
```

### Полный пример docker-compose с настройками

```yaml
version: '3.8'

services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    ports:
      - "3000:8080"
    volumes:
      - open-webui:/app/backend/data
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
      - WEBUI_SECRET_KEY=super-secret-key-change-in-production
      - WEBUI_AUTH=true
      - DATABASE_URL=postgresql://postgres:password@db:5432/openwebui
      - STORAGE_PROVIDER=local
      - ENABLE_RAG_WEB_SEARCH=true
      - RAG_WEB_SEARCH_ENGINE=searxng
      - SEARXNG_QUERY_URL=http://searxng:8080/search?q=<query>
      - LOG_FORMAT=json
      - GLOBAL_LOG_LEVEL=INFO
    extra_hosts:
      - host.docker.internal:host-gateway
    restart: unless-stopped
    depends_on:
      - db
      - searxng

  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped

  db:
    image: postgres:15
    container_name: open-webui-db
    volumes:
      - postgres-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=openwebui
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    restart: unless-stopped

  searxng:
    image: searxng/searxng:latest
    container_name: searxng
    volumes:
      - ./searxng:/etc/searxng
    environment:
      - SEARXNG_BASE_URL=http://localhost:8080/
    restart: unless-stopped

volumes:
  open-webui: {}
  ollama: {}
  postgres-data: {}
```

---

## Первые шаги после установки

### 1. Первоначальная настройка

1. Откройте браузер и перейдите по адресу `http://localhost:3000` (или ваш порт)
2. Создайте учётную запись администратора:
   - Введите email и пароль
   - Это будет первый пользователь с правами администратора

### 2. Подключение моделей

#### Для Ollama:

```bash
# Установите модели через CLI Ollama
ollama pull llama3.2
ollama pull mistral
ollama pull nomic-embed-text  # для эмбеддингов

# Проверьте доступность
curl http://localhost:11434/api/tags
```

#### Для OpenAI:

1. Перейдите в Settings → Connections
2. Добавьте API ключ OpenAI
3. Выберите нужные модели

### 3. Настройка RAG

1. Перейдите в Settings → RAG
2. Выберите модель для эмбеддингов (например, `nomic-embed-text`)
3. Настройте параметры поиска:
   - Top K результатов
   - Порог схожести
   - Включить гибридный поиск (BM25 + vector)

### 4. Первая загрузка документов

1. Нажмите `#` в поле ввода сообщения
2. Выберите "Upload File" или "Knowledge"
3. Загрузите PDF, DOCX, TXT или другие поддерживаемые форматы
4. Документ автоматически обработается и станет доступен для поиска

---

## Работа с моделями

### Добавление новой модели

1. Перейдите в Settings → Models
2. Нажмите "Add Model"
3. Выберите провайдера (Ollama, OpenAI, etc.)
4. Настройте параметры:
   - **Name**: Отображаемое имя
   - **Base Model**: Базовая модель
   - **System Prompt**: Инструкции для модели
   - **Parameters**: Температура, top_p, max tokens и др.

### Создание агента

Агент — это модель с дополнительными возможностями:

1. Создайте новую модель или выберите существующую
2. Включите необходимые компоненты:
   - **Tools**: Внешние инструменты (поиск, калькулятор, код)
   - **Skills**: Предопределённые навыки
   - **Knowledge**: Базы знаний
   - **Actions**: Пользовательские действия
   - **Filters**: Обработчики запросов/ответов

3. Настройте системный промпт с переменными:
   ```
   Ты полезный ассистент {{user.name}}.
   
   Контекст: {{chat.variables.context}}
   
   Отвечай в стиле: {{user.preferences.tone}}
   ```

### Мульти-модельные разговоры

1. Начните новый чат
2. Нажмите "+" рядом с выбором модели
3. Добавьте несколько моделей
4. Отправьте сообщение — все модели ответят параллельно
5. Сравните ответы и выберите лучший

### Управление контекстом

#### Context Compaction (автоматическое сжатие)

При длинных разговорах система автоматически summarizes старые сообщения:

```bash
# Настройки администратора
CONTEXT_COMPACT_THRESHOLD=4000  # токенов
CONTEXT_COMPACT_MODEL=gpt-4o-mini  # модель для summarization
CONTEXT_COMPACT_RETAIN_RATIO=0.2  # сколько последних сообщений сохранить
```

#### Ручное сжатие

Введите команду `/compact` в чате для немедленного сжатия истории.

---

## Основные функции интерфейса

### Чаты

#### Создание и организация

- **Новый чат**: Кнопка "+" или Ctrl/Cmd+N
- **Папки**: Перетаскивание чатов для организации
- **Поиск**: Фильтрация по названию и содержимому
- **Архив**: Архивация неактивных чатов

#### Функции чата

- **Форк ответа**: Кнопка fork создаёт новую ветку от выбранного момента
- **Таймеры**: AI может установить напоминание
- **Переменные чата**: Поля для ввода контекста
- **Предпросмотр**: Наведите на чат в сайдбаре для быстрого просмотра

### Заметки

Заметки — отдельное пространство для работы с контентом:

1. Перейдите в раздел Notes
2. Создайте новую заметку
3. Используйте AI для:
   - Переписывания текста
   - Исправления ошибок
   - Генерации контента
4. Прикрепите файлы
5. Свяжите с чатом для полного контекста

### Каналы

Каналы для командной работы:

- **Создание канала**: Workspace → Channels → New
- **Упоминания**: @model для привлечения AI
- **Треды**: Ответы в отдельных ветках
- **Закреплённые сообщения**: Важная информация сверху
- **Реакции**: Эмодзи для быстрых ответов

### Календарь

Встроенный календарь для планирования:

- **Представления**: Месяц, неделя, день
- **События**: Разовые и повторяющиеся
- **Цвета**: Категоризация по цветам
- **Участники**: Приглашение коллег
- **AI-планирование**: Попросите модель создать событие

### Автоматизации

Автоматическое выполнение задач по расписанию:

1. Перейдите в Automations
2. Создайте новую автоматизацию
3. Настройте:
   - **Расписание**: Cron или простой интервал
   - **Промпт**: Что должна сделать модель
   - **Папка назначения**: Куда сохранять результат
4. Результаты появляются в календаре

---

## Продвинутые возможности

### Плагины и расширения

#### Types of Extensions

1. **Tools**: Внешние сервисы через MCP, MCPO, OpenAPI
2. **Functions**: Python-код для обработки запросов/ответов
3. **Filters**: Модификация запросов и ответов
4. **Actions**: Пользовательские команды
5. **Skills**: Предопределённые шаблоны поведения
6. **Pipes**: Конвейеры обработки

#### Создание простого инструмента

```python
from pydantic import BaseModel, Field

class Tool:
    class Valves(BaseModel):
        API_KEY: str = Field(default="", description="API key")
    
    def __init__(self):
        self.valves = self.Valves()
    
    async def search(self, query: str) -> str:
        """Search for information"""
        # Your implementation here
        return f"Results for: {query}"
```

### RAG Pipeline

#### Загрузка документов

Поддерживаемые форматы:
- Текст: TXT, MD, HTML
- Документы: PDF, DOCX, PPTX
- Таблицы: XLSX, CSV
- Код: PY, JS, Java, etc.

#### Процесс обработки

1. **Извлечение**: Tika, Docling, или встроенный парсер
2. **Разбиение**: На чанки по размеру
3. **Эмбеддинг**: Векторизация текста
4. **Индексация**: Сохранение в векторную БД
5. **Поиск**: Семантический + ключевые слова
6. **Reranking**: Переупорядочивание результатов

#### Hybrid Search

```bash
# Включение гибридного поиска
ENABLE_RAG_HYBRID_SEARCH=true

# BM25 вес
RAG_HYBRID_SEARCH_BM25_WEIGHT=0.5

# Reranking модель
RAG_RERANKING_MODEL=ms-marco-MiniLM-L-12-v2
```

### Web Search Integration

#### Настройка SearXNG

```bash
# docker-compose.yaml
services:
  searxng:
    image: searxng/searxng:latest
    volumes:
      - ./searxng:/etc/searxng
    environment:
      - SEARXNG_BASE_URL=http://localhost:8080/
```

#### Использование в чате

- **Команда**: `/search ваш запрос`
- **Автоматически**: Модель сама решит когда искать
- **URL**: Вставьте ссылку с `#` для загрузки страницы

### Image Generation

#### Настройка ComfyUI

```bash
# docker-compose.yaml
services:
  comfyui:
    image: yanwk/comfyui-boot
    ports:
      - "8188:8188"
    volumes:
      - comfyui-workflow:/home/runner/workflows
```

#### Использование

1. Включите генерацию изображений в настройках
2. Выберите движок (ComfyUI, AUTOMATIC1111, DALL-E)
3. В чате: "Нарисуй [описание]"
4. Или используйте `/imagine [prompt]`

### Voice & Video

#### Настройка STT (Speech-to-Text)

```bash
# Local Whisper
AUDIO_STT_ENGINE=whisper
AUDIO_STT_WHISPER_MODEL=base

# OpenAI
AUDIO_STT_ENGINE=openai
AUDIO_STT_OPENAI_API_KEY=your-key
AUDIO_STT_MODEL=whisper-1
```

#### Настройка TTS (Text-to-Speech)

```bash
# ElevenLabs
AUDIO_TTS_ENGINE=elevenlabs
AUDIO_TTS_ELVENLABS_API_KEY=your-key
AUDIO_TTS_VOICE=Josh

# OpenAI
AUDIO_TTS_ENGINE=openai
AUDIO_TTS_OPENAI_API_KEY=your-key
AUDIO_TTS_MODEL=tts-1
```

### Sub-Agents

Суб-агенты позволяют делегировать задачи:

```bash
# Включение
ENABLE_SUBAGENTS=true

# Максимум параллельных агентов
SUBAGENTS_MAX_CONCURRENT=3

# Максимум итераций
SUBAGENTS_MAX_ITERATIONS=5

# Системный промпт для суб-агентов
SUBAGENTS_SYSTEM_PROMPT="Ты специализированный помощник..."
```

---

## Администрирование

### Роли и разрешения

#### Встроенные роли

1. **Admin**: Полный доступ ко всем функциям
2. **User**: Стандартный пользователь
3. **Pending**: Ожидает подтверждения
4. **Custom**: Пользовательские роли

#### Настройка групп

1. Admin Panel → Users → Groups
2. Создайте группу
3. Назначьте разрешения:
   - Доступ к моделям
   - Использование инструментов
   - Веб-поиск
   - Генерация изображений
   - Общий доступ к ресурсам
   - Webhooks

### LDAP/Active Directory интеграция

```bash
# Включение LDAP
ENABLE_LDAP=true

# Настройки подключения
LDAP_SERVER_URL=ldap://ldap.example.com
LDAP_BIND_DN=cn=admin,dc=example,dc=com
LDAP_BIND_PASSWORD=secret
LDAP_USER_SEARCH_BASE=ou=users,dc=example,dc=com
LDAP_USER_SEARCH_FILTER=(uid={{username}})

# Маппинг атрибутов
LDAP_EMAIL_ATTRIBUTE=mail
LDAP_NAME_ATTRIBUTE=cn
LDAP_GROUP_ATTRIBUTE=memberOf

# Синхронизация групп
LDAP_GROUP_SYNC_ENABLED=true
LDAP_GROUP_SEARCH_BASE=ou=groups,dc=example,dc=com
LDAP_GROUP_CREATE_MISSING=true  # Создавать отсутствующие группы
```

### OAuth/SSO

#### GitHub OAuth

```bash
ENABLE_OAUTH_GITHUB=true
GITHUB_CLIENT_ID=your-client-id
GITHUB_CLIENT_SECRET=your-client-secret
OAUTH_CALLBACK_URL=https://your-domain.com/oauth/callback
```

#### Google OAuth

```bash
ENABLE_OAUTH_GOOGLE=true
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
```

#### Trusted Headers (для reverse proxy auth)

```bash
WEBUI_AUTH_TRUSTED_EMAIL_HEADER=X-Forwarded-Email
WEBUI_AUTH_TRUSTED_NAME_HEADER=X-Forwarded-Name
WEBUI_AUTH_TRUSTED_GROUPS_HEADER=X-Forwarded-Groups
WEBUI_AUTH_TRUSTED_ROLE_HEADER=X-Forwarded-Role
```

### SCIM 2.0 Provisioning

Автоматическая синхронизация пользователей:

```bash
ENABLE_SCIM=true
SCIM_BASE_URL=https://your-domain.com/scim/v2
```

Настройте ваш IdP (Okta, Azure AD, Google Workspace) для подключения к этому endpoint.

### Мониторинг и аналитика

#### Встроенная аналитика

Admin Panel → Analytics:
- Количество сообщений по пользователям
- Потребление токенов
- Стоимость использования моделей
- Активность по времени

#### OpenTelemetry

```bash
# docker-compose.otel.yaml
services:
  grafana:
    image: grafana/otel-lgtm:latest
    ports:
      - "3000:3000"  # UI
      - "4317:4317"  # OTLP/gRPC
      - "4318:4318"  # OTLP/HTTP
```

### Backup и восстановление

#### Резервное копирование базы данных

```bash
# SQLite
cp /path/to/webui.db /backup/webui-backup-$(date +%Y%m%d).db

# PostgreSQL
pg_dump -U postgres openwebui > /backup/openwebui-$(date +%Y%m%d).sql

# Docker volume
docker run --rm -v open-webui:/data -v $(pwd):/backup alpine tar czf /backup/open-webui-backup.tar.gz /data
```

#### Восстановление

```bash
# SQLite
cp /backup/webui-backup.db /path/to/webui.db

# PostgreSQL
psql -U postgres openwebui < /backup/openwebui.sql

# Docker volume
docker run --rm -v open-webui:/data -v $(pwd):/backup alpine tar xzf /backup/open-webui-backup.tar.gz -C /data
```

---

## Безопасность

### Best Practices

1. **Смените секретный ключ**:
   ```bash
   WEBUI_SECRET_KEY=$(openssl rand -base64 32)
   ```

2. **Используйте HTTPS**:
   - Настройте reverse proxy (nginx, traefik)
   - Получите SSL сертификат (Let's Encrypt)

3. **Ограничьте доступ**:
   ```bash
   # Firewall правила
   ufw allow from 192.168.1.0/24 to any port 3000
   ```

4. **Регулярно обновляйтесь**:
   ```bash
   docker pull ghcr.io/open-webui/open-webui:main
   docker-compose up -d
   ```

5. **Мониторинг логов**:
   ```bash
   docker logs -f open-webui
   ```

### Security Headers (для reverse proxy)

```nginx
# nginx configuration
server {
    listen 443 ssl;
    server_name your-domain.com;
    
    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self' data:; connect-src 'self' ws: wss:;" always;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Audit Logging

```bash
# Включение аудита
ENABLE_AUDIT_LOGGING=true
AUDIT_LOG_LEVEL=ALL  # ALL, ADMIN, USER
AUDIT_LOG_FILE=/var/log/open-webui/audit.log
```

---

## Решение проблем

### Сервер не запускается

**Проблема**: Container exits immediately

**Решение**:
```bash
# Проверьте логи
docker logs open-webui

# Частые причины:
# 1. Недостаточно памяти
# 2. Конфликт портов
# 3. Неправильный WEBUI_SECRET_KEY

# Проверка портов
netstat -tlnp | grep 3000

# Освобождение порта
docker kill $(docker ps -q --filter "publish=3000")
```

### Нет соединения с Ollama

**Проблема**: "Server connection error"

**Решение**:

1. **Проверьте URL Ollama**:
   ```bash
   curl http://localhost:11434/api/tags
   ```

2. **Для Docker с host network**:
   ```bash
   docker run -d \
     --network=host \
     -e OLLAMA_BASE_URL=http://127.0.0.1:11434 \
     -v open-webui:/app/backend/data \
     --name open-webui \
     ghcr.io/open-webui/open-webui:main
   ```

3. **Для Docker без host network**:
   ```bash
   docker run -d \
     --add-host=host.docker.internal:host-gateway \
     -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
     -v open-webui:/app/backend/data \
     --name open-webui \
     ghcr.io/open-webui/open-webui:main
   ```

### Медленные ответы

**Проблема**: Таймаут при генерации

**Решение**:
```bash
# Увеличьте таймаут
AIOHTTP_CLIENT_TIMEOUT=600  # 10 минут

# Для streaming
AIOHTTP_CLIENT_STREAM_IDLE_TIMEOUT=120
```

### Проблемы с RAG

**Проблема**: Документы не индексируются

**Решение**:

1. Проверьте модель эмбеддингов:
   ```bash
   ollama pull nomic-embed-text
   ```

2. Проверьте векторную БД:
   ```bash
   # Для ChromaDB
   ls -la /app/backend/data/chroma
   ```

3. Пересоздайте индекс:
   - Admin Panel → Settings → RAG
   - Нажмите "Reset Vector Database"

### Ошибки аутентификации

**Проблема**: Не могу войти как администратор

**Решение**:

1. **Сброс пароля** (SQLite):
   ```bash
   sqlite3 /path/to/webui.db
   UPDATE user SET password_hash='новый_hash' WHERE email='admin@example.com';
   ```

2. **Создание нового админа**:
   ```bash
   # Через API
   curl -X POST http://localhost:3000/api/v1/auths/signup \
     -H "Content-Type: application/json" \
     -d '{"email":"newadmin@example.com","password":"password","name":"Admin"}'
   ```

### Проблемы с памятью

**Проблема**: Out of memory errors

**Решение**:

1. **Ограничьте размер контекста**:
   ```bash
   CONTEXT_COMPACT_THRESHOLD=2000
   ```

2. **Уменьшите batch size**:
   ```bash
   RAG_EMBEDDING_BATCH_SIZE=8
   ```

3. **Используйте более лёгкую модель**:
   ```bash
   RAG_EMBEDDING_MODEL=all-MiniLM-L6-v2
   ```

### Обновление без потери данных

```bash
# Docker
docker pull ghcr.io/open-webui/open-webui:main
docker stop open-webui
docker rm open-webui
docker run -d \
  -p 3000:8080 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main

# Миграции выполнятся автоматически
```

---

## Полезные ссылки

- **Официальная документация**: https://docs.openwebui.com
- **GitHub репозиторий**: https://github.com/open-webui/open-webui
- **Discord сообщество**: https://discord.gg/5rJgQTnV4s
- **Библиотека моделей**: https://openwebui.com
- **Enterprise версия**: https://docs.openwebui.com/enterprise

## Поддержка

Если у вас возникли вопросы или проблемы:

1. Проверьте документацию: https://docs.openwebui.com
2. Поищите в Issues на GitHub
3. Задайте вопрос в Discord
4. Создайте новый issue с подробным описанием проблемы

---

*Документация актуальна для версии Open WebUI 0.11.0*
*Последнее обновление: 2024*
