# 5. Data Preparation

We created a shared Databricks workspace and uploaded our data to a Unity Catalog Volume:

`/Volumes/workspace/default/farewise_data/`

The data is organized into the following directories:

- `taxi/` — Taxi datasets.
- `weather/` — Weather datasets.
- `zones/` — Taxi zones GeoJSON dataset.
- `processed/` and `processed_final/` — Directories for saving the processed taxi–weather dataset.

## Work Completed So Far

- Uploaded the complete taxi CSV, weather CSV, and taxi zones GeoJSON files.
- Loaded all three file formats using PySpark and inspected their structures.
- Examined record counts, missing values, and unusual taxi values.
- Converted taxi features to numeric values and created pickup timestamps.
- Matched taxi trips with weather records by pickup hour.
- Created a cleaned taxi–weather dataset and saved it in Parquet format.
- Read the GeoJSON file, extracted its features, and counted **263 zones**.
