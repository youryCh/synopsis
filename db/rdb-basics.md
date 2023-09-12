# Основы реляционной базы данных

> **Реляционная модель данных** - в виде связанных таблиц.

> **PostgreSQL** - реляционная СУБД; стоит на той же машине, что и db.

> **psql** - утилита для работы с postgres.

> [**pgAdmin**](https://www.pgadmin.org/) - free postgres tool

> **scheme** - пространство имен для группировки таблиц (public - by default)

## Полезные команды psql

```\d``` - список таблиц

```\d <table_name>``` - структура таблицы

```\l``` - список всех db

```\?``` - help

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
  - ``FALSE`` (или 'f', 'false', 'n', 'no', 'off', '0')

```NULL``` - значение не заполненного поля любого типа

## Вставка и модификация

> **DML** - (Data Manipulation Language) подмножество SQL для работы с данными;  
>           включает: INSERT, UPDATE, DELETE

> [**INSERT**](https://www.postgresql.org/docs/current/sql-insert.html) - вставить строку в таблицу

```INSERT INTO <table> (field) VALUES ('value');``` - вставка данных

*example:*
```
INSERT INTO users (name, age, is_admin)
  VALUES ('Ann', 38, FALSE);
```

> Каждый INSERT создаёт новую запись (строку) в таблице

*example:*
```
-- частичное заполнение полей
-- остальные поля строки будут null

INSERT INTO table (field_1) VALUES ('value');
```

*example:*
```
-- вставка сразу нескольких строк

INSERT INTO table (field_1, field_2) VALUES
  ('JS', 1), ('PHP', 2), ('PY', 3);
```

*example:*
```
-- вставка всех полей
-- если не указаны поля это тоже что перечислить все поля

INSERT INTO table VALUES ('JS', 'PHP', 'PY');
```

```SELECT``` - извлечь данные, посмотреть содержимое

*example:*
```
-- * - все данные
SELECT * FROM table;
```
### Обновление данных

> [**UPDATE**](https://www.postgresql.org/docs/current/sql-update.html) - обновить строки

> Обновятся все записи, удовлетворяющие условию после WHERE.  
> Повторные вызовы того же UPDATE не сделают никаких изменений.

```UPDATE <table_name> SET field = 'value' WHERE field = 'value';``` - обновление записи в таблице

*example:*
```
-- обновление сразу нескольких полей

UPDATE users SET name = 'John', age = 44 WHERE name = 'Ann';
```

*example:*
```
-- без WHERE обновит поля во всех строках!

UPDATE users SET name = 'John';
```

*example:*
```
-- использование сравнения и логических операций AND OR

UPDATE users SET name = 'Ann' WHERE age > 18;

UPDATE users SET name = 'John' WHERE name = 'Ann' AND age < 40;

-- использование скобок для приоритизации

UPDATE users SET name = 'Ann' WHERE (age > 20 AND age < 40); OR age = 65;
```

### Удаление данных

> [**DELETE**](https://www.postgresql.org/docs/current/sql-delete.html)

> Без указания WHERE удалит все поля!

*example:*
```
DELETE FROM users WHERE name = 'Ann';
```

> [**TRUNCATE**](https://www.postgresql.org/docs/current/sql-truncate.html) - для полной очистки таблицы/таблиц.

*example:*
```
TRUNCATE users;
```