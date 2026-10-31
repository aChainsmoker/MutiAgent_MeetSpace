# MeetSpace — Backend

Backend системы бронирования переговорных комнат на ASP.NET Core.

## Технологии

- .NET 10
- ASP.NET Core Web API

> Стек будет расширяться в последующих тикетах (EF Core/Npgsql,
> JWT, S3/MinIO, AutoMapper, Swagger и др.).

## Структура

```
Backend/
├── MeetSpace.sln
└── src/
    └── MeetSpace.Api/        # точка входа Web API (GET /health)
```

## Требования

- Установленный .NET 10 SDK (`dotnet --version`).

## Сборка

Из каталога `Backend/`:

```bash
dotnet restore
dotnet build
```

Или одной командой для конкретного проекта:

```bash
dotnet build src/MeetSpace.Api/MeetSpace.Api.csproj
```

## Запуск

Из каталога `Backend/`:

```bash
dotnet run --project src/MeetSpace.Api
```

По умолчанию используется launch-профиль `http` (см.
`Properties/launchSettings.json`), приложение слушает:

- http://localhost:5231

## Проверка

Health-check:

```bash
curl http://localhost:5231/health
```

Ожидаемый ответ: `HTTP 200`, тело `{"status":"healthy"}`.

## Окружение

- Переменная `ASPNETCORE_ENVIRONMENT` устанавливается в `Development`
  через launch-профиль.
- Локальные секреты и пользовательские настройки хранятся в
  `appsettings.Development.json`, который не попадает в Git
  (см. `Backend/.gitignore`).
