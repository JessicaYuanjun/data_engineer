# Data Engineer
## 0. Technical Skills
  - Database
    - [SQL](https://github.com/JessicaYuanjun/data_engineer/tree/main/SQL)
    - Database & Data Modeling
  - Python
  - Airflow
  - Spark
  - Hadoop
  - Cloud
    - AWS 
## 1. Ingest data from different sources
### 1.1 Data Sources
  - Application
  - Database
  - Web
### 1.2 Data Type
  - **Structured data**
    - Easy to search and organized
    - Consistent model, rows, and columns
    - Define types
    - Can be grouped to form relations
    - Store in relational database and can be queried
    - Abount 20% of the data is structured
  - **Semi-structed data**
    - Relatively easy to search and organize
    - Consistent model, less-grid, different observation have different size
    - Different types
    - Can be group but need more work
    - NoSQL: JSON, XML, YAML
  - **Unstructured data**
    - Does not follow a model, can’t be contained in rows and columns
    - Difficult to search and orgainze
    - e.g. text, sound, pictures, videos
    - Usually store in data lakes, can appear in data warehouses or databases
    - Most of the data is unstructured
    - Can be extremely valuable
    - Processed by AI or add infor make it semi-structured
### 1.3 Think about data source
  - How much data
  - What kind of data
  - How frequent
  - How accurate
  - Who use it
## 2. Build up data pipelines
### Automate flow from one station to the next and provide up-to-date, accurate, and relevant data
<img width="544" alt="截屏2022-05-07 下午12 46 18" src="https://user-images.githubusercontent.com/47366120/167321041-a503b421-95be-4f6b-8792-ac08f46a27ab.png">

### 2.1 Data piplines
  - Move data from one system to another
  - May not be transfored
  - May directly loaded in applications
  - May flow ETL
### 2.2 ETL - A framework for designing data pipelines. Data is processed before stored
  - Extract
  - Transform
  - Load    
### 2.3 Airflow

## 3. Set up storage
### 3.1 Think about 
  - Reliability
  - Autonomy
  - Scalability
  - Speed
### 3.2 NoSQL
  - Non-relational Database
  - Store structure or unstructured data
  - Key-value store
### 3.3 SQL Database (SQLite, MySQL, PostgreSQL, Oracal, MS SQL)
  - Data modeling
  - Schema, tables, relationship
  - Store and orgaize data
  - For rapid search and retrival
  - Made by tables (rows and columns)
  - Govern how tables are related
  - **Appication databases**
    - Transactions - insert and change rows, e.g. change user's name
    - OLTP
    - Row-Oriented
    - **Store transactional data**
      - Describes business events
      - Data is straightforward, real-time, looks similar like business
      - Offer current view of latest transctions
      - Collected for every event that happens on the system, be it item purchased, order modified, status changed etc.
    - **Store operational data**
      - Describes IT operations
      - Related to information and technology assets of the organization
    - E.g.
      - There is an online store, every purchase done a user is captured as a transaction, this will be transactional data. 
      - There will a time dimension, like at what time it is purchased, when it is purchased.
      - To support the purchase, there is some supporting data, for example, we need to set up the "Avaliable" as "Sold".
      - Data for the whole system is called operational data.
      
### 3.4 Data Warehouse
  - Data Warehouse is a type of database
    - Analytical databases
    - OLAP
    - Column-oriented
    - Connection strings/URL   
  - Store specific data for specific use
  - Use for analytics work or data product, e.g. business analysis
  - Store mainly structred data
  - More costly to update
  - Optimized for data query
  - Ad-hoc, read-only queried
  - Often star schema
  - Usually store analytical data
    - contain the historical infomation to support long-term decision making
    - data is more complex 
    - can be categoried by different snapshot
### 3.5 Data Lake
  - Store all the data - sturctrued, semi-structured, and unstructured
  - Store raw data - unprocessed and massive
  - Cost-effective 
    - because function is simplicity and scalability
    - petabyte
  - Used by big data or real-time analytics
  - **Data Governance - to understand data and aviod data lake become data swamp**
    - Definition
      - An enterprise framework that aligns people, processes, and technology to help data users understand and transform data into a business asset and deliver data visibility to reduce risk 
    - Process
      - Identify the data owners to be responsible for data quality, regulatory compliance, and data usage
      - Identify ownership to ensure that someone is responsible for the data source, definition, business attributes, relationships, dependencies...
      - Assigns data stewards to supervise data analysis, produce reports for data users, and an answer questions
      - Estabilished governance guidelines and policies
      - Ensure organization collaborates across the enterprise to agree on the ownership and terms
      - **Data Catelog - to ensure the data remains trustworthy and protected**
        - A collection of metadata 
          - where is the source of the data
          - where is this data used
          - who is responsible for maintain it
          - how often is the data updated
      - **Data lineage**
        - Identifies data's movement across an enterprise
          - from system to systme
          - user to user
        - Provide an audit trail througout the data lifecyle 
        - Know and use data better
          - understand data’s relationships with other data sets 
          - identify crucial business process relationships
          - discover the flow and dependencies of data
        - Protect data and reduce risk
          - e.g. avoid some of the responsibilities associated with not knowing where data is at a given stage in the process  
      - **Data classification**
        - Gat data to classify data to maintain compliance and answer question about data
        - e.g. organization need to classify personal data and label it to ensure the sensitive data is being used and acessed appropriately 
        - Content-Based Classification 
          - addresses security or privacy and protect it from unauthorized access
          - inspect the sensitive, personal, and confidential infomation  
        - Context-Based Classification
          - how information is being used
          - who use it
          - determine whether information is sensitive
          - e.g. local regulation mandate how data can be used within specific region
        - User-based Data Classification
          -  manual processes based on people's unique knowledge
          -  e.g. data steword label data as sensitive based on the data lineage
## 4. Moving and processing data
### 4.1 Data processing - convert raw data into meaningful information
  - **Remove unwanted data**
    - Optimize memory, process and network cost
    - Cannot afford or stream big file  
  - Convert data into usable type or format
  - Organized data
    - Reorganized data from data lake or data warehouse
  - Fit data into a schema/structure
  - Increate data productivity
### 4.2 How to process data
  - Data manipulation, cleaning, and tidying 
    - Reject corrupt files
    - Why missing metadata
  - Store data in a structred database
  - Create views on top of the database table for analyst easy access
    - e.g. create a view to conbine the Artists table and Albums table 
  - Optimizing the performance of the database
    - Indexing
### 4.3 Processing Type
  - Batch processing
    - Group record at intervals
    - Cheaper
    - Can be scheduled
  - Stream processing
    - Individuale records right away
      - e.g. new user sign in need to be interval with database right away
### 4.4 Scheduling
  - Automation and decrease manul work
  - Hold each piece and organize how they work together
  - Run task in specific order and resolve all dependency   
  - Ways
    - Manually
    - Automated at a specific time
      - at 6 am every day 
    - Automated at a specific condition 
      - Sensor schedule
      - e.g. if new files is updated, load data in a table 
  - Job dependency
    - DAG - Directed Acyclic Graph
      - Set of nodes
      - Directed adges - upstream & downstream
      - No cycles - connot run more than once
      - tool
        - Linix's cron
        - Luigi
        - Airflow
### 4.5 Parallel computing
  <img width="649" alt="截屏2022-05-07 下午3 37 22" src="https://user-images.githubusercontent.com/47366120/167338604-595e9397-b30e-4577-91e5-501893ed0126.png">

  - How
    - Split task up into several smaller subtasks
    - Distribute those subtasks over several computers   
  - Pros
    - Extra processing power
    - Reduce memory
  - Cons
    - Moving data incur cost
    - Communication time
    - Need several processing units
  - Tools
    - Python multiprocessing
    - Computation frameworks
      - Hadoop
      - Spark
### 4.6 Cloud computing
  - Non-cloud: costly, need room for server, unsafe, hard to optimize  
  - Pros
    - Rented server and no hardware cost
    - Do not need rooms and hydro for hardware 
    - Use service when need - scalability
    - Database reliability
      - Data replication 
      - Protect data from disaster
      - Protect sensitive data 
        - Easy to assign security group
  - AWS
  - Azure
  - GIC
  - Multi-could
    - Pro
      - Reducing reliance on a single vendor
      - Cost-effciencies
      - Agains disasters
    - Cons
      - Hard to manage
