🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

## ⏺️ Kafka Architecture (Top → Bottom)

`A Kafka cluster contains many brokers,
each broker holds multiple topics,
each topic has multiple partitions,
each partition stores the actual messages in order with unique offset IDs.`

### ➡️ Cluster

- A Kafka cluster is made up of multiple brokers (servers).
- All brokers work together to store and manage the data.

### ➡️ Broker

- Each broker can store multiple topics.
- Think of each broker as a server with several folders (topics) inside.

### ➡️ Topic

- Each topic is divided into multiple partitions.
- This allows parallel processing and better scalability.

### ➡️ Partition

- A partition is where the actual messages are stored.
- Messages are written in order, like entries in a log file.
- Each partition belongs to exactly one topic.

### ➡️ Offset

- Inside each partition, every message gets a unique offset ID.
- This ID represents the position of the message within that partition.
- Consumers use offsets to track what they’ve already read.

### ➡️ Additional Concepts

##### 🟦 Prodcuers

##### 🟦 Consumers

- They read messages from the kafka topics.
- They subscribe to one or more topics and consume messages by reading from specific partitions withing those topics.
- Each consumer maintains its offset to track its progress in the topics.

##### 🟦 Replication:

Partitions are typically replicated across multiple brokers for fault tolerance. One broker acts as the leader for a partition, handling reads and writes, while other brokers hold replicas (followers) that sync with the leader.

##### 🟦 Consumer Groups:

Consumers read messages from partitions, and each consumer in a consumer group is assigned specific partitions. Offsets are used to track which messages a consumer has processed.

##### 🟦 Data Retention:

Messages in partitions are retained based on a configurable retention policy (e.g., time-based or size-based), after which they are deleted.
