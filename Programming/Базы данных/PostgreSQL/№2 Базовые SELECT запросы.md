# **SELECT** - выборка
Работаем с учебной базой данных Northwind из [курса](obsidian://open?vault=thebefast&file=remote-blog%2FEducation%2F%D0%9A%D1%83%D1%80%D1%81%D1%8B%2F2024%2F%D0%91%D0%B0%D0%B7%D1%8B%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.%20SQL.%20PostgreSQL.) по SQL.
```sql
SELECT *
FROM products
```
*SELECT - выборка, \* - выбираем все columns, FROM products - из таблицы продуктов.*

Сделаем запрос нужных нам столбцов:
```sql
SELECT product_id, product_name, unit_price
FROM products
```
*получили нужные нам три столбца, с максимальным количеством строк*
# Арифметические операции SQL
- + сложение
- - вычитание
- * умножение
- / деление
- ^ степень
- |/ квадратный корень
- % деление по модулю
- множество других операций и функций

Попробуем использовать оператор умножения.
```sql
SELECT product_id, product_name, unit_price*units_in_stock
FROM products
```
При помощи этого запроса, мы получим столбцы *product_id*, *product_name*, и третий столбец, в котором получим данные о произведении(умножении) значений из столбцов *unit_price* и *units_in_stock*. 

# Операторы сравнения
- a = b
- a > b
- a >= b
- a <= b
- a <> b *или* a != b
# **DISTINCT** - оператор уникальности
При использовании оператора _DISTINCT_ для двух и более колонок будут удаляться записи, которые имеют одинаковые значения по всем полям
```sql
SELECT DISTINCT country
FROM employees
```
*Выборка УНИКАЛЬНЫХ строк из колонок. (скрываются дубликаты)*

# Функции и логические операторы в **SELECT** запросах
## Функция COUNT
```sql
SELECT COUNT (DISTINCT country)
FROM employees
```
*из таблицы с нашими работниками, подсчитываем (==count==) уникальное количество стран DISTINCT. Если не использовать DISTINCT то получим повторения, и не узнаем сколько разных стран в нашей таблице*

## Оператор WHERE
### WHERE с операторами сравнения
```sql
SELECT *
FROM table -- из таблица
WHERE condition -- условие
```

==boolean операторы сравнения==
- a = b
- a > b
- a >= b
- a <= b
- a <> b *или* a != b

```sql
SELECT company_name, contact_name, phone
FROM customers
WHERE country = 'USA'
```
*выводим customers с страной США*

```sql
SELECT *
FROM products
WHERE unit_price > 20
```
*выводим строки с товаром, цена которого больше 20*

```sql
SELECT COUNT(*)
FROM products
WHERE unit_price > 20
```
*выводим количество продуктов, цена которого больше 20*

```sql
SELECT *
FROM products
WHERE discontinued = 1
```
*выводим строки с товаром, который мы уже НЕ продаем*

```sql
SELECT *
FROM customers
WHERE city != 'Berlin'
```
*выводим строки customers не из Берлина*

```sql
SELECT *
FROM orders
WHERE order_date > '1998-03-01' -- дату заключаем в одинарные кавычки
```
*по стандарту ГОД-МЕСЯЦ-ЧИСЛО
выводим заказы позднее 3 марта 1998 года*

### WHERE с операторами 'and' и 'or'
**and**
```sql
SELECT *
FROM table -- из таблицы
WHERE condition1 AND condition2 -- где 1 и 2 условие истина (true)
```

**or**
```sql
SELECT *
FROM table -- из таблицы
WHERE condition1 OR condition2 -- где 1 или 2 условие истина (true)
```

```sql
SELECT *
FROM products
WHERE unit_price > 25 AND units_in_stock > 40
```
*выводим продукты, цена которых >25 и количество на складе >40*

```sql
SELECT *
FROM customers
WHERE city = 'Berlin' OR city = 'London' OR city = 'San Francisco'
```
*выводим заказчиков из Берлина, Лондона, Сан-Франциско*

```sql
SELECT *
FROM orders
WHERE shipped_date > '1998-04-30' AND (freight < 75 OR freight > 150)
```
*выводим заказы отгруженные после 30 апреля 1998 года и весом <75 или >150*


## Оператор BETWEEN
BETWEEN - интервал между, *не строгое неравенство*, включая значения от и до.
*включая границы*

```sql
SELECT COUNT(*)
FROM orders
WHERE freight BETWEEN 20 AND 40; -- including boundaries
```

```sql
SELECT *
FROM orders
WHERE order_date BETWEEN '1998-03-30' and '1998-04-03'
```
*выводим список заказов, осуществленных в интервале от 30 марта до 03 апреля 1998 г.*

## Оператор IN
```sql
SELECT *
FROM customers
WHERE country IN ('Mexico','Germany','USA','Canada')
```
*выводим список customers, из стран Мексика, Германия, США, Канада*

```sql
SELECT *
FROM products
WHERE category_id IN (1,3,5,7)
```
*оператор IN работает также и с числами, датами и т.д.*

## Оператор NOT
```sql
SELECT *
FROM customers
WHERE country NOT IN ('Mexico','Germany','USA','Canada')
```
*выводим список customers, из любых стран кроме: Мексика, Германия, США, Канада*
## Оператор сортировки ORDERBY
```sql
SELECT DISTINCT country
FROM customers
ORDER BY country ASC -- ORDER BY сортировка ASC - ascending (по возрастанию)
-- или наоборот DESC - descending (по убыванию)
```
*выводим страны по возрастанию*

```sql
SELECT DISTINCT country, city
FROM customers
ORDER BY country DESC, city, ASC
```
*выводим страны и города, где страны отсортированы по Z-A, а города A-Z*

## Скалярные функции MIN и MAX
```sql
SELECT ship_city, order_date
FROM orders
WHERE ship_city = 'London'
ORDER BY order_date
```
*получаем список заказов из Лондона, отсортированных по дате (от старых к новым)*

```sql
SELECT MIN(order_date)
FROM orders
WHERE ship_city = 'London'
```
*получаем самый старый заказ из Лондона (самый ранний)

```sql
SELECT MIN(order_date)
FROM orders
WHERE ship_city = 'London'
ORDER BY order_date DESC
```
или
```sql
SELECT MAX(order_date)
FROM orders
WHERE ship_city = 'London'
```
*наоборот получим самый новейший заказ из Лондона (самый новый)*

## Функция AVG
```sql
SELECT AVG(unit_price)
FROM products
WHERE discontinued != 1
```
*получаем среднюю цену продуктов, находящихся в продаже (не discontinued)*

## Функция SUM
```sql
SELECT SUM(units_in_stock)
FROM products
WHERE discontinued != 1
```
*получаем сколько единиц товаров в продаже*

# Pattern Matching with **LIKE**
**%** - placeholder (заполнитель) означающий 0, 1 и более символов
**\_**  - ровно 1 любой символ
- LIKE'U%' - строки, начинающиеся с U
- LIKE'%a' - строки, кончающиеся на a
- LIKE'%John%' - строки, содержащие John
- LIKE'J%n' - строки, начинающиеся на J и кончающиеся на n
- LIKE'\_oh\_' - строки, где 2,3 символы - oh, а первый (1) и последний (4) - любые
- LIKE '\_oh%' - строки, где 2,3 символы - oh, первый (1) любой, и в конце 0, 1 и более любых символов

```sql
SELECT last_name, first_name
FROM employees
WHERE first_name LIKE '%n'
```
*найти сотрудников у которых ==имя== кончается на n*

```sql
SELECT last_name, first_name
FROM employees
WHERE last_name LIKE 'B%'
```
*найти сотрудников у которых ==фамилия== начинается на B*

```sql
SELECT last_name, first_name
FROM employees
WHERE last_name LIKE '_uch%'
```
*найти сотрудников у которых ==фамилия== начинается на любой символ, содержит uch и заканчивается 0, 1 или любым кол-вом символов*
# **LIMIT**
Конструкция `LIMIT` позволяет получить только часть строк от результата запроса.

Применяя `LIMIT`, важно использовать также предложение `ORDER BY`, чтобы строки результата выдавались в определённом порядке. Иначе будут возвращаться непредсказуемые подмножества строк. Вы можете запросить строки с десятой по двадцатую, но какой порядок вы имеете в виду? Порядок будет неизвестен, если не добавить `ORDER BY`

Дополнение - `OFFSET` сколько строк пропустить.
```sql
SELECT product_name, unit_price
FROM products
WHERE discontinued != 1
ORDER BY unit_price DESC
LIMIT 10
OFFSET 3
```

# **IS NULL**, **IS NOT NULL**
```sql
SELECT ship_city, ship_region, ship_country
FROM orders
WHERE ship_region IS NULL
```

# Группировка **GROUP BY**
```sql
SELECT ship_country, COUNT(*)
FROM orders
WHERE freight > 50
GROUP BY ship_country
ORDER BY COUNT(*) DESC
```
*получаем на вывод - количество заказов, вес которых >50, сгруппировали по странам и отсортировали по количеству (от больших к меньшему)*

# **WHERE** и пост-фильтр **HAVING**
```sql
SELECT category_id, SUM(unit_price * units_in_stock)
FROM products
WHERE discontinued != 1
GROUP BY category_id
HAVING SUM(unit_price * units_in_stock) > 5000
ORDER BY SUM(unit_price * units_in_stock)
```
Заметим, что пост-фильтр `HAVING` должен находиться после `WHERE` и до `ORDER BY`.

# **UNION**, **INTERSECT**, **EXCEPT** - операции над множествами
## UNION (объединение)
```sql
SELECT country
FROM customers
UNION
SELECT country
FROM employees
```
*объединяет 2 запроса и удаляет дубликаты
UNION как бы выполняет DISTINCT*

## INTERSECT (пересечение)
```sql
SELECT country
FROM customers
INTERSECT
SELECT country
FROM suppliers
```
*выводим список стран, из которых одновременно происходят и ==customers== и ==suppliers==*

## EXCEPT (исключение)
```sql
SELECT country
FROM customers
EXCEPT
SELECT country
FROM suppliers
```
*выводим те страны, где живут ==customers== но не проживают ==suppliers==*
