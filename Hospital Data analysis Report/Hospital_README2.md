<h1 align="center">🏥 Hospital Patient Encounters Analysis</h1>
<h3 align="center">Retail Inventory & Sales</h3>
<p align="center"><em>Analyzing patient visit trends, healthcare costs, and insurance coverage gaps to support hospital decision-making using Python and Power BI.</em></p>

---

## 🗂 Table of Contents
- <a href="#overview">Overview</a>
- <a href="#business-problem">Business Problem</a>
- <a href="#dataset">Dataset</a>
- <a href="#tools--technologies">Tools & Technologies</a>
- <a href="#project-structure">Project Structure</a>
- <a href="#data-cleaning--preparation">Data Cleaning & Preparation</a>
- <a href="#feature-engineering">Feature Engineering</a>
- <a href="#exploratory-data-analysis-eda">Exploratory Data Analysis (EDA)</a>
- <a href="#dashboard">Dashboard</a>
- <a href="#key-findings">Key Findings</a>
- <a href="#how-to-run-this-project">How to Run This Project</a>
- <a href="#final-recommendations">Final Recommendations</a>
- <a href="#author--contact">Author & Contact</a>

---

<h2><a class="anchor" id="overview"></a>Overview</h2>

This project performs a complete end-to-end analysis of hospital patient records from **Massachusetts General Hospital**, sourced from **Maven Analytics**. A full data pipeline was built using Python for data cleaning, feature engineering and EDA, and Power BI for interactive dashboard and visualization.

---

<h2><a class="anchor" id="business-problem"></a>Business Problem</h2>

Healthcare cost management and insurance coverage are critical challenges for hospitals. This project aims to:

- Identify what percentage of patients have zero insurance coverage
- Understand the financial burden on patients vs payers
- Analyze which encounter types and medical reasons drive the most cost
- Track patient visit trends over 12 years (2011–2022)
- Understand payer performance across insurance companies

---

<h2><a class="anchor" id="dataset"></a>Dataset</h2>

**Source:** Maven Analytics — Massachusetts General Hospital
**Period:** 2011 – 2022

| Table | Rows | Description |
|---|---|---|
| `patients` | 974 | Patient demographics — age, gender, race, location |
| `encounters` | 27,891 | Hospital visits — type, cost, payer, reason |
| `procedures` | 47,701 | Medical procedures — description, cost, duration |
| `payers` | 9 | Insurance companies |
| `organizations` | 1 | Hospital information |

---

<h2><a class="anchor" id="tools--technologies"></a>Tools & Technologies</h2>

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=flat)
![PowerBI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-F2C811?style=flat&logo=powerbi&logoColor=black)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---

<h2><a class="anchor" id="project-structure"></a>Project Structure</h2>

```
📦 hospital-patient-analysis
 ┣ 📓 Hospital_data_Analysis.ipynb    ← Jupyter Notebook (Cleaning + EDA)
 ┣ 📊 Hospital_Dashboard.pbix         ← Power BI Dashboard
 ┣ 📄 hospital_cleaned_final.csv      ← Main cleaned dataset (41 cols)
 ┣ 📄 procedures_cleaned.csv          ← Cleaned procedures table
 ┣ 📄 patients_cleaned.csv            ← Cleaned patients table
 ┣ 📄 encounters_cleaned.csv          ← Cleaned encounters table
 ┗ 📝 README.md                       ← Project documentation
```

---

<h2><a class="anchor" id="data-cleaning--preparation"></a>Data Cleaning & Preparation</h2>

### Patients Table
| Issue | Action |
|---|---|
| 820 nulls in `deathdate` | Created `is_deceased` flag — null = alive patient |
| `suffix`, `maiden`, `address`, `birthplace` — 95% empty | Dropped — no analytical value |
| 1 null in `marital` | Filled with mode |
| 142 nulls in `zip` | Filled with 0 (unknown) |

### Encounters Table
| Issue | Action |
|---|---|
| 19,541 nulls in `reasondescription` | Filled with `'General / No Specific Reason'` — routine visits |
| `reasoncode` nulls | Filled with 0 |
| Date columns stored as string | Converted to datetime format |

### Procedures Table
| Issue | Action |
|---|---|
| 36,945 nulls in `reasondescription` (77%) | Filled with `'Routine Procedure'` |
| `reasoncode` nulls | Filled with 0 |

---

<h2><a class="anchor" id="feature-engineering"></a>Feature Engineering</h2>

**20 New Features Created Across 3 Tables**

### Patients (5 features)
| Feature | Description |
|---|---|
| `age` | Current age calculated from birthdate |
| `age_group` | Young / Middle-Aged / Senior / Elderly / Very Elderly |
| `gender_full` | M → Male, F → Female |
| `marital_full` | M → Married, S → Single |
| `patient_status` | Alive / Deceased |

