# Maji Ndogo Water Access & Improvement Tracker

## 📚 Table of Contents

1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Visuals](#8-visuals)
9. [Key Insights](#9-key-insights)
10. [Recommendations](#10-recommendations)
11. [Assumptions & Limitations](#11-assumptions--limitations)
12. [Future Enhancements](#12-future-enhancements)
13. [Deliverables](#13-deliverables)
14. [Author](#14-author)
---

## 1.Project Overview

This repository contains the Power BI dashboard for tracking the **Maji Ndogo Water Improvement Project**, a nationwide initiative aimed at improving access to clean water for millions of underserved people across Maji Ndogo. The project focuses on infrastructure improvements such as drilling wells, installing water filtration systems, and addressing water quality issues. The goal is to reach 100% access to safe water sources in the region.

## 2.Objectives

- **Key Objectives:**
  - Track and visualize the progress of water improvement projects across different provinces.
  - Monitor costs against the planned budget and ensure project efficiency.
  - Analyze the impact of improvements on the population's access to clean water.

## 3. Project Scope & Tools

The project’s scope involves several key tools for analysis and reporting:
- **Power BI**: For interactive data visualization and reporting.
- **DAX (Data Analysis Expressions)**: For calculating key metrics such as cumulative costs and completion percentages.

## 4. Repository Structure
 
```
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
└── README.md                 # You are here

```

---

## 5. Data Workflow

1. **Source**: Multi-table dataset from the Maji Ndogo national water survey — water source records, visits, well pollution, location data, improvement plans, vendor costs, and project progress.
2. **Ingestion**: Tables imported into Power BI Desktop, connected using shared keys like `source_id`.
3. **Cleaning**: Standardized improvement categories (e.g., "Install RO filter", "Install public tap") using DAX.
4. **Phase 1**: Created columns for `Average_queue_time`, `Basic_water_access`, `Rural_adjusted_cost`, and `Budgeted_improvement_cost`.
5. **Phase 2**: Time-aware DAX measures for project tracking: `pct_project_complete`, `cumulative_budget`, `cumulative_cost`.
6. **Output**: A live Power BI report that updates automatically when new data is uploaded — no manual recalculation necessary.

---

## 6. Data Model & Schema

### Key Tables:

- **`water_source`**: Contains records of water sources, their types, and population served.
- **`project_progress`**: Tracks progress of improvements, costs, completion dates, and vendor information.
- **`infrastructure_cost`**: Holds cost data for different improvement types, including rural and urban cost adjustments.
- **`vendor`**: Vendor information for performance tracking and cost benchmarking.

---

## 7. Analysis & Metrics

### Key Metrics:

| Metric                         | Definition | Baseline → Final |
|---------------------------------|-----------|-----------------|
| `Basic_water_access %`          | % of population with access to clean water | 34% → 100% |
| `pct_project_complete`          | % of completed improvements vs. total planned | 0% → 100% |
| `cumulative_cost`               | Running total of actual spend | $0 → $154.5M |
| `cumulative_budget`             | Running total of budgeted spend | $0 → $109.3M |
| `population_with_basic_access`  | Total number of people with basic water access | 0 → 18M |
| `number_completed_projects`     | Number of improvements completed | 0 → 25,398 |

### Key DAX Measures

```dax
-- Total improvements in a given town
total_improvements = CALCULATE(COUNTROWS('project_progress'), ALLEXCEPT('project_progress', 'project_progress'[town]))

-- Percentage of projects completed
pct_project_complete = DIVIDE(number_completed_projects, total_improvements, 0)

-- Cumulative cost
cumulative_cost = CALCULATE(SUM('project_progress'[cost]), FILTER(ALL('project_progress'[date_of_completion]), 'project_progress'[date_of_completion] <= MAX('project_progress'[date_of_completion]) && NOT(ISBLANK('project_progress'[date_of_completion]))))

 
```

## 8. Visuals
### Maji Ndogo Water Access dashboard

![Maji Ndogo Water Access National](https://github.com/shileyzee22/-change-in-Maji-Ndogo/blob/f278977f52cd7008f5d5444907f9949d91defad8/visuals/National.png)
![Maji Ndogo Water Access Vendors](https://github.com/shileyzee22/-change-in-Maji-Ndogo/blob/f278977f52cd7008f5d5444907f9949d91defad8/visuals/Vendors.png)
![Maji Ndogo Water Access Key Indication](https://github.com/shileyzee22/-change-in-Maji-Ndogo/blob/f278977f52cd7008f5d5444907f9949d91defad8/visuals/key%20Indication.png)
![Maji Ndogo Water Access Sokoto Report](https://github.com/shileyzee22/-change-in-Maji-Ndogo/blob/f278977f52cd7008f5d5444907f9949d91defad8/visuals/Sokoto.png)
![Maji Ndogo Water Access Akatis Report](https://github.com/shileyzee22/-change-in-Maji-Ndogo/blob/f278977f52cd7008f5d5444907f9949d91defad8/visuals/Akatis.png)


*Above: Screenshot of the interactive Power BI dashboard.*

---


## Key Insights

1. **Access to Water**: Only 34% of Maji Ndogo's population currently has access to clean water.
2. **Budget Allocation**: **Kilimani** and **Sokoto** require the largest share of the budget.
3. **Cost Drivers**: RO filter installation and well drilling account for over half the project’s total cost.

## Recommendations

| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | Prioritize **Kilimani** and **Sokoto** in the next budget release. | Budget Analysis | National Treasury |
| Medium | Plan procurement for **RO filter installations** and **well drilling** contractors. | Cost Analysis | Infrastructure Team |
| Low | Track **Basic Water Access %** monthly per province. | Post-project monitoring | Data Team |

## Assumptions and Limitations

### Assumptions:
- Survey data is assumed to be complete and accurate.
- Well contamination types are classified correctly.

### Limitations:
- Dataset represents a snapshot and does not account for seasonal variations.
- Cost estimates may vary based on actual procurement rates.

## Future Enhancements

- **Progress Tracking**: Add live updates to the `Basic_water_access %`.
- **Seasonal Adjustments**: Incorporate seasonal data to adjust queue times.
- **Cost vs. Impact Analysis**: Add scatter plots for ROI (people served per dollar spent).

## Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| Power BI Report (.pbix) | Interactive dashboard showing progress and cost analysis. | `/reports/` |
| Dashboard Screenshots | Visual previews of the dashboard. | `/visuals/` |
| Data Dictionary | Field-level descriptions for all tables. | `/data/raw/Md_water_services_data.xlsx` |
| Docs | Documentation for team and project management. | `/docs/` |
| README | Full project documentation (this file). | `/README.md` |

## Author

**[Farinde Olasile Lateefat]**  
Data Analyst

- 🔗 [LinkedIn Profile](https://www.linkedin.com/in/olasile/)
- 💼 [GitHub Profile](https://github.com/shileyzee22)
- 📧 Email: [olasileopeyemi3079@gmail.com]
