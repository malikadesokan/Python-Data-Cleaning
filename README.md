# Python-Data-Cleaning
This project uses Python and pandas to clean and standardise a messy healthcare admissions dataset. It fixes inconsistent date formats, normalises categorical fields (e.g., smoker status), and cleans numeric columns such as BMI and billing amounts so they can be reliably used for analysis, reporting, and downstream modelling.

This notebook walks through a full cleaning and standardisation workflow on a raw hospital admissions dataset using Python and pandas.
I start with basic exploratory checks (head, info, describe) to understand column types, missing values and obvious anomalies. From there, I systematically clean each group of fields:
•	Patient details:
o	Standardised patient names to title case so the same person isn’t represented with multiple capitalisation styles.
o	Cleaned the age column by flagging implausible ages (e.g. under-18 in an adult dataset) and capping them using an IQR-based outlier rule, reducing the impact of data entry errors.
o	Normalised gender values (e.g. “F”, “female”, “M”, “male”, “N/A”) into a consistent set of categories (Male, Female, Other) and treated “N/A” as missing.
•	Physical measurements:
o	Fixed impossible height and weight values (e.g. negative numbers) by inverting their sign where appropriate, assuming simple data entry sign errors.
o	Recalculated BMI from height and weight (converting height to metres, squaring, and dividing weight), then replaced the original BMI values with this consistent calculation to ensure internal consistency across measurements.
•	Clinical data:
o	Cleaned a messy blood pressure field that appeared in various free-text formats (e.g. “120/80”, “120 over 80”, with/without “mmHg”). I normalised the text and split it into two numeric columns: systolic and diastolic, ready for analysis and visualisation.
o	Standardised the diagnosis column by filling missing values with an explicit "Unknown" category so that group-by operations don’t silently drop patients.
•	Dates and timelines:
o	Harmonised multiple admission date and discharge date formats (e.g. 12 Aug 2025, 08-Dec-2025, 13/01/2023, 2023-01-26) using regex masks and pd.to_datetime, converting everything to a standard ISO format (YYYY-MM-DD).
o	Performed a sanity check to make sure admission dates do not fall after discharge dates, flagging any invalid hospital stays.
•	Lifestyle and billing:
o	Normalised the smoker column by mapping inconsistent labels ("yes", "Y", "former", "EX-SMOKER", "no", "N") into clean categories (Yes, No, Ex Smoker, Unknown).
o	Created a new active smoker flag to distinguish current smokers from ex-smokers in a more analysis-friendly way.
o	Cleaned billing amount field by detecting values that still contained currency symbols or “USD”, stripping all non-numeric characters via regex, and converting the column to a numeric dtype so it can be aggregated and used in cost analyses.
Finally, the cleaned Data Frame is saved out as a new CSV (patient_record.csv), giving an analysis-ready dataset with:
•	Consistent data types (numeric, categorical, and date fields),
•	Standardised labels across key categorical variables,
•	Corrected outliers and obvious data entry errors,
•	And derived features (e.g. recalculated BMI, active smoker flag) that are ready for descriptive stats, visualisation, and modelling.

