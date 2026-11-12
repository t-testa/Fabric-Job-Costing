# Fabric Job Costing

A work-in-progress data foundation for job and operations analytics, with Parquet data, synthetic fixture generation and an in-memory DuckDB loader.

## Current state

The repository contains useful starting material, but its committed data and Python loader describe **different schemas**:

- `data/silver/` contains `dim_change_order`, `dim_cost_type`, `dim_date`, `dim_employee`, `dim_job`, `fact_job_budget` and `fact_job_cost` Parquet files.
- `scripts/load_duckdb.py` expects five files named `silver_customers`, `silver_jobs`, `silver_expenses`, `silver_invoices` and `silver_targets`.
- `scripts/generate_fixtures.py` generates the five files expected by the loader, using synthetic operations data.

The loader therefore cannot load the seven committed job-costing files directly. These schemas need reconciliation before this is presented as an end-to-end job-costing project.

## Run the synthetic operations fixture path

From the repository root:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python scripts/generate_fixtures.py --out data/generated_operations
python scripts/load_duckdb.py --data data/generated_operations
```

A separate output folder keeps the generated operations fixtures distinct from the committed job-costing data. The loader creates in-memory DuckDB views and prints row counts. The generator uses seed 42 and a fixed reference date of 2025-12-31; these fixtures do not represent real business results.

## Documentation and model authority

- [Data dictionary](docs/data-dictionary.md): a generated artifact, **not the source of truth**.
- [Decisions](docs/decisions.md): design decisions and recorded project instructions.

The intended model source of truth is TMDL, but no TMDL files are committed in this snapshot. Relationships described by the dictionary cannot be confirmed against that missing model; they should not be inferred or silently added.

## Remaining work

Confirm the intended schema, recover the model definitions, align the generator and loader, and add validation and report artifacts. No complete Fabric deployment, Power BI report or application is included here.
