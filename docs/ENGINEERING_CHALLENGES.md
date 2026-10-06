# Engineering Challenges

Descriptions of the design problems in the codebase and the approach taken. The streaming pipeline was not run for this portfolio; only the demo dashboard was.

## Scoring each transaction without a single heavy step

**Challenge:** Fraud checks combine cheap rule checks with heavier scikit-learn models (IsolationForest, DBSCAN), and all of it must run on a stream of events.

**Approach:** Spark Structured Streaming reads the Kafka topic and hands each micro-batch to the anomaly detector. The detector layers rule checks (high amount, velocity, time of day, geography, merchant) and model-based detection, then combines the results into one risk score and a set of alerts per transaction.

**Trade-off:** Each micro-batch is converted to pandas and scored in a loop, which is simple but does not distribute model inference across Spark executors.

## Windowed aggregations

**Challenge:** Dashboards need rolling volumes by country and category, not just per-transaction results.

**Approach:** A separate streaming query groups events in a sliding window by country and merchant category, with a watermark, and computes counts, totals, averages, maximums and flagged counts.

**Trade-off:** In the code the aggregation query writes to the console rather than the database, so it is a starting point for the analytics tables rather than a finished path.

## Handling deprecated Streamlit and pandas APIs

**Challenge:** The demo dashboard uses `st.experimental_rerun` (removed from current Streamlit) and pandas 'H' frequency strings (removed in pandas 3).

**Approach:** A small wrapper, written for this portfolio, shims `st.experimental_rerun` to `st.rerun`, patches the frequency strings and fixes the random seed, then runs the original demo script unchanged.

**Trade-off:** The wrapper is specific to these breaking changes; later deprecations would need new patches.

## Keeping the dashboard responsive on reruns

**Challenge:** Streamlit reruns the whole script on every interaction, so expensive work must not sit in the main flow.

**Approach:** In the demo, data generation is cached for a short time and the sidebar controls (dataset size, refresh, auto-refresh) drive it. In the full design, scoring happens in the streaming job and the dashboard only reads results.

## Synthetic data without real payment details

**Challenge:** Fraud detection needs realistic-looking transactions, but real payment data should not be used.

**Approach:** The Faker library and NumPy generate fictional merchants, locations, amounts and timestamps, with a share of flagged outliers. No real transactions are used.

**Trade-off:** Synthetic data cannot represent all adversarial behaviour. In the demo, risk scores and flags are randomly generated and do not come from the detector.

## Multiple dashboard variants

**Challenge:** The repository contains five dashboard variants (a standard dashboard, an enhanced one, a professional one and two standalone demos) with overlapping logic.

**Trade-off:** Maintaining several variants duplicates code; consolidating them would be the next cleanup.

Continue with [privacy and source](PRIVACY_AND_SOURCE.md).
