
# Chicago Taxi Trips — Data Analytics Engineering Project

## 1. Overview
This project transforms raw Chicago taxi trip data into structured insights using a scalable data pipeline built on Google Cloud Platform.

The pipeline demonstrates how raw data can be cleaned, modeled, and translated into actionable insights for decision-making.

---

## 2. Architecture
Data flows through the following layers:

BigQuery (Raw Public Dataset)  
↓  
Dataform (Transformation Layer)  
↓  
Curated Analytical Tables  
↓  
Looker Studio Dashboard  

Raw trip-level data is processed in BigQuery, transformed using Dataform, and exposed as analytical tables for visualization.

---

## 3. Technology Stack
- **Data Warehouse:** Google BigQuery  
- **Transformation:** GCP Dataform  
- **Visualization:** Looker Studio  

---

## 4. Dataset
- **Source:** `bigquery-public-data.chicago_taxi_trips.taxi_trips`  
- **Granularity:** Trip-level  

**Key fields:**
- `taxi_id`  
- `trip_start_timestamp`, `trip_end_timestamp`  
- `trip_seconds`  
- `fare`, `tips`, `trip_total`  
- `company`, `payment_type`  

---

## 5. Data Modeling Approach
A layered modeling approach is used to ensure clarity, reusability, and scalability.

### 5.1 Staging Layer (`stg_trips`)
- Cleans and standardizes raw data  
- Converts trip duration into hours  
- Extracts date fields for analysis  
- Filters invalid records (e.g. null `taxi_id`)  

### 5.2 Analytical Models

#### `top_tip_earners.sqlx`
Identifies the top 100 drivers based on total tips in the most recent 3 months.

- Uses latest available data to avoid incomplete period bias  

---

#### `overworkers.sqlx`
Identifies drivers with potentially unsustainable working patterns based on:

- Long shift duration  
- Short rest between shifts  
- Meaningful active driving time (≥ 4 hours/day)  
- Limited idle time within shifts (< 8 hours/day)  

---

#### `public_holiday.sqlx`
Compares trip demand across:

- Weekdays  
- Weekends  
- Selected US public holidays  

Used to evaluate demand shifts during special periods.

---

#### `idle_distribution.sqlx`
Bonus Insight 1: Measures driver idle time within a working day.

- Breaks idle time into hourly buckets  
- Aggregates driver-day observations  
- Highlights operational inefficiencies  

---

#### `tipping.sqlx`
Bonus Insight 2: Analyzes tipping behavior across payment types.

- Splits analysis into:
  - Before 2020 (pre-COVID)
  - After 2020  
- Evaluates impact of digital payment adoption  

---

## 6. Key Insights

### 6.1 Idle Time is Structurally High
- Many drivers spend **5–8 hours per day idle**  
- Idle time is a consistent pattern across driver-days  

**Business value:**
- Indicates supply-demand imbalance  
- Opportunity to improve dispatch and driver allocation  

---

### 6.2 Digital Payments Drive Higher Tips
- Credit card trips generate higher tips  
- Tipping increased after 2020  

**Business value:**
- Encouraging digital payments improves driver earnings  
- Supports platform monetization  

---

## 7. Assumptions & Limitations
- Trip data is used as a proxy for driver behavior  
- No direct driver login/logout data is available  
- Public holiday list is not exhaustive  
- Idle time may include breaks or off-platform time
  
---

## 8. Data Pipeline Execution
All models are built and executed using Dataform on BigQuery.

**Key practices applied:**
- Modular model design using `ref()`  
- Clear separation between staging and analytical layers  
- Readable and maintainable SQL structure  

---

## 9. Engineering Considerations
- **Data freshness control:** Uses latest available dataset instead of system time  
- **Efficiency:** Avoids unnecessary recomputation  
- **Reusability:** Staging layer ensures consistent downstream usage  

---

## 10. Setup Instructions

### 10.1. Google Cloud Setup
- Create a GCP project  
- Enable BigQuery  
- Create dataset: `chicago_taxi_analysis`  

---

### 10.2. Dataform Setup
- Create a Dataform repository  
- Configure default dataset  
- Add models:
  - `stg_trips`  
  - `top_tip_earners`  
  - `overworkers`  
  - `public_holiday`  
  - `idle_distribution`  
  - `tipping`  
- Run workflow to build tables  

---

### 10.3. Push Project to GitHub

The project was developed in Dataform within Google Cloud, which is not directly shareable outside the platform.  
To make the project accessible and meet submission requirements, the code was uploaded to GitHub.

#### Approach Used: Manual Git Push
```bash
git init
git remote add origin https://github.com/BryanChin991/chicago-taxi-analytics.git

git add .
git commit -m "Initial commit - Dataform project"
git push -u origin main
```

### Challenges Faced
- It was initially unclear how to access and export the project files from Dataform in GCP
- Setting up the Git repository (e.g. linking to GitHub, managing branches) required some trial and error
- Managing updates after making changes to SQLX files and the README required understanding how Git tracks changes

### Reason for Manual Push
- Provides better control over when and how changes are saved
- Makes it easier to track updates through version history
- Ensures the project is properly structured and ready for review on GitHu

---

## 11. Dashboard

**Public Looker Studio Dashboard:**  
https://lookerstudio.google.com/reporting/b7f7b021-6e9d-4efc-84ec-ca20f01ee331
