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
