# Data Warehouse: Concepts, Design, and Architecture

## 1. Why do we need a Data Warehouse?
*   **Key Benefits:** 
    *   Data Consolidation and Integration (harmonizing data from ERPs, external sources, etc.).
    *   Improved Data Quality (standardization, cleansing).
    *   Historical Analysis (storing long-term data for trend identification).
    *   Business Intelligence (platform for reporting, analytics, and tactical planning).

## 2. Defining a Data Warehouse
As defined by **W.H. Inmon**, a data warehouse is a **subject-oriented, integrated, time-variant, and non-volatile** collection of data in support of management's decision-making process.

*   **Subject-oriented:** Organized around major business subjects (e.g., customer, product, sales) rather than specific applications.
*   **Integrated:** Data from multiple domains (patients, finance, HR) is consolidated into a single physical image, reformatted, and standardized.
*   **Non-volatile:** Data is loaded (usually en masse) and accessed, but not updated in real-time like a transactional system.
*   **Time-variant:** Records include time-marking to show when data was accurate, allowing for historical trend analysis.
### Steps to build a data warehouse
- **Business questions:** You cannot build a data warehouse if you don't know what you are trying to solve.
- **Data Profiling:** Once you know the questions, you look at your source systems to see what data you actually have. You examine it to understand its quality, format, and structure.
- **Data Acquisition:** After knowing what data exists and is "healthy," you move it from the source systems into your staging/loading environment.
- **Data Cleansing:** Finally, you "clean" the acquired data (fixing nulls, standardizing formats, removing duplicates) so that it is ready to be loaded into the warehouse for the final analysis
---

## 3. Data Modeling
Data warehousing uses **Dimensional Modeling** .
**Dimensional Modeling** is a design technique used in data warehousing to make data easy for end-users to understand and fast for BI (Business Intelligence) tools to query.
### 1. Data Modeling: The Life cycle
Data modeling isn't just one step; it’s a progression from abstract business needs to technical reality:
*   **Conceptual:** Defining "what" you need ("We need to track Customers and Sales").
*   **Logical:** Defining "how" entities relate ("One Customer can place many Orders").
*   **Physical:** Defining the technical implementation ("The 'Orders' table will use a Surrogate Key and be indexed by 'Date'").

---

### 2. The Core Structure: Facts vs. Dimensions
This is the heart of every Data Warehouse.
*   **Fact Tables:** These contain the **numbers** (measures) that you want to calculate (Total Sales, Quantity Sold, Profit). They are usually long, thin tables with many Foreign Keys connecting to Dimensions.
* **Atomic fact table:** is a fact table that stores data at the lowest possible level of detail captured by a business process.
*   **Dimension Tables:** These contain the **descriptive context** (the "Who, What, Where, When"). If a Fact says "10," a Dimension explains that "10" means "10 Units of *iPhone 15* sold in *New York* on *Monday*."
*  A **conformed dimension** is a dimension table that is designed to be used consistently across multiple fact tables or data marts.
* A **mesure group** is a logical collection of measures that share the same fact table.
##### Why a fact table:
- Storage Efficiency 
- Performance: it's much faster to scan a thin table than a big one.
---
### Joins between fact and dimension tables
#### 1. Inner joins:
- most common
- This is the most common join. It returns only the records that have a match in both tables.
- You take the **Foreign Key (FK)** from the Fact table and match it to the **Primary Key (PK)** of the Dimension table.

#### 2. Left outer join:
- This returns all records from the Fact table, even if there is no corresponding record in the Dimension table (the result will show NULL for the missing dimension attributes).

#### 3. Cross Join:
- This join combines every single row of the Fact table with every single row of the Dimension table.
---

### 3. Fact Table Measures & Granularity
*   **Granularity:** This is the "level of detail." 
    *   *Low granularity* = Data is summarized (e.g., Monthly Sales).
    *   *High granularity* = Data is very detailed (e.g., Each individual transaction swipe). 
    *   *Note:* The deeper you drill, the more granular the data becomes.
