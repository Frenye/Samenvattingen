# Databanken 2

[TOC]

## Hoofdstuk 1: Basic concepts revisited

###Introduction

Basic form of a SELECT statement

```mssql
SELECT [ALL | DISTINCT] {*|expression[, expression...]} 
FROM tablename
[WHERE conditions(s)]
[GROUP BY column name [, column name ...]
[HAVING conditions(s)]
[ORDER BY {column name |seqnr}{ASC|DESC}[,...]
```



CASE statement

```mssql
SELECT 
	CASE region
		WHEN 'W' THEN 'WEST'
		WHEN 'N' THEN 'NORHT'
	END
FROM supplier

SELECT
	CASE
		WHEN PRICE IS NULL THEN 'NO PRICE'
		WHEN PRICE < 10 THEN 'CHEAP'
		WHEN PRICE > 10 THEN 'EXPENSIVE'
	END AS 'PRICING'
FROM product
```



### Joins

JOIN (twee tables joinen)

Inner join (twee tables joinen based on common criteria)

```mssql
SELECT TEAMNO, NAME
FROM TEAMS JOIN PLAYERS 
ON TEAMS.PLAYERNO = PLAYERS.PLAYERNO
```

```sql
SELECT e1.employeeID,e1.lastname & ' ' & e1.firstname AS werknemer, e2.lastname & ' ' & e2.firstname AS baas 
FROM employee e1 
INNER JOIN employee e2 ON e1.reportsto = e2.employeeid	
```



Outer join (Returns all records from 1 table, even if there is no corresponding record in the other table)

Left outer join (Returns all rows of the first table in the FROM clause)

Right outer join (Returns all rows of the second table in the FROM clause)

Full outer join (Returns all rows of the first and second table in the FROM clause)

```mssql
-- give for all players their number and name and the list of their penalties
SELECT p.playerno, name, amount 
FROM players p LEFT OUTER JOIN penalties pe 
ON p.playerno = pe.playerno
ORDER BY p.playerno
```




Cross join (Generate all combinations)

```mssql
SELECT p1.name + ' ' + p1.initials PLAYER1,'-'AGAINST, p2.name + ' ' + p2.initials PLAYER2 
FROM players p1 CROSS JOIN players p2
WHERE p1.playerno < p2.playerno 
ORDER BY p1.playerno
```



### Unions

Union

```SQL
SELECT ... FROM ... WHERE ...
UNION
SELECT ... FROM ... WHERE ...
ORDER BY ...
```



Intersect

```SQL
SELECT ... FROM ... WHERE ...
INTERSECT
SELECT ... FROM ... WHERE ...
```



Except

```sql
SELECT ... FROM ... WHERE ...
INTERSECT
SELECT ... FROM ... WHERE ...	
```



## Hoofdstuk 2: Subqueries, Views, CTE, CRUD

### Subqueries

```sql
-- Who has the heighest salary?
SELECT lastname, firstname, salary
FROM employee WHERE salary = (SELECT MAX(salary) FROM employee)
```


```sql
-- Who is the youngest employee of Canada?
SELECT lastname, firstname
FROM employee
WHERE country = 'Canada' AND birthdate =
	(SELECT MAX(birthdate) FROM employee WHERE country = 'Canada')
```


```sql
-- give all the players that played matches
SELECT playerno 
FROM players 
WHERE playerno IN 
	(SELECT playerno FROM matches)
```

Other options are =, <=, >=, >= ALL, > ANY



Correlated subqueries (inner query depends on info from the outer query)

```sql
-- Give the employees whose salary is larger than the average of the salary of the employees who report to the same boss
SELECT lastname, firstname, salary
FROM employee AS e 
WHERE salary >
	(SELECT AVG(salary) FROM employee WHERE reportsto = e.reportsto)
```



Subqueries and (NOT) EXISTS


```sql
-- Give the players that did play matches
SELECT * 
FROM players AS p
WHERE EXISTS (SELECT * FROM matches WHERE playerno = p.playerno);
```



### CRUD

INSERT


```sql
INSERT INTO product (productname, categoryid)
VALUES( 'Chocolate', 1)
```



INSERT WITH SELECT

```sql
-- insert all the employees into customers
INSERT INTO customers
SELECT lastname, firstname, title, address, city, region, postalcode, country
FROM Employees
```



UPDATE

```sql
-- increase the unit price of the products from suppliers in the USA
UPDATE products
SET unitprice = (unitprice * 1.1)
WHERE supplierid IN
	(SELECT supplierid FROM supplier WHERE country = 'USA')
```



DELETE


```sql
-- delete the orderdetails of the orders dated $date
DELETE FROM orderdetails
WHERE orderid IN 
	(SELECT orderidFROM orders WHERE orderdate = '4/14/98')
```


### Views

VIEWS

-   Saved select statement (virtual table composed of other tables and does not save the data rather it re-executes the underlying SELECT statement)
-   Hides complex database design
-   Reuse of complex queries
-   Partial solution for complex problems
-   Securing data access (not everyone has to see al the columns)
-   Organizing data for other applications
-   Cannot contain and ORDER BY
-   Column names are mandatory if they contain calculations

