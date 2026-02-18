# Провайдеры, совместимые с Gemini

Настройте вышестоящие провайдеры, совместимые с Gemini, через `gemini-api-key`.

- `api-key`: API-ключ для провайдера
- `base-url`: базовый URL провайдера
- `proxy-url`: необязательный URL прокси для провайдера
- `headers`: необязательные дополнительные HTTP-заголовки, отправляемые только на переопределенный эндпоинт Gemini

Пример:
```yaml
gemini-api-key:
  - api-key: "AIzaSy...01"
    base-url: "https://generativelanguage.googleapis.com"
    headers:
      X-Custom-Header: "custom-value"
    proxy-url: "socks5://proxy.example.com:1080"
  - api-key: "AIzaSy...02" # используйте официальный Gemini API key, базовый url указывать не нужно
```

> [!NOTE]  
> Если вы укажете только `api-key`, `base-url` будет автоматически установлен на `https://generativelanguage.googleapis.com`.   
> `base-url` требуется только в том случае, если вы используете стороннего провайдера, совместимого с Gemini.