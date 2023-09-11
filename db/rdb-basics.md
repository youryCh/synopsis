# Основы реляционной базы данных

> **Реляционная модель данных** - в виде связанных таблиц.

> **PostgreSQL** - реляционная СУБД; стоит на той же машине, что и db.

> **psql** - утилита для работы с postgres.

> [**pgAdmin**](https://www.pgadmin.org/) - free postgres tool


## SQL

```CREATE DATABASE <name>``` - создать db

```DROP DATABASE <name>``` - удалить db

```--comment``` - комментирование

Example:
```
CREATE TABLE courses (
  name varchar(255),
  body text
);
```