```sql
CREATE VIEW verkopen(maand, totaal) AS 
  SELECT format(orderdate, 'yyyy-MM'), sum(orderamount) 
  FROM orders 
  GROUP BY format(orderdate, 'yyyy-MM')
  WITH CHECK OPTION; 
  -- Error is generated if after the update the row is no longer part of the view
  -- WITHOUT CHECK OPTION After the update a row can dissapear from the view

SELECT * FROM MyView

DROP VIEW MyView

ALTER VIEW MyView
```
VIEWS **can** be updated if they do not contain

-   distinct, top, statistical functions, calculations, union, group by in the select statement



### Common table expressions

COMMON TABLE EXPRESSIONS (CTE) (WITH)

-   Simplify SQL-instructions, ex. avoid repetition of SQL constructs
-   Traverse recursively hierarchical and network structures
-   Only exists during the select statement (execution of the query)

```sql
-- Give the average number of penalties of all players
WITH fines(number) AS 
	(SELECT COUNT(pe.playerno) 
     FROM players AS pl 
     LEFT JOIN penalties AS pe 
     ON pl.PLAYERNO = pe.PLAYERNO 
     GROUP BY pl.PLAYERNO)
SELECT AVG(number *1.0)
FROM fines;
```


VIEWS AND COMMON TABLE EXPRESSIONS

| **SIMILARITIES**                         | **DIFFERENCES**                          |
| ---------------------------------------- | ---------------------------------------- |
| WITH  ~ CREATE VIEW                      | CTE only exists during the SELECT statement |
| Virtual tables: content is derived from other tables | CTE is not visible for other users and applications |



CTE AND SUBQUERIES

| SIMILARITIES                             | DIFFERENCES                              |
| ---------------------------------------- | ---------------------------------------- |
| Virtual tables: content is derived from other tables | CTE can be reused in the same query      |
|                                          | Subquery is defined in the clause where it is used |
|                                          | CTE is defined on top of the query       |



CTE to avoid repetition

```sql
--Give the payment numbers and penalty amount that are not equal to the highest and lowest penalty ever paid by player 44. Also show this highest and lowest amount in the result
-- before
SELECT paymentno, amount, 
	(SELECT MIN(amount) FROM penalties WHERE playerno = 44),
	(SELECT MAX(amount) FROM penalties WHERE playerno = 44)
FROM penalties
WHERE amount <>
	(SELECT MIN(amount) FROM penalties WHERE playerno = 44)
	AND	amount <> 
	(SELECT MAX(amount) FROM penalties WHERE playerno = 44)

-- after
WITH min_max(min_amount, max_amount) as
	(SELECT MIN(amount), MAX(amount) FROM penalties WHERE playerno = 44)
SELECT p.paymentno, p.amount, mm.min_amount, mm.max_amount 
FROM penalties p 
CROSS JOIN min_max mm 
WHERE p.amount <> mm.max_amount AND p.amount <> mm.min_amount;
```


Recursive SELECTs

```sql
-- Give the integers from 1 to 5
WITH numbers(number) AS 
	(SELECT 1 UNION ALL SELECT number + 1 FROM numbers WHERE number < 5)
SELECT * 
FROM numbers
-- OPTION (maxrecursion 1000); -- as 100 is the default max recursion count
```



## Hoofdstuk 3: Data Definition Language (DDL)

### Creation of Database


```sql
CREATE DATABASE myDatabase
ON
	PRIMARY (
    	NAME = ....,
      	FILENAME = ....,
      	SIZE = ...
    )
    LOG ON (
    	NAME = ...
      	FILENAME = ...
      	SIZE = ...
    );

ALTER DATABASE myDatabase
MODIFY FILE (NAME = 'newName')
-- ADD FILE

DROP DATABASE myDatabase;
```



### Creation of Tables


```sql
CREATE TABLE myTable (
  	userId int IDENTITY PRIMARY KEY NOT NULL
	firstName varchar(50) NOT NULL,
  	lastName varchar(50) NOT NULL,
  	age int NOT NULL
)

ALTER TABLE myTable
ALTER COLUMN firstName varchar (500) NOT NULL
-- ADD firstName
-- DROP COLUMN firstName

DROP TABLE myTable;
```



### type conversion


```sql
-- Date formatting
select id, format(date, 'mm-dd-YYYY') from items

-- Cast
select id, cast(unitPrice as int) from items

-- Convert 
select id, convert(int, unitPrice) from items

```



### Constraints

-   Identity column contains
    -   A unique value for each row
    -   System generated sequential values
-   Cannot contain null
-   Cannot be updated
-   Only one per table


