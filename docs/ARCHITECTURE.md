# Architecture

All diagrams are original and describe system responsibilities and data flow as designed in the codebase. The demo shown in the screenshots runs only the Streamlit dashboard on generated fictional data; the Kafka, Spark and PostgreSQL pipeline below is in the codebase but was not run for this portfolio.

## System overview

```mermaid
flowchart LR
    GEN[Data Generator]
    PROD[Kafka Producer]
    KAFKA[Kafka Topics]
    SPARK[PySpark<br/>Structured Streaming]
    ANOM[Anomaly Detector]
    DB[(PostgreSQL)]
    DASH[Streamlit Dashboard]

    GEN -->|synthetic txns| PROD
    PROD -->|events| KAFKA
    KAFKA -->|stream| SPARK
    SPARK -->|micro-batches| ANOM
    ANOM -->|risk scores, alerts| DB
    DB -->|read analytics| DASH
```

Synthetic transactions are published to Kafka, read by a Spark Structured Streaming job, scored per micro-batch by the anomaly detector, written to PostgreSQL and visualised in Streamlit.

## Processing pipeline

```mermaid
flowchart TD
    TXN["Transaction event"]
    KAFKA["Kafka topic"]
    BATCH["Spark micro-batch<br/>(foreachBatch)"]
    RULES["Rule checks<br/>high amount, velocity, time of day,<br/>geography, merchant"]
    ML["ML models<br/>IsolationForest, DBSCAN"]
    SCORE["Combined risk score<br/>flagged or clear + alerts"]
    STORE["PostgreSQL"]
    WIN["Windowed aggregations<br/>sliding window by country and category"]

    TXN --> KAFKA
    KAFKA --> BATCH
    BATCH --> RULES
    BATCH --> ML
    RULES --> SCORE
    ML --> SCORE
    SCORE --> STORE
    KAFKA --> WIN
```

Each micro-batch is converted to pandas and passed through the anomaly detector, which combines rule-based checks with scikit-learn models and per-user profiles. A separate streaming query computes sliding-window aggregations (counts, totals, averages, flagged counts) by country and merchant category.

## Deployment model

The full system is described by a Docker Compose file covering PostgreSQL, Zookeeper, Kafka, a Kafka UI, Spark master and worker containers, and the application containers. A Makefile and CLI launchers wrap the common commands. The demo dashboard runs as a standalone Python script that needs only Streamlit, Plotly, pandas, NumPy and Faker.
