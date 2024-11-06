*sqrtsincos2 - 5432*
[Источник](obsidian://open?vault=thebefast&file=remote-blog%2FEducation%2F%D0%9A%D1%83%D1%80%D1%81%D1%8B%2F2024%2F%D0%91%D0%B0%D0%B7%D1%8B%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.%20SQL.%20PostgreSQL.)
# Почему PostgreSQL?
- Free (Open Source)
- Лучший выбор для изучения: проинсталлировал и "погнали"!
- "Взрослая" СУБД, которая хорошо поддерживает транзакционность из коробки
- Весьма развитый диалект SQL
- В сравнении с MySQL есть свои плюсы и минусы
- 90% возможностей диалекта SQL поддерживаемого **PostgreSQL** можно без изменений использовать и в других СУБД
# Основные типы данных в PostgreSQL
![[Pasted image 20241106160949.png]]
![[Pasted image 20241106161356.png]]![[Pasted image 20241106161759.png]]
# Другие типы данных
- Arrays
- JSON
- XML
- Геометрические типы и другие спец. типы
- Custom-типы
- NULL - отсутствие данных

# Создание и базовое редактирование БД 
Используем установленный клиент **pgAdmin 4**, запустим его.
Создадим БД при помощи SQL запроса:
```sql
CREATE DATABASE testdb
```
*не указывая параметры  при создании, будут применены параметры по умолчанию*
## Создание таблиц в БД
Начнем Query сеанс с ==**testdb**==, затем
Создадим таблицу при помощи SQL запроса:
```sql
CREATE TABLE publisher
(
	publisher_id integer PRIMARY KEY,
	org_name varchar(128) NOT NULL,
	address text NOT NULL
);

CREATE TABLE book
(
	book_id integer PRIMARY KEY,
	title text NOT NULL,
	isbn varchar(32) NOT NULL
)
```
*этим запросом мы создали две таблицы publisher и book*

Таблицы также можно создать через интерфейс pgAdmin, для этого нужно *"встать"* на нужную нам схему *(schema)*.
![[Pasted image 20241106164658.png]]

## Добавление данных в таблицы
```sql
INSERT INTO book
VALUES
(1, 'The Diary of Young Girl', '0199535566'),
(2, 'Pride and Prejudice', '9780307594006'),
(3, 'To Kill a Mockingbird', '0446310786'),
(4, 'The Book of Gutsy Women: Favourite Stories of Courage and Resilience', '1501178415'),
(5, 'War and Peace', '1788886526');

INSERT INTO publisher
VALUES
(1, 'Everyman''s Library', 'NY'),
(2, 'Oxford University Press', 'NY'),
(3, 'Grand Central Publishing', 'Washington'),
(4, 'Simon & Schuster', 'Chicago');
```
*добавляем данные в таблицу при помощи SQL запроса*

Объединим сущности ==**publisher**== и ==**book**== в новой таблице.
Для начала создадим SQL запрос на создание новой колонки в таблице **==book==**
```sql
ALTER TABLE book
ADD COLLUMN fk_publisher_id;

ALTER TABLE book
ADD CONSTRAINT fk_book_publisher
FOREIGN KEY(fk_publisher_id) REFERENCES publisher(publisher_id)
```
*добавляем колонку fk_publisher_id
далее связываем fk_book_publisher с внешним ключом fk_publisher_id
REFERENCES - ссылаемся на таблицу publisher с колонкой publisher_id*

###### ==Отношение "Один ко многим"== это когда один **publisher** может опубликовать множество книг (*book*).
Данное отношение моделируется при помощи создании дополнительной колонки ==**foreign key**== которая ссылается на ==**primary key**==.

##### ==Отношение "Один к одному"== это когда у одного **person** есть один **passport**.
Данное отношение также моделируется при помощи создании дополнительной колонки ==**foreign key**== которая ссылается на ==**primary key**==.

##### ==Отношение "Многие ко многим"==
Данное отношение моделируется при помощи создании дополнительной таблицы, в которой содержатся соотношения между ==**primary key**== двух разных таблиц.

###### Пример SQL запроса для создания двух разных таблиц и третьей дополнительной, объединяющей ==**primary key**== книг *book* и авторов *author*.
```sql
CREATE TABLE book
(
	book_id integer PRIMARY KEY,
	title text NOT NULL,
	isbn text NOT NULL
);

CREATE TABLE author
(
	author_id integer PRIMARY KEY,
	full_name text not NULL,
	rating real
);

CREATE TABLE book_author
(
	book_id integer REFERENCES book(book_id),
	author_id integer REFERENCES author(author_id),
	CONSTRAINT book_author_pkey PRIMARY KEY (book_id, author_id) -- composite key
);

INSERT INTO book
VALUES
	(1, 'Book for Dummies', '123456'),
	(2, 'Book for Smart Guys', '7890123'),
	(3, 'Book for Happy People', '4567890'),
	(4, 'Book for Unhappy People', '1234567');

INSERT INTO author
VALUES
(1, 'Bob', 4.5),
(2, 'Alice', 4.0),
(3, 'John', 4.7);

INSERT INTO book_author
VALUES
(1, 1),
(2, 1),
(3, 1),
(3, 2),
(4, 1),
(4, 2),
(4, 3);
```
