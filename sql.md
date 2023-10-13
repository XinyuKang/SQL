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

## IFNULL
The IFNULL function in SQL is a function that allows you to return an alternative value if the first expression evaluates to NULL.
```
IFNULL(grouped.attended_exams, 0)
```
This will check the value of grouped.attended_exams:

If grouped.attended_exams is NULL, then 0 will be returned.

If grouped.attended_exams is not NULL, then the value of grouped.attended_exams will be returned.

## GROUP BY
Group `rows` that have the same values into summary rows.
`GROUP BY` is often used with aggregate functions:
- `COUNT()`: COUNT ([ALL | DISTINCT] expression); By default, SQL Server Count Function uses All keyword. It means that SQL Server counts all records in a table. It also includes the rows having duplicate values as well.
- 'MAX()'
- 'MIN()'
- 'SUM()'
- 'AVG()'

```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```
### COUNT
Expressions that are not encapsulated within the COUNT function and must be included in the GROUP BY clause at the end of the SQL statement.

1. COUNT(*):
This counts all rows in the group, including those with NULL values in any columns.
Essentially, it counts the number of rows in each group.
2. COUNT(column_name):
This counts all non-NULL values in the specified column within the group.
If the specified column has a NULL value in any row within the group, that row will not be counted.
So, the distinction between COUNT(*) and COUNT(column_name) primarily lies in how NULL values are treated:

- COUNT(*) counts rows, irrespective of whether any of the columns have NULL values.
- COUNT(column_name) counts only non-NULL values in the specified column.

## HAVING
Like WHERE, the HAVING clause `filters the rows of a table`. Whereas WHERE tried to `filter the whole table`, HAVING filters `rows within each of the groups defined by GROUP BY`.
```
SELECT 
    product_line,
    AVG(unit_price) AS avg_price,
    SUM(quantity) AS tot_pieces,
    SUM(total) AS total_gain
FROM sales
GROUP BY product_line
HAVING SUM(total) > 40000
ORDER BY total_gain DESC
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
In the above INNER JOIN example, we only see customers with an order (i.e. for a given customer, if he has an order, return that record) but there might be customers that don't have any orders. 
If we want customers whether they have an order or not.

- There are two types of OUTER JOIN: `LEFT JOIN` and `RIGHT JOIN`.

#### LEFT JOIN
**All the customers from the left table are returned** whether the `ON` condition is true or not.
```
SELECT 
     c.customer_id,
     c.first_name,
     o.order_id,
     sh.name AS shipper
FROM customers c
LEFT JOIN orders o
     ON c.customer_id = o.customer_id
LEFT JOIN shipper sh
     ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id
```
- When performing a LEFT JOIN, if there are multiple rows in the right table that match the join condition with a row from the left table, then the row from the left table will be repeated for each matching row from the right table in the result set.

#### RIGHT JOIN
**All the customers from the right table are returned** whether the `ON` condition is true or not.
```
SELECT 
     c.customer_id,
     c.first_name,
     o.order_id
FROM customers c
RIGHT JOIN orders o
     ON c.customer_id = o.customer_id
ORDER BY c.customer_id
```
- avoid RIGHT JOINs, use as many LEFT JOINs as possible.

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

### JOIN ... USING ...
When joining two tables, if the condition after `ON` have the same column names for two tables, we can use `USING` instead of `ON` to simplify.
```
SELECT *
FROM orders O
JOIN customers C
     -- ON O.customer_id = C.customer_id
     USING (customer_id)
JOIN shippers sh
     USING (shipper_id)t
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
OR 

```
SELECT *
FROM order_items oi
JOIN order_item_notes oin
     USING (order_id, product_id)
```

### Natural join (not suggested to use)
We don't explicity specify the join's column name, the database engine will join the tables on the common columns. May have unexpected results.
```
SELECT
     o.order_id,
     c.first_name
FROM orders o
NATURAL JOIN customers c
```

### Cross join
Join every record from the first table and every record from the second table
```
SELECT *
     c.first_name AS customer,
     p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name
```
OR
```
SELECT *
     c.first_name AS customer,
     p.name AS product
FROM customers c, products p
ORDER BY c.first_name
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

## UNION
Combine records for multiple queries (can between different tables or for the same table).
```
SELECT
     order_id,
     order_date,
     `Active` AS status
FROM orders
WHERE order_date >= `2019-01-01`
UNION
SELECT
     order_id,
     order_date,
     `Archived` AS status
FROM orders
WHERE order_date < `2019-01-01`;
```

```
SELECT first_name
FROM customers
UNION
SELECT name
FROM shippers
```

## DATEDIFF
- MySQL uses DATEDIFF to compare two date type values.
- DATEDIFF(a, b) means a is a day lafter than b
```
SELECT w2.id
FROM Weather w1
LEFT JOIN Weather w2
ON DATEDIFF(w2.recordDate, w1.recordDate) = 1
WHERE w1.temperature < w2.temperature
```

- Note that UNION will append queries vertically (i.e. not like join, which appends horizontally) even if the unioned column might have different names

## Other tricks
### Three ways to represent dates
1. DATEDIFF(A, B)
2. A >= B
3. A BETWEEN B AND C

# Sources
- [Programming with Mosh](https://www.youtube.com/watch?v=7S_tz1z_5bA)