```sql
CREATE TABLE myTable (
	itemID int IDENTITY(100, 5) PRIMARY KEY,
  	-- anotherValue_PK int PRIMARY KEY(itemID)
  	gender char(1) DEFAULT 'M' CHECK(gender in {'M', 'F'}) NOT NULL,
  	-- check(name NOT LIKE '%[0-9]%')
  	-- check(price BETWEEN 10 AND 500)
  	CONSTRAINT itemID_u UNIQUE(itemID),
  	CONSTRAINT item_FK FOREIGN KEY(otherTable) references otherTable(primKey)
)
```


```sql
ALTER TABLE myTable
ADD newItem int NOT NULL UNIQUE
```





## Hoofdstuk 4: DB programming

-   Transact SQL is the programming language of SQL server
-   PSM = Persistent Stored Modules with stored procedures and stored functions



### Stored procedures

```mssql
-- example
CREATE PROCEDURE usp_Customers_Delete
	@custno nchar(5) = NULL
AS
  IF @custno IS NULL BEGIN
      RAISERROR('customerID is NULL', 10, 1)
      RETURN
  END

  IF NOT EXISTS (SELECT * FROM customers WHERE customerid = @custno) BEGIN
      RAISERROR('Klant bestaat niet', 10, 1)
      RETURN
  END

  IF EXISTS (SELECT * FROM orders WHERE customerid = @custno) BEGIN
      RAISERROR('Klant heeft orders', 10, 1)
      RETURN
  END
  
DELETE FROM customers WHERE customerid = @custno

-- calling it in MS SQL Server
EXEC usp_Customers_Delete 'theCustNo' -- WITH RECOMPILE (can be usefull)

-- alter a stored procedure
ALTER  PROCEDURE usp_Customers_Delete
	@customerID nchar(5)
AS
SELECT * FROM orders
WHERE customerID = @customerID

-- drop a stored procedure
DROP PROCEDURE usp_Customers_Delete
```



RETURN PARAMETERS

```sql
CREATE PROCEDURE usp_OrdersSelectAll
AS
SELECT * FROM orders 
RETURN @@ROWCOUNT;

-- Execute the stored procedure that returns a value
DECLARE @returnCode int
EXEC @returnCode = usp_OrdersSelectAll
PRINT 'Er zijn ' + @returnCode  ' values';
```

```sql
CREATE PROCEDURE usp_OrdersSelectAllForCustomer
	@customerID nchar(5) = 'ALFKI',
	@amountOfLines int OUTPUT --output parameter and no return required
AS
SELECT @amountOfLines = count(*) FROM orders WHERE customerID = @customerID

-- execute the stored procedure
DECLARE @number int
EXEC usp_OrdersSelectAllForCustomer 'ALFKI', @number OUTPUT
-- explicit use of formal parameters 
-- EXEC usp @customerID = 'ALFKI', @count = @numberOUTPUT
PRINT @number
```



ERROR HANDLING

```sql
CREATE PROCEDURE usp_ProductsInsert
	@productName nvarchar(40),
	@categoryID int, 
	@unitprice money 
AS 
INSERT INTO products(productname, categoryID, unitprice) 
VALUES (@productname, @categoryID, @unitprice)

-- @@ is a system variable
IF @@error = 515 
	PRINT 'ERROR! productname is NULL.'
ELSE IF @@error = 547 
	PRINT 'ERROR! CategoryID is not in CATEGORY table.' 
ELSE 
	PRINT 'ERROR! Unable to add new product.'
RETURN @@error	
```

```sql
ALTER PROCEDURE myProcedure
AS
BEGIN
	BEGIN TRY
		BEGIN TRANSACTION
		SELECT * FROM ...
		INSERT * INTO ...
		COMMIT
	END TRY
	BEGIN CATCH
		ROLLBACK
	END CATCH
END
```



### User Defined Functions


```sql
-- creation
CREATE FUNCTION GetAge( 
  @birthdate AS DATE, -- input parameter
  @eventdate AS DATE  -- input parameter
) 
RETURNS INT -- RETURNS TABLE
AS -- RETURN SELECT * FROM myTable
BEGIN 
	RETURN DATEDIFF(year, @birthdate, @eventdate) - 
		CASE WHEN 100 * MONTH(@eventdate) + DAY(@eventdate) < 100 * 					MONTH(@birthdate) + DAY(@birthdate) 
		THEN 1 ELSE 0 
	END; 
END;

-- usage
select dbo.GetAge(birthDate, getdate()) as leeftijd from users
```



### Persistent Stored Modules (PSM)

| Advantages                               | Disadvantages                            |
| ---------------------------------------- | ---------------------------------------- |
| code modularisation: remove redundant code and less maintenance on schema updates | Reduced scalability: business logic and DB logic run on same server |
| Stored procedures and triggers can influence the desired behaviour | Two programming languages (application and DB) |
| Security: exclude direct queries on the tables and determine yourself what is allowed and avoid SQL injection | Two debug environments                   |

-   Avoid PSM for larger business logic
-   Use PSM for technical stuff (logging, validation)



### Local Variables

```sql
DECLARE @variable_name int 

-- Assign a value to a variable (two ways)
set @max = (select max(invoiceTotal) from invoices)
select @max = max(invoiceTotal) from invoices

-- print content
print 'This is the max of the table' + @max
```



