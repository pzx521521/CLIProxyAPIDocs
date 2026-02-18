# Codex

Запустите сервер CLIProxyAPI, а затем отредактируйте файлы `~/.codex/config.toml` и `~/.codex/auth.json`.

config.toml:
```toml
# Отключает все запросы на подтверждение действий пользователем. Чрезвычайно опасно — удалите этот параметр, если вы новичок в Codex.
# approval_policy = "never"

# Предоставляет неограниченный доступ к системе: AI может читать/записывать любые файлы и выполнять команды с доступом к сети. Высокий риск — удалите этот параметр, если вы новичок в Codex.
# sandbox_mode = "danger-full-access"

model_provider = "cliproxyapi"
model = "gpt-5-codex" # Или gpt-5, вы также можете использовать любую из поддерживаемых нами моделей.
model_reasoning_effort = "high"

[model_providers.cliproxyapi]
name = "cliproxyapi"
base_url = "http://127.0.0.1:8317/v1"
wire_api = "responses"
```

auth.json:
```json
{
  "OPENAI_API_KEY": "sk-dummy"
}
```