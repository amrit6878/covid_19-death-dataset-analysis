
# 🦠 COVID-19 SQL Exploratory Data Analysis

This project performs exploratory data analysis (EDA) on a global COVID-19 dataset using **MySQL**. The dataset comes from [Our World in Data (OWID)](https://ourworldindata.org/covid-deaths) and was sourced via Kaggle.

---

## 📁 Dataset

**File:** `covid_deaths.csv`  
**Source:** Kaggle / Our World in Data  
**Data Includes:**  
- Total cases and deaths  
- New daily cases and deaths  
- Cases per million  
- ICU patients, hospitalizations  
- Date-wise breakdown by country and continent

---

## 🛠️ Tools Used

- MySQL Workbench
- SQL (MySQL 8+)
- Kaggle dataset
- Git & GitHub

---

## 🧱 Database Schema

```sql
CREATE DATABASE covid_19;
USE covid_19;

CREATE TABLE covid_deaths (
    iso_code VARCHAR(10),
    continent VARCHAR(50),
    location VARCHAR(100),
    date DATE,
    population BIGINT,
    total_cases DOUBLE,
    new_cases DOUBLE,
    total_deaths DOUBLE,
    new_deaths DOUBLE,
    total_cases_per_million DOUBLE,
    new_cases_per_million DOUBLE,
    total_deaths_per_million DOUBLE,
    new_deaths_per_million DOUBLE,
    reproduction_rate DOUBLE,
    icu_patients DOUBLE,
    hosp_patients DOUBLE,
    weekly_icu_admissions DOUBLE,
    weekly_hosp_admissions DOUBLE
);
```

---

## 🔍 Key SQL Queries & Insights

### 1. Total Cases and Deaths by Country
```sql
SELECT location, MAX(total_cases) AS total_cases, MAX(total_deaths) AS total_deaths
FROM covid_deaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY total_cases DESC;
```

### 2. Global Highest Daily New Cases
```sql
SELECT location, date, new_cases
FROM covid_deaths
WHERE new_cases IS NOT NULL
ORDER BY new_cases DESC
LIMIT 1;
```

### 3. Death Rate by Country
```sql
SELECT location, MAX(total_deaths) / MAX(total_cases) * 100 AS death_rate_percent
FROM covid_deaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY death_rate_percent DESC;
```

### 4. 7-Day Average of New Cases
```sql
SELECT location, date,
       AVG(new_cases) OVER (PARTITION BY location ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS avg_new_cases_7_days
FROM covid_deaths
WHERE continent IS NOT NULL;
```

### 5. Top 10 Countries by Cases Per Million
```sql
SELECT location, MAX(total_cases_per_million) AS cases_per_million
FROM covid_deaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY cases_per_million DESC
LIMIT 10;
```

### 6. Highest ICU Patients in a Single Day
```sql
SELECT date, location, icu_patients
FROM covid_deaths
WHERE icu_patients IS NOT NULL
ORDER BY icu_patients DESC
LIMIT 1;
```

---

## 📊 Insights

- Countries with highest total and per million cases
- Highest single-day case spikes globally
- Top countries by death rate
- ICU and hospitalization trends
- Rolling averages of new infections

---

## 🚀 How to Use

1. Download `covid_deaths.csv` from Kaggle
2. Create database and table using the schema above
3. Import CSV via MySQL Workbench > Table Data Import Wizard
4. Run the SQL queries to explore insights

---

## 👨‍💻 Author

**Your Name**  
GitHub: [@your-username](https://github.com/your-username)

---

## 📌 License

This project is open-source and available under the [MIT License](LICENSE).
