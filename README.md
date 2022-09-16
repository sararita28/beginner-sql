# Databases (SQL)

- A database is a collection of data stored in a format that can easily be accessed. In order to manage our databases we use a software called DBMS (DataBase Management System). We connect to a DBMS and give it instructions (such as querying or modifying our data). The DBMS will execute our instructions and send results back.
- There are 2 types of databases; relational (SQL) and non-relational (NoSQL).
- Relational databases use tables that are related to each other (using a relationship).
- Every row in a table represents a record.
- The first step to write a query to get data from a DB is to select a DB.
- SQL is not a case-sensitive language but as a best practice we should capitalize the keywords and use lowercase characters for everything else.
- When you have multiple SQL statements, you need to terminate each statement using a semicolon
- To comment a single line in SQL you use 2 hyphens -- in front of the line.

### DATA TYPES (3 categories)

<em>It's important to properly define the fields in a table</em>

- Numeric: INT, INT(), FLOAT(), DOUBLE(), DECIMAL()
- Date & Time: DATE, DATETIME
- String: CHAR, CHAR(), VARCHAR(), VARCHAR2, BLOB, TEXT

### Constraints

Constraints are used to specify rules for the data in a table (specified in the CREATE TABLE statement or in the ALTER TABLE statement). (if there's any violation between the constraint and the data action the action is aborted by the constraint).

- `NOT NULL` - Ensures that a column can't have a null value
- `UNIQUE` - Ensures that all values in a column are different
- `PRIMARY KEY` - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- `FOREIGN KEY` - Prevents actions that would destroy links between tables
- `CHECK` - Ensures that the values in a column satisfies a specific condition
- `DEFAULT` - Sets a default value for a column if no value is specified
- `CREATE INDEX` - Used to create and retrieve data from the database very quickly

### SELECT statement

- Basic syntax: `SELECT <column_names_comma_separated> FROM <table_toQuery>`
- The above statement has 2 clauses; SELECT and FROM.
- Other clauses to filter and sort data include: WHERE (to filter). Example: `SELECT * FROM customers WHERE customer_id = 1` or ORDER BY (to sort). Example: `SELECT * FROM customers ORDER BY first_name`
- The order of the clauses matters. SELECT > FROM > JOIN > WHERE > ORDER BY > LIMIT
- You can put everything in 1 line but it is better to put every clause on a single line.

### SELECT clause

- With the SELECT clause you can specify the columns that you want. Note that the order in which you specify the columns matter as it will show you the results in the same order.
- You can also use arithmetic expressions (+, -, /, \*, %). If you need to change the order of operations you can add parentheses.
- If a line gets too long you can break up a clause by placing each column on a new line.
- You can give columns aliases using the `AS` keyword. You separate words with an underscore but you can also add spaces by using quotes.
- You use the `DISTINCT` keyword to only display unique data from your query

### WHERE clause

- Comparison operators: >, >=, <, <=, =, !=, <> (not equal)
- Whenever you're dealing with strings or textual data, you need to enclose your values with quotes.
- To combine multiple search conditions when filtering data you use the `AND` and `OR` and `NOT` keywords. If you need to use many of these operators you must be cautious of the order. The `AND` operator is ALWAYS evaluated first. To change the priority you need to use parentheses.

### IN operator

- In SQL we cannot combine a string with a boolean expression that produces a boolean value.
- A better way than combining multiple OR operators is to use the `IN` keyword
  For example, this: WHERE state = 'VA' OR state = 'GA' or state = 'FL'
  becomes this: WHERE state IN ('VA', 'GA', 'FL')
- The opposite of IN is `NOT IN`.

### BETWEEN keyword

- When you need to query data between 2 ranges a better way than using the AND operator is to use the `BETWEEN` operator.
  For example, this: WHERE points >= 1000 AND points <=3000
  becomes this: WHERE points BETWEEN 1000 AND 3000

### LIKE keyword (older)

- The `LIKE` operator is used to retrieve rows that match a specific string pattern
- The `%` sign is to indicate any number of characters. It is also not case sensitive
- An underscore `_` matches a single character
  For example:
  WHERE last_name LIKE 'b%' will return all the results where the last name start with b
  WHERE last_name LIKE '%b%' will return all the results where the last name has a b
  WHERE last_name LIKE '\_b' will return all the results where the last name has exactly 2 characters and the last one is a b
  WHERE last_name LIKE 'y\_\_\_\_b' will return all the results where the last name has exactly 6 characters and the first one is a y and the last one is a b
- The opposite of `LIKE` is `NOT LIKE`

### REGEXP operator (newer)

- LIKE '%b%' becomes `REGEXP 'b'`
- `^` indicates the beginning of a string so '^b' means it must start with b
- `$` indicates the end of a string
- `|` indicates OR so: WHERE last_name REGEXP 'b|a' will return last names that have either a b or a
- `[]` square brackets are used to indicate that before or after a letter there must be any of the given characters. For example '[gim]e' will match : ge, ie, me. For ranges you use a `-` for example a-h means from a to h

### IS NULL

- `IS NULL` operator will return all the null values
- The opposite is `IS NOT NULL`

### ORDER BY

- In SQL, every table should have a primary key column and the values in that column should uniquely identify the records in that table.
- When using the ORDER BY keyword you are automatically sorting in ascending order. To reverse that order you use DESC. For example: `ORDER BY first_name DESC`
- You can also sort data by multiple columns (comma separated)
- If you use numbers in the ORDER BY clause then those are the orders of the columns in the SELECT clause (but this is to be avoided)

### LIMIT clause

- returns the first x records
- the `LIMIT` clause should always come at the end

### JOIN keyword (Inner Joins)

- To combine columns from one table with the columns of another we use the JOIN keyword. You can optionally type INNER JOIN
- In SQL, you have 2 types of joins; inner and outer.

```
SELECT *
FROM orders
JOIN customers
    ON orders.customer_id = customers.customer_id
```

In the above snippet, we are selecting everything from the orders table. We are combining the columns of the orders table with the columns of the customers table (using the JOIN keyword). We use the ON keyword to specify on which basis we want to join the tables (i.e how do you want to display the results/line up the records). Finally, do avoid repeating certain words over and over we can shorten them by using aliases right after each table name. So the above snippet becomes:

```
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id
```

- Note that if you use an alias you need to apply it everywhere else.

### Self joins

- Joining a table with itself is similar to joining a table with another table. The only difference is that we have to use different aliases and we have to prefix each column with an alias (SELECT \* would repeat everything twice)

### Joining multiple tables (more than 2)

- To join multiple tables you need to use multiple JOIN statements

### Compound join conditions

- Compound join conditions is when you have multiple conditions to join tables
- Sometimes we cannot use a single column to uniquely identify the rows in a given table (if the values are duplicated for example). In this instance, we should use the combination of the values in 2 columns to uniquely identify each element.
- A composite primary key contains more than 1 column.
- Example of compound join conditions: ON x.abc = y.abc AND x.def = y.def

### Implicit join syntax (not recommended)

- It is best to avoid using implicit join syntaxes because you might get a cross join

### Outer joins

- Unless we specify the keyword OUTER then whenever we are using joins we are using INNER joins.
- With inner joins you are only returning records that match the condition in ON... so if you have records that happen to have a null value for that particular field then you will not see them in the results.
- OUTER JOIN shows these (null) results
- In SQL there are 2 types of OUTER joins; left and right.
- When you use a LEFT join, all the records from the left table are returned whether the condition is true or not.
- When you use a RIGHT join, all the records from the right table are returned whether the condition is true or not.
- Syntax is the same as inner joins except you need to specify LEFT or RIGHT before the JOIN keyword. You can also optionally add the OUTER keyword. Ex: LEFT OUTER JOIN
- As best practice, avoid right joins and use left joins instead.

### USING clause

- The USING clause simplifies the JOIN conditions. If the the column name is exactly the same across tables we can replace the ON clause with a USING clause. So `ON x.abc = y.abc` becomes `USING(abc)`
- With compound join conditions, you simply separate the conditions with a comma. for example this: `ON x.abc = y.abc AND x.def = y.def` becomes `USING (abc, def)`

### Natural joins (not recommended)

- It is best to avoid natural joins as they can produce unexpected results

### Cross joins

- We use cross joins to combine every record from the first table with every record from the second table
  Implicit syntax:
  FROM table1, table2
  Explicit syntax:
  FROM table 1
  CROSS JOIN table2

### Unions

- Joins allow us to combine columns from multiple tables. Unions allows us to combine rows from multiple tables.
- You do that by using the keyword `UNION` between the different queries.
- Keep in mind that the number of columns that each query returns should be equal otherwise you'll get an error
- Whatever you have as a first query is used to determine the name of the columns

### Column Attributes

- VARCHAR(x) VS CHAR(x) : Say x is an integer value (ex: 50). If you have a variable that is 5 characters long, both varchar(50) and char(50) will let you store your string, since 50(which is the max char length) is greater than your chars length. However, using varchar will allow you not to waste the extra space, while char will accommodate and insert those extra 45 chars to fill the column even though you're not using them.
- PK: Primary Key
- NN: Not Null (determines if column can accept null values or not)
- AI: Auto Increment

### Inserting a single row

- To add values in a single row, you start by specifying the table in which you wish to insert your data. Then you write VALUES followed by the values which you wish to insert (the order matters so look at the column attributes table as you're manually inputting them) between parentheses. For example:

```
INSERT INTO customers
VALUES (
    DEFAULT,
    'John',
    'Smith',
    NULL,
    NULL,
    'ADDRESS',
    'CITY',
    'STATE',
    DEFAULT)
```

- An alternative would be to supply a list of columns that you want to insert values into after the table name. Example:

```
INSERT INTO customers (
    first_name,
    last_name,
    address,
    city,
    state)
VALUES (
    'John',
    'Smith',
    'ADDRESS',
    'CITY',
    'STATE',
)
```

- The latter approach allows for more flexibility as you don't have to list the values in the order of the column attributes table. Rather, you need to list them in the order you have in the INSERT INTO parentheses.

### Inserting multiple rows

- To insert multiple rows at once you need to separate the values (parentheses) with commas

### Inserting hierarchical rows (Insert into multiple tables)

- 1 row in 1 table can have 1 or more children inside another table.
- If that is the case, then when you insert data into a table, you'll need to wait for your DB to generate an id for that new insert in order to also insert into the other table. To do that, MySQL comes with a bunch of built in functions. One of which is LAST_INSERT_ID(), it returns the id that MySQL generates when we insert a new row. Once you have that id, you can use it to insert the child records.
  For example:

```
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, NULL, 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 3.99)
```

### Creating a table

USE classroom;

CREATE TABLE students (
StudentID int,
FirstName varchar(255),
LastName varchar(255),
CourseID varchar(255)
);

### Creating a copy of a table

- First you need to create a new table with the CREATE TABLE ... AS statement
- Then you need to create a SELECT \* FROM ... statement to get everything from the table you want to copy
  Example:

```
CREATE TABLE copy_of_orders AS
SELECT * FROM orders
```

The above snippet will copy everything from orders into this new copy_of_orders table, however, it will ignore the column attributes

- To copy a subset of records of a table into a new table you can use a modifier SELECT statement as a subquery into the CREATE TABLE .. AS statement.
  Example :

```
USE sql_invoicing;

CREATE TABLE invoices_archive AS
SELECT
	i.invoice_id,
	i.number,
	c.name AS client_name,
	i.invoice_total,
	i.payment_total,
	i.invoice_date,
	i.due_date,
	i.payment_date
FROM invoices i
JOIN clients c
	USING (client_id)
WHERE payment_date
```

### Updating in a single row

- first use the UPDATE statement to update 1 or more records in a table
- then use the SET clause (where you specify the new value for 1 or more columns)
- then you identify a condition (with WHERE) where you identify the record(s) that need(s) to be updated
  Example:
  ```
  UPDATE invoices
  SET payment_total = 10, payment_date = '2019-03-01'
  WHERE invoice_id = 1
  ```
- If you want to update all the records in a table you leave out the WHERE clause as it is only used for conditions
- Instead of hard coding values, if you want to update a record to a value that already exists in the table, you can use another subquery that you will put in parenthesis at the assignment of the record in the conditional statement WHERE. Example:

```
USE sql_invoicing;

UPDATE invoices
SET payment_total = invoice_total * 0.5
WHERE client_id =
			(SELECT client_id
			FROM clients
			WHERE name = 'Myworks')
```

- Note that if the subquery contains multiple records you have to replace the assignment opertator = with IN

### Updating multiple rows

- Specific to MySQL: By default, MySQL workbench works in the safe update mode so it allows you to update a single record, so you need to uncheck the box 'safe updates' in the Preferences

### Deleting rows

- If you want to delete all the records in a table you use the DELETE FROM table
- If you want to narrow down your records you use the WHERE clause

Example:

```
USE sql_invoices;

DELETE FROM invoices
WHERE cliend_id = (
    SELECT *
    FROM clients
    WHERE name = 'Myworks'
)
```

### Sub Languages in MySQL

- DDL (Data Definition Language): statements are used to define the database structure or schema. <b>CREATE, ALTER, DROP, TRUNCATE</b>
- DML (Data Manipulation Language): statements are used for managing data within schema objects (deals with data manipulation). <b>SELECT, INSERT, UPDATE, DELETE</b> 
- DCL (Data Control Language): statements control the level of access users have on database. <b>GRANT(allows users to read/write on certain objects), REVOKE(keeps users from read/write)</b>
- TCL/DTL (Data Transaction Language): statements allow to control and manage transactions to maintain the integrity of data. <b>BEGIN TRAN(opens a transaction), COMMIT TRAN(commits a transaction), ROLLBACK(rollback a transaction in case of any error)</b>

#### DDL Queries

- DDL is a vocabulary used to define data structures. 
- CREATE is used to define new entities
- ALTER is used to modify the definition of exising entities
- DROP is used to remove existing entities
- TRUNCATE TABLE removes all rows from a table or specified partitions of a table.

#### DML Queries

- DML is used to work with the data IN tables. They affect records in a table.
- INSERT is used to insert new records
- UPDATE is used to update/modify existing records
- DELETE is used to delete existing records

