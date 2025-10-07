# ⏺️ 3.2. Apache Kafka

- real-time data streaming, handle high volume of data
- Scalable data processing
- high throughput
- Fault Tolerant
- Disk-Based Storage:
  - Uses sequential I/O on disks (not random access), which is efficient for large volumes.

## ➡️ Kafka features

### 🟦 Real-Time Data Streaming

- Kafka allows you to process and transfer data as soon as it is generated, instead of waiting for batch jobs

###### How Kafka achieves it:

- Producers send messages immediately to Kafka topics.
- Consumers can read and process them instantly.
- Latency is in milliseconds.
- **Example:**
  - When you book a ride on Uber, Kafka streams your location data in real time so drivers and customers see updates instantly.
- **Interview Answer (short):**
  - Kafka supports real-time data streaming because producers can continuously send data and consumers can process it right away with very low latency.

### 🟦 Scalable Data Processing

- Kafka can easily handle growing workloads by adding more brokers and partitions.

##### How Kafka achieves it:

- Partitions divide a topic into smaller chunks.
- Producers and consumers can work on different partitions in parallel.
- Adding new brokers distributes the partitions across more servers.
- **Example:**
  If an e-commerce app goes from handling 10K orders/day to 1M orders/day, Kafka can scale by adding brokers and partitions—without changing the application code.
- **Interview Answer (short):**
  Kafka is scalable because topics are partitioned and can be spread across multiple brokers, allowing parallel processing and easy horizontal scaling.

### 🟦 High Throughput

- Kafka can handle millions of messages per second with low latency.

##### How Kafka achieves it:

- Sequential disk writes (fast compared to random writes).
- Batching messages to reduce overhead.
- Zero-copy technology to speed up data transfer between disk and network.
- Distributed architecture for parallelism.

- **Example:**
  - A payment gateway using Kafka can handle thousands of transactions per second reliably without delays.
- **Interview Answer (short):**
  - Kafka achieves high throughput through partitioning, batching, sequential disk writes, and zero-copy transfer, making it capable of handling millions of messages per second.

## 🟦 Fault Tolerant

- Kafka is fault tolerant because it replicates partitions across brokers and automatically elects a new leader if one broker fails

### How Kafka achieves it:

- Each partition’s data is replicated across multiple brokers.
- One broker is the leader, others are followers.
- If the leader fails, a follower becomes the new leader automatically.

- **Example:**

  - If you have 3 replicas of a topic and one broker crashes, the other two brokers still have the data, so no messages are lost.

- **Interview Answer (short):**
  - Kafka is fault tolerant because it replicates partitions across brokers and automatically elects a new leader if one broker fails