### Cursors

-   Allows to process individual rows to perform row specific operations instead of operation on the whole table with a SELECT statement

```sql
DECLARE CURSOR -- creates and defines the cursor
OPEN -- opens the declared cursor
FETCH -- fetches 1 row
CLOSE --closes the cursor (counterpart of OPEN)
DEALLOCATE --remove the cursor definition (counterpart of DECLARE)
```



```sql
DECLARE @orderid int, @orderamount decimal(18,2), @customername nvarchar(40)

DECLARE orders_cursor CURSOR FOR 
SELECT OrderID, OrderAmount, customername
FROM orders o 
JOIN customer c on o.CustomerID = c.customerid 
WHERE OrderAmount > 10000;

-- open the cursor
OPEN orders_cursor

-- fetch the first line
FETCH NEXT FROM orders_cursor INTO @orderid, @orderamount, @customername

WHILE @@FETCH_STATUS = 0
BEGIN
	PRINT 'Order: ' + str(@orderid) + ', Amount:  ' + str(@orderamount) + ', 			Customer: ' + @customername 
	
	FETCH NEXT FROM orders_cursor INTO @orderid, @orderamount, @customername
END 

-- close the cursor (can be reopened)
CLOSE orders_cursor 
			
-- deallocate the cursors (definition will be removed and will be closed)
DEALLOCATE orders_cursor
```



UPDATE AND DELETE VIA CURSORS

```sql
-- deletes row from the table where cursor is currently at
DELETE FROM <table name> 
WHERE CURRENT OF <cursor name>

-- update row in the table where cursor is currently at
UPDATE <table name> 
SET... 
WHERE CURRENT OF <cursor name>
```



### Triggers

-   Does **not** support parameters
-   Cannot be called explicitly (a Stored Procedure can be)
-   Called by the DBMS when a certain condition is met
-   DML triggers: insert, update, delete
-   Has inserted and deleted table that contains copies of the last inserted/deleted row



When to use:

-   Validation of data and complex constraints
-   Automatic generation of values
-   Support for alerts
-   Auditing (keep track of who did what on a certain table)
-   Replication and controlled update of redundant data (update datawarehouse)






**INSERTED EN DELETED TABLES**




| Advantages                               | Disadvantages                            |
| ---------------------------------------- | ---------------------------------------- |
| Store functionality in the DB            | Complex                                  |
| Execute consistently with each change of the data in the DB | Hidden functionality (unexpected behaviour or cascading deletes) |
| No redundant code in the different applications | Performance (trigger is executed on each DB change) |
| Written and tested once                  |                                          |
| Secure                                   |                                          |

```sql
--INSERT TRIGGER
CREATE TRIGGER penalty_insert ON penalties
FOR INSERT
AS
  DECLARE @pen smallint, @pnr smallint 
  SELECT @pen = amount, @pnr = playerno 
  FROM inserted 

  UPDATE players SET sum_penalties = sum_penalties + @pen
  WHERE playerno = @pnr
  
--UPDATE DELETE TRIGGERS
CREATE TRIGGER pen_del_upd ON penalties
FOR UPDATE,DELETE
AS
	DECLARE @pen smallint, @pnras smallint 
	SELECT @pnr = playerno FROM deleted
	SELECT @pen = SUM(amount) FROM penalties WHERE playerno = @pnr
	UPDATE players SET sum_penalties = @pen WHERE playerno = @pnr

```



### User defined types

-   Local temporary tables: #table
-   Global tempory tables: ##table
-   Table variables: @table

```sql
CREATE TYPE myType FROM nvarchar(50) NOT NULL;
```

```sql
CREATE TYPE TotalOrdersPerYear AS TABLE (
  year INT NOT NULL PRIMARY KEY,
  quantity INT NOT NULL
);

DECLARE @myordersperyear AS TotalOrdersPerYear;

INSERT INTO @myordersperyear 
SELECT YEAR(orderdate) AS year, round(sum(unitprice*quantity),2) 
FROM orders o 
JOIN ordersdetail od ON o.OrderID = od.orderid 
GROUP BY YEAR(orderdate);

SELECT * FROM @myordersperyear;
```



### Large objects

-   BLOB: Binary large object
-   CLOB: Character large object
-   NCLOB: National character large object




### Dynamic SQL

-   SQL injection

```sql
DECLARE @sqlstring nvarchar(100) = 'select * from supplier where region=@region';

PRINT @sqlstring;
EXEC sp_executesql @sqlstring, '@region varchar(50)', @region='OR';
```



## Hoofdstuk 5: XML Document

XML: Extensible Markup Language

What is XML:

-   Meta language
-   Fully extensible
-   Structured (tags and elements - hierarchical structure)
-   Validated by structure
-   Exchange format

| Advantages          | Disadvantages |
| ------------------- | ------------- |
| Single source       |               |
| Consistency of Data |               |

Structure: 

