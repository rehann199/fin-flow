<div align="center">

# FinFlow

**A real-time finance analytics dashboard for transaction monitoring and fraud detection, built around a Kafka, Spark and PostgreSQL streaming pipeline.**

**Web dashboard**<br/>
Python · Streamlit · Plotly · Kafka · PySpark · PostgreSQL · scikit-learn

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?logo=plotly&logoColor=white)
![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?logo=apachekafka&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?logo=apachespark&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

<table>
  <tr>
    <td><img src="assets/screenshots/finflow-dashboard-overview.webp" width="460" alt="FinFlow dashboard header with KPI cards, hourly volume chart and category donut chart" /></td>
    <td><img src="assets/screenshots/finflow-alerts-transactions.webp" width="460" alt="Recent alerts with severity colour bars next to recent transactions with status badges" /></td>
  </tr>
  <tr>
    <td><img src="assets/screenshots/finflow-geo-risk-charts.webp" width="460" alt="Bar chart of transaction volume by country and risk score histogram" /></td>
    <td><img src="assets/screenshots/finflow-data-explorer.webp" width="460" alt="Data Explorer with category filter, amount range and transaction table" /></td>
  </tr>
</table>
<sub>Screenshots are real captures of a local build running on fictional demo data.</sub>

</div>

---

## Important: what is real in this demo

The Kafka, Spark and PostgreSQL pipeline, including the anomaly detector, is in the codebase. The demo shown here does not run it. It runs a self-contained Streamlit dashboard on generated fictional data (Faker and NumPy), and its risk scores and flags are randomly generated.

## Product at a Glance

| | |
|---|---|
| **What it is** | A transaction monitoring dashboard with KPI cards, charts, alert cards and a filterable data explorer |
| **Platform** | Web (Streamlit) |
| **Designed pipeline** | Synthetic generator, Kafka, PySpark Structured Streaming, anomaly detector, PostgreSQL, Streamlit |
| **Detection (in code)** | Rule checks (amount, velocity, time, geography, merchant) plus IsolationForest and DBSCAN models and per-user profiles |
| **Demo** | Standalone dashboard on fictional generated data, with dataset size selector and refresh |
| **Export** | CSV and JSON download from the Data Explorer |
| **Orchestration** | Docker Compose, Makefile and CLI launchers |
| **Code size** | About 6.4k lines of Python |

## Product Showcase

<table>
  <tr>
    <td align="center"><img src="assets/screenshots/finflow-volume-category-charts.webp" width="440" alt="Hourly transaction volume line chart and category donut chart"/><br/><sub><b>Volume and categories</b><br/>Hourly volume and share by category</sub></td>
    <td align="center"><img src="assets/screenshots/finflow-geo-risk-charts.webp" width="440" alt="Volume by country bar chart and risk score histogram"/><br/><sub><b>Geography and risk</b><br/>Volume by country, risk score distribution</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="assets/screenshots/finflow-alerts-transactions.webp" width="440" alt="Recent alerts and recent transactions cards"/><br/><sub><b>Alerts and transactions</b><br/>Severity-coded alert cards, status badges</sub></td>
    <td align="center"><img src="assets/screenshots/finflow-flagged-only.webp" width="440" alt="Data Explorer filtered to flagged transactions only"/><br/><sub><b>Flagged only</b><br/>Explorer narrowed to flagged records</sub></td>
  </tr>
</table>

## Key Features

- **KPI cards.** Transactions, volume, flagged count, average amount and active alerts.
- **Charts.** Hourly volume line chart, category donut chart, country bar chart and risk score histogram, styled with a consistent palette.
- **Alerts and recent transactions.** Card lists with severity colours and status badges.
- **Data Explorer.** Category filter, amount range slider, flagged-only toggle, and CSV and JSON download.
- **Sidebar controls.** Refresh, dataset size selector and optional auto-refresh.
- **Detection design.** Rule checks, scikit-learn models and per-user profiles combined into one risk score (in the codebase, not exercised by the demo).

## Engineering Highlights

- **Streaming design.** Kafka feeds a Spark Structured Streaming job that scores each micro-batch with the anomaly detector and runs sliding-window aggregations by country and category.
- **Layered detection.** Five rule checks, IsolationForest and DBSCAN models and per-user profile statistics are combined into one risk score with alerts.
- **Stack orchestration.** Docker Compose defines PostgreSQL, Zookeeper, Kafka, Spark and the application containers, with a Makefile and CLI launchers.
- **Modern-library compatibility.** A small wrapper shims removed Streamlit and pandas APIs so the original demo runs on current versions without editing it.
- **Presentation layer.** The dashboard is styled through injected CSS and a Streamlit theme, with no changes to the dashboard's logic.

More: [docs/ENGINEERING_CHALLENGES.md](docs/ENGINEERING_CHALLENGES.md).

## Architecture

```mermaid
flowchart LR
    GEN[Generator] --> K[Kafka]
    K --> S[Spark Structured Streaming]
    S --> A[Anomaly detector<br/>rules + ML]
    A --> DB[(PostgreSQL)]
    DB --> UI[Streamlit dashboard]
```

Details: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Status & Scope

- **Demo mode.** The screenshots come from a standalone dashboard on generated fictional data. No real transactions or people are shown.
- **Real code, not run here.** The Kafka producer and consumer, Spark processor, anomaly detector and PostgreSQL layer are in the codebase. The full stack needs Docker and was not run for this portfolio.
- **Mock in the demo.** Risk scores, flags and alerts are randomly generated rather than produced by the detector.
- **Compatibility wrapper.** The demo runs through a small wrapper for current Streamlit and pandas versions.
- **Source.** Production source is private; this repository holds documentation and screenshots only.

## Documentation

- [Product overview](docs/PRODUCT_OVERVIEW.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Technical case study](docs/TECHNICAL_CASE_STUDY.md)
- [Engineering challenges](docs/ENGINEERING_CHALLENGES.md)
- [Privacy and source](docs/PRIVACY_AND_SOURCE.md)

## Rights

See [RIGHTS.md](RIGHTS.md).
