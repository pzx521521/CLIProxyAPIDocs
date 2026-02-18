---
outline: 'deep'
---

# Amp CLI

В данном руководстве объясняется, как использовать CLIProxyAPI с Amp CLI и расширениями Amp IDE, что позволяет вам использовать ваши существующие подписки Google/ChatGPT/Claude (через OAuth) с CLI от Amp.

## Обзор

Интеграция Amp CLI добавляет специализированную маршрутизацию для поддержки паттернов API Amp, сохраняя при этом полную совместимость со всеми существующими функциями CLIProxyAPI. Это позволяет использовать как традиционные возможности CLIProxyAPI, так и Amp CLI с одним и тем же прокси-сервером.

### Ключевые особенности

- **Алиасы маршрутов провайдеров**: сопоставляют паттерны Amp `/api/provider/{provider}/v1...` с обработчиками CLIProxyAPI.
- **Прокси управления**: пересылает запросы управления аккаунтом в control plane Amp, используя защищенный upstream API key.
- **Умный fallback**: автоматически перенаправляет запросы для ненастроенных моделей на ampcode.com.
- **Безопасность прежде всего**: маршруты управления требуют аутентификации по API key (опциональное ограничение localhost).
- **Автоматическая обработка gzip**: декомпрессия ответов от upstream Amp.

### Что вы можете сделать

- Использовать Amp CLI со своим аккаунтом Google (Gemini 3 Pro Preview, Gemini 2.5 Pro, Gemini 2.5 Flash).
- Использовать Amp CLI со своей подпиской ChatGPT Plus/Pro (модели GPT-5, GPT-5 Codex).
- Использовать Amp CLI со своей подпиской Claude Pro/Max (Claude Sonnet 4.5, Opus 4.1).
- Использовать расширения Amp IDE (VS Code, Cursor, Windsurf и т. д.) с тем же прокси.
- Запускать несколько инструментов CLI (Factory + Amp) через один прокси-сервер.
- Автоматически направлять запросы для ненастроенных моделей через ampcode.com.

### Каких провайдеров следует аутентифицировать?

**Важно**: провайдеры, которых вам необходимо аутентифицировать, зависят от того, какие модели и функции использует ваша установленная версия Amp. Amp применяет разных провайдеров для различных режимов агентов и специализированных субагентов:

- **Smart mode**: использует модели Google/Gemini (Gemini 3 Pro).
- **Rush mode**: использует модели Anthropic/Claude (Claude Haiku 4.5).
- **Oracle subagent**: использует модели OpenAI/GPT (GPT-5 medium reasoning).
- **Librarian subagent**: использует модели Anthropic/Claude (Claude Sonnet 4.5).
- **Search subagent**: использует модели Anthropic/Claude (Claude Haiku 4.5).
- **Review feature**: использует модели Google/Gemini (Gemini 2.5 Flash-Lite).

