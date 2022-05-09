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
      ```
      CREATE SCHEMA Sales AUTHORIZATION [Contoso\Mary];  
      GO

      CREATE TABLE Sales.Region   
      (Region_id INT NOT NULL,  
      Region_Name CHAR(5) NOT NULL)  
      WITH (DISTRIBUTION = REPLICATE);  
      GO  
      ```
    - TABLE
      - 