*   **Types of Facts:**
    *   **Additive:** You can safely add these across any dimension (e.g., adding up Sales).
    *   **Semi-Additive:** You can add them across *some* dimensions but not others. 
    *   **Non-Additive:** You cannot add them at all. (Example: "Temperature." If you have two rooms at 20°C, you don't say the house is 40°C).

---

### 4. Types of Fact Tables
*   **Transaction:** The most common. Captures a single event (e.g., one sale).
*   **Periodic Snapshot:** Captures the state of something at regular intervals (e.g., daily total balance).
*   **Accumulating Snapshot:** Tracks an event from start to finish (e.g., an order process: "Ordered" -> "Packed" -> "Shipped" -> "Delivered").
*   **Factless Fact Table:** These don't have a "number" to add up. They just record that an event *happened* (e.g., "Student attended class"). They are used to count occurrences (e.g., count how many days a student was present).

---

### 5. Slowly Changing Dimensions (SCDs)
In a Data Warehouse, information about people or products often changes (e.g., an employee gets a promotion). How you handle that change is called an **SCD strategy**.

*   <mark style="background: #BBFABBA6;">SCD Type 0 (Retain Original):</mark> Never change the record. If someone moves, you ignore the new address or keep the old one forever.
*   <mark style="background: #BBFABBA6;">SCD Type 1 (Overwrite): </mark> You replace the old data with the new data. 
    *   *Pro:* Easy to maintain - space efficient - current state accuracy - fast quering.   
    *   *Con:* You lose history. 

- <mark style="background: #BBFABBA6;">SCD Type 2: </mark> keep history by creating new rows for changes
	- *Pros:* full historical tracking - No data loss - Operations stay attached to the conditions that existed at that moment
---

## 4. Architectural Design
### Data Integration Area (Kitchen):
- The Integration Area is where all the raw data from your various source systems (like your CRM, your website, your accounting software, and your shipping app) is gathered and cleaned.
- What happens there?
	- <mark style="background: #FFF3A3A6;">Cleaning</mark>
	- <mark style="background: #FFF3A3A6;">Integration:</mark> Merging data from different systems into a single format
	- <mark style="background: #FFF3A3A6;">Normalization: </mark>Storing the data into a 3NF structured way.
### Presentation Area (Dining Room):
- It is the final layer that the end-user (analyst or manager) actually interacts with
- **Primary Goal:** Provide fast, easy-to-understand data for decision-making.
- What happens there?
	-  **Denormalization:** The data is reshaped (often into Star or Snowflake schemas) to be very easy to "slice and dice."
	- **Aggregation:** is when data had been combined, summarized, or "rolled up" from a detailed level to a higher, more general level (e.g., instead of storing every individual sale, it stores the "Monthly Revenue by Store").
	- **Business Logic:** This is where you apply specific rules (e.g., defining what "Customer Churn" or "Gross Margin" means for your company).
### Staging Area
A physical database used to load, transform, and cleanse data from multiple sources before feeding it into the warehouse.

### Approaches
#### Inmon Approach (top-down):
- **Normalization Level:** High (3NF - Third Normal Form)
- **Schema Type:** Normalized/Relational
- **The Logic:** You build a massive, highly normalized, central Data Warehouse first. It acts as the "Single Version of the Truth" for the entire corporation.
- **The Flow:** Data goes Source`→`Staging`→`Big Central Warehouse`→`Smaller Data Marts`→`Reports.
- **Pros:** Very high data integrity and consistency. Because all data is centralized, you don't have conflicting reports.
- **Cons:** Takes a **long time** and a lot of money to build. You can't show results to the business until the massive central warehouse is finished.

#### The kimball approach(Bottom-up):
- **Denormalized data:** Kimball uses dimensional modeling(keeps data in dimensions tables).
- Uses star schema.
- **The Logic:** You build "Data Marts" for specific business processes (like Sales, Inventory, or HR) first. You then link them together using **"Conformed Dimensions"** (shared data like common Customer IDs or Product IDs). This linking is the "Data Warehouse Bus."
- **The Flow:** Data goes Source`→`Staging`→`Data Marts (linked by Bus)`→`Reports.
- **Pros:** **Fast to implement.** You can deliver a "Sales" report to the business in weeks rather than months. It is agile and user-focused.
- **Cons:** If you aren't careful with "Conformed Dimensions," you can end up with "Data Silos" where the Sales department’s numbers don't match the Marketing department’s numbers because they aren't talking to each other.
#### Which one should you choose?

- **Choose Inmon if:** You are a massive enterprise  where **data accuracy and strict consistency are more important than speed.** You have the budget and time to build a robust, centralized, unified repository.
    
- **Choose Kimball if:** You are an agile business that needs to provide **fast insights to different departments.** You want to prove the value of data analytics quickly, and you have strong data governance to ensure that all your "Data Marts" use the same definitions (e.g., agreeing on exactly what a "Customer" is across all departments)




---

## 5. OLAP vs OLTP
![[Pasted image 20260326162408.png]]
![[Pasted image 20260326162531.png]]

### OLAP
OLAP enables analysts to gain insight through fast, consistent, and interactive access to information.
#### Operations
*   **Roll-up:** Aggregating data; taking many small items and collapsing them into a single total
* **Drill up:** moving from a specific member of a category to a broader, parent category. To see the "Big Picture"
*   **Drill-down:** Accessing deeper details.
*   **Slicing:** Selecting one dimension value (a "slice" of the cube).
*   **Dicing:** Extracting a smaller sub-cube.
*   **Pivot:** Rotating the data view. Example: instead of comparing product sales by month we rotate to compare monthly sales by products

#### Storage Models
*   **MOLAP(Multidimensional OLAP):** Data stored in multi-dimensional hypercubes.Instead of rows and columns, the data is pre-calculated and stored in a multi-dimensional array. If you want to know "Sales by Region by Product by Month," the answer is already pre-computed inside the cube.
*   **ROLAP:** Relational OLAP (uses existing relational DBMS:Database Management System).
*   **HOLAP:** Hybrid (combines ROLAP and MOLAP).
*   **DOLAP:** Desktop OLAP. It is designed for **individual users** on their own computers

---

## 6. Data Marts
A **subset of data** from the Data Warehouse, focused on a **single subject area**. 
*   **Dependent:** Created from an existing EDW(enterprise Data Warehouse.) (logical or physical).
*   **Independent:** Stand-alone system (cheaper to start, but cumbersome to manage long-term).
*   **Hybrid:** Combines EDW data with other operational sources.

---
# Schemas
-  Ways to organize your Fact and Dimension tables.
## Data models:
### 1. Star Schema (Denormalized)
It looks like a star: one central Fact Table surrounded by "spikes" (Dimension Tables).

*   **Structure:** Every Dimension Table is directly connected to the Fact Table. There is **no further splitting** of the dimensions.
*   **Pros:** 
    *   **Extremely Fast:** Since everything is flattened, the database doesn't have to do many "joins."
    *   **Easy to understand:** The structure is simple and intuitive.
*   **Cons:** 
    *   **Data Redundancy:** You end up repeating data. (e.g., if 100 products are in the "Electronics" category, the word "Electronics" is typed 100 times in the table).
*   **Choose Star Schema if:** You want **speed.** In the world of Data Warehousing, we usually prioritize "Read Performance" over storage space. Because modern storage is cheap, most data warehouses use the Star Schema to give analysts the fastest possible answers.

---

### 2. Snowflake Schema (Normalized)
It looks like a snowflake: the main dimensions "branch out" into sub-dimensions, creating a branching pattern.

*   **Structure:** The Dimension Tables are normalized. Instead of putting all product info in one table, you split it into: `Product` $\rightarrow$ `Subcategory` $\rightarrow$ `Category`.
*   **How it works:** To get a report by "Category," the database must join the `Sales` table to the `Product` table, then the `Product` table to the `Subcategory` table, then the `Subcategory` table to the `Category` table.
*   **Pros:**
    *   **Saves Storage Space:** Because data is normalized, you aren't repeating "Electronics" 100 times. You store it once in the `Category` table and just link to it.
    *   **Easy Maintenance:** If the "Category Manager" changes, you only update it in one place.
*   **Cons:**
    *   **Slower Performance:** The database has to perform multiple complex joins to get the data, which slows down reporting significantly.
    *   **Complex:** Much harder for business users to query.
*   **Choose Snowflake Schema if:** You have **massive amounts of data** where storage space is a genuine concern, or if you have **very complex hierarchical data** that changes very frequently and must be kept perfectly synchronized
---
### 1. New Section: SSIS (SQL Server Integration Services)
**Purpose:** ETL (Extract, Transform, Load)

- **Core Components:** Every SSIS package are: **Control Flow** (the workflow of tasks like executing SQL, file management, or FTP), a **Data Flow** (the movement and transformation of data), and **Event Handlers** (actions triggered by events like errors or completion).
    
- **Common Tasks:**
    - **Execute SQL Task:** Used to run stored procedures or SQL commands against a database.
    - **Foreach Loop Container:** To repeat a set of actions multiple times 
	- **Data Flow Task:** It is used to move data from a source to a destination.
        
- **Scheduling & Incremental Loads:**
    
    - **Scheduling:** SSIS packages are typically scheduled using **SQL Server Agent**.
    - **Incremental Loads:** Instead of "Full Loads" (truncating/reloading all data), use a **Timestamp column** or a "Last Modified" date to identify only new or updated records since the previous load.
        

### 2. New Section: SSAS (SQL Server Analysis Services)
**Purpose:** Data Modeling and Rapid Analytics
- **Modes of Deployment:**
    - **Tabular:** Based on in-memory storage, optimized for performance and rapid development.
    - **Multidimensional:** Based on traditional OLAP cubes, uses MDX, and is better for complex hierarchies.
- **Cube Components:**
    - **Measure Group:** A collection of related measures (like Sales Amount, Profit) linked to a specific fact table.
    - **Aggregations:** Pre-calculated summaries stored within the cube to ensure query performance is instantaneous.
- **Storage Modes:**
    - **MOLAP:** The data and the aggregations are stored in the SSAS engine's native, optimized format.

### 3. Strengthening "Data Modeling" (Refining your current notes)

You can update your "Fact Tables" and "Schemas" sections with these clarifications:

- **Granularity:** Emphasize that **"The lowest level of detail needed for business questions"** is the golden rule. Avoid summarizing data (like Daily Totals) in the fact table if the business might need transaction-level insights later.
    
- **Fact-to-Dimension Relationship:** Update your notes to state clearly: **"Fact tables contain Foreign Keys that reference the Primary Keys of Dimension tables."**

