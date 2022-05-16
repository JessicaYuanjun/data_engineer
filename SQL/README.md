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
#### 2.1.1 CREATE
  - **SCHEMA - usually cannot create an empty schema, need to create a table**
    ``` SQL
    CREATE SCHEMA Sales AUTHORIZATION [Contoso\Mary];  
    GO

    CREATE TABLE Sales.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
    WITH (DISTRIBUTION = REPLICATE);  
    GO  
    ```  
  - **TABLE**
    - **Data Type**
      - char() & varchar()
        - char is used for fixed length, varchar is used for variable length
      - text
      - int
      - float
      - date
      - datatime
      - time
      - timestamp

    - **Partitioned**  
      - create partition function

    - **Keys**
      - Primary key: a set of attributes that can be used to uniquely identify every record
      - Foreigner key:
        - Maintains referential integrity by enforcing a link between the data in two tables
        - In child table its primary key
        - Constrain prevents actions that would destroy links between child and parent
      - Unique key:
        - Identifies a single row in the table
        - Multiple and null allowed but duplicate are not allowed 

    - **Constraints**
      - NOT NULL
      - UNIQUE
      - DEFAULT
      - CHECK
      - INDEX: Performance tuning method to allow faster retrieval of records from the table
        - Unique index: not allow the field to have duplicates. If the primary key is defined, a unique index is applied automatically   
        - Clustered index: reorder the physical order and search based on key values
          - Easily retrieval of data from the database and it's faster
          - Alter the way records are stored in the database as it sorts out rows by the column which is set to be clustered index
          - One table can only have one clustered index
        - Non cluster index: an index where the order of the rows does not match the physical order of the actual data
          - Slower
          - not alter the way stored but create a separate object within a table that points back to the original table rows after searching
          - can habe more non cluster index
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
  - **Temporary tables**
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
#### 2.1.2 QUERY
- **SELECT**
  - **Aggregate function** 
    - Min(), Max()
    - Count(), Avg(), Sum()
  - **TOP (MS SQL)**
    ``` SQL
    SELECT TOP 3 * FROM Customers;
    ```
  - **NULL**
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
  - **COALESCE - 可以接受多个参数，并且返回第一个非null的参数，如果都是null则函数返回null**
    ``` SQL
    SELECT name, price - COALESCE(discount, 0) AS real_price FROM for_sale;
    ``` 
  - **CASE WHEN - goes through conditions and return a value when the first condition is met**
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
  - **WINDOW FUNCTION**
    - VALUE
      - FIRST_VALUE():Value of argument from first row of window frame     
      - LAST_VALUE(): Value of argument from last row of window frame
      - NTH_VALUE: Value of argument from N-th row of window frame
      - LAG(field, num, defaultvalue)): Value of argument from row lagging current row within partition
      - LEAD(field, num, defaultvalue)): Value of argument from row leading current row within partition      
      ``` SQL
      SELECT student_id, score, course,
             FRIST_VALUE(course) OVER(PARTITION BY student_id ORDER BY score DESC) AS best_course,
             FRIST_VALUE() OVER(PARTITION BY student_id ORDER BY score DESC) AS best_grade,
             LAST_VALUE() OVER(PARTITION BY student_id ORDER BY score DESC) AS worse_grade,
             NTH_VALUE(score,2) OVER(PARTITION BY student_id ORDER BY score DESC) AS second_score,
             LAG(score,1,0) OVER(PARTITION BY student_id ORDER BY score DESC) AS gab_from_last_test
             LEAD(score,1,0) OVER(PARTITION BY student_id ORDER BY score DESC) AS gab_from_next_test                
      FROM Student
      ``` 
    - RANKING
      - RANK(): gap rank 1,2,2,4
      - DENSE_RANK(): continuesely rank 1,2,2,3
      - ROW_NUMBER(): row number
      - PERCENT_RANK(): 每行按照公式(rank-1) / (rows-1)进行计算。其中，rank为RANK()函数产生的序号，rows为当前窗口的记录总行数
      - CUME_DIST(): 分组内小于、等于当前rank值的行数 / 分组内总行数
      - NTILE(): 将分区中已经排讯的行，分成尽量数量相等的，指定数量的组
      ``` SQL
      SELECT student_id, score, course,
             RANK() OVER(PARTITION BY student_id ORDER BY score DESC) AS gap_ranking,
             DENSE_RANK() OVER(PARTITION BY student_id ORDER BY score DESC) AS continuesly_ranking,
             ROW_NUMBER() OVER(PARTITION BY student_id ORDER BY score DESC) AS row_number,
             PERCENT_RANK() OVER(PARTITION BY student_id ORDER BY score DESC) AS percent_rank,
             CUME_DIST() OVER (PARTITION BY BY student_id ORDER BY score) AS 'CUME_DIST1'   
             NTILE(4) OVER (PARTITION BY BY student_id ORDER BY score) AS 'CUME_DIST1' 
      FROM Student
      ``` 
    - AGGREGATE 
      - AVG()
      - COUNT()
      - MAX()
      - MIN()
      - SUM() 
      ``` SQL
      SELECT student_id, score,course
             SUM(score) OVER (PARTITION BY student_id ORDER BY score) AS grade_total,
             COUNT(score) OVER (PARTITION BY student_id ORDER BY score) AS grade_count,
             AVG(score) OVER(PARTITION BY student_id ORDER BY score) AS grade_avg
             MIN(score) OVER(PARTITION BY student_id ORDER BY score) AS grade_min
             MAX(score) OVER(PARTITION BY student_id ORDER BY score) AS grade_max
      FROM Student
      WHERE course='Math'      
      ``` 
    - window function just will not change table structure because it excuted only before order by
    - NULLS 值将被视为其自己的组，并根据 NULLS FIRST 或 NULLS LAST 选项进行排序和排名。默认情况下，按 ASC 顺序最后对 NULL 值进行排序和排名，按 DESC 顺序首先对 NULL 值进行排序和排名。 

#### 2.2.3 FROM 
  - **JOIN: used to combine rows from two or more tables, based related columns**
    - full join: return all records where there is a match in left or right
    - inner join: matching value in both tables
    - left join: return records from all left table, use left thing to match right thing
    - right join: 
    - cross join: used to generate a paired combination of each row of the first table with each row of the second table
    - self join 
  - temporary table
- **WHERE**
  - between and
  - in / not in
  - Like / not like
    - '%'
    - '_'
  - is null / is not null
- **ORDER BY ASC/DESC**
- **GROUP BY - used for agg function**
- **HAVING - used for agg function to filter data**
- **UNION - The UNION operator is used to combine the result-set of two or more SELECT statements.**
  - Select only distinct values 
  - Every SELECT statement within UNION must have the same number of columns
  - The columns must also have similar data types
  - The columns in every SELECT statement must also be in the same order
- **UNION ALL**
  - Allow duplicate values 

