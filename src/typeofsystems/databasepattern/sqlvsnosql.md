# SQL vs NoSQL Terminology
| SQL (MySQL/PostgreSQL)     | NoSQL (MongoDB)                                |
|----------------------------|-------------------------------------------------|
| `CustomerDB` (Database)    | `CustomerDB` (Database)                         |
| `Users` (Table)            | `Users` (Collection)                            |
| `id, name, age` (Columns)  | `"_id", "name", "age"` (Fields)                 |
| `1, 'John', 30` (Row)      | `{ "_id": 1, "name": "John", "age": 30 }` (Document) |


# SQL and NoSQL Databases

| **Feature**        | **SQL (Relational DB)**                                            | **NoSQL (Non-Relational DB) **                                  |
|--------------------|--------------------------------------------------------------------|-------------------------------------------------------------|
| **Data Model**     | Tables with rows and columns                                       | Document, Key-Value, Wide-Column, or Graph                  |
| **Schema**         | Fixed schema, predefined before use, altering structure is costly. | Dynamic schema, flexible structure                          |
| **Joins**          | Support                                                            | Avoid Joins(Uses denormalized data and embedded documents.) |
| **Transactions**   | Full ACID compliance                                               | Eventual consistency; some support ACID at document level   |
| **Scalability**    | Vertically scalable (scale up)                                     | Horizontally scalable (scale out)                           |
| **Use Cases**      | Complex queries, structured data (e.g., ERP, Banking)              | Big Data, real-time analytics, IoT, content management      |
| **Query Language** | Structured Query Language (SQL)                                    | Varies by type; e.g., JSON-like queries in MongoDB          |
| **Examples**       | MySQL, PostgreSQL, Oracle, SQL Server                              | MongoDB, Cassandra, Redis, Couchbase, Neo4j                 |