Для получения актуальной информации о том, какие модели использует Amp, см. **[Документацию моделей Amp](https://ampcode.com/models)**.

#### Поведение Fallback

CLIProxyAPI использует умную систему fallback:

1. **Провайдер аутентифицирован локально** (`--login`, `--codex-login`, `--claude-login`):
   - Запросы используют **вашу OAuth подписку** (ChatGPT Plus/Pro, Claude Pro/Max, аккаунт Google).
   - Вы используете квоты, включенные в вашу подписку.
   - Кредиты Amp не расходуются.

2. **Провайдер НЕ аутентифицирован локально**:
   - Запросы автоматически пересылаются на **ampcode.com**.
   - Используются подключения к провайдерам через бэкенд Amp.
   - **Требуются кредиты Amp**, если провайдер платный (платные уровни OpenAI, Anthropic).
   - Может привести к ошибкам, если баланс кредитов Amp недостаточен.

**Рекомендация**: аутентифицируйте всех провайдеров, на которых у вас есть подписки, чтобы максимизировать выгоду и минимизировать использование кредитов Amp. Если у вас нет подписок на всех провайдеров, которых использует Amp, убедитесь, что у вас достаточно кредитов Amp для fallback-запросов.

## Архитектура

### Поток запросов

```
Amp CLI/IDE
  ↓
  ├─ Запросы к API провайдеров (/api/provider/{provider}/v1/...)
  │   ↓
  │   ├─ Модель настроена локально?
  │   │   ДА → Использовать локальные OAuth токены (обработчики OpenAI/Claude/Gemini)
  │   │   НЕТ → Переслать на ampcode.com (reverse proxy)
  │   ↓
  │   Ответ
  │
  └─ Запросы управления (/api/auth, /api/user, /api/threads, ...)
      ↓
      ├─ Аутентификация с помощью `api-keys` в CLIProxyAPI
      ↓
      ├─ Опциональное ограничение localhost
      ↓
      └─ Reverse proxy на ampcode.com (используя `upstream-api-key`)
          ↓
          Ответ (автоматически декомпрессируется, если сжат gzip)
```

### Компоненты

Интеграция Amp реализована как модульный компонент маршрутизации (`internal/api/modules/amp/`) со следующими составляющими:

1. **Алиасы маршрутов** (`routes.go`): сопоставляют пути в стиле Amp со стандартными обработчиками.
2. **Reverse Proxy** (`proxy.go`): пересылает запросы управления на ampcode.com.
3. **Обработчик Fallback** (`fallback_handlers.go`): направляет запросы для ненастроенных моделей на ampcode.com.
4. **Управление секретами** (`secret.go`): разрешение API key из нескольких источников с кэшированием.
5. **Основной модуль** (`amp.go`): координирует регистрацию и конфигурацию.

## Конфигурация

### Базовая конфигурация

Добавьте блок `ampcode` в ваш `config.yaml` (начиная с v6.5.37, устаревшие ключи `amp-upstream-*` автоматически мигрируют и перезаписываются в эту структуру при загрузке):

```yaml
# API-ключи для клиентов (например, Amp CLI, VS Code) для аутентификации в CLIProxyAPI
api-keys:
  - "your-client-secret-key" # Пример ключа для ваших клиентов

ampcode:
  # Upstream control plane Amp (требуется для маршрутов управления)
  upstream-url: "https://ampcode.com"
  # API-ключ для аутентификации CLIProxyAPI на ampcode.com
  # Получите его на https://ampcode.com/settings
  upstream-api-key: "your-ampcode-api-key-goes-here"
  # Опционально: ограничить маршруты управления только для localhost (по умолчанию: false)
  restrict-management-to-localhost: false
  # Опционально: сопоставление отсутствующих моделей Amp с локальными
  # model-mappings:
  #   - from: "claude-opus-4.5"
  #     to: "claude-sonnet-4"
```

### Настройки безопасности

#### Аутентификация по API Key для маршрутов управления

Начиная с v6.6.15, маршруты управления Amp (`/api/auth`, `/api/user`, `/api/threads`, `/threads` и т. д.) защищены стандартным middleware аутентификации по API key в CLIProxyAPI.

- Если вы настроите `api-keys` в `config.yaml` (рекомендуется), эти маршруты будут требовать валидный API key в запросе (`Authorization: Bearer <key>` или `X-Api-Key: <key>`), иначе они вернут `401 Unauthorized`.
- После успешной локальной аутентификации прокси удаляет клиентский `Authorization`/`X-Api-Key` и использует `ampcode.upstream-api-key` для вызова upstream-сервиса ampcode.com.

#### `ampcode.restrict-management-to-localhost`

**По умолчанию: `false`**

При включении маршруты управления (`/api/auth`, `/api/user`, `/api/threads` и т. д.) принимают соединения только с localhost (127.0.0.1, ::1). Это предотвращает:
- Атаки через браузер (drive-by).
- Удаленный доступ к эндпоинтам управления.
- Атаки на основе CORS.
- Атаки с подменой заголовков (например, `X-Forwarded-For: 127.0.0.1`).

#### Как это работает

Это ограничение использует **фактический адрес TCP-соединения** (`RemoteAddr`), а не HTTP-заголовки типа `X-Forwarded-For`. Это предотвращает атаки с подменой заголовков, но имеет важные последствия:

- ✅ **Работает для прямых соединений**: запуск CLIProxyAPI непосредственно на вашей машине или сервере.
- ⚠️ **Может не работать за reverse proxy**: при развертывании за nginx, Cloudflare или другими прокси соединение будет выглядеть так, будто оно пришло с IP прокси, а не с localhost.

#### Развертывание с Reverse Proxy

Если вам нужно запустить CLIProxyAPI за reverse proxy (nginx, Caddy, Cloudflare Tunnel и т. д.):

1. **Оставьте ограничение localhost отключенным (по умолчанию)**:
   ```yaml
   ampcode:
     restrict-management-to-localhost: false
   ```

2. **Убедитесь, что аутентификация по API key включена** (рекомендуется):
   - Настройте `api-keys` в `config.yaml` и заставьте клиентов отправлять ключ.
   - Комбинируйте с настройками firewall/VPN/zero-trust для уменьшения рисков.

3. **Пример конфигурации nginx** (блокирует внешний доступ к маршрутам управления):
   ```nginx
   location /api/auth { deny all; }
   location /api/user { deny all; }
   location /api/threads { deny all; }
   location /api/internal { deny all; }
   ```

**Примечание**: `ampcode.restrict-management-to-localhost` — это дополнительная опция усиления защиты; за reverse proxy она обычно остается `false`.

## Настройка

### 1. Настройте CLIProxyAPI

Создайте или отредактируйте `config.yaml`. Вам понадобятся два типа API-ключей:
1.  `api-keys`: для клиентов, таких как Amp CLI, для подключения к вашему экземпляру CLIProxyAPI.
2.  `ampcode.upstream-api-key`: для CLIProxyAPI для подключения к бэкенду `ampcode.com`. Получите его на **[https://ampcode.com/settings](https://ampcode.com/settings)**.

```yaml
port: 8317
auth-dir: "~/.cli-proxy-api"

# API-ключи для использования клиентами (например, Amp CLI)
api-keys:
  - "your-client-secret-key" # Вы можете изменить этот секрет

# Интеграция Amp
ampcode:
  upstream-url: "https://ampcode.com"
  # Ваш персональный API-ключ с ampcode.com
  upstream-api-key: "paste-your-ampcode-api-key-here"
  restrict-management-to-localhost: false

# Другие стандартные настройки...
debug: false
logging-to-file: true
```

### 2. Аутентификация у провайдеров

Выполните OAuth login для провайдеров, которых вы хотите использовать:

**Аккаунт Google (Gemini 2.5 Pro, Gemini 2.5 Flash, Gemini 3 Pro Preview):**
```bash
./cli-proxy-api --login
```

**ChatGPT Plus/Pro (GPT-5, GPT-5 Codex):**
```bash
./cli-proxy-api --codex-login
```

**Claude Pro/Max (Claude Sonnet 4.5, Opus 4.1):**
```bash
./cli-proxy-api --claude-login
```

Токены сохраняются в:
- Gemini: `~/.cli-proxy-api/gemini-<email>.json`
- OpenAI Codex: `~/.cli-proxy-api/codex-<email>.json`
- Claude: `~/.cli-proxy-api/claude-<email>.json`

### 3. Запустите прокси

```bash
./cli-proxy-api --config config.yaml
```

### 4. Настройте Amp CLI

#### Вариант А: Файл настроек

Amp CLI использует два конфигурационных файла:

1. **Файл настроек**: `~/.config/amp/settings.json` — общие настройки.
2. **Файл секретов**: `~/.local/share/amp/secrets.json` — конфиденциальные данные (API-ключи).

Отредактируйте `~/.config/amp/settings.json` для указания URL:

```json
{
  "amp.url": "http://localhost:8317"
}
```

Отредактируйте `~/.local/share/amp/secrets.json` для указания API-ключа:

```json
{
  "apiKey@http://localhost:8317": "your-client-secret-key"
}
```

#### Вариант Б: Переменные окружения

```bash
export AMP_URL=http://localhost:8317
export AMP_API_KEY=your-client-secret-key
```

При такой конфигурации команда `amp login` больше не требуется.

### 5. Используйте Amp

Теперь вы готовы использовать Amp. Все запросы будут направляться через ваш прокси.

```bash
amp "Write a hello world program in Python"
```

### 6. (Опционально) Настройте расширение Amp IDE

Прокси также работает с расширениями Amp IDE для VS Code, Cursor, Windsurf и т. д.

1.  Откройте настройки расширения Amp в вашей IDE.
2.  Установите **Amp URL** на `http://localhost:8317`.
3.  Установите **Amp API Key** на ключ, который вы настроили (например, `your-client-secret-key`).
4.  Начните использовать Amp в вашей IDE.
И CLI, и IDE могут использовать прокси одновременно.

## Использование

### Поддерживаемые маршруты

#### Алиасы провайдеров (всегда доступны)

Эти маршруты работают даже без настроенного `ampcode.upstream-url`:

- `/api/provider/openai/v1/chat/completions`
- `/api/provider/openai/v1/responses`
- `/api/provider/anthropic/v1/messages`
- `/api/provider/google/v1beta/models/:action`

Amp CLI вызывает эти маршруты с вашими OAuth-аутентифицированными моделями, настроенными в CLIProxyAPI.

#### Маршруты управления (требуют `ampcode.upstream-url`)

Эти маршруты проксируются на ampcode.com:

- `/api/auth` — аутентификация
- `/api/user` — профиль пользователя
- `/api/meta` — метаданные
- `/api/threads` — ветки обсуждений
- `/api/telemetry` — телеметрия использования
- `/api/internal` — внутренние API

**Безопасность**: требуется API key; `ampcode.restrict-management-to-localhost` опционально (по умолчанию: false).

### Поведение Fallback для моделей

Когда Amp запрашивает модель:

1. **Проверка локальной конфигурации**: есть ли у CLIProxyAPI OAuth токены для провайдера этой модели?
2. **Если ДА**: направить к локальному обработчику (использовать вашу OAuth подписку).
3. **Если НЕТ**: переслать на ampcode.com (использовать маршрутизацию Amp по умолчанию).

Это обеспечивает бесшовное смешанное использование:
- Модели, которые вы настроили (Gemini, ChatGPT, Claude) → ваши OAuth подписки.
- Модели, которые вы не настраивали → провайдеры Amp по умолчанию.

### Примеры вызовов API

**Chat completion с локальным OAuth:**
```bash
curl http://localhost:8317/api/provider/openai/v1/chat/completions \
  -H "Authorization: Bearer <your-cli-proxy-api-key>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```
**Эндпоинт управления (требуется API key):**
```bash
curl http://localhost:8317/api/user \
  -H "Authorization: Bearer <your-cli-proxy-api-key>"
```

## Устранение неполадок

### Распространенные проблемы

| Симптом | Вероятная причина | Решение |
|---------|--------------|-----|
| 404 на `/api/provider/...` | Неверный путь маршрута | Убедитесь в точности пути: `/api/provider/{provider}/v1...` |
| 401 на `/api/user` | Отсутствующий или недействительный API key | Настройте `api-keys` и отправьте `Authorization: Bearer <key>` или `X-Api-Key: <key>` |
| 403 на `/api/user` | Включено ограничение localhost, а запрос является удаленным | Запустите с той же машины или установите `ampcode.restrict-management-to-localhost` в значение `false` |
| 401/403 от провайдера | Отсутствующий или истекший OAuth | Повторно запустите `--codex-login` или `--claude-login` |
| Ошибки Amp gzip | Проблема с декомпрессией ответа | Обновитесь до последней сборки; автоматическая декомпрессия должна справиться с этим |
| Модели не используют прокси | Неверный Amp URL | Проверьте настройку `amp.url` или переменную окружения `AMP_URL` |
| Ошибки CORS | Защищенный эндпоинт управления | Используйте CLI/терминал, а не браузер |

### Диагностика

**Проверьте логи прокси:**
```bash
# Если logging-to-file: true
tail -f logs/requests.log

# Если запущен в tmux
tmux attach-session -t proxy
```

**Включите режим отладки** (временно):
```yaml
debug: true
```

**Проверьте базовое соединение:**
```bash
# Проверьте, запущен ли прокси
curl http://localhost:8317/v1/models

# Проверьте маршрут, специфичный для Amp
curl http://localhost:8317/api/provider/openai/v1/models
```

**Проверьте конфигурацию Amp:**
```bash
# Проверьте, использует ли Amp прокси
amp config get amp.url

# Или проверьте окружение
echo $AMP_URL
```

### Контрольный список безопасности

- ✅ Настройте и защитите `api-keys` (маршруты управления требуют API key)
- ✅ Включите `ampcode.restrict-management-to-localhost: true` для дополнительного усиления защиты, когда это возможно (по умолчанию: false)
- ✅ Не выставляйте прокси в открытый доступ (привяжите к localhost или используйте firewall/VPN)
- ✅ Безопасно храните ваш `ampcode.upstream-api-key` в файле конфигурации.
- ✅ Периодически обновляйте OAuth-токены, повторно запуская команды входа
- ✅ Храните конфигурацию и `auth-dir` на зашифрованном диске при работе с конфиденциальными данными
- ✅ Своевременно обновляйте бинарный файл прокси для получения исправлений безопасности

## Дополнительные ресурсы

- [Официальное руководство Amp CLI](https://ampcode.com/manual)