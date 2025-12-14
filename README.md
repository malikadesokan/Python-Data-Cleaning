# Python-Data-Cleaning
This project uses Python and pandas to clean and standardise a messy healthcare admissions dataset. It fixes inconsistent date formats, normalises categorical fields (e.g., smoker status), and cleans numeric columns such as BMI and billing amounts so they can be reliably used for analysis, reporting, and downstream modelling.

# Hospital Admissions Data Cleaning & Standardisation (Python + pandas)

This notebook walks through a complete **cleaning and standardisation workflow** for a raw hospital admissions dataset using **Python** and **pandas**.  
It starts with quick exploratory checks (`head`, `info`, `describe`) to understand **column types**, **missing values**, and **obvious anomalies**. From there, each group of fields is cleaned systematically.

---

## Workflow Overview

### 1) Patient Details
- **Patient names**
  - Standardised names to **Title Case** so the same person isn’t represented with inconsistent capitalisation.
- **Age**
  - Flagged implausible ages (e.g., under-18 in an adult dataset).
  - Capped extreme values using an **IQR-based outlier rule** to reduce the impact of data entry errors.
- **Gender**
  - Normalised values like `F`, `female`, `M`, `male`, `N/A` into a consistent set of categories:
    - **Male**, **Female**, **Other**
  - Treated `"N/A"` as **missing**.

---

### 2) Physical Measurements
- **Height & weight**
  - Corrected impossible values (e.g., negative numbers) by inverting their sign where appropriate, assuming simple sign-entry mistakes.
- **BMI**
  - Recalculated BMI from height and weight:
    - Converted height to **metres**
    - Squared height
    - Divided weight by height²
  - Replaced original BMI values with this consistent calculation to ensure internal consistency.

---

### 3) Clinical Data
- **Blood pressure**
  - Cleaned a messy free-text blood pressure field that appeared in formats like:
    - `120/80`, `120 over 80`, with/without `mmHg`
  - Normalised the text and split into two numeric columns:
    - **systolic**
    - **diastolic**
- **Diagnosis**
  - Standardised missing values by filling with an explicit **"Unknown"** category so group-by operations don’t silently drop records.

---

### 4) Dates & Timelines
- **Admission and discharge dates**
  - Harmonised mixed formats (e.g., `12 Aug 2025`, `08-Dec-2025`, `13/01/2023`, `2023-01-26`) using regex masks + `pd.to_datetime`.
  - Converted all dates to **ISO format (YYYY-MM-DD)**.
- **Sanity checks**
  - Validated that **admission dates do not fall after discharge dates**, flagging invalid stays.

---

### 5) Lifestyle & Billing
- **Smoker status**
  - Normalised inconsistent labels (`yes`, `Y`, `former`, `EX-SMOKER`, `no`, `N`) into clean categories:
    - **Yes**, **No**, **Ex Smoker**, **Unknown**
- **Active smoker flag**
  - Created a new boolean-style feature to distinguish **current smokers** from **ex-smokers** for easier analysis.
- **Billing amount**
  - Detected values containing currency symbols or `"USD"`.
  - Stripped non-numeric characters using regex.
  - Converted the column to a **numeric dtype** so it can be aggregated reliably in cost analyses.

---

## Output

At the end of the workflow, the cleaned DataFrame is saved as:

- **`patient_record.csv`**

This produces an **analysis-ready dataset** with:
- Consistent data types (numeric, categorical, date fields)
- Standardised labels for key categorical variables
- Corrected outliers and obvious data entry errors
- Derived features (e.g., recalculated BMI, active smoker flag) ready for:
  - descriptive statistics
  - visualisation
  - modelling

---


