# Claude Code

Запустите сервер CLIProxyAPI, а затем установите следующие переменные окружения:

- `ANTHROPIC_BASE_URL`
- `ANTHROPIC_AUTH_TOKEN`
- `ANTHROPIC_DEFAULT_OPUS_MODEL`
- `ANTHROPIC_DEFAULT_SONNET_MODEL`
- `ANTHROPIC_DEFAULT_HAIKU_MODEL`

Или установите эти переменные окружения для версии 1.x.x

- `ANTHROPIC_MODEL`
- `ANTHROPIC_SMALL_FAST_MODEL`

Использование моделей Gemini:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gemini-2.5-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=gemini-2.5-flash
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gemini-2.5-flash-lite
# версия 1.x.x
export ANTHROPIC_MODEL=gemini-2.5-pro
export ANTHROPIC_SMALL_FAST_MODEL=gemini-2.5-flash
```

Использование моделей OpenAI GPT 5:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5(minimal) # мы не рекомендуем использовать gpt-5(minimal), используйте вместо этого qwen3-coder-flash или gemini-2.5-flash-lite
# версия 1.x.x
export ANTHROPIC_MODEL=gpt-5
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5(minimal) # мы не рекомендуем использовать gpt-5(minimal), используйте вместо этого qwen3-coder-flash или gemini-2.5-flash-lite
```

Использование моделей OpenAI GPT 5 Codex:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5-codex(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5-codex(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5-codex(low) # мы не рекомендуем использовать gpt-5-codex(low), используйте вместо этого qwen3-coder-flash или gemini-2.5-flash-lite
# версия 1.x.x
export ANTHROPIC_MODEL=gpt-5-codex
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5-codex(low) # мы не рекомендуем использовать gpt-5-codex(low), используйте вместо этого qwen3-coder-flash или gemini-2.5-flash-lite
```

Использование моделей Claude:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-1-20250805
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-5-20250929
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-3-5-haiku-20241022
# версия 1.x.x
export ANTHROPIC_MODEL=claude-sonnet-4-20250514
export ANTHROPIC_SMALL_FAST_MODEL=claude-3-5-haiku-20241022
```

Использование моделей Qwen:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-coder-flash
# версия 1.x.x
export ANTHROPIC_MODEL=qwen3-coder-plus
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-coder-flash
```

Использование моделей iFlow:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# версия 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-max
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-235b-a22b-instruct
# версия 1.x.x
export ANTHROPIC_MODEL=qwen3-max
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-235b-a22b-instruct
```