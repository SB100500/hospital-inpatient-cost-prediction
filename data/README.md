# 📁 Data

This folder contains the raw dataset used for the [Hospital Inpatient Cost Prediction](../README.md) project.

## File

| File | Rows | Columns | Size context |
|---|---:|---:|---|
| `Hospital_Inpatient_Discharges__SPARCS_De-Identified__2009.csv` | 32,033 | 34 | De-identified inpatient discharge records |

## Source

- **Dataset:** SPARCS Hospital Inpatient Discharges
- **Publisher:** New York State Department of Health
- **Portal:** [health.data.ny.gov](https://health.data.ny.gov/)
- **License:** Public open data (NY State Open Data)
- **Identifiability:** De-identified — no patient names, MRNs, or direct identifiers are present

> ⚠️ Due to file size and/or licensing terms, the raw CSV may not be committed directly to this repo. If it isn't present here, download it from the source link above and place it in this folder before running the notebook.

## Target Variable

| Column | Description |
|---|---|
| `Total Charges` | Total billed inpatient charges for the encounter (USD) — the regression target |

`Total Costs` is also present and represents the hospital's actual cost (as opposed to billed charges); it is a near-leak feature for the target and is dropped/handled carefully during modeling.

## Column Reference

| Column | Type | Description |
|---|---|---|
| Health Service Area | Categorical | Regional health service area of the facility |
| Hospital County | Categorical | County where the hospital is located |
| Operating Certificate Number | Categorical/ID | Facility's operating certificate number |
| Facility ID | Categorical/ID | Unique facility identifier |
| Facility Name | Categorical | Hospital/facility name |
| Age Group | Categorical (ordinal) | Patient age bracket (e.g. `50 to 69`) |
| Zip Code - 3 digits | Categorical | Patient's 3-digit ZIP prefix |
| Gender | Categorical | Patient gender |
| Race | Categorical | Patient race |
| Ethnicity | Categorical | Patient ethnicity |
| Length of Stay | Numeric | Number of inpatient days |
| Type of Admission | Categorical | e.g. `Elective`, `Emergency`, `Urgent` |
| Patient Disposition | Categorical | Discharge destination (e.g. `Home or Self Care`) |
| Discharge Year | Numeric | Year of discharge |
| CCS Diagnosis Code / Description | Categorical | Clinical Classification Software diagnosis grouping |
| CCS Procedure Code / Description | Categorical | CCS procedure grouping |
| APR DRG Code / Description | Categorical | All Patient Refined Diagnosis Related Group |
| APR MDC Code / Description | Categorical | Major Diagnostic Category |
| APR Severity of Illness Code / Description | Categorical (ordinal) | `Minor` → `Extreme` |
| APR Risk of Mortality | Categorical (ordinal) | `Minor` → `Extreme` |
| APR Medical Surgical Description | Categorical | `Medical` vs `Surgical` classification |
| Source of Payment 1 / 2 / 3 | Categorical | Primary/secondary/tertiary payer (insurance, Medicare, Medicaid, etc.) |
| Birth Weight | Numeric | Birth weight in grams (0 for non-newborn records) |
| Abortion Edit Indicator | Categorical (flag) | `Y`/`N` |
| Emergency Department Indicator | Categorical (flag) | `Y`/`N` |
| Total Charges | Numeric ($) | **Target variable** — total billed charges |
| Total Costs | Numeric ($) | Actual hospital cost (handled as a leakage-risk feature) |

## Known Data Quality Notes

- `Total Charges` and `Total Costs` are stored as currency strings (e.g. `"$25,598.10"`) and need parsing to numeric before modeling.
- `Source of Payment 2` and `Source of Payment 3` contain a high proportion of missing values (secondary/tertiary payers aren't always present).
- Several fields (`CCS Diagnosis Code`, `APR DRG Code`, `Facility ID`, `Zip Code`) are high-cardinality categoricals and are grouped/encoded during feature engineering — see the [notebook](../notebooks/hospital-inpatient-cost-prediction-analytics.ipynb), Phase 10–12.
- `Birth Weight` is `0` for the vast majority of non-newborn records, not a true missing-value marker.

## Usage

```python
import pandas as pd

med_df = pd.read_csv("data/Hospital_Inpatient_Discharges__SPARCS_De-Identified__2009.csv")
med_df.shape  # (32033, 34)
```

See the main [project README](../README.md) for the full modeling pipeline built on top of this data.
