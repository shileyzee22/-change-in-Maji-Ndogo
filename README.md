# Maji Ndogo Water Access & Improvement Tracker

> *Analysed water access across 27.6M people in five provinces of Maji Ndogo to identify infrastructure gaps, quantify improvement costs, and delivered a decision-ready Power BI report for national and provincial leadership — then tracked a 5-year infrastructure improvement project through to 100% completion.*

---

## ⚙️ Project Type

- [x] Dashboard / Data Visualization
- [x] Exploratory Data Analysis (EDA)
- [x] Data Cleaning

---

## Table of Contents

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

## 1. Project Overview

**Context:** Maji Ndogo is a fictional nation facing a widespread water access crisis. A national survey was conducted to assess water source types, queue times, water quality, and population distribution across five provinces — Akatsi, Amanzi, Hawassa, Kilimani, and Sokoto. Following the baseline analysis, a national 5-year infrastructure improvement project was launched in December 2022, tracked live through a Power BI dashboard, and completed in December 2027.

**Problem Statement:** National leadership (President Aziza Naledi) needed to understand the current state of water access, how many people lacked basic water, and how much it would cost to resolve the crisis at both national and provincial levels. Once the project was underway, leadership also needed live visibility into spend vs. budget, project progress by town, and vendor performance — so that decisions could be made in real time rather than at year-end reviews.

**Approach:** Survey data was modelled in Power BI across multiple relational tables. Phase 1 used DAX to classify water sources against UN standards, calculate adjusted improvement costs, and measure baseline access rates. Phase 2 introduced time-aware DAX measures for cumulative cost and budget tracking, progress metrics responsive to both location and date filters, and a vendor analysis page. The dashboard refreshed automatically each time new data was loaded — no manual rebuilding required.

**Outcome:** A multi-page Power BI report covering a national overview, five provincial drill-through pages, an interactive project tracker, a cost analysis page, and a vendor benchmarking page. Final data (December 2027) confirms 100% project completion: all 25,398 improvements finished, 18M people brought to basic water access, and a total actual spend of **$154.5M** against a budget of **$109.3M** (+41.4% overspend, primarily driven by underestimated rural access costs in Sokoto).

---

## 2. Objectives

- **Primary Objective:** Build an interactive Power BI dashboard enabling President Naledi and provincial leaders to understand water access status and make budget allocation decisions.
- **Secondary Objective 1:** Classify all water sources in Maji Ndogo as "Basic Access" or "Below Basic Access" using UN water quality standards.
- **Secondary Objective 2:** Calculate the total and province-level cost of all required infrastructure improvements, adjusting for rural vs. urban cost differences.
- **Secondary Objective 3:** Design province-specific drill-through pages so local leaders can explore data relevant only to their region.
- **Secondary Objective 4:** Track project progress and cumulative spend vs. budget over time, with measures that respond correctly to both location and date filters simultaneously.
- **Secondary Objective 5:** Analyse vendor performance to identify cost drivers and surface route optimisation opportunities that could reduce budget overruns.

> 💡 *Every visual and DAX measure in this project traces back to one of these objectives.*

---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|-----------|---------|
| **In Scope** | All five provinces (Akatsi, Amanzi, Hawassa, Kilimani, Sokoto); water source types, queue times, well pollution results, population counts, infrastructure improvement costs, vendor actuals, project completion tracking |
| **Out of Scope** | Historical trend data prior to the survey; individual household-level identifiers; external economic or climate data |
| **Time Period** | Baseline water survey (point-in-time snapshot) + project tracking Dec 2022 → Dec 2027 |
| **Granularity** | Water source level; aggregated to town, province, and national level for reporting |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Storage | Power BI data model (imported tables from Excel) |
| Data Processing | Power Query (M), DAX calculated columns and measures |
| Analysis | DAX — `CALCULATE`, `FILTER`, `ALLEXCEPT`, `COUNTROWS`, `IF`, `AND`, `OR`, `CONTAINSSTRING`, `ISBLANK` |
| Visualization | Power BI Desktop — shape maps, KPI visuals, bar/donut charts, card visuals, key influencer charts, tables, bookmarks, date slicers |
| Version Control | Git / GitHub |
| Documentation | Markdown |