-   Prolog (header)
-   Epilog (optional)
-   Body
    -   Elements (<myElement>)
        -   Start tag
        -   Content
        -   End tag
        -   Naming rules:
            -   A tag starts with a letter or _ (not a number)
            -   Can contain both letters and numbers
            -   Spaces are not allowed
            -   Should not start with xml, this is a reserved word
            -   Avoid using . in tagnames (namespaces)
            -   Avoid using – or _, but use camelCasing notation
            -   Use significant names
    -   Attributes (name - value pairs) (<myElement myAttribute="value">)
        -   May only occur once in an element while elements can be repeated
        -   Order is unimportant
        -   Used for extra information about the data (meta-data)
        -   Reasons for not using attributes:
            -   Attributes can't contain multiple values
            -   Attributes can't contain substructures
            -   Attributen are not ordered, elements are
            -   More difficult to manipulate by programs
    -   Entity references (starts with & and ends with ;) (& lt;)
        -   Can be used if predefined in DTD
        -   Markup that parser replaces by referenced object
        -   Example: <!ENTITY copyright “copyright Hogeschool Gent”>
            -   Gebruik : <footer>&copyright;</footer>
    -   CDATA section
        -   Not treated while parsing the XML document.
    -   Comment
        -   <!–-This is comment -->
        -   Cannot be used inside a tag
        -   No spaces in the begin and end tag
    -   White space




Well-formed XML

-   One unique root element
-   Each tag has an opening and closing tag
-   Elements are nested correctly
-   Tags are case sensitive
-   Attribute values are between apostrophes
-   Symbols  < en & are encoded as & lt; and & amp;



```xml
<?xml version="1.0"?>
<shippingOrder>
  <shippingId>09887</shippingId>
  	<origin>
      <name>Ayesha Malik</name>
      <street>100 Wall Street</street>
      <city>New York</city>
      <country>USA</country>
  </origin>
  <destination>
    <name>Mai Madar</name>
    <street>Liivalaia 33</street>
    <city>Tallinn</city>
    <country>Estonia</country>
  </destination>
  <order>
    <item>
      <description>Ten Strawberry Jam bottles</description>
      <weight>3.141</weight>
      <tax>7.60</tax>
    </item>
  </order>
</shippingOrder>
```



Xml Parser:

-   Reads
-   Interprets
    -   Validating
    -   Non validating
-   Transfers to application



## Hoofdstuk 6: XML Schema

Schema:

-   Determines the exact structure of the XML document
-   Contains reference to schema in root element
-   The XML document should comply to the schema to be a valid document
-   External file with extension .xsd and consists of:
    -   XML declaration
    -   Schema element:
        -   Element declarations
        -   Attribute declarations
        -   Datatype declarations
            -   Simple
            -   Complex

XML schema declaration example:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Root element of each XML schema -->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <!–- definition of elements, attributes and datatypes -->
 </xs:schema>
```


XML element declaration example:

```xml
<!-- Built in data type -->
<xs:element name="myElement" type="xs:string"/>

<!-- Self defined data type -->
<xs:element name="myElement" type="myElementType"/>
```


XML element example:

```xml
<product>Tafel</product>
<age>34</age>
<dateborn>1969-03-27</dateborn>
<name>
  <firstname>John</firstname>
  <lastname>Peters</lastname>
</name>
```


Validation for XML element

```xml
<xs:element name="product" type="xs:string"/>
<xs:element name="age" type="xs:positiveInteger"/>
<xs:element name="dateborn" type="xs:date"/>
<xs:element name="name" type="nameType"/>
<xs:element name="firstname" type="xs:string"/>
<xs:element name="lastname" type="xs:string"/>
```


Attribute declaration:

```xml
<!-- Built in data type -->
<xs:attribute name="myElement" type="xs:string"/>

<!-- Self defined data type -->
<xs:attribute name="myElement" type="myElementType"/>

<!-- Examples -->
<xs:attribute name="zip" type="xs:string"/>
<xs:attribute name="code" type="codeType"/>
```



Simple Datatypes:

-   Elements without attributes and without children (\<age\>23\<age\>)
-   Attributes (\<person id="23">...\<person\>)

```xml
<xs:simpleType name="ageType">
  <xs:restriction base="xs:integer">    
    <xs:minInclusive value="0"/> <!--Facets (restrictions) -->
    <xs:maxInclusive value="100"/> <!-- Facets (restrictions) -->
  </xs:restriction>
</xs:simpleType>

<xs:simpleType name="paswoordType">
  <xs:restriction base="xs:string">
	<xs:minlength value="5"/> <!-- For fixed length use xs:length value=X -->
    <xs:maxlength value="8"/>
  </xs:restriction>
</xs:simpleType>

<!-- Reference to this data type -->
<xs:element name="age" type="ageType"/>
```

Enumerations: Definition of a list of allowed values


```xml
<xs:simpleType name="carType">
  <xs:restriction base="xs:string">
	<xs:enumeration value="GT"/>
    <xs:enumeration value="Golf"/>  
  </xs:restriction>
