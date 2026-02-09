# POSTGRESQL REFERENCE

![PostgreSQL logo](./../images/postgresql.svg)

I have created this reference document to help me reference or recall commands I have learnt and avoid many google searches for the command I want.Please note that this list only reflect on commands that I have learnt and does not exhaust all the commands.It can also serve as a starting point for anyone who wants to learn about PostgreSQL but do not know where to begin.Hope this helps.

# Prerequisites

You need to have POSTGRESQL installed on your system.

#### Installation on windows

https://www.youtube.com/watch?v=GpqJzWCcQXY

#### Installation on linux

https://www.youtube.com/watch?v=cD32EHVWRXY

#### Installation on mac

https://www.youtube.com/watch?v=a78hxM4-99A

After successful installation you can then run these commands in the psql command line.
<br>

# Commands

### CREATE A DATABASE

#### Command Syntax

```sql
CREATE DATABASE [database_name]
```

#### Example

```sql
CREATE DATABASE demo_db
```

<br>

### DROP A DATABASE

#### Command Syntax

```sql
DROP DATABASE [database_name]
```

#### Example

```sql
DROP DATABASE demo_db
```

**_NB: This command is dangerous to use especially in production.It results in permanent deletion of data and if there is no backup of said data,it will be lost forever._**

<br>

### CREATE A TABLE IN A DATABASE

#### Command Syntax

```sql
CREATE TABLE [table_name](
    Column name + data type + constraints
)
```

#### Example

```sql
CREATE TABLE product(
    id BIGSERIAL NOT NULL PRIMARY KEY,
    product_name VARCHAR(50) NOT NULL,
    product_desc VARCHAR(100) NOT NULL,
    product_price NUMERIC NOT NULL
)
```

#### Explanation

- We are creating a table called product.
- The table will hold data with columns: id, product_name, product_desc, product_price
- We also attached data types to the respective columns: id, product_name, product_desc, product_price
- We also attached constraints to the columns: NOT NULL, PRIMARY KEY
- Constraint is just a rule enforces to a column or table.This improves data reliability, accuracy and integrity.
- In the above example we are saying: id cannot be null and it is also a primary key.

<br>

### DROP A TABLE IN A DATABASE

#### Command Syntax

```sql
DROP DATABASE [table_name]
```

#### Example

```sql
DROP DATABASE product
```

**_NB: This command is dangerous to use especially in production.It results in permanent deletion of data and if there is no backup of said data,it will be lost forever._**

### INSERTION OF A RECORD IN A DATABASE

#### Command Syntax

```sql
INSERT INTO TABLE [database_name](column 1, column 2)
VALUES(value1, value2);
```

#### Example

```sql
INSERT INTO product(product_name, product_desc, product_price)
VALUES('Carrots', 'Carrots Desc', 10.00);
```

#### Explanation

- We are inserting a row in our product table.
- We specified the columns that we are populating with data in that table e.g product_name, product_desc, product_price.
- You noticed that we did not add id to the row and this is because it is auto incremented by the database.
