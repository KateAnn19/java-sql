# Java SQL

A student that completes this project shows that they can:

* Query data from a single table
* Query data from multiple tables
* Create a new database using PostgreSQL

## Introduction

Working with SQL

## Instructions

Reimport the Northwind database into PostgreSQL using pgAdmin. This is the same data we used during the guided project.

* [ ] ***pgAdmin data refresh***

* Select the northwind database created during the guided project.

* Tools -> Query Tool
  * Open file northwind.sql (you used this script during the guided project)
  * Execute

* Look under
  * northwind -> Schemas -> public -> tables

* Clear query windows

### Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below in the provided SQL code block. You will be submitting that through the regular fork, change, pull process

* [ ] ***find all customers that live in London. Returns 6 records***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses

  SELECT *
  FROM customers 
  Where city = 'London'

  </details>

```SQL

```

* [ ] ***find all customers with postal code 1010. Returns 3 customers***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses

  SELECT *
  FROM customers 
  Where postal_code = '1010'

  </details>

```SQL

```

* [ ] ***find the phone number for the supplier with the id 11. Should be (010) 9984510***

  <details><summary>hint</summary>

SELECT *
FROM suppliers 
Where supplier_id = '11'

  * This can be done with SELECT and WHERE clauses
  </details>

```SQL

```

* [ ] ***list orders descending by the order date. The order with date 1998-05-06 should be at the top***


SELECT *
FROM orders  
ORDER BY order_date DESC  

  <details><summary>hint</summary>

  * This can be done with SELECT, WHERE, and ORDER BY clauses
  </details>

```SQL

```

* [ ] ***find all suppliers who have names longer than 20 characters. Returns 11 records***

SELECT * 
 FROM suppliers 
 WHERE length(company_name) > 20 

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses
  * You can use `length(company_name)` to get the length of the name
  </details>

```SQL

```

* [ ] ***find all customers that include the word 'MARKET' in the contact title. Should return 19 records***

SELECT *
FROM customers
WHERE upper(contact_title) LIKE '%MARKET%'

  <details><summary>hint</summary>

  * This can be done with SELECT and a WHERE clause using the LIKE keyword
  * Don't forget the wildcard '%' symbols at the beginning and end of your substring to denote it can appear anywhere in the string in question
  * Remember to convert your contact title to all upper case for case insensitive comparing so upper(contact_title)
  </details>

```SQL

```

* [ ] ***add a customer record for***
* customer id is 'SHIRE'
* company name is 'The Shire'
* contact name is 'Bilbo Baggins'
* the address is '1 Hobbit-Hole'
* the city is 'Bag End'
* the postal code is '111'
* the country is 'Middle Earth'
  <details><summary>hint</summary>


  INSERT INTO customers(customer_id, company_name, contact_name, address, city, postal_code, country)
VALUES('SHIRE', 'The Shire', 'Bilbo Baggins', '1 Hobbit-Hole', 'Bag End', '111', 'Middle Earth')


-- INSERT INTO customers(customer_id, company_name, contact_name)
-- VALUES(9191, 'Lambda School', 'John Mitchell')

-- INSERT INTO customers(customer_id, company_name, contact_name)
-- SELECT 9192, company_name, contact_name
-- FROM suppliers
-- WHERE company_name LIKE 'Big%'

  * This can be done with the INSERT INTO clause
  </details>

```SQL

```

* [ ] ***update _Bilbo Baggins_ record so that the postal code changes to _"11122"_***

UPDATE customers
SET postal_code = '11122'
Where customer_id = 'SHIRE'

  <details><summary>hint</summary>

  * This can be done with UPDATE and WHERE clauses
  </details>

```SQL

```

* [ ] ***list orders grouped and ordered by customer company name showing the number of orders per customer company name. _Rattlesnake Canyon Grocery_ should have 18 orders***

SELECT company_name,
       (SELECT COUNT(o.order_id)
        FROM orders o
        WHERE o.customer_id = c.customer_id) as ordercount  
