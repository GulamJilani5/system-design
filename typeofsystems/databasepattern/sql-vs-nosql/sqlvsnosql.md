🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕ 🔵 🟦 ➡️ ⏺️

# ️ ️➡️SQL vs NoSQL Terminology

| SQL (MySQL/PostgreSQL)    | NoSQL (MongoDB)                                      |
| ------------------------- | ---------------------------------------------------- |
| `CustomerDB` (Database)   | `CustomerDB` (Database)                              |
| `Users` (Table)           | `Users` (Collection)                                 |
| `id, name, age` (Columns) | `"_id", "name", "age"` (Fields)                      |
| `1, 'John', 30` (Row)     | `{ "_id": 1, "name": "John", "age": 30 }` (Document) |

# ➡️SQL and NoSQL Databases

| **Feature**        | **SQL (Relational DB)**                                            | **NoSQL (Non-Relational DB)**                               |
| ------------------ | ------------------------------------------------------------------ | ----------------------------------------------------------- |
| **Data Model**     | Tables with rows and columns                                       | Document, Key-Value, Wide-Column, or Graph                  |
| **Schema**         | Fixed schema, predefined before use, altering structure is costly. | Dynamic schema, flexible structure                          |
| **Joins**          | Support Joins                                                      | Avoid Joins(Uses denormalized data and embedded documents.) |
| **Transactions**   | Full ACID compliance                                               | Eventual consistency; some support ACID at document level   |
| **Scalability**    | Vertically scalable (scale up)                                     | Horizontally scalable (scale out)                           |
| **Use Cases**      | Complex queries, structured data (e.g., ERP, Banking)              | Big Data, real-time analytics, IoT, content management      |
| **Query Language** | Structured Query Language (SQL)                                    | Varies by type; e.g., JSON-like queries in MongoDB          |
| **Examples**       | MySQL, PostgreSQL, Oracle, SQL Server                              | MongoDB, Cassandra, Redis, Couchbase, Neo4j                 |

# ➡️ SQL and NoSQL Databases(In Simple words)

| Feature             | SQL                                                              | NoSQL                                                                          |
| ------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Schema**          | Fixed schema (altering structure is costly)                      | Dynamic schema (fields can be added on the fly)                                |
| **Joins**           | Supports joins (Foreign Keys)                                    | Avoid joins (uses denormalized data or embedded documents)                     |
| **Model**           | Follows **ACID** (Atomicity, Consistency, Isolation, Durability) | Follows **BASE** (Basically Available, Soft state, Eventually Consistent)      |
| **Scalability**     | Vertical scaling (increase CPU/RAM of single server)             | Horizontal scaling (distribute data across multiple servers)                   |
| **Best suited for** | Complex queries, transactions (e.g., **Banking, ERP**)           | Unstructured data, high-speed read/writes (e.g., **IoT, Real-time analytics**) |
| **Updates**         | Optimized for transactions and consistency                       | Most built for updates and high availability                                   |
