# Technical Case Study

## Implementation scope

The codebase contains roughly 6.4k lines of Python across data generation, a Kafka producer and consumer, a PySpark processor, an anomaly detector built on rule checks and scikit-learn models, PostgreSQL access, and five Streamlit dashboard variants. A Docker Compose file, Makefile and CLI launchers orchestrate the full stack.

## Anomaly detection

The anomaly detector layers three things:

1. **Rule checks:** high amount, velocity, time of day, geography and merchant checks, each returning a result with a severity and a risk score.
2. **Machine learning models:** IsolationForest and DBSCAN models from scikit-learn, with feature extraction from the transaction and user statistics.
3. **Per-user profiles:** a profile tracker keeps running statistics per user that the rules and models compare against.

Results are combined into a single risk score, and flagged transactions produce alerts.

## Stream processing

Spark Structured Streaming reads transactions from Kafka. Each micro-batch is converted to pandas, run through the anomaly detector, and the results are prepared for the database. A separate query computes sliding-window aggregations by country and merchant category, and another monitors high-risk transactions.

## Dashboard variants

- **demo_dashboard_working.py:** self-contained, Faker-generated data. This is the one shown in the screenshots.
- **dashboard.py, enhanced_dashboard.py, professional_dashboard.py:** variants of the dashboard over the database.
- **demo_dashboard_seeded.py:** another standalone demo variant.

All use Streamlit and Plotly.

## Data generation

The demo generates fictional transactions with Faker and NumPy: merchants, categories, locations, payment methods, amounts with occasional outliers, and timestamps. Risk scores and flags are random in the demo.

## Trade-offs and limitations

- The full pipeline needs Docker (Kafka, Zookeeper, Spark, PostgreSQL) and was not run for this portfolio; only the demo dashboard was executed.
- The demo does not exercise the anomaly detector or the ML models.
- The original demo uses deprecated `st.experimental_rerun` and pandas 'H' frequency strings; a portfolio wrapper shims them for current library versions.
- Configuration lives in `.env` files that are excluded from this repository.

Continue with [engineering challenges](ENGINEERING_CHALLENGES.md).