</xs:simpleType>
```

Whitespace: Restriction on the whitespace

-   Preserve : all white\-space show as is
-   Replace : all white\-space (blanks, tabs, linefeed,...)  replace by blank
-   Collapse : all white\-space replace by 1 blank


```xml
<xs:simpleType name="beschrijvingType">
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="preserve"/>
  </xs:restriction>
</xs:simpleType>
```

Patterns


```xml
<xs:simpleType name="smallLetterType">
  <xs:restriction base="xs:string">
    <xs:pattern value="[a-zA-Z]{1,3}"/>
  </xs:restriction>
</xs:simpleType>
```





Complex Datatypes:

-   Elements that have other elements
-   Mixed elements (text and children)
-   Empty elements


```xml
<!-- XML -->
<product artnr="1234">table</product>

<!-- XSD -->
<xs:element name="product" type="productType"/>
<xs:attribute name="artnr" type="xs:positiveInteger" />
<xs:complexType name="productType">
	<xs:simpleContent>
  		<xs:extension base="xs:string">
      		<xs:atribute ref="artnr" use="required"/>
      	</xs:extension>
  	</xs:simpleContent>
</xs:complexType>
```

Example


```xml
<!-- XML -->
<product>
  <prodnr>1234</prodnr>
  <name>Table</name>
</product>

<!-- XSD -->
<xs:element name="product" type="productType"/>
<xs:element name="prodnr" type="xs:positiveInteger"/>
<xs:element name="name" type="xs:string"/>
<xs:complexType name="productType">
	<xs:sequence>
  		<xs:element ref="prodnr"/>
  		<xs:element ref="name"/>
	</xs:sequence>
</xs:complexTypename>
```



Complex content with only elements:

-   xs:all = every child should appear once, order does not matter
-   xs:choice = only one of the elements should appear
-   xs:sequence = all should appear and in fixed order inside of parent element

Frequency


```xml
<xs:complexType name="productListType">
  <xs:sequence>
    <xs:element ref="product" minOccurs="0" maxOccurs="unbounded"/>
  </xs:sequence>
</xs:complexType>

<xs:complexType name="productLijstType">
  <xs:sequence minOccurs="0" maxOccurs="unbounded">
    <xs:element ref="product"/>
  </xs:sequence>
</xs:complexType>
```



ComplexContent empty elements


```xml
<!-- XML -->
<product artnr="1234"/>

<!-- XSD -->
<xs:element name="product"type="productType"/>
<xs:attribute name="artnr"type="xs:positiveInteger"/>
<xs:complexType name="productType">
	<xs:attribute ref="artnr" use="required"/>
