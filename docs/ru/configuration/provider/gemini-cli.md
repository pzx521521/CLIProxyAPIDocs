# Gemini CLI (Gemini через OAuth):

```bash
./cli-proxy-api --login
```

Если вы являетесь существующим пользователем Gemini Code, вам может потребоваться указать идентификатор проекта:

```bash
./cli-proxy-api --login --project_id <your_project_id>
```

Локальный OAuth callback использует port `8085`.

Опции: добавьте `--no-browser`, чтобы вывести URL для входа вместо открытия браузера. Локальный OAuth callback использует port `8085`.