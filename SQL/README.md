# SQL
## 1. Definition and basic concepts
SQL (Structured Query Language) is a programming language designed for managing data in a relational database. 
### 1.1 Type
  - DDL: Data definition language
    - consist of the commands that can be used to define the database schema
    - CREATE, DROP, ALTER, TRUNCATE, COMMENT, RENAME
  - DQL: Data query language
    - Perform queries
    - SELECT
  - DML: Data manipulation language
    - Deal with manipulation
    - INSERT, UPDATE, DELETE, LOCK, CALL EXPLAINPLAN
  - DCL: Data control language
    - Deal with the rights, permissions, and other controls
    - GRANT, REVOKE 
### 1.2 Syntax
  - SQL keywords are not case sensitive
  - Some systems require a semicolon at the end of each SQL statements
### 1.3 Execution Order
  FROM - WHERE - GROUP BY - HAVING - SELECT - WINDOW FUNCTION - ORDER BY
## 2. SQL commands and statements
### 2.1 Conmands about table
  - CREATE
    - SCHEMA - usually cannot create an empty schema, need to create a table
      ``` SQL
      CREATE SCHEMA Sales AUTHORIZATION [Contoso\Mary];  
      GO

      CREATE TABLE Sales.Region   
      (Region_id INT NOT NULL,  
      Region_Name CHAR(5) NOT NULL)  
      WITH (DISTRIBUTION = REPLICATE);  
      GO  
      ```  
    - TABLE
      - data type
        - char() & varchar()
          - char is used for fixed length, varchar is used for variable length
        - text
        - int
        - float
        - date
        - datatime
        - time
        - timestamp
      - Partitioned  
        - create partition function
      - Constraints
        - PRIMARY KEY
          - CLUSTERED | NONCLUSTERED the index type
          - （column_name1[, column_name2,…,column_name16]）
        - UNIQUE
        - FOREIGN
        - DEFAULT
        - CHECK
        -      
      ``` SQL
      CREATE TABLE dbo.PurchaseOrderDetail
      (
      PurchaseOrderID int NOT NULL
          -- FOREIGN KEY constraints
          REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
      LineNumber smallint NOT NULL,
      ProductID int NULL
          REFERENCES Production.Product(ProductID),
      UnitPrice money NULL,
      OrderQty smallint NULL,
      ReceivedQty float NULL,
      RejectedQty float NULL,
      DueDate datetime NULL,
      rowguid uniqueidentifier ROWGUIDCOL NOT NULL
          CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
      ModifiedDate datetime NOT NULL
          CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
      LineTotal AS ((UnitPrice*OrderQty)),
      StockedQty AS ((ReceivedQty-RejectedQty)),
      CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
                 PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
                 WITH (IGNORE_DUP_KEY = OFF)
      )
      ON PRIMARY;
      ``` 
    - Temporary tables
      - useful for sorting the immediate result sets that are accessed multiple times
      - local temporary table (add # before table name)
        - visible only in the current session
      - global temporary tables (add ## before table name)
        - visible to all serrions
      - Cannot be partitioned
      - Automatically deleted when the connection is closed or drop table
      ``` SQL
      CREATE TABLE #MyTempTable (
      col1 INT PRIMARY KEY
      );

      INSERT INTO #MyTempTable
      VALUES (1);
      
      -- OR
      
      SELECT name, age, gender
      INTO #MyTempTable
      FROM student
      WHERE gender = 'Male'
      ``` 
- DQL
  - Select
    - Aggregate function 
      - Min(), Max()
      - Count(), Avg(), Sum()
    - top (MS SQL)
      ``` SQL
      SELECT TOP 3 * FROM Customers;
      ```
    - NULL
      - ifnull() - MySQL
      ``` SQL
      SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
      FROM Products;
      ``` 
      - isnull() - sql server
      ``` SQL
      SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0))
      FROM Products;
      ``` 
    - COALESCE - 可以接受多个参数，并且返回第一个非null的参数，如果都是null则函数返回null
      ``` SQL
      SELECT name, price - COALESCE(discount, 0) AS real_price FROM for_sale;
      ``` 
    - CASE WHEN - goes through conditions and return a value when the first condition is met
      ``` SQL
      SELECT OrderID, Quantity,
      CASE
        WHEN Quantity > 30 THEN 'The quantity is greater than 30'
        WHEN Quantity = 30 THEN 'The quantity is 30'
        ELSE 'The quantity is under 30'
      END AS QuantityText
      FROM OrderDetails;
      
      -- 行转列 
      -- 如果是分组后再转置；也就是需要先group by再转置，则需要对case when 或 if 使用聚合函数。
      -- 即： group by和聚合函数要么都出现，要么都不出现。
      SELECT createtime,
           CASE paytype WHEN '支付宝' THEN money END AS '支付宝',
           CASE paytype WHEN '手机短信' then money end as '手机短信'
      FROM inpours ;      
      
      ``` 
      
  - from 
    - join: used to combine rows from two or more tables, based related columns
      - full join
      - inner join
      - left join
      - right join
      - cross join
      - self join 
    - temporary table
  - where
    - between and
    - in / not in
    - Like / not like
      - '%'
      - '_'
    - is null / is not null
     
