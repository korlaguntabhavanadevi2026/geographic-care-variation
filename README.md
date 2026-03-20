# 🏥 Geographic Variation in Care Delivery

> **INFO-B512: Scientific and Clinical Data Management** — Final Project  
> Indiana University | Fall 2025 | Group 4

---

## 📌 Project Overview

This project studies how **healthcare delivery differs across patient locations and facilities** using data extracted from **OpenEMR** — an open-source electronic medical records system. By building a normalized MySQL database and performing complex SQL queries with Python-based visualizations, the project uncovers geographic disparities in immunization rates, encounter frequency, prescription patterns, and preventive care adherence.

Understanding these regional differences is critical for improving **health equity**, optimizing **resource allocation**, and guiding **population-level healthcare planning**.

---

## ❓ Research Questions

1. Are immunization rates higher in certain states or cities?
2. Do patients from different regions show differences in encounter frequency or follow-up rates?
3. What is the average number of prescriptions per patient across different states or cities?
4. Which facility types serve the largest share of encounters?
5. Are certain medications prescribed more frequently in specific geographic locations?

---

## 🗂️ Data Source

- **OpenEMR** — Read-only access provided via phpMyAdmin
- **100 patients' data** extracted and exported as CSV files
- Tables used:

| Table | Key Attributes |
|---|---|
| `patient_data` | pid, DOB, sex, city, state |
| `facility` | id, name, city, state |
| `form_encounter` | id, pid, facility_id, date, reason_for_visit |
| `immunizations` | id, pid, administered_date, cdc_vaccine_code |
| `prescriptions` | id, encounter_id, date_prescribed, drug_name |

---

## 🧱 Database Design

### Normalization (3NF)
- **1NF:** All attributes are atomic; each table has a clearly defined primary key.
- **2NF:** All non-key attributes depend entirely on the whole primary key; no partial dependencies.
- **3NF:** No transitive dependencies. City/state stored only in `patient_data` or `facility` — never duplicated across tables.

### Entity Relationships
- `Patient` → `Encounter` (1-to-many)
- `Encounter` → `Facility` (many-to-1)
- `Patient` → `Immunizations` (1-to-many)
- `Encounter` → `Prescriptions` (1-to-many)

### Data Integrity
- **Entity Integrity:** Unique primary keys on all tables (pid, facility_id, encounter id, etc.)
- **Referential Integrity:** Foreign keys ensure no orphaned records — every encounter references a valid patient and facility; every prescription and immunization references a valid patient.
- **Domain Integrity:** Drug names and medication categories follow consistent clinical naming conventions.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **MySQL** | Local relational database server |
| **OpenEMR** | Source EHR system (read-only) |
| **Python** | Data loading, querying, and analysis |
| **Jupyter Notebook** | Interactive analysis environment |
| **mysql-connector-python** | Python–MySQL connectivity |
| **pandas** | Data manipulation and SQL result processing |
| **matplotlib / seaborn** | Data visualization |

---

## 📁 Repository Structure

```
├── geographic_variation.ipynb   # Main Jupyter Notebook (queries + visualizations)
├── patient_data.csv             # Exported patient records from OpenEMR
├── facility.csv                 # Facility data
├── form_encounter.csv           # Encounter records
├── immunizations.csv            # Immunization records
├── prescriptions.csv            # Prescription records
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.x
- MySQL Server (local or remote)
- Jupyter Notebook

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/geographic-variation-care-delivery.git
cd geographic-variation-care-delivery

# Install dependencies
pip install mysql-connector-python pandas matplotlib seaborn numpy
```

### Database Setup

1. Start your MySQL server and create a new database:
   ```sql
   CREATE DATABASE geographic_care;
   ```

2. Open `geographic_variation.ipynb` in Jupyter Notebook.

3. Update the database connection credentials in the notebook:
   ```python
   connection = mysql.connector.connect(
       host="127.0.0.1",
       user="your_username",
       password="your_password",
       database="geographic_care"
   )
   ```

4. Run the **"Run only once"** section to create all tables and load the CSV data.

5. Proceed to run the analysis cells for queries and visualizations.

---

## 📊 Analysis & Key Queries

The notebook addresses each research question with SQL queries and corresponding visualizations:

- **Q1 — Immunization Rates by State/City:** Counts distinct vaccinated patients grouped by location using multi-table JOINs.
- **Q2 — Encounter Frequency & Follow-up Rates:** Aggregates encounter counts and 30-day follow-up rates by region.
- **Q3 — Average Prescriptions per Patient:** Computes per-patient prescription averages grouped by state and city; surfaces the most commonly prescribed medications.
- **Q4 — Facility Encounter Share:** Ranks facilities by total encounters served, highlighting dominant care sites.
- **Q5 — Medication Patterns by Location:** Identifies which drug categories are most frequently prescribed in specific cities and states.

### SQL Techniques Used
- Multi-table `JOIN`s across patient, encounter, prescription, and facility tables
- Aggregation with `COUNT`, `AVG`, and `GROUP BY`
- Time-based filtering for 30-day follow-up windows
- Subqueries for percentage calculations

- **Instructor:** Yan Zhuang, Ph.D.
- **Semester:** Fall 2025
- **Scenario:** Scenario 5 — Geographic Variation in Care Delivery
