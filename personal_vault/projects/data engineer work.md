
### 1. Advanced Data Structures

- **Why for a D.E.?** Data engineers don't just deal with simple Excel tables. They handle complex, unstructured, and massive datasets.
    
- **The Work:** To store data efficiently and make sure it can be retrieved fast, they must use advanced structures (like Trees, Graphs, or specialized file formats like Parquet and Avro). They need to understand how data is organized in memory to optimize the "pipelines" they build.
    

### 2. Distributed Computing (Informatique distribuée)

- **Why for a D.E.?** "Big Data" is too large for one single computer. It has to be spread across hundreds or thousands of servers (a cluster).
    
- **The Work:** A Data Engineer must know how to write code that runs across these multiple machines simultaneously. They ensure that if one machine fails, the data isn't lost and the process continues. This is the foundation of modern data storage and processing.
    

### 3. Concurrent Programming (Programmation simultanée)

- **Why for a D.E.?** Efficiency and speed. If you process data "line by line" (sequentially), it would take years to process a petabyte of data.
    
- **The Work:** They write programs that perform many tasks at the exact same time (parallelism). This is crucial for "ETL" (Extract, Transform, Load) processes where they are simultaneously pulling data from one source, cleaning it, and moving it to another.
    

### 4. Knowledge of Tools (Hadoop, Spark, Kafka, Hive)

These are the "industrial machines" the Data Engineer uses to build their systems:

- **Hadoop:** Used for storing massive amounts of data across clusters.
    
- **Spark:** The "engine" used to process that data at lightning speed using the distributed computing mentioned above.
    
- **Kafka:** Used for **real-time** data. If a company wants to analyze data the second it happens (like a credit card transaction), the D.E. uses Kafka to "stream" that data through the system.
    
- **Hive:** Allows the D.E. to organize big data into a format that looks like a traditional database (SQL), making it easier for Data Scientists to use.
    

---

### Summary: The "System Builder"

As Slide 65 states, **"a D.E. builds systems that consolidate, store, and retrieve data."**

In the context of your **Deep Learning course**:  
A Deep Learning model (like a Transformer or a CNN) requires massive amounts of clean data to train. Without the **Data Engineer** using these skills to build the pipelines, the **Data Scientist** would have no data to feed into their models. The D.E. provides the "raw material" at scale.