</xs:complexType>
```



![Capture](img/Capture.PNG)





## Hoofdstuk 7: Business Intelligence

### Introduction

Business intelligence, or BI, is a blanket term for technology that helps companies organize and query their data to help them improve operations and make more money.

-   Organize and query data
-   Improve operations
-   Make more money



Reasons for Business Intelligence

-   Digitalisations
-   Reduce inefficiencies, inaccurasies
-   Decrease costs
-   Support decision making
-   Connector between BI software and Business Software



### Datawarehouse

#### What

a data warehouse is an integrated, subject oriented, time variant and non volatile collection of data to support decisions taken on management level.

-   **Integrated: ** integrates corporate application-oriented data from different source systems
-   **Subject oriented: **organized around the major subjects of the enterprise (customers, products, and sales) rather than the major application areas (stock control, product sales)
-   **Time:** only accurate and valid at some point in time or over some time interval. 
-   **Non volatile: **is not normally updated in real-time (RT) but is refreshed from operational systems on a regular basis. New data is always added as a supplement to the database, rather than a replacement.
-   **Aggregated data: **generated by GROUP BY



#### Goals

-   Reporting
-   Analysis of events in the past or actual events
-   Predictions based on trend analysis
-   Multidimensional reporting
-   Empowerment of end user by offering simplified reporting tools 
-   Data mining



#### Advantages

-   high ROI (return on investment)
    -   large investments with potentials high ROI after a short period
-   Competitive advantage
    -   decision makers get access to data that was not available, unknown or unused before. 
-   Increased productivity of corporate decision makers
    -   decision maker gets one consistent view on the enterprise 
        -   because data of different sources can be integrated to one consistent view, that is subject oriented and keeps history. 
    -   decision makers can make more substantial, more accurate and more consistent analysis. 
        -   tools can help turn data into useful information



### Architecture

![Capture2](img/Capture2.PNG)





### Data Mart

a DB existing of a subset of company data to support the needs of a particular business unit  for data analysis, or, to support users sharing the same needs for analyzing business  processes.

#### Why

-   to give users access to the data they analyse most frequently
-   to offer data in a way that corresponds to the collective view of a group of users in a department or a group of users in the same business process
-   to improve response time by offering lower data volumes
-   to offer data in a format that fits the tools used by end users (OLAP, datamining tools)
-   reduction of complexity in the ETL process
-   reduction of cost as opposed to the setup of a complete DWH



#### Problems

-   Underestimation of resources (costs) for ETL (Extraction, Transformation, Loading)
-   Hidden problems with source systems (missing, inconsistent data)
-   Required data not captured
-   Increased end-user demands (demand for more user friendly, powerful tools, training)
-   Data homogenization 
    -   trying to focus on similarities between data might lower the use of the data
-   Need for concurrent support of several (historical) versions
-   High demand for resources
-   Data ownership (everyone can access highly secured data (e.g HR))
-   High maintenance (every change in business processes influences DWH and ETL)
-   Long projects
-   DWH creates expectation of user ‘empowering’ 
    -   They think they no longer need specialists as they can do more themselves
-   complexity of integration
-   Complex change and version management
-   Night might be too short for ETL
    -   If new data is added to the DWH each night, it might take too long



### Design Methods

#### Immon

-   Creation of a data model based on all data of the organisation
-   Enterprise Data Warehouse (EDW)
-   Used to distil data marts for each department
-   Uses traditional methods for describing EDW:
    -   ERD
    -   tables in normal form



#### Kimball

-   Starts by identifying the information requirements (referred to as analytical themes) and associated business processes of the enterprise. 
-   This first data mart is critical in setting the scene for the later integration of other data marts as they come online. 
-   The integration of data marts ultimately leads to the development of an EDW. 
-   Uses dimensionality modelling to establish the data model (called star schema) for each data mart.



#### Star Schema

A Star schema is a logical structure that has a fact table (containing factual data) in the center, surrounded by **denormalized** dimension tables (containing reference data)

-   **fact table** contains data about facts
    -   e.g. factual data about sales of property: sales price, commission percentage, ...
    -   facts are generated by events that happened (e.g. a sale)
    -   most probably facts never change
    -   Bulk of data in data warehouse is in fact tables, which can be extremely large.
    -   Important to treat fact data as read-only reference data that will not change over time. 
    -   Most useful fact tables contain one or more numerical measures, or ‘facts’ that occur for each record and are numeric and additive
-   **dimension tables** contain reference information
    -   e.g. property data (address, etc), buyer, owner, ...
    -   Dimension tables usually contain descriptive textual information. 
    -   Dimension attributes are used as the constraints in data warehouse queries. (e.g. in where or having clauses)
    -   Star schemas can be used to speed up query performance by denormalizing reference information into a single dimension table.



#### Snowflake Schema

Snowflake schema is a variant of the star schema that has a fact table in the centre, surrounded by **normalised** dimension tables



#### Advantages of the dimensional model

-   Efficiency
    -   a consistent DB structure allows tools to have efficient access to the data
-   Ability to handle changing requirements
    -   the model can easily adapt to changing needs because each dimension is equivalent to the fact table
    -   ideal for ad hoc queries
-   Extensibility
    -   adding new facts
    -   adding new dimensions
    -   adding attributes to dimensions
-   Ability to model common business situations 
-   Predictable query processing 
    -   the way tables are used is predictable (not the queries themselves)



## 8: Indexes And Performance

### Index

####  What

-   ordered structure imposed on records from a table
-   Fast access through tree structure (B\-tree=balanced tree)



#### Advantages

-   Can speed up data retrieval
-   Can force unicity of rows



#### Disadvantages

-   indexes consume storage (overhead)
-   Indexes can slow down updates, deletes and inserts because index has to update too.





```sql
CREATE [UNIQUE] [NONCLUSTERED] --if unique, column must have NOT NULL constraint
INDEX index_name ON table(kolom[,...n])

DROP INDEX table_name.index
```



-   Which columns **should** be indexed?
    -   primary and unique columns are automatically indexed 
    -   foreign keys often used in joins 
    -   columns often used in search conditions (WHERE, HAVING, GROUP BY ) or in joins
    -   columns often used in the ORDER BY clause
-   Which columns **should not** be indexed?
    -   columns that are rarely used in queries
    -   columns with a small number of possible values (e.g. gender)
    -   columns in small tables
    -   columns of type bit, text of image




#### Include vs add table on creation

Use include if the columns you are including are only referenced in the select and not in the where clauses

Add them to in the index if they are used in the where clauses



**USE INCLUDE IF THE COLUMNS ARE ONLY REFERENCED IN THE SELECT CLAUSE**

**ADD TO THE CREATION OF THE INDEX IF THE COLUMNS APPEAR IN THE WHERE CLAUSES**



EX:

```sql
-- 1
CREATE INDEX index1 ON table1 (col1, col2, col3)
-- 2
CREATE INDEX index2 ON table1 (col1) INCLUDE (col2, col3)


-- Index 1 is better suited for this query
SELECT * FROM table1 WHERE col1 = x AND col2 = y AND col3 = z


