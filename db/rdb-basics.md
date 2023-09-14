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

*example:*
```
DROP TABLE IF EXISTS users;
```

## Типы данных

- String:
  - ```varchar()``` - (varying character) с ограничением максимальной длины
  - ``text`` - без ограничения

- Number:
  - ```integer``` - целое число
  - ```bigint``` - большое целое число
  - ```numeric``` - число высокой точности в расчётах (напр. для работы с деньгами)

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

## Select data

> [**SELECT**](https://www.postgresql.org/docs/current/sql-select.html) - выборка данных из таблиц

*example:*
```
SELECT * FROM users;

-- порядок полей в выводе соответствует порядку в запросе
SELECT name, age FROM users;

SELECT * FROM user WHERE birth_date > '10.01.1988';
```

> LIMIT - ограничивает количество записей в выборке.  
> Пагинация лучше делать в запросе к базе, а не в коде.

*example:*
```
SELECT * FROM users LIMIT 30;
```

> ORDER BY - упорядочить вывод; SQL не гарантирует упорядоченность в выводе;  
> по умолчанию прямой порядок, DESC - обратный порядок.

*example:*
```
SELECT * FROM users ORDER BY age DESC;

-- длинные запросы форматируют
SELECT
  name,
  age
FROM users
WHERE age > 18
ORDER BY age
LIMIT 10;
```

## Реляционная модель данных

> Популярные представления данных: 
> - иерархическая модель
> - сетевая модель
> - реляционная модель

> **Иерархическая модель** - данные представлены в виде дерева, где дочерние элементы зависят от родительских;  
> сложно представить элементы с несколькими родителями.

> **Сетевая модель** - представлена графом, позволяет иметь несколько предков;  
> недостаток - алгоритм выборки данных связан со структурой данных.

> [**Реляционная модель**](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%BB%D1%8F%D1%86%D0%B8%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85) - данные это набор отношений.

> **Нормальная форма** - уменьшает избыточность данных и дублирование, что сокращает возможность ошибок;  
> существует 6 нормальных форм, каждая последующая включает предыдущие.

> **Первая нормальная форма**:
> - ячейка должна хранить только одно значение
> - данные в колонке должны быть одного типа
> - каждая запись должна однозначно отличаться от остальных записей

> [**Primary key**](https://ru.wikipedia.org/wiki/%D0%9F%D0%B5%D1%80%D0%B2%D0%B8%D1%87%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BB%D1%8E%D1%87) - первичный ключ, поле которое содержит уникальный id записи;
> два вида:  
> - естественный - значение из реального мира (номер паспорта, телефон)
> - [суррогатный](https://ru.wikipedia.org/wiki/%D0%A1%D1%83%D1%80%D1%80%D0%BE%D0%B3%D0%B0%D1%82%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BB%D1%8E%D1%87) - автоматически генерится базой (число, хеш)  
>
> Обычно это первая колонка id со специальным указателем PRIMARY KEY (db сама будет следить за уникальностью значений в такой колонке).

*example:*
```
CREATE TABLE users (
  id bigint PRIMARY KEY,
  name varchar(255)
);
```
