# Claude Code

Start CLIProxyAPI server, and then set these environment variables:

- `ANTHROPIC_BASE_URL`
- `ANTHROPIC_AUTH_TOKEN`
- `ANTHROPIC_DEFAULT_OPUS_MODEL`
- `ANTHROPIC_DEFAULT_SONNET_MODEL`
- `ANTHROPIC_DEFAULT_HAIKU_MODEL`

Or set these environment variables for version 1.x.x

- `ANTHROPIC_MODEL`
- `ANTHROPIC_SMALL_FAST_MODEL`

Using Gemini models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gemini-2.5-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=gemini-2.5-flash
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gemini-2.5-flash-lite
# version 1.x.x
export ANTHROPIC_MODEL=gemini-2.5-pro
export ANTHROPIC_SMALL_FAST_MODEL=gemini-2.5-flash
```

Using OpenAI GPT 5 models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5(minimal) # we don't recommend using gpt-5(minimal), use qwen3-coder-flash or gemini-2.5-flash-lite instead
# version 1.x.x
export ANTHROPIC_MODEL=gpt-5
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5(minimal) # we don't recommend using gpt-5(minimal), use qwen3-coder-flash or gemini-2.5-flash-lite instead
```

Using OpenAI GPT 5 Codex models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5-codex(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5-codex(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5-codex(low) # we don't recommend using gpt-5-codex(low), use qwen3-coder-flash or gemini-2.5-flash-lite instead
# version 1.x.x
export ANTHROPIC_MODEL=gpt-5-codex
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5-codex(low) # we don't recommend using gpt-5-codex(low), use qwen3-coder-flash or gemini-2.5-flash-lite instead
```

Using Claude models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-1-20250805
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-5-20250929
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-3-5-haiku-20241022
# version 1.x.x
export ANTHROPIC_MODEL=claude-sonnet-4-20250514
export ANTHROPIC_SMALL_FAST_MODEL=claude-3-5-haiku-20241022
```

Using Qwen models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-coder-flash
# version 1.x.x
export ANTHROPIC_MODEL=qwen3-coder-plus
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-coder-flash
```

Using iFlow models:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# version 2.x.x
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-max
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-235b-a22b-instruct
# version 1.x.x
export ANTHROPIC_MODEL=qwen3-max
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-235b-a22b-instruct
```