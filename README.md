# Maji Ndogo Water Access & Improvement Tracker

## ⚙️ Project Overview

**Context:**  
Maji Ndogo is a fictional nation facing a widespread water access crisis. A national survey was conducted to assess water source types, queue times, water quality, and population distribution across five provinces: Akatsi, Amanzi, Hawassa, Kilimani, and Sokoto. Following the baseline analysis, a national 5-year infrastructure improvement project was launched in December 2022, tracked live through a Power BI dashboard, and completed in December 2027.

**Problem Statement:**  
National leadership (President Aziza Naledi) needed to understand the current state of water access, how many people lacked basic water, and how much it would cost to resolve the crisis at both national and provincial levels. Once the project was underway, leadership also needed live visibility into spend vs. budget, project progress by town, and vendor performance — so that decisions could be made in real-time rather than at year-end reviews.

**Approach:**  
Survey data was modeled in Power BI across multiple relational tables. Phase 1 used DAX to classify water sources against UN standards, calculate adjusted improvement costs, and measure baseline access rates. Phase 2 introduced time-aware DAX measures for cumulative cost and budget tracking, progress metrics responsive to both location and date filters, and a vendor analysis page. The dashboard refreshed automatically each time new data was loaded — no manual rebuilding required.

**Outcome:**  
A multi-page Power BI report covering a national overview, five provincial drill-through pages, an interactive project tracker, a cost analysis page, and a vendor benchmarking page. Final data (December 2027) confirms 100% project completion: all 25,398 improvements finished, 18M people brought to basic water access, and a total actual spend of **$154.5M** against a budget of **$109.3M** (+41.4% overspend, primarily driven by underestimated rural access costs in Sokoto).

---

## 📊 Objectives

- **Primary Objective:** Build an interactive Power BI dashboard enabling President Naledi and provincial leaders to understand water access status and make budget allocation decisions.
- **Secondary Objective 1:** Classify all water sources in Maji Ndogo as "Basic Access" or "Below Basic Access" using UN water quality standards.
- **Secondary Objective 2:** Calculate the total and province-level cost of all required infrastructure improvements, adjusting for rural vs. urban cost differences.
- **Secondary Objective 3:** Design province-specific drill-through pages so local leaders can explore data relevant only to their region.
- **Secondary Objective 4:** Track project progress and cumulative spend vs. budget over time, with measures that respond correctly to both location and date filters simultaneously.
- **Secondary Objective 5:** Analyse vendor performance to identify cost drivers and surface route optimisation opportunities that could reduce budget overruns.

---

## ⚙️ Project Scope & Tools

### Scope

| Dimension         | Details |
|-------------------|---------|
| **In Scope**      | All five provinces (Akatsi, Amanzi, Hawassa, Kilimani, Sokoto); water source types, queue times, well pollution results, population counts, infrastructure improvement costs, vendor actuals, project completion tracking |
| **Out of Scope**  | Historical trend data prior to the survey; individual household-level identifiers; external economic or climate data |
| **Time Period**   | Baseline water survey (point-in-time snapshot) + project tracking Dec 2022 → Dec 2027 |
| **Granularity**   | Water source level; aggregated to town, province, and national level for reporting |

### Tools & Technologies

| Category          | Tool(s) Used |
|-------------------|-------------|
| Data Storage      | Power BI data model (imported tables from Excel) |
| Data Processing   | Power Query (M), DAX calculated columns and measures |
| Analysis          | DAX — `CALCULATE`, `FILTER`, `ALLEXCEPT`, `COUNTROWS`, `IF`, `AND`, `OR`, `CONTAINSSTRING`, `ISBLANK` |
| Visualization     | Power BI Desktop — shape maps, KPI visuals, bar/donut charts, card visuals, key influencer charts, tables, bookmarks, date slicers |
| Version Control   | Git / GitHub |
| Documentation     | Markdown |

---

## 📂 Repository Structure
maji-ndogo-water-access/
│
├── data/
│ ├── raw/ # Original survey data (Md_water_services_data.xlsx) — never edited
│ └── processed/ # Cleaned tables used in the Power BI model
│
├── reports/
│ └── Maji_Ndogo.pbix # Power BI report file
│
├── visuals/ # Dashboard screenshots — one per report page
│ ├── Nations_Report.png
│ ├── Water_Source.png
│ ├── Queue.png
│ ├── Pollution.png
│ ├── Crime_Related.png
│ └── Sokoto_Report.png
│
├── docs/
│ ├── Part_3.pdf # Presidential brief & original project instructions
│ └── Part_4_1.pdf # Dalila's live project narrative (full 5-year arc)
│
└── README.md # You are here


---

## 📈 Data Workflow

1. **Source:** Multi-table dataset from the Maji Ndogo national water survey — water source records, visit logs with queue times, well pollution results, location data, improvement plans, infrastructure cost estimates, and vendor cost actuals.
2. **Ingestion:** Tables imported into Power BI Desktop and connected via a star-schema-style data model with `source_id` as the primary key across most tables.
3. **Cleaning:** Improvement categories consolidated using DAX `CONTAINSSTRING` — e.g., all "Install N taps nearby" variants grouped into "Install public tap(s)", and "Diagnose local infrastructure" renamed to "Repair infrastructure" for report clarity.
4. **Phase 1 Transformation:** `Average_queue_time` averaged across all visits per source; `Basic_water_access` applied UN classification logic; `Rural_adjusted_cost` applied a ×1.5 uplift for rural sources; `Budgeted_improvement_cost` looked up rural/urban cost per improvement type.
5. **Phase 2 Transformation:** Time-aware measures built using `ALLEXCEPT` and date comparisons so that `pct_project_complete` and `total_population` respond correctly to town and province filters without being disrupted by the date slicer. `cumulative_budget` and `cumulative_cost` sum values for all completed projects up to the selected date, with `ISBLANK` guards to exclude null completion dates.
6. **Output:** A single `.pbix` file that updated automatically when the source Excel was overwritten with newer data — no manual recalculation required.

---

## 📉 Key Insights

1. **Initial Water Access:** Only 34% of Maji Ndogo's 27.6M people had basic water access at the project start, rising to 48% in the first year. By 2027, 100% access was achieved.
2. **Sokoto Overspend:** Sokoto finished ~40% over budget — rural geography was systematically underpriced.
3. **Total National Overspend:** The total overspend reached $45.2M (+41.4% above budget).
4. **High-Cost Improvements:** RO filters and well drilling drove more than half of the total project cost.
5. **Weekend Queue Time:** Saturday queue times were three times longer than weekdays, highlighting shared tap capacity issues.
6. **Vendor Performance:** Some vendors charged significantly more, but their performance (completion rate) justified the cost.

---

## 🏗️ Future Enhancements

- Replace the flat 50% rural uplift with province-specific cost multipliers.
- Add a cost-per-person-helped metric to enable direct ROI comparison across improvement types and provinces.
- Incorporate seasonal water availability data to refine queue time classifications.
- Develop a vendor job-routing recommendation tool to reduce travel costs.
- Publish to Power BI Service with row-level security (RLS) configured per province.
- Add a cost-vs-impact scatter plot to help leadership identify the highest-ROI improvements.

---

## 🧑‍💻 Author

**[Farinde Olasile Lateefat]**  
Data Analyst  

- 🔗 [LinkedIn Profile](https://www.linkedin.com/in/olasile/)
- 💼 [GitHub Profile](https://github.com/shileyzee22)
- 📧 [Email - olasileopeyemi3079@gmail.com]

---
*Built as part of the ExploreAI Data Science programme · Last updated: April 2026*
