# Основы реляционной базы данных

> **Реляционная модель данных** - в виде связанных таблиц.

> **PostgreSQL** - реляционная СУБД; стоит на той же машине, что и db.

> **psql** - утилита для работы с postgres.

> [**pgAdmin**](https://www.pgadmin.org/) - free postgres tool


## SQL

```CREATE DATABASE <name>;``` - создать db

```DROP DATABASE <name>;``` - удалить db

```--comment``` - комментирование

```CREATE TABLE <db_name> (column);``` - создать таблицу в db

*example:*
```
CREATE TABLE courses (
  name varchar(255),
  body text
);
```

```DROP TABLE <name>;``` - удалить таблицу

## Типы данных

- String:
  - ```varchar()``` - (varying character) с ограничением максимальной длины
  - ``text`` - без ограничения

- Number:
  - `integer` - целое число
  - ```bigint``` - большое целое число

- Date:
  - ```date``` - дата без времени
  - ```timestamp``` - дата/время без часового пояса
  - ```time``` - время

- Boolean:
  - ```TRUE``` (или 't', 'true', 'y', 'yes', 'on', '1')
  - ```FALSE``` (или 'f', 'false', 'n', 'no', 'off', '0')

```NULL``` - значение не заполненного поля любого типа