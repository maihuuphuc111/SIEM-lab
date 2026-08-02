# Apache Kafka

## Why Kafka used?

Imagine an enterprise environment with hundreds or even thousands of Windows endpoints continuously generating security events. Every login, process creation, PowerShell execution, and network activity is collected by Vector and sent to the central logging system.

If Vector forwards logs directly to ClickHouse, the database becomes a single point of failure. During maintenance, unexpected downtime, or periods of high load, ClickHouse may be temporarily unavailable. Without an intermediate layer, incoming logs could be delayed or even lost.

To address this challenge, **Apache Kafka** is introduced between Vector and ClickHouse.

Kafka acts as a **durable message queue** and **buffering layer**, temporarily storing incoming log events until ClickHouse is ready to consume them. This architecture decouples log collection from log storage, allowing each component to operate independently while ensuring reliable log delivery.

```
Windows Endpoints
        │
        ▼
      Vector
        │
        ▼
  Apache Kafka
(Buffer & Message Queue)
        │
        ▼
   ClickHouse
        │
        ▼
     Grafana
```

By introducing Kafka into the pipeline, the system gains several important benefits:

- **Reliable buffering** – Log events are retained even if ClickHouse is temporarily unavailable.
- **Decoupled architecture** – Vector and ClickHouse operate independently without directly depending on each other.
- **High-throughput ingestion** – Kafka efficiently handles large volumes of incoming events from hundreds or thousands of endpoints.
- **Scalability** – Topics can be partitioned to support multiple consumers and increased workloads.
- **Fault tolerance** – Messages remain available until they are successfully consumed, reducing the risk of data loss.

This design makes the logging pipeline more resilient, scalable, and suitable for Security Operations Center (SOC) environments where continuous log collection is critical.


## Installing Kafka

For this project, **Docker** is the recommended approach because it provides a quick, reproducible, and portable deployment.

<img width="449" height="215" alt="image" src="https://github.com/user-attachments/assets/48cf22ee-1729-4a00-9474-85142ae38f3e" />


---

### Topic

A **Topic** is a logical channel where messages/logs are published and consumed.

In this project, **Vector** continuously collects Windows Event Logs and publishes them to the Kafka topic named:

```text
windows-events-raw-topic
```

Therefore, the Kafka topic **must be created with the exact same name**. Otherwise, Vector will not be able to publish log events successfully, and ClickHouse will not be able to consume them.

The data flow is illustrated below:

```text
Windows Event Logs
        │
        ▼
      Vector
        │
        ▼
windows-events-raw-topic
        │
        ▼
      Kafka Broker
```



## Verifying Kafka Messages

After configuring the Kafka broker address and topic in `vector.yaml`, start the Vector service.

Vector performs a health check on its configured sink during startup. If the configuration is correct and the Kafka broker is reachable, the health check will succeed, indicating that Vector has successfully established a connection to Kafka.

<img width="1280" height="719" alt="Screenshot 2026-08-01 154352" src="https://github.com/user-attachments/assets/973977c8-4976-4c60-9115-974579b3c280" />

```text
Windows Event Logs
        │
        ▼
      Vector
 (Health Check ✓)
        │
        ▼
+---------------------------+
|       Kafka Broker        |
|                           |
|  Topic:                   |
|  windows-events-raw-topic |
+---------------------------+
        │
        ▼
 Ready for ClickHouse
```



