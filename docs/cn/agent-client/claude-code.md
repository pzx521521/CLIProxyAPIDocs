# Claude Code

启动 CLIProxyAPI 服务器, 设置如下系统环境变量：

- `ANTHROPIC_BASE_URL`
- `ANTHROPIC_AUTH_TOKEN`
- `ANTHROPIC_DEFAULT_OPUS_MODEL`
- `ANTHROPIC_DEFAULT_SONNET_MODEL`
- `ANTHROPIC_DEFAULT_HAIKU_MODEL`

或对应 1.x.x 版本设置如下系统环境变量：

- `ANTHROPIC_MODEL`
- `ANTHROPIC_SMALL_FAST_MODEL`

使用 Gemini 模型：
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=gemini-2.5-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=gemini-2.5-flash
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gemini-2.5-flash-lite
# 1.x.x 版本
export ANTHROPIC_MODEL=gemini-2.5-pro
export ANTHROPIC_SMALL_FAST_MODEL=gemini-2.5-flash
```

使用 OpenAI GPT 5 模型：
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5(minimal) # 不推荐使用 gpt-5(minimal)，建议使用 qwen3-coder-flash 或者 gemini-2.5-flash-lite 代替
# 1.x.x 版本
export ANTHROPIC_MODEL=gpt-5
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5(minimal) # 不推荐使用 gpt-5(minimal)，建议使用 qwen3-coder-flash 或者 gemini-2.5-flash-lite 代替
```

使用 OpenAI GPT 5 Codex 模型:
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5-codex(high)
export ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5-codex(medium)
export ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5-codex(low) # 不推荐使用 gpt-5-codex(low)，建议使用 qwen3-coder-flash 或者 gemini-2.5-flash-lite 代替
# 1.x.x 版本
export ANTHROPIC_MODEL=gpt-5-codex
export ANTHROPIC_SMALL_FAST_MODEL=gpt-5-codex(low) # 不推荐使用 gpt-5-codex(low)，建议使用 qwen3-coder-flash 或者 gemini-2.5-flash-lite 代替
```

使用 Claude 模型：
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-1-20250805
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-5-20250929
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-3-5-haiku-20241022
# 1.x.x 版本
export ANTHROPIC_MODEL=claude-sonnet-4-20250514
export ANTHROPIC_SMALL_FAST_MODEL=claude-3-5-haiku-20241022
```

使用 Qwen 模型：
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-coder-flash
# 1.x.x 版本
export ANTHROPIC_MODEL=qwen3-coder-plus
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-coder-flash
```

使用 iFlow 模型：
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8317
export ANTHROPIC_AUTH_TOKEN=sk-dummy
# 2.x.x 版本
export ANTHROPIC_DEFAULT_OPUS_MODEL=qwen3-max
export ANTHROPIC_DEFAULT_SONNET_MODEL=qwen3-coder-plus
export ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen3-235b-a22b-instruct
# 1.x.x 版本
export ANTHROPIC_MODEL=qwen3-max
export ANTHROPIC_SMALL_FAST_MODEL=qwen3-235b-a22b-instruct
```