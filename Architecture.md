## Architecture

The architecture of this project is designed to separate log collection, data transportation, storage, and visualization into independent components. This approach improves scalability, simplifies maintenance, and allows each component to perform its own responsibility without being tightly coupled.

Instead of sending logs directly to the database, Vector forwards events to Apache Kafka, which acts as a message broker and buffering layer. Kafka decouples log producers from ClickHouse, preventing storage bottlenecks during traffic spikes while providing reliable event delivery. ClickHouse consumes logs from Kafka at its own processing rate and stores them for efficient analytical queries. Finally, Grafana connects to ClickHouse to visualize security metrics, dashboards, and detection results.

<img width="2752" height="1536" alt="Modern_Real-Time_Log_Pipeline" src="https://github.com/user-attachments/assets/a93df945-4154-420d-98ee-8eff7dcdba78" />
