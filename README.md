# Some Structured Query Language (SQL) knowledge using mySQL

## SELECT
从table中挑选出几`列(columns)`
```
SELECT column_name1, column_name2
FROM table_name;
```
- Note: The `*` symbol selects all the columns from a table.

## WHERE
The WHERE clause enables us to select `rows` that match specified conditions.
```
SELECT age, country
FROM Customers
WHERE age < 27;

SELECT age, country
FROM Customers
WHERE country <> 'USA';

-- Select multiple conditions
SELECT name, email
From Customers
WHERE state = 'California' OR state = 'Texas';  --  Note that the second state keyword cannot be omitted here!
```

- Operators for a WHERE clause: `<, <=, >, >=, =, <> or !=`
-  Strings in SQL must be enclosed within quotation marks (preferably `single quotation marks`)

### NOT Operator (WHERE NOT)
The NOT operator is used to select a row if the filter condition is FALSE.
```
SELECT *
FROM Customers
WHERE NOT country = 'UK';
```

## IN
Instead of combining multiple conditions using the `OR` operator, we can use a single 'IN' operator.
```
SELECT *
FROM Customers
WHERE country NOT IN ('UK', 'USA', 'Canada');
```

## BETWEEN
The BETWEEN keyword is `all inclusive`.
```
SELECT age, country
FROM Customers
WHERE age BETWEEN 10 AND 30;
```
Can also be used to dates, not only numbers:
```
SELECT age, country
FROM Customers
WHERE bithday BETWEEN `1990-01-01` AND '2000-01-01';
```

## LIKE
Retrive rows matching a specific `string pattern`.
- The `%` sign indicates any number of characters.
- The `_` sign indicates any single character (exactly one). And `___` means exactly 3 characters etc.

```
-- get all the customers whose last name starts with b
SELECT *
FROM Customers
WHERE last_name LIKE 'b%';

SELECT *
FROM Customers
WHERE last_name NOT LIKE 'O_i___';
```

## REGEXP
Retrive rows matching a specific `string pattern`.
- `^` beginning
- `$` end
- `|` logical or
- '[abcd]' choose one
- '[a-f]' choose one from the range
```
SELECT *
FROM Customers
WHERE last_name REGEXP 'field'; -- contains field

SELECT *
FROM Customers
WHERE last_name REGEXP '^field'; -- must starts with field

SELECT *
FROM Customers
WHERE last_name REGEXP 'field$'; -- must ends with field

SELECT *
FROM Customers
WHERE last_name REGEXP 'field$|mac|rose'; -- must ends with field or contains either rose or mac

SELECT *
FROM Customers
WHERE last_name REGEXP '[gim]e';  -- contains ge, ie, or me

SELECT *
FROM Customers
WHERE last_name REGEXP '[a-h]e';  -- contains any character from a to h and then follows an e
```

## IS NULL
Look for records that miss an arribute/field
```
SELECT *
FROM Customers
WHERE phone IS NULL;
```

## ORDER BY (DESC/ASC)
Every table has a `primary column` and values of that column should uniquely identify the records in that table.
And records will be sorted by this primary column by default. To sort the records by a different `column`, use ORDER BY
```
SELECT name, 10 + 1 AS Points
FROM Customers
ORDER BY state DESC, first_name ASC

SELECT state, first_name, name, 10 + 1 AS Points
FROM Customers
ORDER BY 1 DESC, 2 ASC

SELECT *
FROM order_items
WHERE order_id = 2
ORDER BY quantity * unit_price DESC
```
- In mySQL we can sort data by any column whether that column is inselect column or not
- The `10 + 1 AS Points` here will add a new column to the `selected table` (not the original databse table!) and 
- `1` represents the first selected column and '2' likewise
- ORDER BY `doesn't have to be a column name`, can be an `alias` or an `arithmatic expression`
- 

## LIMIT
Limit the number of records got from the query: `LMIT offset, number`
```
SELECT *
FROM Customers
LIMIT 3

SELECT *
FROM Customers
LIMIT 6, 3;   -- skip the first 6 records and select 3 records
```
- if LIMIT number is > than records numbers, will get all records back
- The LIMIT clause should always come at the end

## JOIN
- The `JOIN` keyword forces you to add the `ON` keyword otherwise syntax error
### INNER JOIN
```
SELECT order_id, orders.customer_id, first_name, last_name
FROM orders
JOIN customers ON orders.customer_id = customers.customer_id
```
- Join is by default INNER JOIN so don't need the 'INNER' keyword
- if the selected column name is shared by both joint tables, need to specify the table name from which the column is selected: `orders.customer_id`.
- Add alias, note that after you use an alias in the table, you have to use that alias everywhere:
```
SELECT order_id, orders.customer_id, first_name, last_name
FROM orders O
JOIN customers C
     ON O.customer_id = C.customer_id
```

### OUTER JOIN
- There are two types of OUTER JOIN: `LEFT JOIN` and `RIGHT JOIN`.
```
SELECT 
     c.customer_id,
     c.first_name,
     o.order_id
FROM customers c
JOIN orders o
     ON c.customer_id = o.customer_id
ORDER BY c.customer_id
```
In thr above INNER JOIN example, we only see customers with an order but there might be customers that don't have any orders. 
If we want customers whether they have an order or not.

### Self join
```
-- match employees with their managers:

SELECT e.emloyee_id,
       e.first_name,
       m.first_name AS manager
FROM employees e
JOIN employees m
     ON e.reports_to = m.employee_id
```
- When self join with the same table, we need to give them different alias

### Join more than two tables
```
SELECT *
FROM orders O
JOIN customers C
     ON O.customer_id = C.customer_id
JOIN order_statuses os
     ON O.status = os.order_status_id
```

### Compound join conditions
There can be multiple primary columns (`composite primary key` contains more than on column).
And the `combination of the keys uniquely represents a record`.
```
SELECT *
FROM order_items oi
JOIN order_item_notes oin
     ON oi.order_id = oin.order_id
     AND oi.product_id = oin.product_id
```

### The implicit join syntax (not suggested to use)
```
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id
```

is the same as:

```
SELECT *
FROM orders o
JOIN customers c
     ON o.customer_id = c.customer_id
```
- not suggested to use because if without the where clause the two tables will be [cross-joined](https://www.w3resource.com/sql/joins/cross-join.php).
- 
# Sources
- [Programming with Mosh](https://www.youtube.com/watch?v=7S_tz1z_5bA)
