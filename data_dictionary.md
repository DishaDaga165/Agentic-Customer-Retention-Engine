# Data Dictionary — IBM Telco Customer Churn

**Source file:** `Telco_customer_churn.xlsx` → exported to `data/telco_customer_churn.csv`
**Rows:** 7,043 | **Columns:** 33 | **Grain:** one row per customer (`CustomerID` unique)

| Column | Type | Unique | Missing | Description |
|---|---|---|---|---|
| CustomerID | string | 7043 | 0 | Unique customer identifier. Primary key. |
| Count | int | 1 | 0 | Always 1; used for aggregation in source tool. Not useful for modeling. |
| Country | string | 1 | 0 | Always "United States". No variance — drop. |
| State | string | 1 | 0 | Always "California". No variance — drop. |
| City | string | 1129 | 0 | Customer's city. High cardinality — geographic feature. |
| Zip Code | int | 1652 | 0 | Customer's ZIP code. |
| Lat Long | string | 1652 | 0 | Combined "lat, long" string. Redundant with Latitude/Longitude. |
| Latitude | float | 1652 | 0 | Customer location latitude. |
| Longitude | float | 1651 | 0 | Customer location longitude. |
| **Gender** | string | 2 | 0 | Male / Female. |
| **Senior Citizen** | string | 2 | 0 | Yes / No — customer is 65+. |
| **Partner** | string | 2 | 0 | Yes / No — has a partner. |
| **Dependents** | string | 2 | 0 | Yes / No — has dependents. |
| **Tenure Months** | int | 73 | 0 | Months the customer has stayed with the company. Range 0–72. |
| **Phone Service** | string | 2 | 0 | Yes / No — subscribes to phone service. |
| **Multiple Lines** | string | 3 | 0 | Yes / No / No phone service. |
| **Internet Service** | string | 3 | 0 | DSL / Fiber optic / No. |
| **Online Security** | string | 3 | 0 | Yes / No / No internet service. |
| **Online Backup** | string | 3 | 0 | Yes / No / No internet service. |
| **Device Protection** | string | 3 | 0 | Yes / No / No internet service. |
| **Tech Support** | string | 3 | 0 | Yes / No / No internet service. |
| **Streaming TV** | string | 3 | 0 | Yes / No / No internet service. |
| **Streaming Movies** | string | 3 | 0 | Yes / No / No internet service. |
| **Contract** | string | 3 | 0 | Month-to-month / One year / Two year. |
| **Paperless Billing** | string | 2 | 0 | Yes / No. |
| **Payment Method** | string | 4 | 0 | Electronic check / Mailed check / Bank transfer (automatic) / Credit card (automatic). |
| **Monthly Charges** | float | 1585 | 0 | Current monthly bill amount ($18.25–$118.75). |
| **Total Charges** | object (⚠) | 6531 | 0 raw / 11 effective | Stored as **text**, not numeric. 11 rows are blank strings (`" "`) — these are customers with `Tenure Months = 0` (brand-new customers, no charges yet). Must `pd.to_numeric(..., errors="coerce")` before analysis. |
| **Churn Label** | string | 2 | 0 | **Target (text form).** Yes / No — did the customer leave. |
| **Churn Value** | int | 2 | 0 | **Target (numeric form).** 1 = churned, 0 = retained. Same info as Churn Label. |
| Churn Score | int | 85 | 0 | IBM-provided propensity-to-churn score, 0–100 (higher = more likely to churn). |
| CLTV | int | 3438 | 0 | Customer Lifetime Value score, 2003–6500 (higher = more valuable). |
| Churn Reason | string | 20 | 5,174 (73.5%) | Free-text reason for churn. Only populated for churned customers (`Churn Value = 1`); NaN for retained customers — this is *expected*, not a data quality issue. |

## Target distribution
- **Churn Label / Churn Value:** No (0) = 5,174 (73.46%) | Yes (1) = 1,869 (26.54%)
- Moderately imbalanced (~2.8:1) — worth using stratified splits and/or class weighting for modeling.

## Key data quality notes
1. **`Total Charges`** is read as text/object because 11 rows contain a blank space instead of a number (all have `Tenure Months = 0`, i.e., brand new customers who haven't been billed yet). Convert with `pd.to_numeric(errors="coerce")`; decide whether to impute as 0 or drop.
2. **`Count`, `Country`, `State`** are constant across all 7,043 rows — no predictive value, safe to drop.
3. **`Lat Long`** duplicates `Latitude`/`Longitude` — pick one representation.
4. **"No internet service" / "No phone service"** categories in several columns are technically a third answer, not truly missing — encode as-is or collapse to "No" depending on modeling approach.
5. **`Churn Reason`** missingness is structural (only present when the customer churned), not a random gap — don't impute; consider dropping or using only in churn-driver analysis, not as a leakage-free model feature (it's an outcome descriptor).
6. **`Churn Score`** and **`Churn Value`** are related but distinct — `Churn Score` is IBM's own predicted probability-like score, so using it as a model *feature* for predicting `Churn Value` risks target leakage; treat with caution.