---

## 4. Repository Structure

```
maji-ndogo-water-access/
│
├── data/
│   ├── raw/                    # Original survey data (Md_water_services_data.xlsx) — never edited
│   └── processed/              # Cleaned tables used in the Power BI model
│
├── reports/
│   └── Maji_Ndogo.pbix         # Power BI report file
│
├── visuals/                    # Dashboard screenshots — one per report page
│   ├── Nations_Report.png
│   ├── Water_Source.png
│   ├── Queue.png
│   ├── Pollution.png
│   ├── Crime_Related.png
│   └── Sokoto_Report.png
│
├── docs/
│   ├── Part_3.pdf              # Presidential brief & original project instructions
│   └── Part_4_1.pdf            # Dalila's live project narrative (full 5-year arc)
│
└── README.md                   # You are here
```

---

## 5. Data Workflow

```
Raw Survey Data
(water_source, visits, well_pollution, location,
 project_progress, infrastructure_cost, vendor)
        │
        ▼
Loaded into Power BI via Import mode
        │
        ▼
Relationships established on shared keys
(source_id → water_source, visits, well_pollution, project_progress)
        │
        ▼
── PHASE 1: Baseline Analysis ──────────────────────────────────────
DAX Columns:   Average_queue_time | Basic_water_access
               Rural_adjusted_cost | Budgeted_improvement_cost
               Aggregated_improvements
DAX Measures:  Basic_water_access % | Improvement % | Total Budget
        │
        ▼
── PHASE 2: Live Project Tracking ──────────────────────────────────
DAX Measures:  total_improvements | number_completed_projects
               pct_project_complete | total_population
               population_with_basic_access | population_now_basic_access
               pct_population_now_basic_access
               cumulative_budget | cumulative_cost
        │
        ▼
Multi-page Power BI Report
(National overview → 5 Provincial pages → Project tracker
 → Cost analysis → Vendor benchmarking)
        │
        ▼
Data refreshed: Jan 2024 (year-one update) → Dec 2027 (final close-out)
```

**Step-by-step:**

1. **Source:** Multi-table dataset from the Maji Ndogo national water survey — water source records, visit logs with queue times, well pollution results, location data, improvement plans, infrastructure cost estimates, and vendor cost actuals.
2. **Ingestion:** Tables imported into Power BI Desktop and connected via a star-schema-style data model with `source_id` as the primary key across most tables.
3. **Cleaning:** Improvement categories consolidated using DAX `CONTAINSSTRING` — e.g., all "Install N taps nearby" variants grouped into "Install public tap(s)", and "Diagnose local infrastructure" renamed to "Repair infrastructure" for report clarity.
4. **Phase 1 Transformation:** `Average_queue_time` averaged across all visits per source; `Basic_water_access` applied UN classification logic; `Rural_adjusted_cost` applied a ×1.5 uplift for rural sources; `Budgeted_improvement_cost` looked up rural/urban cost per improvement type.
5. **Phase 2 Transformation:** Time-aware measures built using `ALLEXCEPT` and date comparisons so that `pct_project_complete` and `total_population` respond correctly to town and province filters without being disrupted by the date slicer. `cumulative_budget` and `cumulative_cost` sum values for all completed projects up to the selected date, with `ISBLANK` guards to exclude null completion dates.
6. **Output:** A single `.pbix` file that updated automatically when the source Excel was overwritten with newer data — no manual recalculation required.

---

## 6. Data Model & Schema

### `water_source`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `source_id` | string | Unique identifier per water source | `SRC-00482` |
| `type_of_water_source` | string | Source category | `shared_tap` |
| `number_of_people_served` | int | Population dependent on this source | `2,340` |
| `Average_queue_time` | float (DAX col) | Average minutes queued across all visits | `42.5` |
| `Basic_water_access` | string (DAX col) | UN classification result | `Basic Access` |

### `visits`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `source_id` | string | FK → `water_source` | `SRC-00482` |
| `time_in_queue` | int | Minutes queued on this visit | `55` |
| `visit_count` | int | Number of survey visits to this source | `3` |

