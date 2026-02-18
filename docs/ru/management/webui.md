# Web UI

URL проекта:
[Cli-Proxy-API-Management-Center](https://github.com/router-for-me/Cli-Proxy-API-Management-Center)

Официальный веб-центр управления для CLIProxyAPI.

Базовый путь: `http://localhost:8317/management.html`

Установите `remote-management.disable-control-panel` в значение `true`, если вы предпочитаете размещать Web UI управления в другом месте; сервер пропустит загрузку `management.html`, и `/management.html` будет возвращать 404.

Вы можете установить переменную окружения `MANAGEMENT_STATIC_PATH`, чтобы выбрать директорию, в которой хранится `management.html`.

## Использование кастомного Web UI

Вы можете указать серверу на ваш собственный GitHub-репозиторий для панели управления:

```yaml
remote-management:
  panel-github-repository: "https://github.com/your-org/your-management-ui"
```

- Формат URL репозитория: `https://github.com/<org>/<repo>`; сервер автоматически преобразует его в `https://api.github.com/repos/<org>/<repo>/releases/latest`.
- Формат API URL: установите его напрямую как `https://api.github.com/repos/<org>/<repo>/releases/latest`.
- Обновлятор периодически проверяет последний релиз, ищет ассет с именем `management.html` и скачивает его в статическую директорию (по умолчанию `static/` рядом с файлом конфигурации или путь, заданный через `MANAGEMENT_STATIC_PATH`). Если ассет содержит поле `digest` (рекомендуется `sha256:<hex>`), оно будет использовано для проверки целостности.

## Как опубликовать кастомный Web UI на GitHub

1. Соберите вашу кастомную панель и создайте один файл `management.html` (по возможности объедините все ресурсы в один файл).
2. Создайте репозиторий GitHub и отправьте туда ваш код.
3. Создайте релиз (обновлятор ориентируется на `latest`) и загрузите ассеты:
   - Должен включать **`management.html`**.
   - Настоятельно рекомендуется: добавьте поле метаданных `digest` с `sha256:<file hash>` для проверки контрольной суммы.
4. Установите `remote-management.panel-github-repository` в CLIProxyAPI на URL репозитория или API URL.
5. Перезапустите или выполните hot-reload конфигурации; сервер автоматически загрузит и заменит панель управления.