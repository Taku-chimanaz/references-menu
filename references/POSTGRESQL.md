# POSTGRESQL REFERENCE

This serves as a reference document for the postgresql commands.

# PREREQUISITES

For you to be able to follow along you need to have the following installed on your system:

1. POSTGRESQL

For installation of POSTGRESQL correctly please watch the video below.It will take you through installing the POSTGRESQL on your system and adding it to your environmental variables.

[![Video Title](https://img.youtube.com/vi/GpqJzWCcQXY&t=492s/0.jpg)](https://www.youtube.com/watch?v=GpqJzWCcQXY&t=492s)

# Commands

## CREATE A DATABASE

```postgresql
CREATE DATABASE [database_name]

-- Example

CREATE DATABASE demo_db
```

## DROP A DATABASE

```postgresql
DROP DATABASE [database_name]

-- Example

DROP DATABASE demo_db
```

**_NB: This command is dangerous to use especially in production.It results in permanent deletion of data and if there is no backup of said data,it will be lost forever._**

## CREATE A TABLE IN A DATABASE

```postgresql
CREATE TABLE [table_name](
    Column name + data type + constraints
)

-- Example

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

## DROP A TABLE IN A DATABASE

```postgresql
DROP TABLE [table_name]

-- Example

DROP TABLE product
```

**_NB: This command is dangerous to use especially in production.It results in permanent deletion of data and if there is no backup of said data,it will be lost forever._**

## INSERTION OF A RECORD IN A DATABASE

```postgresql
INSERT INTO TABLE [database_name](column 1, column 2)
VALUES(value1, value2);

-- Example

INSERT INTO product(product_name, product_desc, product_price)
VALUES('Carrots', 'Carrots Desc', 10.00);
```

#### Explanation

- We are inserting a row in our product table.
- We specified the columns that we are populating with data in that table e.g product_name, product_desc, product_price.
- You noticed that we did not add id to the row and this is because it is auto incremented by the database.