### Encounters (11 features)
| Feature | Description |
|---|---|
| `duration_hours` | Encounter length in hours |
| `duration_bucket` | < 1hr / 1–8hrs / 8–24hrs / 24+hrs |
| `out_of_pocket` | Total cost − payer coverage |
| `coverage_ratio` | Payer coverage ÷ total claim cost |
| `is_high_cost` | 1 if above 75th percentile cost |
| `zero_coverage` | 1 if no insurance coverage |
| `year / month / quarter` | Time breakdown features |
| `weekday` | Day of the week |
| `is_weekend` | 1 if Saturday or Sunday |
| `season` | Winter / Spring / Summer / Fall |
| `payer_name` | Insurance company name mapped from ID |

### Procedures (4 features)
| Feature | Description |
|---|---|
| `proc_duration_min` | Procedure duration in minutes |
| `proc_count` | Number of procedures per encounter |
| `total_proc_cost` | Total procedure cost per encounter |
| `avg_proc_cost` | Average procedure cost per encounter |

---

<h2><a class="anchor" id="exploratory-data-analysis-eda"></a>Exploratory Data Analysis (EDA)</h2>

Five sets of visualizations created in Jupyter Notebook:

| # | Analysis | Charts Used |
|---|---|---|
| 1 | Patient Demographics | Pie chart, Bar chart |
| 2 | Encounter Trends (2011–2022) | Line chart, Bar chart |
| 3 | Cost Analysis | Histogram, Bar charts |
| 4 | Coverage Insights | Pie chart, Bar chart |
| 5 | Procedures Analysis | Bar charts |

---

<h2><a class="anchor" id="dashboard"></a>Dashboard</h2>

### 📄 Page 1 — Patient & Encounter Overview

| Visual | Description |
|---|---|
| 4 KPI Cards | 28K encounters, 974 patients, $3.64K avg cost, 30.63% coverage rate |
| Funnel Chart | Encounter class breakdown — ambulatory dominates at 50% |
| Line Chart | 12-year encounter and cost trend (dual axis) |
| Stacked Column | Encounter class mix per year |
| Map | City-wise patient distribution across Massachusetts |
| Matrix | Age group vs Payer claim cost with conditional formatting |

### 📄 Page 2 — Clinical Cost & Coverage Analysis

| Visual | Description |
|---|---|
| 4 KPI Cards | $101M total cost, $70M out of pocket, 14K zero coverage, 48K procedures |
| Bubble Chart | Payer performance — cost vs encounters vs coverage size |
| Horizontal Bar | NO_INSURANCE contributes $44M — highest total claim cost |
| Donut Chart | Medicare covers 61.8% of all insured encounters |
| Treemap | Top 5 costly medical reasons |
| 100% Stacked Bar | Encounter class % distribution every year |

**14 DAX Measures Created**

---

<h2><a class="anchor" id="key-findings"></a>Key Findings</h2>

| # | Finding |
|---|---|
| 🔴 | **49%** of encounters had **zero insurance coverage** |
| 💰 | Patients paid **$70M** out of **$101M** total — **70% financial burden on patients** |
| 🏥 | **NO_INSURANCE** contributes **$44M** — highest of all payers |
| 📊 | **Medicare** covers **61.8%** of all insured encounters |
| 🏨 | **Inpatient** visits cost **3x more** than ambulatory despite being only 9% of visits |
| 👴 | **Very Elderly (85+)** is the largest patient group — 363 patients |
| 💊 | **Renal Dialysis** is the most frequently performed procedure |
| 📈 | **Ambulatory** visits consistently dominate at **50%+** every year |

---

<h2><a class="anchor" id="how-to-run-this-project"></a>How to Run This Project</h2>

**Step 1 — Clone the repository**
```bash
git clone https://github.com/yourusername/hospital-patient-analysis.git
cd hospital-patient-analysis
```

**Step 2 — Install required libraries**
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

**Step 3 — Open Jupyter Notebook**
```bash
jupyter notebook Hospital_data_Analysis.ipynb
```

**Step 4 — Update CSV file path**
```python
# Cell 2 mein apna path update karo
patients = pd.read_csv('your_path/Patients.csv')
```

**Step 5 — Run all cells**
```
Kernel → Restart & Run All
```

**Step 6 — Open Power BI Dashboard**
- Open `Hospital_Dashboard.pbix` in Power BI Desktop
- Update data source path to your exported CSVs
- Refresh data

---

<h2><a class="anchor" id="final-recommendations"></a>Final Recommendations</h2>

Based on the analysis, the following actions are recommended for Massachusetts General Hospital:

- **Insurance Assistance Program** — 49% uninsured patients need financial support programs
- **Focus on Inpatient Cost Reduction** — highest cost per visit, needs optimization
- **Payer Negotiation** — NO_INSURANCE contributing $44M suggests need for better coverage policies
- **Elderly Care Program** — Very Elderly (85+) is the largest group, specialized care needed

---

<h2><a class="anchor" id="author--contact"></a>Author & Contact</h2>

**Your Name**
📧 your.email@gmail.com
🔗 [LinkedIn Profile](https://www.linkedin.com/in/namrata-ojha-743b50170/)
🐙 [GitHub Profile]()

---

<p align="center"><em>Data Source: Maven Analytics — Massachusetts General Hospital | Project Type: End-to-End Data Analytics</em></p>
