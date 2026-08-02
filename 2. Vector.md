## Why Vector?

Several popular log collection tools were evaluated before selecting **Vector** as the primary log collector for this project, including **Logstash**, **Fluentd**, **Fluent Bit**, and **Filebeat**.

### Comparison

| Tool | Language | Advantages | Limitations |
|------|----------|------------|-------------|
| **Vector** | Rust | High performance, low memory usage, built-in reliability, efficient buffering, excellent throughput | Smaller plugin ecosystem compared to Logstash and Fluentd |
| **Logstash** | Java | Rich plugin ecosystem, powerful data transformation capabilities | High CPU and memory consumption, slower startup |
| **Filebeat** | Go | Lightweight, efficient log shipping, simple deployment | Limited transformation capabilities |
| **Fluent Bit** | C | Extremely lightweight, low resource usage, ideal for edge devices and containers | Less flexible for complex log processing |
| **Fluentd** | Ruby | Highly extensible with a large plugin ecosystem, suitable for complex pipelines | Higher memory consumption and lower performance than Vector and Fluent Bit |

### Why Vector Was Chosen

Vector was selected because it provides an excellent balance between **performance**, **resource efficiency**, and **reliability**.

Its implementation in **Rust** enables low CPU and memory usage while ensuring memory safety and high throughput. Compared with Logstash and Fluentd, Vector consumes significantly fewer system resources. Unlike Filebeat and Fluent Bit, it also provides more powerful data transformation capabilities without introducing substantial overhead.

For this project, Vector continuously collects Windows Event Logs and forwards them to Apache Kafka with minimal latency. Kafka then acts as a durable buffering layer before ClickHouse stores the logs for analysis and Grafana visualizes them through real-time dashboards.

Overall, Vector is well suited for centralized log collection pipelines where performance, scalability, and reliability are key requirements.



## Installing Vector

Before collecting Windows Event Logs, Vector must be installed on the machine where log collection will take place.

Vector supports multiple operating systems, including Windows and Linux.

### Download

Install Vector by following the official installation guide:

https://vector.dev/docs/setup/installation/operating-systems/

Choose the appropriate installation package for your operating system:

- Windows
- Linux
- macOS (optional)

After installation, verify that Vector has been installed successfully.

---

## Configuring Vector

Vector uses a YAML configuration file (`vector.yaml`) to define how logs are collected, processed, and forwarded.

In this project, the configuration consists of three main sections:

- **Sources** – Defines where Vector reads data from. In this project, the source is the Windows Event Log.
- **Transforms** – (Optional) Parses, enriches, or filters log events before forwarding them.
- **Sinks** – Specifies the destination where logs are sent. Here, Apache Kafka is used as the output.

The overall data flow is illustrated below:

```
Windows Event Log
        │
        ▼
     Source
        │
        ▼
   Transform (Optional)
        │
        ▼
   Kafka Sink
```

---

## Configuration File

Create a file named `vector.yaml` and place it in the Vector configuration directory.

The configuration file defines:

- Which Windows Event Log channels are monitored.
- How frequently Vector polls for new events.
- Kafka broker addresses.
- Kafka topic name.
- Message encoding format.
- Buffering and retry behavior.

An example configuration is provided below.

<img width="853" height="460" alt="image" src="https://github.com/user-attachments/assets/6489d87c-1853-4a82-8748-a4100c9a61f6" />

You can customize it further by adjusting sources, transforms, sinks, buffering, filtering, and other settings according to your requirements. See the official Vector Configuration Reference for more details:

https://vector.dev/docs/reference/configuration/
