# System Architecture

## Overview

The system is designed to provide a scalable and reliable centralized log management pipeline for Windows Event Logs. It leverages Vector for log collection, Apache Kafka as a distributed message broker and buffering layer, ClickHouse for high-performance log storage and analytics, and Grafana for real-time visualization.

```
Windows Endpoints
        │
        ▼
     Vector Agent
        │
        ▼
   Apache Kafka
(Buffer & Message Queue)
        │
        ▼
    ClickHouse
(Log Storage & Analytics)
        │
        ▼
      Grafana
 (Visualization Dashboard)
```

---
The architecture of this project is designed to separate log collection, data transportation, storage, and visualization into independent components. This approach improves scalability, simplifies maintenance, and allows each component to perform its own responsibility without being tightly coupled.

Instead of sending logs directly to the database, Vector forwards events to Apache Kafka, which acts as a message broker and buffering layer. Kafka decouples log producers from ClickHouse, preventing storage bottlenecks during traffic spikes while providing reliable event delivery. ClickHouse consumes logs from Kafka at its own processing rate and stores them for efficient analytical queries. Finally, Grafana connects to ClickHouse to visualize security metrics, dashboards, and detection results.

<img width="2752" height="1536" alt="Modern_Real-Time_Log_Pipeline" src="https://github.com/user-attachments/assets/a93df945-4154-420d-98ee-8eff7dcdba78" />

## Components

### 1. Vector

Vector is deployed on Windows endpoints to collect Windows Event Logs from the operating system. It acts as the log collection agent responsible for:

- Reading Windows Event Logs in real time.
- Normalizing log records into a structured format.
- Forwarding logs to Kafka with minimal resource consumption.
- Providing reliable delivery through batching and retry mechanisms.

Using Vector as the collector reduces CPU and memory overhead while supporting high-throughput log ingestion.

---

### 2. Apache Kafka

Kafka serves as the central messaging and buffering layer between log producers and consumers.

Its responsibilities include:

- Decoupling log collection from storage.
- Acting as a durable buffer during ClickHouse downtime.
- Preventing log loss during traffic spikes.
- Supporting horizontal scalability by partitioning topics.
- Enabling asynchronous processing of incoming events.

Instead of sending logs directly to the database, Vector publishes logs to Kafka topics where they remain available until consumed.

---

### 3. ClickHouse

ClickHouse is responsible for long-term storage and analytical querying of security logs.

Key capabilities include:

- High-speed ingestion from Kafka.
- Column-oriented storage optimized for analytical workloads.
- Efficient querying across millions of log records.
- Support for time-series analysis and security investigations.

Kafka Engine tables consume messages from Kafka and insert them into MergeTree tables for permanent storage.

---

### 4. Grafana

Grafana provides visualization and monitoring capabilities by querying ClickHouse directly.

Dashboards can display:

- Log volume over time.
- Authentication events.
- Failed login attempts.
- Process creation events.
- Security event distribution.
- Host activity statistics.

This enables analysts to monitor Windows events in real time and investigate security incidents efficiently.

---

## Data Flow

1. Windows Event Logs are generated on Windows endpoints.
2. Vector continuously collects the events.
3. Vector publishes logs to Apache Kafka.
4. Kafka temporarily buffers and distributes the log stream.
5. ClickHouse consumes logs from Kafka and stores them permanently.
6. Grafana queries ClickHouse to visualize security events through interactive dashboards.

---

## Benefits

- Lightweight log collection with Vector.
- Reliable buffering using Apache Kafka.
- High-performance analytical storage with ClickHouse.
- Real-time dashboards through Grafana.
- Scalable architecture suitable for Security Operations Center (SOC) environments.
