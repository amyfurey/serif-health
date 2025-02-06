# Data Science Take-Home Assignment

## Overview
This project integrates two datasets—hospital data and payer data—to produce a unified output dataset. The goal is to represent data points from both sources, capturing any matches between them and computing relative differences where applicable.

## Approach

### **1. Data Exploration & Understanding**
- Loaded and inspected both datasets to understand structure, key attributes, and potential overlaps.
- Identified common linking fields (e.g., hospitals ein, payer names, code types, and codes) to facilitate integration.

### **2. Data Preprocessing**
- Standardized column names and formats (e.g., lowercasing text fields).
- Extracted the EIN from the **source_file_name** in the payer data and saved as **ein**.
- Converted **payer_name** in the payer data to align with **payer** in the hospital data
- Computed **payer** in the payer data by mapping aetna to aetna, cigna to cigna-corporation, and (united, uhc, united healthcare) to unitedhealthcare to align with values in the hospital data.

### **3. Data Integration**
- Performed an **outer join** on common identifiers to ensure that all data points were represented.
- Computed **match_indicator** where matching records were flagged as "Yes" otherwise "No.
- Computed **payer_rate** to fill in missing values for **standard_charge_negotiated_dollar** such that if missing the **payer_rate** is **rate** (hosptial rate) times  **standard_charge_negotiated_percentage** / 100.
- Computed **rate_delta** where matching records were found such that **rate_delta** = hospital rate - payer rate.

### **4. Output Datasets**
The resulting dataset contains:
- **Hospital & Payer Identifiers:** Unified representation of entities from both datasets.
- **Rate Delta Calculation:** Differences in cost structures where applicable.
- **Match Status Indicator:** Flags indicating whether a record exists in both sources or only in one.

## Assumptions & Decisions
- The first 9 numbers of the column source_file_name in the payer file represent the EIN for the hospital.
- In the payer file, the payer_name column containing united, united healthcare, and uhc all represent united healthcare.
- CPT codes anad local codes are not necessarily the same and thus merging required consideration of the code_type.
- Used an outer join to retain all relevant records from both datasets.
- The payer column **standard_charge_negotiated_percentage** can be used to calculate the payer rate based on the hospital rate.

## Future Considerations
- **Scalability:** The approach assumes a manageable dataset size. For these small datasets only 51% of records were matched. As data grows, strategies should be considered to handle the variations in payer and hospital data.
- **Understanding the Data:**  A deeper understanding of the data is necessary.  What explains the large variation in rates?  Can we use these to get a more accurate comparison?
- **Testing & Validation:** Future iterations could introduce automated tests for data consistency checks.
- **Production Deployment:** A robust ETL pipeline (e.g., using Apache Airflow or dbt) would be ideal for ongoing data integration efforts.

## Files Provided
- `exploration.ipynb`: Jupyter Notebook containing the analysis of the input data.
- `input_output.ipynb`: Jupyter Notebook containing the process of creating the output files.
- `all_df_.csv`: Final dataset resulting from the integration.
- `reduced_df.csv`: A copy of the final dataset with many columns removed.
- `README.md`: This document explaining the approach and decisions.

## Running Notebooks
### Clone the repository
First, open a terminal (or Command Prompt) and run the following command: git clone https://github.com/amyfurey/serif-health
### Navigate to the Repository Folder
### Install Python (if not already installed)
If not installed download and install it from python.org.
### Update the *repo_directory* Path in the Notebooks
repo_directory = "path to serif-health directory/"
### Launch Jupyter Notebook
run bash command *jupyter notebook*
### Run the Notebook
Click "Run All" to execute all cells in the notebook.