### `well_pollution`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `source_id` | string | FK → `water_source` | `SRC-00210` |
| `results` | string | Pollution test outcome | `Contaminated: Biological` |

### `project_progress`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `source_id` | string | FK → `water_source` | `SRC-00482` |
| `improvement` | string | Planned upgrade type | `Install RO filter` |
| `town` | string | Town of the source | `Harare` |
| `province` | string | Province of the source | `Kilimani` |
| `date_of_completion` | date | Date improvement was marked complete | `2024-03-15` |
| `Budgeted_improvement_cost` | float (DAX col) | Rural-adjusted cost per improvement | `5,625` |
| `cost` | float | Actual vendor cost charged | `6,890` |

### `infrastructure_cost`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `improvement` | string | Improvement type | `Drill well` |
| `unit_cost_USD` | float | Base cost per improvement | `8,500` |
| `Rural_adjusted_cost` | float (DAX col) | Base cost × 1.5 for rural sources | `12,750` |

### `vendor`

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `vendor_ID` | string | Unique vendor code | `ERI893` |
| `company_name` | string | Vendor name | `Entebbe RO Installers` |
| `cost` | float | Actual cost charged per improvement | `4,850` |

> **Key relationships:**
> `visits.source_id` → `water_source.source_id`
> `well_pollution.source_id` → `water_source.source_id`
> `project_progress.source_id` → `water_source.source_id`
>
> **Project totals:** 25,398 improvements planned and completed | Budgeted: $109.3M | Actual: $154.5M | Overspend: +41.4%

---

## 7. Analysis & Metrics

### Analytical Approach

This project used a **user-story-driven design** across two phases. In Phase 1, every visual was built to answer a specific question from President Naledi (national picture) or provincial leaders (local decisions). In Phase 2, the data team lead Dalila Lesedi drove the design of a live project tracker and vendor analysis page, with user stories focused on budget accountability and cost control. The key design challenge was building DAX measures that responded correctly to both location filters and a date slicer simultaneously — without the two interfering with each other.

### Key Metrics

| Metric | Definition | Baseline → Final |
|--------|-----------|-----------------|
| `Basic_water_access %` | % of population on a UN-standard source | 34% → 100% |
| `pct_project_complete` | Completed improvements ÷ total planned | 0% → 100% |
| `pct_population_now_basic_access` | Access rate including newly completed upgrades | 34% → 100% |
| `cumulative_cost` | Running total of actual spend up to selected date | $0 → $154.5M |
| `cumulative_budget` | Running total of budgeted spend for completed projects | $0 → $109.3M |
| `Budgeted_improvement_cost` | Planned cost per improvement, +50% for rural | Varies by type |
| `People Helped` | Population moved from below-basic to basic access | 0 → 18M |

### Key DAX Measures

```dax
-- Total improvements, filtered to town only (ignores date slicer)
total_improvements =
CALCULATE(
    COUNTROWS('project_progress'),
    ALLEXCEPT('project_progress', 'project_progress'[town])
)

-- Population with basic access (measure — avoids storing a column of strings)
population_with_basic_access =
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    FILTER(
        ALL(water_source),
        OR(
            OR(
                AND(
                    'water_source'[type_of_water_source] = "well",
                    RELATED(well_pollution[results]) = "Clean"
                ),
                'water_source'[type_of_water_source] = "tap_in_home"
            ),
            AND(
                'water_source'[type_of_water_source] = "shared_tap",
                'water_source'[Average_queue_time] < 30
            )
        )
    )
)

-- Updated access % once completed improvements are counted
pct_population_now_basic_access =
DIVIDE(
    population_with_basic_access + population_now_basic_access,
    total_population
)

-- Cumulative actual spend up to selected date, blanks excluded
cumulative_cost =
CALCULATE(
    SUM('project_progress'[cost]),
    FILTER(
        ALL('project_progress'[date_of_completion]),
        'project_progress'[date_of_completion] <=
            MAX('project_progress'[date_of_completion]) &&
        NOT(ISBLANK('project_progress'[date_of_completion]))
    )
)
```

### Methods Used

