# DEV-001 — Создание основы .NET-проекта

Статус: на проверке

## Цель

Создать запускаемую основу backend системы бронирования
комнат на ASP.NET Core.

## Источники

- AGENTS.md — правила работы.
- goal.md — требования к системе.
- Существующий код и технические решения репозитория.

## Что сделать

- Проверить установленный .NET SDK и существующие файлы проекта.
- Если версия .NET уже определена — использовать её.
- Если версия не определена — согласовать её до создания проекта.
- Создать solution и ASP.NET Core Web API проект в src/.
- Добавить GET /health, возвращающий HTTP 200.
- Удалить демонстрационный endpoint из шаблона, если он создан.
- Проверить правила .gitignore для .NET:
  bin/, obj/, .vs/ и локальные секреты.
- Дополнить README инструкциями сборки и запуска,
  не удаляя существующее содержание.

Выбирать минимальную структуру, достаточную для старта.

## Границы задачи

В задачу не входят:
- бизнес-логика бронирования;
- авторизация;
- подключение базы данных;
- frontend;
- Docker и CI/CD;
- дополнительные архитектурные слои без обоснованной необходимости.

## Критерии готовности

- dotnet restore завершается успешно.
- dotnet build завершается успешно.
- Приложение запускается по инструкции из README.
- GET /health возвращает HTTP 200.
- В Git не попадают результаты сборки и секреты.
- В тикете добавлен отчёт с командами и результатами проверок.
- Статус установлен «На проверке».

## Отчёт

Что сделано:

- Проверено окружение: рабочая ветка `feature/dev-001-dotnet`,
  дерево чистое. Установлены .NET SDK 8.0.412, 9.0.200, 10.0.202.
  Версия .NET определена: целевой фреймворк `net10.0` (latest SDK
  10.0.202). Уточнения у владельца не требовалось.
- В `Backend/` создан solution `MeetSpace.slnx` (формат .slnx,
  поддерживается .NET 10) и проект ASP.NET Core Web API
  `src/MeetSpace.Api` (target `net10.0`, minimal API, без OpenAPI).
- В `Program.cs` удалён демонстрационный эндпоинт `/weatherforecast`
  из шаблона; удалён вызов `UseHttpsRedirection` (иначе http-запрос к
  `/health` возвращал 307 вместо 200). Добавлен `GET /health`,
  возвращающий `200 OK` с телом `{"status":"healthy"}`.
- Обновлён `MeetSpace.Api.http` под новый эндпоинт.
- `Backend/.gitignore` уже содержал корректные правила
  (`bin/`, `obj/`, `.vs/`, `appsettings.Development.json`,
  `appsettings.Docker.json`, `.env`); дополнения не требовались.
- Создан `Backend/README.md` с инструкциями по сборке, запуску и
  проверке health-эндпоинта.

Созданные/изменённые файлы (попадают в Git):

- `Backend/MeetSpace.slnx`
- `Backend/README.md`
- `Backend/src/MeetSpace.Api/MeetSpace.Api.csproj`
- `Backend/src/MeetSpace.Api/Program.cs`
- `Backend/src/MeetSpace.Api/MeetSpace.Api.http`
- `Backend/src/MeetSpace.Api/appsettings.json`
- `Backend/src/MeetSpace.Api/Properties/launchSettings.json`
- `tickets/DEV-001-dotnet-setup.md` (статус и отчёт)

Игнорируются (в Git не попадают): `bin/`, `obj/`,
`appsettings.Development.json` (подтверждено `git check-ignore` и
`git status --ignored`).

Команды и фактические результаты проверок:

1. Восстановление пакетов:
   `dotnet restore` (из `Backend/`)
   Результат: `Все проекты обновлены для восстановления.` — успех.

2. Сборка:
   `dotnet build --no-restore` (из `Backend/`)
   Результат:
   `MeetSpace.Api -> ...\bin\Debug\net10.0\MeetSpace.Api.dll`
   `Сборка успешно завершена. Предупреждений: 0 Ошибок: 0` — успех.

3. Запуск:
   `dotnet run --project src/MeetSpace.Api --no-build --launch-profile http`
   (из `Backend/`)
   Результат: `Now listening on: http://localhost:5231`,
   `Hosting environment: Development` — приложение запустилось.

4. Health-check:
   `curl.exe -s -i http://localhost:5231/health`
   Результат:
   `HTTP/1.1 200 OK`
   `Content-Type: application/json; charset=utf-8`
   Тело: `{"status":"healthy"}` — успех.

5. Проверка отсутствия секретов и артефактов сборки в Git:
   `git status -uall --porcelain Backend`
   Результат: только исходники и README; `bin/`, `obj/`,
   `appsettings.Development.json` помечены как игнорируемые — успех.

Все критерии готовности выполнены.