FROM customers c  
ORDER BY ordercount  

  <details><summary>hint</summary>

  * This can be done with SELECT, COUNT, JOIN and GROUP BY clauses. Your count should focus on a field in the Orders table, not the Customer table
  * There is more information about the COUNT clause on [W3 Schools](https://www.w3schools.com/sql/sql_count_avg_sum.asp)
  </details>

SELECT c.customer_id as idfromcustomer, o.customer_id as idfromorder, o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
ON o.customer_id = c.customer_id 
WHERE o.order_date is NULL


SELECT c.customer_id as idfromcustomer, o.customer_id as idfromorder, o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
ON o.customer_id = c.customer_id 
WHERE o.order_date is null

SELECT c.customer_id as idfromcustomer, o.customer_id as idfromorder, o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
on o.customer_id = c.customer_id 

SELECT order_data, company_name, contact_name
FROM orders JOIN customers 

```SQL

```

* [ ] ***list customers by contact name and the number of orders per contact name. Sort the list by the number of orders in descending order. _Jose Pavarotti_ should be at the top with 31 orders followed by _Roland Mendal_ with 30 orders. Last should be _Francisco Chang_ with 1 order***

SELECT contact_name,
       (SELECT COUNT(o.order_id)
        FROM orders o
        WHERE o.customer_id = c.customer_id) as ordercount  
FROM customers c  
ORDER BY ordercount DESC  


  <details><summary>hint</summary>

  * This can be done by adding an ORDER BY clause to the previous answer and changing the group by field
  </details>

```SQL

```

* [ ] ***list orders grouped by customer's city showing the number of orders per city. Returns 69 Records with _Aachen_ showing 6 orders and _Albuquerque_ showing 18 orders***

SELECT city,
 (SELECT COUNT(o.order_id) //makes another table and grabs from that or filters cities
         FROM orders o
         WHERE o.ship_city = c.city) as ordercount  
FROM customers c  
GROUP BY c.city
ORDER BY ordercount DESC 

  <details><summary>hint</summary>

  * This is very similar to the previous two queries, however, it focuses on the City rather than the Customer Names
  </details>

```SQL

```

## Data Normalization

Note: This step does not use PostgreSQL!

* [ ] ***Take the following data and normalize it into a 3NF database***

| Person Name | Pet Name | Pet Type | Pet Name 2 | Pet Type 2 | Pet Name 3 | Pet Type 3 | Fenced Yard | City Dweller |
|-------------|----------|----------|------------|------------|------------|------------|-------------|--------------|
| Jane        | Ellie    | Dog      | Tiger      | Cat        | Toby       | Turtle     | No          | Yes          |
| Bob         | Joe      | Horse    |            |            |            |            | No          | No           |
| Sam         | Ginger   | Dog      | Miss Kitty | Cat        | Bubble     | Fish       | Yes         | No           |

Below are some empty tables to be used to normalize the database

* Not all of the cells will contain data in the final solution
* Feel free to edit these tables as necessary


Table Name: People

|  Person Id |   Name     |            |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|   1        |   Jane     |            |            |            |            |            |            |            |            |
|   2        |   Bob      |            |            |            |            |            |            |            |
|   3        |   Sam      |            |            |            |            |            |            |
|   4        |            |            |            |            |            |            |            |            |
|   5        |            |            |            |            |            |            |            |            |
|   6        |            |            |            |            |            |            |            |            |
|   7        |            |            |            |            |            |            |            |            |

Table Name: Pets

|  Pet Id    |   PetName  |   Nickname |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|   1        |   Ellie    |            |            |            |            |            |            |            |
|   2        |   Joe      |            |            |            |            |            |            |            |
|   3        |   Ginger   |            |            |            |            |            |            |            |
|   4        |            |            |            |            |            |            |            |            |
|   5        |            |            |            |            |            |            |            |            |
|   6        |            |            |            |            |            |            |            |            |
|   7        |            |            |            |            |            |            |            |            |

Table Name: Pet Types

|  Pet Type  |            |            |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|    Dog     |            |            |            |            |            |            |            |            |
|    Horse   |            |            |            |            |            |            |            |            |
|    Cat     |            |            |            |            |            |            |            |            |
|    Turtle  |            |            |            |            |            |            |            |            |
|    Fish    |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |


Table Name: Locations

|  Locations |            |            |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|Fenced Yard |            |            |            |            |            |            |            |            |
|City Dweller|            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |

---
3NF (third normal form)

No derived fields
Eliminate fields that do not depend on the key
In an employees tables, do not store the name of departments. If a department has no employees, there is no where to store the department name. Store the department name in a separate table and use foreign keys to reference it.



### Stretch Goals

* [ ] ***delete all customers that have no orders. Should delete 2 (or 3 if you haven't deleted the record added) records***

```SQL

```

* [ ] ***Create Database and Table: After creating the database, tables, columns, and constraint, generate the script necessary to recreate the database. This script is what you will submit for review***

* use pgAdmin to create a database, naming it `budget`.
* add an `accounts` table with the following _schema_:

  * `id`, numeric value with no decimal places that should autoincrement.
  * `name`, string, add whatever is necessary to make searching by name faster.
  * `budget` numeric value.

* constraints
  * the `id` should be the primary key for the table.
  * account `name` should be unique.
  * account `budget` is required.

```SQL

```

To see the script

* Right Click on the database name
  * Select Backup...
    * Set a filename
      * To put the file backup.sql in your home directory, you could use `backup.sql`
    * Set format to `Plain`
* The script you want is now in the text file named above.
  * Copy the script from the text file into the SQL code block above!

![Database Script](assets/jx-12-m3-script.gif)


//GUIDED PROJECT 

//QUERIES

//------------------------------------------------
SELECT *
FROM customers
//------------------------------------------------

//------------------------------------------------
SELECT company_name, contact_name, contact_title
FROM customers
//------------------------------------------------

//------------------------------------------------
SELECT company_name, contact_name, country
FROM customers
WHERE country = 'Sweden'
//------------------------------------------------

//------------------------------------------------

SELECT product_name, units_in_stock
FROM products
WHERE units_in_stock < 10

//------------------------------------------------

//------------------------------------------------
SELECT company_name, contact_name, city, country
FROM customers
WHERE UPPER(country) in ('USA', 'JAPAN', 'GERMANY')
ORDER BY country DESC, city
//------------------------------------------------

//------------------------------------------------
SELECT product_id, product_name, unit_price
FROM products
ORDER BY unit_price
LIMIT 5
//------------------------------------------------

//------------------------------------------------
SELECT count(*)
FROM products    

-- this gives us how many products we have -- 
//------------------------------------------------

//------------------------------------------------
SELECT COUNT(DISTINCT unit_price)
FROM products
//------------------------------------------------

//------------------------------------------------

SELECT MIN(unit_price)
FROM products

--find minimum price--

//------------------------------------------------

//------------------------------------------------

SELECT MAX(unit_price)
FROM products
//------------------------------------------------

//------------------------------------------------

//SUB QUERY
SELECT *
FROM products 
WHERE unit_price = 
	(SELECT MAX(unit_price)
	FROM products)

//------------------------------------------------

//--------------------------------------------
SELECT DISTINCT quantity_per_unit
FROM products 

SELECT DISTINCT quantity_per_unit, COUNT(*)
FROM products 
GROUP BY quantity_per_unit

//---------------------------------------------

//---------------------------------------------
SELECT DISTINCT quantity_per_unit, COUNT(*)
FROM products 
GROUP BY quantity_per_unit
ORDER BY 2 DESC
//---------------------------------------------

//---------------------------------------------
SELECT DISTINCT quantity_per_unit, COUNT(*)
FROM products 
GROUP BY quantity_per_unit
HAVING COUNT(*) > 1
ORDER BY 2 DESC
//---------------------------------------------



//---------------------------------------------
SELECT o.order_date, c.company_name, c.contact_name
FROM orders o JOIN customers c 
on o.customer_id = c.customer_id 

-- HOW DO I WANT TO MATCH up the rows 
//---------------------------------------------



//---------------------------------------------
SELECT o.customer_id, c.customer_id
FROM orders o JOIN customers c 
on o.customer_id = c.customer_id
//--------------------------------------------- 

//--------------------------------------------- 
SELECT o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
on o.customer_id = c.customer_id 

--RIGHT 

//--------------------------------------------- 

//--------------------------------------------- 
SELECT c.customer_id as idfromcustomer, o.customer_id as idfromorder, o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
ON o.customer_id = c.customer_id 
//--------------------------------------------- 





//--------------------------------------------- 
SELECT c.customer_id as idfromcustomer, o.customer_id as idfromorder, o.order_date, c.company_name, c.contact_name
FROM orders o RIGHT JOIN customers c 
ON o.customer_id = c.customer_id 
WHERE o.order_date is NULL
//--------------------------------------------- 




//--------------------------------------------- 
INSERT INTO customers(customer_id, company_name, contact_name)
VALUES(9191, 'Lambda School', 'John Mitchell')
//--------------------------------------------- 



//--------------------------------------------- 
SELECT * FROM CUSTOMERS
//--------------------------------------------- 



//--------------------------------------------- 
SELECT 9192, company_name, contact_name
FROM suppliers
WHERE company_name LIKE 'Big%'
//--------------------------------------------- 



//--------------------------------------------- 
INSERT INTO customers(customer_id, company_name, contact_name)
SELECT 9192, company_name, contact_name
FROM suppliers
WHERE company_name LIKE 'Big%'
//--------------------------------------------- 



//UPDATE

//--------------------------------------------- 
UPDATE products 
SET discontinued = 1 
WHERE unit_price < 10.00
//--------------------------------------------- 

//--------------------------------------------- 
UPDATE suppliers 
SET city = 'Oslo', phone = '5555555555', fax = '1234567890'
WHERE supplier_id = 15


---MUST HAVE WHERE CLAUSE !!!!
--MUST HAVE WHERE CLAUSE !!!!  so ONLY the one you 
want is updated and not the entire table

//--------------------------------------------- 

//DELETE

//--------------------------------------------- 
DELETE 
FROM products 
WHERE unit_price < 10.00



GOT THIS:
ERROR:  update or delete on table "products" violates foreign key constraint "fk_order_details_products" on table "order_details"
DETAIL:  Key (product_id)=(13) is still referenced from table "order_details".
SQL state: 23503

THIS MEANS ANOTHER TABLE RELIES ON THIS DATA AND
MUST DELETE FROM OTHER TABLE FIRST 

//--------------------------------------------- 


//--------------------------------------------- 
DELETE 
FROM customers
WHERE customer_id = '9191
//--------------------------------------------- 