- UN water access classification framework applied to categorise all sources (clean well / home tap / shared tap with queue < 30 min = Basic Access; river / broken tap / contaminated well = Below Basic)
- `ALLEXCEPT` to make progress measures town-filter-aware without being disrupted by the date slicer
- KPI visual configured with "Low is good" trend direction to immediately flag budget overruns in red
- Key Influencers visual to identify cost drivers — job type, rural/urban split, province, and project duration all surfaced as statistically significant factors
- Vendor benchmarking adjusted for geography and improvement type before comparing average costs — preventing misleading comparisons between teams working in very different conditions
- Shape map with `pct_project_complete` as colour saturation, dynamically updated across three data snapshots (project start, Jan 2024, Dec 2027)
- Bookmark toggles to switch between province-level and improvement-type budget views on the cost analysis page

---

## 8. Visuals

### Maji Ndogo Water Access Dashboard

![National Report](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Nations%20Report.png)

*National overview — KPI cards, province budget breakdown, shape map, and access rate summary.*

![Water Source](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Water%20Source.png)

*Water source breakdown by type, population served, and UN classification.*

![Queue Analysis](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Queue.png)

*Queue time by day of week — Saturday peak (246 min) vs. weekday average (42–60 min).*

![Pollution](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Pollulation.png)

*Well contamination split — 40.8% chemical, 30.9% biological, 28.3% clean.*

![Crime Related](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Crime%20Related.png)

*Crime data overview — included for context; flagged for deeper analysis in a future phase.*

