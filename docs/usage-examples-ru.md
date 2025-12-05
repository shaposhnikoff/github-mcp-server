# GitHub MCP Server — Руководство на русском языке

## Что делает этот сервер?

**GitHub MCP Server** — это сервер протокола Model Context Protocol (MCP), который соединяет AI-инструменты с платформой GitHub. Он позволяет AI-агентам, ассистентам и чат-ботам:

- **Читать репозитории и файлы кода** — просматривать содержимое файлов, структуру проекта, коммиты
- **Управлять Issues и Pull Requests** — создавать, обновлять, комментировать задачи и запросы на слияние
- **Анализировать код** — проверять безопасность, смотреть Dependabot алерты
- **Автоматизировать рабочие процессы** — мониторить GitHub Actions, запускать воркфлоу
- **Взаимодействовать с командой** — получать уведомления, участвовать в обсуждениях

Всё это происходит через естественный язык — вы просто описываете, что хотите сделать, и AI выполняет действие через GitHub API.

---

## Установка и настройка в VS Code

### Вариант 1: Удалённый сервер (рекомендуется)

Самый простой способ — использовать удалённый сервер GitHub. Добавьте в настройки VS Code:

**Файл:** `.vscode/mcp.json` или настройки пользователя VS Code

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

> **Требования:** VS Code версии 1.101 или выше

### Вариант 2: Локальный сервер через Docker

Если вам нужен локальный контроль, используйте Docker:

**Файл:** `.vscode/mcp.json`

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "github_token",
      "description": "GitHub Personal Access Token",
      "password": true
    }
  ],
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

> **Требования:** Установленный Docker и Personal Access Token от GitHub

### Вариант 3: Сборка из исходников

```bash
# Клонируйте репозиторий
git clone https://github.com/github/github-mcp-server.git
cd github-mcp-server

# Соберите бинарник
go build -v ./cmd/github-mcp-server

# Настройте VS Code
```

**Файл:** `.vscode/mcp.json`

```json
{
  "servers": {
    "github": {
      "command": "/path/to/github-mcp-server",
      "args": ["stdio"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

---

## Практические примеры использования

После настройки сервера откройте GitHub Copilot Chat в VS Code (переключитесь в режим Agent) и попробуйте следующие запросы:

### 1. Работа с репозиториями

#### Получить информацию о репозитории
```
Покажи структуру репозитория github/github-mcp-server
```

#### Найти файл в репозитории
```
Найди файл main.go в репозитории github/github-mcp-server
```

#### Посмотреть содержимое файла
```
Покажи содержимое файла README.md из репозитория microsoft/vscode
```

#### Найти репозитории по теме
```
Найди популярные репозитории на Go для работы с Kubernetes
```

### 2. Работа с Issues (задачами)

#### Создать новую задачу
```
Создай issue в репозитории myuser/myproject с заголовком "Добавить документацию" и описанием "Нужно добавить README на русском языке"
```

#### Посмотреть открытые задачи
```
Покажи открытые issues в репозитории facebook/react
```

#### Добавить комментарий к задаче
```
Добавь комментарий "Начинаю работать над этой задачей" к issue #42 в репозитории myuser/myproject
```

#### Найти свои задачи
```
Найди все мои открытые issues
```

### 3. Работа с Pull Requests

#### Создать Pull Request
```
Создай pull request из ветки feature/new-feature в main в репозитории myuser/myproject с заголовком "Добавлена новая функция"
```

#### Посмотреть открытые PR
```
Покажи открытые pull requests в репозитории kubernetes/kubernetes
```

#### Получить diff изменений
```
Покажи изменения в pull request #123 репозитория myuser/myproject
```

#### Посмотреть статус CI
```
Какой статус проверок у pull request #456 в репозитории myuser/myproject?
```

### 4. Работа с GitHub Actions

#### Посмотреть воркфлоу
```
Покажи список workflow в репозитории github/github-mcp-server
```

#### Проверить статус последнего запуска
```
Какой статус последнего запуска CI в репозитории myuser/myproject?
```

#### Посмотреть логи упавшего джоба
```
Покажи логи неудавшегося джоба в последнем запуске workflow test.yml в репозитории myuser/myproject
```

#### Запустить воркфлоу
```
Запусти workflow deploy.yml в репозитории myuser/myproject на ветке main
```

### 5. Безопасность и анализ

#### Проверить уязвимости
```
Покажи Dependabot alerts для репозитория myuser/myproject
```

#### Найти проблемы кода
```
Есть ли code scanning alerts в репозитории myuser/myproject?
```

#### Проверить секреты
```
Покажи secret scanning alerts для моего репозитория
```

### 6. Работа с командой

#### Получить уведомления
```
Покажи мои непрочитанные уведомления на GitHub
```

#### Найти участников команды
```
Кто входит в команду frontend в организации myorg?
```

#### Получить информацию о пользователе
```
Покажи информацию о моём GitHub профиле
```

### 7. Работа с коммитами и ветками

#### Посмотреть историю коммитов
```
Покажи последние 10 коммитов в репозитории github/github-mcp-server
```

#### Создать ветку
```
Создай ветку feature/my-feature от main в репозитории myuser/myproject
```

#### Посмотреть изменения в коммите
```
Покажи изменения в коммите abc1234 репозитория myuser/myproject
```

### 8. Работа с файлами

#### Создать файл
```
Создай файл docs/CONTRIBUTING.md в репозитории myuser/myproject на ветке main с содержимым "# Как внести вклад"
```

#### Обновить файл
```
Обнови файл README.md в репозитории myuser/myproject, добавив раздел "## Установка"
```

---

## Настройка наборов инструментов (Toolsets)

Вы можете ограничить доступные инструменты для улучшения производительности:

### Минимальный набор (только чтение)
```json
{
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "-e", "GITHUB_TOOLSETS=repos,issues",
        "-e", "GITHUB_READ_ONLY=1",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

### Полный набор инструментов
```json
{
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "-e", "GITHUB_TOOLSETS=all",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

### Доступные наборы инструментов

| Набор | Описание |
|-------|----------|
| `context` | Информация о текущем пользователе и контексте |
| `repos` | Работа с репозиториями |
| `issues` | Работа с задачами |
| `pull_requests` | Работа с pull requests |
| `actions` | GitHub Actions и CI/CD |
| `code_security` | Сканирование кода на уязвимости |
| `discussions` | Обсуждения в репозитории |
| `notifications` | Уведомления |
| `users` | Информация о пользователях |
| `all` | Все инструменты |

---

## Режим только для чтения

Если вы хотите только читать данные без возможности изменений:

```json
{
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "-e", "GITHUB_READ_ONLY=1",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

---

## GitHub Enterprise Server

Для работы с GitHub Enterprise Server добавьте переменную `GITHUB_HOST`:

```json
{
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "-e", "GITHUB_HOST",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}",
        "GITHUB_HOST": "https://github.your-company.com"
      }
    }
  }
}
```

---

## Полезные ссылки

- [Основная документация (README.md)](../README.md)
- [Руководство по настройке сервера](./server-configuration.md)
- [Обработка ошибок](./error-handling.md)
- [Политики и управление](./policies-and-governance.md)

---

## Получение Personal Access Token

1. Перейдите на https://github.com/settings/personal-access-tokens/new
2. Выберите тип токена (рекомендуется Fine-grained)
3. Установите права доступа:
   - `repo` — для работы с репозиториями
   - `read:org` — для работы с организациями
   - `workflow` — для запуска Actions
4. Создайте токен и сохраните его в безопасном месте

---

*Эта документация была создана для русскоязычных пользователей GitHub MCP Server.*
