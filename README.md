# Ola Drive SQL Project — Ride-Hailing KPI & Trip Analysis

A **SQL** project analysing a ride-hailing (Ola-style) platform: core business KPIs, the booking funnel from search to ride completion, and location-wise insights on fares and cancellations, using aggregation, subqueries, and window functions.

## 📊 About the Database

The notebook contains only queries (the data and `CREATE TABLE` statements are not included). The structure below is **inferred from the columns the queries use**.

| Table | Columns used in the queries | Description |
|---|---|---|
| `trip_details` | tripid, loc_from, searches, searches_got_estimate, searches_for_quotes, searches_got_quotes, driver_not_cancelled, customer_not_cancelled, otp_entered, end_ride | Event/funnel data for each trip request |
| `trips` | tripid, driverid, fare, distance, duration, loc_from, loc_to | Successfully booked / completed trips |

## 🎯 Purpose

To practise writing business-oriented SQL: computing platform KPIs, measuring conversion across the booking funnel, and finding which routes, drivers, durations, and areas drive revenue or cancellations.

## 🛠️ Approach

1. **Headline KPIs** — totals and averages with `COUNT(DISTINCT)`, `SUM()`, `AVG()`.
2. **Funnel metrics** — sum the flag columns in `trip_details` (searches → estimates → quotes → OTP → ended rides).
3. **Cancellations** — derived as `COUNT(*) - SUM(not_cancelled_flag)`.
4. **Rankings** — `GROUP BY` + `ORDER BY` for routes and drivers.
5. **Window functions** — `RANK() OVER (ORDER BY ...)` inside nested subqueries to pick the top-ranked duration or area.
6. **Conversion rate** — search-to-estimate percentage.

## ❓ Questions Answered

**KPIs**

| Metric | Query idea |
|---|---|
| Total trips | `COUNT(DISTINCT tripid)` on `trip_details` |
| Total drivers | `COUNT(DISTINCT driverid)` on `trips` |
| Total earnings | `SUM(fare)` on `trips` |
| Total completed trips | `COUNT(DISTINCT tripid)` on `trips` |
| Average distance / fare per trip | `AVG(distance)`, `AVG(fare)` |
| Total distance travelled | `SUM(distance)` |

**Funnel**

| Metric | Query idea |
|---|---|
| Total searches | `SUM(searches)` |
| Searches that got an estimate | `SUM(searches_got_estimate)` |
| Searches for quotes / that got quotes | `SUM(searches_for_quotes)`, `SUM(searches_got_quotes)` |
| Total driver cancellations | `COUNT(*) - SUM(driver_not_cancelled)` |
| Total OTP entered | `SUM(otp_entered)` |
| Total ended rides | `SUM(end_ride)` |
| Search-to-estimate rate (%) | `SUM(searches_got_estimate) * 100 / SUM(searches)` |

**Deeper analysis**

- Which pickup–drop-off route pairs have the most trips?
- Which drivers earn the most in total fare?
- Which trip duration has the most trips (ranked, and the single top one)?
- Which area has the highest total fares?
- Which area has the most driver-side cancellations?
- Which area has the most customer-side cancellations?

## 🗄️ Tech Stack

- **SQL** (MySQL syntax — window functions need MySQL 8+)
- **Jupyter Notebook** — queries with a plain-English explanation under each

## 📁 Repository Structure

```
├── OlaDRIVE_SQL_EDAProject.ipynb   # Notebook with SQL queries and explanations
├── Project_Report.docx             # Full project report
└── README.md
```

## 🚀 How to Run

1. Create a MySQL (8.0+) database and load the `trip_details` and `trips` tables (the data is not bundled in the notebook).
2. Run the queries in order in your MySQL client.
3. Read the explanation under each query for how it works.

## 📝 Notes on the Notebook

- The notebook contains **no result values** (the screenshots attached to some cells are not included in the text), so this README lists the metrics computed rather than their numbers. Add your own outputs once you run the queries.
- **Top-N queries:** the route query ("which 2 locations") has no `LIMIT 2`, and the driver query ("top 5 earnings driver") has no `LIMIT 5`, although their explanations say top two / top five. Add `LIMIT 2` and `LIMIT 5` to match.
- `RANK()` returns **all rows tied at rank 1**, so the "highest" queries can return more than one row when there is a tie.

---
*A SQL project on ride-hailing KPIs, booking funnel, and location-wise fares and cancellations.*