-- Index 2 is better suited for this query
SELECT col2, col3 FROM table1 WHERE col1 = x
```








### Tips and Tricks

-   Avoid the use of functions (substring(), year(), month())
-   Avoid calculations (Salary * 1.10 > 10000 === Salary > 10000/1.10)
-   Prefer OUTER JOIN above UNION
-   Avoid ANY and ALL





## Hoofdstuk 9: Transaction Management

### Transactions

A transaction is an action or a sequence of actions on a database that have to be considered as 1 logical unit

##### Transaction States

-   committed
    -   The transaction was successful
    -   The transaction commits and the database has reached a consistent state
-   aborted
    -   the transaction was not successful
    -   the transaction aborts and the database has to be restored to the consistent state before the transaction started
    -   rollback or undo
-   a committed transaction can never be aborted
    -   if it appears the transaction was a mistake, another, compensating transaction, can be used. 
-   an aborted transaction can be restarted
    -   after the rollback a restart of the transaction can be executed
    -   successful execution will now lead to a committed transaction



```sql
-- tell the DBMS which parts are in the transaction
BEGIN TRANSACTION

COMMIT
ROLLBACK
```



#### ACID

-   **Atomicity**: the commands in a transaction are considered as indivisible
    -   all or nothing
    -   responsibility of DBMS : recovery subsystem
-   **Consistency**: transaction transforms the database one consistent state to another consistent state
    -   responsibility of both DMBS and application
-   **Isolation**: transactions are executed independently of one another
    -   transactions should not have unwanted interferences with each other
    -   partial effects of incomplete transactions should not be visible to other transactions
    -   responsibility of DBMS: concurrency-control subsystem
-   **Durability**: effects of a committed transaction are permanent and must not be lost because of later failure.
    -   after a failure these changes can be recovered
    -   responsibility of DBMS : recovery subsystem



### Concurrency Control

concurrency control is the process of managing simultaneous operations on the database without having them interfere with one another.

-   Prevents interference when two or more users are accessing the database simultaneously and at least one is updating data
-   Interleaving of transactions may produce incorrect results



#### Problems

-   lost update
    -   a lost update problem occurs when a successfully completed update is overridden by another user
-   uncommitted dependency
    -   the uncommitted dependency (or **dirty read**) problem occurs when one transaction can see intermediate results of another transaction before it has committed
-   inconsistent analysis
    -   the inconsistent analysis problem occurs when a transaction reads several values but a second transaction updates some of them during execution of the first. 
    -   non-repeatable (or fuzzy) read
        -   reading twice the same item returns 2 different values: a transaction reads a data item a second time and gets another value
    -   phantom read
        -   a transaction reads a number of records that correspond to a certain condition (WHERE-clause)
        -   the transaction repeats this later on but now gets extra records that meanwhile have been added by another transaction.



#### Schedule

a schedule is a sequence of actions from a number of concurrent transactions that respects the orders of the actions in the individual transactions



### Recovery

-   serializability
    -   the database makes schedules that can be executed without interference problems between transactions
    -   the schedules guarantees consistency and isolation of transactions
    -   assuming no transaction fails
-   recoverability
    -   if transaction fails, atomicity requires effects of transaction to be undone
    -   durability states that once transaction commits, its changes cannot be undone (without running another, compensating, transaction)



#### Locking

locking is a method used to manage  concurrent access to data.

-   a transaction can read or write a data item if and only if it has acquired a lock for that data item, and it has not yet released that lock. 
-   if a transaction acquires a lock on an item, it has to release that lock later on.



**Types of lock:**

-   shared lock (S-lock)
    -   If a transaction has a shared lock on an item, it can read but not update that item.
    -   Reads cannot conflict, so more than one transaction can hold shared locks simultaneously on same item. 
-   exclusive lock (X-lock)
    -   If a transaction has an exclusive lock on an item, can both read and update that item
    -   Exclusive lock gives transactions exclusive access to that item



**Deadlock**

An impasse that may result when two (or more) transactions are each waiting for locks held by the other to be released.

**How to handle deadlocks**

-   **timeouts**
    -   A transaction that claims a lock waits for a certain predefined time for that lock
    -   If the lock is not granted during that timeframe
        -   Assumption of deadlock
        -   Abort and restart of transaction
-   **deadlock prevention**
    -   Uses  transaction time\-stamps
        -   Special timestamp for dead detection
    -   T waits for locks held by U
        -   wait-die algorithm
        -   wound\-wait algorithm
    -   In both cases older transactions are privileged because the cost for redoing (after abort and restart) what has already been done is highest
-   **deadlock detection**
    -   Uses a wait-for graph with transaction dependencies















### DATA WAREHOUSE

1.  **Integrated:** integrates corporate application-oriented data from different source systems
2.  **Subject oriented:** organized around the major subjects of the enterprise
3.  **Time variant:** only accurate and valid at some point in time or over some time interval
4.  **Non volatile:** not normally updated in real\-time but refreshed from operational systems on a regular basis




### ACID

1.  **Atomicity: **the commands in a transaction are considered as indivisible
2.  **Consistency: ** transaction transforms the database from a consistent state to another consistent state
3.  **Isolation: **transactions are executed independently of one another
4.  **Durability: **effects of a committed transaction are permanent and must not be lost because of later failure