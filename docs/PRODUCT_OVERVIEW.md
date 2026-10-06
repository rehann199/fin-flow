# Product Overview

FinFlow is a real-time fraud and anomaly detection dashboard for financial transactions. It monitors streams of payment data to identify suspicious activity and highlight high-risk transactions in a live analytics interface.

## What it is

FinFlow is a fraud and anomaly monitoring dashboard for financial transactions. The codebase is designed as a pipeline: a synthetic transaction generator, an Apache Kafka producer, PySpark Structured Streaming, an anomaly detector (rule checks plus scikit-learn models), PostgreSQL storage and a Streamlit dashboard. The full pipeline is in the codebase but was not run for this portfolio.

The screenshots show the self-contained demo dashboard, which runs on generated fictional data.

## Key views in the demo

- **KPI cards.** Transactions, volume, flagged transactions, average amount and active alerts.
- **Charts.** Hourly volume line chart, category donut chart, country bar chart and risk score histogram.
- **Alerts and transactions.** Card lists of recent alerts and recent transactions with a risk badge.
- **Data Explorer.** Table with category and amount filters, a flagged-only toggle, and CSV and JSON download.
- **Sidebar.** Refresh button, dataset size selector and optional auto-refresh.

## Detection design (in the codebase)

The anomaly detector includes rule checks for high amount, velocity, time of day, geography and merchant, IsolationForest and DBSCAN models, and per-user profiles. These are not exercised by the demo.

## Demo mode

A self-contained demonstration dashboard runs on seed-generated fictional transaction data without requiring Docker or external services. Risk scores and flags are randomly generated, so the demo shows the interface and charts but not the detector.

Continue with the [technical case study](TECHNICAL_CASE_STUDY.md).