![Sokoto Provincial Report](https://github.com/shileyzee22/Maji-Ndogo/blob/09dd2a7b2c40d9c638147e6b20b08070cabc9a66/visuals/Sokoto%20Report.png)

*Sokoto drill-through page — province-specific KPIs, cost breakdown, and completion timeline.*

---

## 9. Key Insights

**Insight 1: Only 34% of Maji Ndogo's 27.6M people had basic water access at the project start**
Roughly 18 million people were relying on unimproved or unsafe sources. By January 2024 (one year in), access had risen to 48%. By December 2027 the project was complete — 100% of the 28M population now has access to basic water, with 18M people directly helped by the improvement programme.

**Insight 2: Sokoto finished ~40% over budget — rural geography was systematically underpriced**
The flat 50% rural cost uplift proved insufficient for Sokoto's terrain and road conditions. The province accounted for 29% of total actual spend ($44.85M) and was nearly 40% over its provincial budget by year one. Every single province exceeded its budgeted allocation by project end.

**Insight 3: Total national overspend reached $45.2M (+41.4% above budget)**
Actual spend hit $154.5M against a $109.3M budget. The KPI tracker flagged the overrun early in year one, triggering the cost investigation that ultimately led to the vendor benchmarking and geography analysis — a clear example of the dashboard earning its keep.

**Insight 4: RO filter installations and well drilling drove more than half of total spend**
Install RO filter (7,093 upgrades) and Drill well (3,379 upgrades) together accounted for over 50% of total budget. These two categories carry the highest unit costs and longest lead times and should anchor any future procurement plan from day one.

**Insight 5: Saturday queue times were three times longer than any weekday**
Saturday averaged 246 minutes vs. 42–60 minutes on weekdays. Shared tap capacity was severely over-stretched on weekends — an issue completely hidden when looking at average daily queue times rather than a day-of-week breakdown.

**Insight 6: The most expensive vendors were working in the hardest conditions — not overcharging**
Initial analysis flagged several vendors as expensive outliers. Contextualising by province and improvement type revealed that the four most expensive drilling teams were operating exclusively in rural Sokoto and Kilimani. Entebbe RO Installers (ERI893), initially the most expensive filter installer nationally, proved to be the cheapest when compared only against teams in comparable rural conditions — and completed the most projects of any purification team. Vendors who clustered jobs geographically rather than travelling across provinces consistently achieved lower per-project costs.

---

## 10. Recommendations

| Priority | Recommendation | Evidence | Owner |
|----------|---------------|----------|-------|
| 🔴 High | Replace the flat 50% rural uplift with province-specific cost multipliers for future national project budgets — Sokoto requires a significantly higher adjustment | Insight 2 — Sokoto 40% over budget | National Treasury / Planning |
| 🔴 High | Begin procurement for RO filter and well drilling contractors at project inception — these categories have the longest lead times and represent over 50% of spend | Insight 4 — improvement cost breakdown | Infrastructure / Supply Chain |
| 🟡 Medium | Share data-driven route-optimisation guidance with vendors showing how clustering nearby jobs reduces travel cost and increases throughput | Insight 6 — vendor geography analysis | Data team / Vendor management |
| 🟡 Medium | Redesign shared tap capacity at highest-queue sources — Saturday peaks show current infrastructure cannot meet weekend demand | Insight 5 — Saturday queue anomaly | Provincial leaders / Operations |
| 🟢 Low | Publish to Power BI Service with row-level security (RLS) so each provincial leader views only their own province's data | Governance best practice | BI team |

---

## 11. Assumptions & Limitations

### Assumptions

- Survey data is assumed complete and representative for all five provinces — no cross-validation against an external population register was performed.
- The 50% rural cost uplift is a planning estimate from project managers; actual procurement costs varied significantly, particularly in Sokoto.
- Well contamination classifications (chemical vs. biological) are treated as accurate and final with no re-testing or margin of error incorporated.
- Queue times were averaged across multiple visits per source, assuming visit timing was representative of typical usage patterns.
- Vendor cost comparisons assume improvements are broadly comparable within each aggregated improvement type — granular scope differences between contracts are not modelled.

### Limitations

- The baseline dataset is a point-in-time snapshot and does not capture seasonal variation in water availability, queue length, or contamination levels.
- The analysis cannot distinguish between wells contaminated by natural geology vs. human activity (e.g., agricultural runoff), which may affect long-term remediation strategy.
- Crime data is included in the dashboard but not deeply analysed — the relationship between crime patterns and water collection behaviour (particularly for women and children) is noted but not quantified.
- The 41.4% budget overspend was identified in arrears; the planning model lacked mechanisms to flag escalating costs in high-difficulty provinces early enough for year-one corrective action.
- Provincial cost totals assume uniform per-unit costs within each improvement type — economies of scale or local procurement differences between vendors are not captured at the planning stage.

---

## 12. Future Enhancements

- [ ] Build a province-specific cost model that replaces the flat rural multiplier with terrain and accessibility variables — improving budget accuracy for future national infrastructure projects
- [ ] Add a cost-per-person-helped metric to enable direct ROI comparison across improvement types and provinces
- [ ] Incorporate seasonal water availability data to refine queue time classifications — a source with < 30 min average queue may still be inadequate during dry season
- [ ] Develop a vendor job-routing recommendation tool that surfaces nearby available improvements, reducing travel days and improving throughput
- [ ] Analyse the crime dataset in depth to quantify safety risk by province and time of day, specifically as it affects water collection by women and children
- [ ] Publish to Power BI Service with row-level security (RLS) configured per province
- [ ] Add a cost-vs-impact scatter plot to help leadership identify the highest-ROI improvements (most people served per dollar spent)

---

## 13. Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| Power BI Report (.pbix) | Multi-page interactive dashboard — national overview, 5 provincial drill-throughs, project tracker, cost analysis, vendor benchmarking | [`/reports/`] |
| Dashboard Screenshots | Visual previews of all key report pages | [`/visuals/`] |
| Raw Data | Original survey Excel file — water sources, visits, pollution results, project progress, vendor actuals | [`/data/raw/Md_water_services_data.xlsx`] |
| Project Brief | Presidential instructions and scope document | [`/docs/Part_3.pdf`] |
| Project Narrative | Dalila's live commentary covering the full 5-year project arc | [`/docs/Part_4_1.pdf`] |
| README | Full project documentation (this file) | [`/README.md`] |

---

## 14. Author
 
**[Farinde Olasile Lateefat]**
Data Analyst
 
- 🔗 [https://www.linkedin.com/in/olasile/]
- 💼 [[GitHub Profile URL](https://github.com/shileyzee22)]
- 📧 [Email - olasileopeyemi3079@gmail.com]
---
 

*Built as part of the ExploreAI Data Science programme · Last updated: April 2026*

---
