# Apache Kafka Architecture

## What is Apache Kafka?

- Apache Kafka is a **distributed event streaming platform** used for high-throughput, low-latency data processing. It's widely used to build real-time data pipelines and streaming applications.

---

## Kafka Architecture Overview

Kafka follows a **distributed architecture**:

```
Producers --> Kafka Cluster (Brokers + Zookeeper) --> Consumers
```

### Kafka Cluster

* A **Kafka Cluster** is a group of Kafka brokers (servers) working together.
* It is a **running instance** of Kafka's architecture.
* Usually, you run one cluster per environment (e.g., dev, staging, prod).

> Kafka is a distributed system, but not globally distributed by default.

---

## Key Kafka Components (Terms)

### 1. Broker

* A **Kafka broker** is a server that stores and manages messages.
* Brokers receive messages from producers and serve them to consumers.
* Each broker manages partitions and is identified by a unique ID.
**Example:** If you have 3 brokers, a topic with 6 partitions can be distributed across them.

### 2. Topic

* A **topic** is a part of Broker.
* A **topic** is a named stream of records/messages.
* Producers publish to topics and consumers subscribe to them.
**Example:** A topic called user-signups might store messages about new user registrations.

### 3. Partition

* Topics are split into **partitions** to allow parallelism and scalability.
* Each partition is an **ordered, immutable sequence** of records.
**Example:** user-signups might have 3 partitions to balance load and increase throughput.

### 4. Offset

* A unique identifier for each message within a partition.
* Offsets are used by consumers to track their reading position.

### 5. Producer

* A **producer** sends messages to Kafka topics.
* Producers can send messages to specific partitions or let Kafka decide.

### 6. Consumer

* A **consumer** reads messages from a topic.
* Consumers use offsets to keep track of consumed messages.

### 7. Consumer Group

* A **group of consumers** sharing the load of reading from a topic.
* Each partition is read by only one consumer in the group at a time.

### 8. Zookeeper

* Used to **manage and coordinate** Kafka brokers.
* Handles metadata like broker info, leader election, and cluster configs.
* Kafka is moving away from Zookeeper (replaced by KRaft in newer versions).

### 9. Controller

* A broker that acts as the **controller** for the cluster.
* Responsible for leader election and broker coordination.

### 10. Replication

* Kafka supports **replication** of partitions across brokers.
* Ensures high availability and fault tolerance.
* Each partition has a leader and one or more followers.

### 11. Retention

* Kafka retains messages for a **configurable period** (e.g., 7 days).
* Consumers can read messages at their own pace.

---

### Kafka Architecture Diagram
- Refer to the following diagram for visual understanding of the Kafka Architecture:  
[Kafka-Architecture.png](https://github.com/yogeshmangal/Java-Interview-Notes/blob/main/Kafka-Architecture.png)

---

## Kafka: Distributed vs Globally Distributed

### Is Kafka Distributed?

✅ Yes. Kafka is designed as a distributed system across multiple brokers.

### Is Kafka Globally Distributed?

🚫 Not by default. Kafka isn't meant to have brokers spread across the globe in a single cluster.

**Why not?**

* Global latency issues
* Partition leadership coordination problems
* Risk of data inconsistency

---

## Summary Table

| Term           | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| Kafka Cluster  | Group of Kafka brokers working together                                 |
| Broker         | Kafka server/node managing topics and partitions                        |
| Topic          | Named data stream to which messages are published                       |
| Partition      | Subdivision of topic for parallel processing                            |
| Offset         | Unique ID for each message in a partition                               |
| Producer       | Sends data to Kafka                                                     |
| Consumer       | Reads data from Kafka                                                   |
| Consumer Group | Set of consumers sharing work of consuming messages                     |
| Zookeeper      | Coordinates brokers and manages metadata (phased out in newer versions) |
| Controller     | Special broker managing leadership and failovers                        |
| Replication    | Redundant copies of partitions for high availability                    |
| Retention      | Duration Kafka retains messages                                         |

---

## Final Notes

* Kafka is optimized for **throughput, durability, and fault tolerance**.
* Ideal for building **event-driven systems** and **streaming data pipelines**.
* For global scale, always consider **multi-cluster architectures**.

---
