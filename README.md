# Yandex Disk MCP Server

MCP-сервер для управления файлами на Яндекс Диске. Работает с любым MCP-клиентом: Claude Desktop, Claude Code, Cursor, и др.

## Возможности (18 инструментов)

| Инструмент | Описание |
|---|---|
| `disk_info` | Информация о диске (объём, использовано, корзина) |
| `list_files` | Список файлов и папок по пути |
| `get_file_info` | Метаданные файла/папки |
| `create_folder` | Создание папки |
| `delete` | Удаление файла/папки (в корзину или навсегда) |
| `copy` | Копирование |
| `move` | Перемещение / переименование |
| `get_download_link` | Получить ссылку для скачивания |
| `get_upload_link` | Получить URL для загрузки файла |
| `upload_from_url` | Загрузить файл на диск по внешнему URL |
| `publish` | Сделать файл/папку публичным |
| `unpublish` | Убрать публичный доступ |
| `list_public` | Список всех публичных ресурсов |
| `list_trash` | Содержимое корзины |
| `restore_from_trash` | Восстановить из корзины |
| `clear_trash` | Очистить корзину |
| `search_files` | Поиск/фильтрация файлов по типу |
| `last_uploaded` | Недавно загруженные файлы |
| `operation_status` | Статус асинхронной операции |

## Установка

```bash
git clone <repo-url> yandex-disk-mcp
cd yandex-disk-mcp
npm install
npm run build
```

## Получение OAuth-токена

1. Перейдите на https://oauth.yandex.ru
2. Нажмите «Создать приложение»
3. Укажите название (например, «MCP Yandex Disk»)
4. В разделе «Платформы» выберите «Веб-сервисы» и добавьте redirect URI: `https://oauth.yandex.ru/verification_code`
5. В разделе «Доступы» выберите **Яндекс.Диск REST API** → отметьте все нужные права:
   - `cloud_api:disk.app_folder` — доступ к папке приложения
   - `cloud_api:disk.read` — чтение
   - `cloud_api:disk.write` — запись
   - `cloud_api:disk.info` — информация о диске
6. Сохраните приложение и скопируйте **Client ID**
7. Откройте в браузере:
   ```
   https://oauth.yandex.ru/authorize?response_type=token&client_id=ВАШ_CLIENT_ID
   ```
8. Авторизуйтесь и скопируйте полученный токен

## Настройка

### Claude Desktop / Claude Code

Добавьте в файл конфигурации MCP (`claude_desktop_config.json` или `settings.json`):

```json
{
  "mcpServers": {
    "yandex-disk": {
      "command": "node",
      "args": ["/path/to/yandex-disk-mcp/dist/index.js"],
      "env": {
        "YANDEX_DISK_TOKEN": "ваш_oauth_токен"
      }
    }
  }
}
```

### Cursor / другие MCP-клиенты

Аналогично — укажите команду запуска и переменную окружения `YANDEX_DISK_TOKEN`.

### Через npx (после публикации в npm)

```json
{
  "mcpServers": {
    "yandex-disk": {
      "command": "npx",
      "args": ["yandex-disk-mcp"],
      "env": {
        "YANDEX_DISK_TOKEN": "ваш_oauth_токен"
      }
    }
  }
}
```

## Примеры использования

После подключения к MCP-клиенту можно просить:

- «Покажи что лежит на моём Яндекс Диске»
- «Создай папку disk:/Projects/new-project»
- «Скопируй файл disk:/report.pdf в disk:/backup/report.pdf»
- «Сделай файл disk:/photo.jpg публичным и дай ссылку»
- «Что в корзине? Восстанови последний удалённый файл»
- «Сколько места осталось на диске?»

## Технический стек

- TypeScript + Node.js
- @modelcontextprotocol/sdk (stdio transport)
- Yandex Disk REST API v1

## Лицензия

MIT
