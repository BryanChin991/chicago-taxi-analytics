\# Chicago Taxi Trips — Data Analytics Engineering Project



\---



\## 1. Objective



Design and implement a \*\*production-ready data pipeline\*\* on Google Cloud Platform to transform raw taxi trip data into structured analytical models that support business decision-making.



This project demonstrates how raw data can be cleaned, modeled, and translated into \*\*actionable insights\*\* using a simple and scalable approach.



\---



\## 2. Architecture Overview

BigQuery (Raw Public Dataset)

↓

Dataform (Transformation Layer)

↓

Curated Analytical Tables

↓

Looker Studio Dashboard







\---



\## 3. Technology Stack



\- \*\*Data Warehouse:\*\* Google BigQuery  

\- \*\*Transformation:\*\* GCP Dataform  

\- \*\*Visualization:\*\* Looker Studio  



\---



\## 4. Dataset



\- \*\*Source:\*\* `bigquery-public-data.chicago\_taxi\_trips.taxi\_trips`  

\- \*\*Granularity:\*\* Trip-level  



\*\*Key fields:\*\*

\- `taxi\_id`

\- `trip\_start\_timestamp`, `trip\_end\_timestamp`

\- `trip\_seconds`

\- `fare`, `tips`, `trip\_total`

\- `company`, `payment\_type`



\---



\## 5. Data Modeling Approach



A simple \*\*layered approach\*\* is used to keep the pipeline easy to maintain and extend.



\### 5.1 Staging Layer (`stg\_trips`)



\- Cleans and standardizes raw data  

\- Converts trip duration into hours  

\- Extracts date fields for analysis  

\- Filters out invalid records (e.g. null `taxi\_id`)  



\---



\### 5.2 Analytical Models



\#### `top\_tip\_earners.sqlx`

\- Identifies top 100 drivers based on total tips in the most recent 3 months  

\- Uses latest available data to avoid bias from incomplete periods  



\#### `overworkers.sqlx`

\- Detects drivers with consistently long working patterns  

\- Combines multiple signals:

&#x20; - Long shift duration  

&#x20; - Short rest between shifts  

&#x20; - Active driving hours  

&#x20; - Idle time within shifts  



\#### `public\_holiday.sqlx`

\- Compares trip demand across:

&#x20; - Weekdays  

&#x20; - Weekends  

&#x20; - Public holidays (selected US holidays)  

\- Evaluates demand shifts during special periods  



\#### `idle\_distribution.sqlx`

\- Measures how much time drivers spend without trips within a working day  

\- Groups driver-day observations into idle hourly buckets  

\- Highlights operational inefficiencies  



\#### `tipping.sqlx`

\- Compares tipping behavior across payment types  

\- Splits analysis into:

&#x20; - Before 2020  

&#x20; - After 2020  

\- Evaluates impact of digital payment adoption  



\---



\## 6. Data Pipeline Execution



All models are built using \*\*Dataform\*\* and executed in \*\*BigQuery\*\*.



\*\*Key practices applied:\*\*

\- Modular models using `ref()`  

\- Clear separation between staging and analytical layers  

\- Readable and maintainable SQL  



\---



\## 7. Setup Instructions



\### 7.1 Google Cloud Setup



\- Create a \*\*GCP project\*\*  

\- Enable \*\*BigQuery\*\*  

\- Create dataset: `chicago\_taxi\_analysis`  



\---



\### 7.2 Dataform Setup



\- Create a \*\*Dataform repository\*\*  

\- Configure default dataset  

\- Add models:

&#x20; - `stg\_trips`

&#x20; - `top\_tip\_earners`

&#x20; - `overworkers`

&#x20; - `public\_holiday`

&#x20; - `idle\_distribution`

&#x20; - `tipping`



\- Run the workflow to build tables  



\---



\### 7.3 Push Project from GCP (Dataform) to GitHub



Since Dataform runs inside GCP, code needs to be exported to \*\*GitHub\*\* for submission.



\#### Option A — Direct Git Integration



1\. Open Dataform repository  

2\. Go to \*\*Settings → Git\*\*  

3\. Connect to your GitHub account  

4\. Link to your repository  

5\. Commit and push changes  



\#### Option B — Manual Push



```bash

git init

git remote add origin <your\_github\_repo\_url>



git add .

git commit -m "Initial commit - Dataform project"

git push -u origin main



Why GitHub is Required



Even though development is done in GCP:



GCP environment is not directly shareable

The requirement explicitly asks for a Git repository



GitHub provides:



Version control (commit history)

Transparency of development process

Easy access for reviewers

8\. Visualization

Connect Looker Studio to BigQuery

Build dashboard using analytical tables

9\. Analytical Questions \& Approach

9.1 Top Tip Earners



Drivers are ranked based on total tips earned over the latest 3-month window.



Why it matters:

Helps identify high-performing drivers and understand earning concentration.



9.2 Overworkers



Drivers are identified based on repeated long working hours and short breaks (< 8 hours).



Why it matters:

Supports workforce planning and sustainability.



9.3 Impact of Public Holidays



Trip volume is compared across holidays, weekends, and weekdays.



Why it matters:

Guides demand forecasting and supply planning.



9.4 Additional Insights

Insight 1: Idle Time is Structurally High

Many drivers spend 5–8 hours per day without trips

Idle time is a consistent pattern



Business value:

Indicates supply-demand mismatch and opportunity to improve dispatch and driver allocation



Insight 2: Digital Payments Drive Higher Tips

Credit card usage results in higher tips per trip

Tipping increased after 2020



Business value:

Encouraging digital payments can improve driver earnings and platform monetization



10\. Engineering Considerations

Data freshness control

Uses latest available dataset instead of system time

Efficient processing

Avoids unnecessary recomputation

Reusability

Staging layer ensures consistent downstream usage

11\. Assumptions \& Limitations

Trip data is used as a proxy for driver behavior

No direct login/logout data is available

Public holidays are not exhaustive

Idle time includes breaks and off-platform time

12\. Dashboard



Public Looker Studio Dashboard:

https://lookerstudio.google.com/s/uEB3YE0REv0

