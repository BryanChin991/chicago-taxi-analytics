Chicago Taxi Trips — Data Analytics Engineering Project



1\. Objective

Design and implement a production-ready data pipeline on Google Cloud Platform to transform raw taxi trip data into structured analytical models that support business decision-making.



This project demonstrates how raw data can be cleaned, modeled, and translated into actionable insights using a simple and scalable approach.



2\. Architecture Overview

BigQuery (Raw Public Dataset)

&#x20;       ↓

Dataform (Transformation Layer)

&#x20;       ↓

Curated Analytical Tables

&#x20;       ↓

Looker Studio Dashboard



3\. Technology Stack

Data Warehouse: Google BigQuery

Transformation: GCP Dataform

Visualization: Looker Studio



4\. Dataset

Source: bigquery-public-data.chicago\_taxi\_trips.taxi\_trips

Granularity: Trip-level

Key fields:

taxi\_id

trip\_start\_timestamp, trip\_end\_timestamp

trip\_seconds

fare, tips, trip\_total, company, payment\_type



5\. Data Modeling Approach

A simple layered approach is used to keep the pipeline easy to maintain and extend.



5.1. Staging Layer (stg\_trips)

\- Cleans and standardizes raw data

\- Converts trip duration into hours

\- Extracts date fields for analysis

\- Filters out invalid records (e.g. null taxi\_id)



5.2. Analytical Models



top\_tip\_earners.sqlx

\- Identifies top 100 drivers based on total tips in the most recent 3 months

\- Uses latest available data to avoid bias from incomplete periods



overworkers.sqlx

\- Detects drivers with consistently long working patterns

\- Combines multiple signals:

&#x20; i. Long shift duration

&#x20; ii. Short rest between shifts

&#x20; iii. Active driving hours

&#x20; iv. Idle time within shifts



public\_holiday.sqlx

\- Compares trip demand across:

&#x20; i. Weekdays

&#x20; ii. Weekends

&#x20; iii. Public holidays (selected US holidays)

\- Used to evaluate demand shifts during special periods



idle\_distribution.sqlx

\- Measures how much time drivers spend without trips within a working day

\- Groups driver-day observations into idle hourly buckets

\- Highlights operational inefficiencies



Tipping.sqlx

\- Compares tipping behavior across payment types

\- Splits analysis into:

&#x20; i. Before 2020

&#x20; ii. After 2020

\- Evaluates impact of digital payment adoption



6\. Data Pipeline Execution

All models are built using Dataform and executed in BigQuery.



Key practices applied:

\- Models are modular and reusable using ref()

\- Logic is separated into staging and analytical layers

\- Queries are written to be readable and easy to debug



7\. Setup Instructions

7.1. Google Cloud Setup

&#x20; - Create a GCP project

&#x20; - Enable BigQuery

&#x20; - Create dataset: chicago\_taxi\_analysis



7.2. Dataform Setup

\- Create a Dataform repository

\- Configure default dataset

\- Add models:

&#x20;   stg\_trips

&#x20;   top\_tip\_earners

&#x20;   overworkers

&#x20;   public\_holiday

&#x20;   idle\_distribution

&#x20;   tipping

\- Run the workflow to build tables



8\. Visualization

\- Connect Looker Studio to BigQuery

\- Build dashboard using the analytical tables



9\. Analytical Questions \& Approach

9.1. Top Tip Earners

Drivers are ranked based on total tips earned over the latest 3-month window.



Why it matters:

Helps identify high-performing drivers and understand earning concentration.



9.2. Overworkers

Drivers are identified based on repeated long working hours and short breaks between shifts (less than 8 hours).



Why it matters:

Helps identify drivers with intensive working patterns and supports better workforce planning.



9.3. Impact of Public Holidays

Trip volume is compared across holidays, weekends, and weekdays.



Why it matters:

Shows how demand shifts during non-working days and helps guide supply planning.



9.4. Additional Insights

Insight 1: Idle Time is Structurally High

Many drivers spend 5–8 hours per day without trips

Idle time is a common pattern, not an exception



Business value:

Indicates mismatch between supply and demand → opportunity to improve dispatch and driver allocation.



Insight 2: Digital Payments Drive Higher Tips

Credit card usage results in higher tips per trip

Tipping increased after 2020 alongside digital adoption



Business value:

Encouraging digital payments can improve driver earnings and platform monetization



10.Engineering Considerations

To keep the pipeline scalable and production-friendly:

\- Data freshness control

&#x20; Models use the latest available data in the dataset rather than system time

\- Efficient processing

&#x20; Transformations are structured to avoid unnecessary recomputation and keep queries simple

\- Reusability

&#x20; Staging layer ensures downstream models use consistent and clean data



11\. Assumptions \& Limitations

&#x20; - Trip data is used as a proxy for driver working behavior

&#x20; - No direct login/logout data is available

&#x20; - Public holidays are based on a selected subset (not exhaustive)

&#x20; - Idle time includes breaks and off-platform time



12\. Dashboard

Public Looker Studio Dashboard: https://lookerstudio.google.com/s/uEB3YE0REv0

