# Провайдеры, совместимые с Codex

Настройте вышестоящие провайдеры, совместимые с Codex, через `codex-api-key`.

- api-key: API-ключ для провайдера
- base-url: базовый URL провайдера
- proxy-url: необязательный URL прокси для провайдера

Пример:
```yaml
codex-api-key:
  - api-key: "sk-atSM..."
    base-url: "https://www.example.com" # используйте кастомную конечную точку Codex API
    proxy-url: "socks5://proxy.example.com:1080" # необязательно: переопределение прокси для конкретного ключа
```