# Security Information and Event Management (SIEM) Lab

A self-built Security Information and Event Management (SIEM) lab designed to understand how a SIEM platform works from the ground up. Rather than deploying an existing SIEM solution, this project builds each component independently to gain practical experience with the complete security monitoring pipeline, including log collection, event processing, storage, detection, and visualization.

<p align="center">
  <img width="900" alt="SIEM Architecture" src="https://github.com/user-attachments/assets/bebc627a-7290-438b-9abd-b1cb01170119" />
</p>

## Components

| Component | Purpose |
|-----------|---------|
| Vector | Collects, parses, normalizes, and forwards logs |
| Apache Kafka | Message broker and buffering layer |
| ClickHouse | High-performance analytical database |
| Grafana | Dashboards and visualization |


<img width="2752" height="1536" alt="Modern_Real-Time_Log_Pipeline" src="https://github.com/user-attachments/assets/1ed2aa0f-1d9f-4da6-b6fc-f376a5e64396" />



## 🛠️ Technology Stack

This pipeline is built using lightweight, high-performance open-source tools to replace heavy traditional stacks (like ELK):

*   **Log Collector / Agent:** [Vector](https://vector.dev/) (Rust-based, extremely fast and memory-efficient).
*   **Message Broker:** [Apache Kafka](https://kafka.apache.org/) (Running in KRaft mode, acting as the shock-absorber for log spikes).
*   **Storage Engine:** [ClickHouse](https://clickhouse.com/) (Columnar database designed for lightning-fast OLAP and time-series queries).
*   **Visualization & Alerting:** [Grafana](https://grafana.com/) (SOC Dashboarding and Threat Detection tracking).

## 🎯 Key Detection Use Cases

This lab simulates real-world SOC monitoring scenarios. Current detection capabilities include:

*   **Authentication Anomalies:** Detecting SSH Brute-force attacks and abnormal Windows Logons (Event ID 4624/4625).
*   **Privilege Escalation:** Monitoring unauthorized `sudo` command executions and Admin group modifications (Event ID 4720/4672).
*   **Network Scanning & Threat Intel:** Tracking suspicious outbound connections and port scanning activities.
*   **System Integrity:** Alerting on critical service crashes or configuration file tampering.

## 📂 Repository Structure

The documentation is modularized to explain the exact configuration and purpose of each component:

```text
SIEM-Lab/
├── docker-compose.yml       # Core infrastructure deployment
├── installation.md      # Step-by-step setup guide
├── architecture.md      # Deep dive into the Decoupled Data Pipeline
├── vector.md            # Vector parsing rules and source configs
├── kafka.md             # Kafka KRaft mode and Topic structures
├── clickhouse.md        # Columnar schema and Materialized Views
├── grafana.md           # Dashboard configurations and Data Sources
├── detection-rules.md   # SIEM detection logic and Use Cases
├── dashboards.md        # Screenshots and explanations of SOC Panels
