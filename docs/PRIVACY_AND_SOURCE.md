# Privacy and Source

## Source code status

The production source code is private and not included in this portfolio. This repository contains analysis, documentation, and a demonstration of the dashboard.

The demo dashboard (`src/demo_dashboard_working.py`) is provided as a self-contained example. It generates synthetic transaction data on-the-fly using the Faker library and Streamlit, so no external services are required. The full pipeline (Kafka, Spark, PostgreSQL) is in the codebase but was not executed for this portfolio.

## Data in this repository

All screenshots and visualizations use fictional, demo-generated data. No real transactions, user identities, amounts, or locations are shown. The data generator uses seeded random values to produce realistic-looking transaction records with:

- Merchant names drawn from a list (fictional transactions, no real payments)
- Locations assigned at random, so city and country combinations are not meaningful
- Synthetic amounts with occasional outliers
- Randomly generated risk scores and flags

Transaction records are transient (generated at runtime) and not persisted.

## Configuration security

Environment-specific configuration (such as service addresses and connection settings) is stored in `.env` files and excluded from version control. This repository does not contain:

- Database credentials
- API keys or tokens
- Webhook URLs or endpoints
- Kafka broker addresses
- Private S3 buckets or cloud storage paths

## Third-party assets and licenses

The project uses open-source libraries:

- **Streamlit:** Apache 2.0 – UI framework
- **Plotly:** MIT – Interactive charting
- **PySpark:** Apache 2.0 – Distributed stream processing
- **pandas, NumPy, scikit-learn:** BSD 3-Clause – Data manipulation and ML
- **kafka-python, confluent-kafka:** Apache 2.0 – Kafka clients
- **Faker:** MIT – Synthetic data generation
- **PostgreSQL, SQLAlchemy, psycopg2:** BSD/PostgreSQL license – Database
- **Docker Compose:** Apache 2.0 – Container orchestration

All libraries are used in accordance with their licenses. No proprietary assets are included.

## Scope

This portfolio demonstrates technical architecture and engineering practices on fictional data. It makes no claims about regulatory compliance or production hardening.
