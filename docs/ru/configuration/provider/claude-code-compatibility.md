# Провайдеры, совместимые с Claude Code

Настройте вышестоящие провайдеры, совместимые с Claude Code, через `claude-api-key`.

- `api-key`: API-ключ для провайдера
- `base-url`: базовый URL провайдера
- `proxy-url`: необязательный URL прокси для провайдера
- `models`: список сопоставлений имени (`name`) вышестоящей модели с локальным псевдонимом (`alias`)

Пример:
```yaml
claude-api-key:
  - api-key: "sk-atSM..." # используйте официальный API-ключ Claude, указывать base-url не нужно
  - api-key: "sk-atSM..."
    base-url: "https://www.example.com" # используйте кастомную конечную точку API Claude
    proxy-url: "socks5://proxy.example.com:1080" # необязательно: переопределение прокси для конкретного ключа
    models:
      - name: "claude-3-5-sonnet-20241022" # имя вышестоящей модели
        alias: "claude-sonnet-latest" # псевдоним клиента, сопоставленный с вышестоящей моделью
```

> [!NOTE]  
> Если вы укажете только `api-key`, `base-url` будет автоматически установлен на `https://api.anthropic.com`.   
> `base-url` требуется только в том случае, если вы используете стороннего провайдера, совместимого с Claude Code.