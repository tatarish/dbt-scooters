dbt_scooters
dbt_scooters — проект аналитической трансформации данных для сервиса аренды электросамокатов с использованием dbt. Проект включает модели для расчёта ключевых метрик, построения витрин и автоматизации процессов аналитики.

Быстрый старт
Клонируйте репозиторий и перейдите в папку проекта

git clone https://github.com/tatarish/dbt-scooters
cd dbt_scooters
Настройте соединение с вашей БД в profiles.yml

Установите зависимости

dbt deps
Выполните базовые dbt-команды:

| Действие | Команда | |-------------------------|--------------------------| | Проверить соединение | dbt debug | | Собрать все модели | dbt run | | Выполнить тесты | dbt test | | Сгенерировать документы | dbt docs generate | | Открыть документацию | dbt docs serve | | Пересобрать модель | dbt run --select <model_name> |

Структура проекта
models/ — sql-модели трансформаций
tests/ — пользовательские тесты
snapshots/ — снимки данных для анализа изменений

4. Активируйте PostGIS в базе PostgreSQL, если еще этого не делали:

```sql
create extension postgis schema public;
```

## Полезные макросы

- `dbt run-operation create_role --args "name: finance"` - создание роли в базе (на примере роли "finance")

## Основные команды dbt

- `dbt debug` - проверка подключения к хранилищу данных (проверка профиля)
- `dbt parse` - парсинг файлов проекта (проверка корректности)
- `dbt compile` - компилирует dbt-модели и создает SQL-файлы
- `dbt run` - материализация моделей в таблицы и представления
- `dbt test` - запускает тесты для проверки качества данных
- `dbt seed` - загружает данные в таблицы из CSV-файлов
- `dbt build` - основная команда, комбинирует run, test и seed
- `dbt source freshness` - проверка актуальности данных в источниках
- `dbt docs generate` - генерирует документацию проекта
- `dbt docs serve` - запускает локальный сервер для просмотра документации

## desc
select
    ns.nspname as schema_name,
    cls.relname as table_name,
    des.description as table_description,
    cls.relkind = 'r' as is_table
from
    pg_catalog.pg_class cls
    join pg_catalog.pg_namespace ns
      on ns.oid = cls.relnamespace
    left join pg_catalog.pg_description des
      on des.objoid = cls.oid
      and des.objsubid = 0
where
    ns.nspname in ('dbt', 'finance')
    and cls.relkind in ('r', 'v') -- only tables and views
order by
    1,
    2;