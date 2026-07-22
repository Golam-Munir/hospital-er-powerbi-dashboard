# Hospital Emergency Room Analytics Dashboard (Power BI)

An end-to-end Power BI project analyzing emergency room operations: patient flow, wait times, referrals, and satisfaction, built as a structured self-learning exercise to strengthen data modeling and DAX skills for a Data Engineering / BI career transition.

---

## Problem

Hospital ER stakeholders need a fast, visual way to answer operational questions:

- Are patients being seen within target wait times?
- Which departments receive the most referrals, and when?
- How does patient satisfaction relate to wait times?
- When are peak admission periods, by day and hour?

Raw ER visit data (~9,200 rows, 12 columns, April 2023 to October 2024) on its own answers none of these questions. It needed structure, relationships, and a reporting layer.

## Dataset at a glance

~9,216 rows · 12 source columns · April 2023 to October 2024

| Field | Description |
|---|---|
| Patient ID | Unique identifier per visit |
| Patient Admission Date | Date/time of ER arrival |
| Patient Name | First initial + last name |
| Patient Gender | Male / Female / Not Confirmed |
| Patient Age | Age at admission |
| Patient Race | Self-reported race/ethnicity |
| Department Referral | Specialty referred to, or None |
| Patient Admission Flag | True/False, admitted beyond ER |
| Patient Satisfaction Score | Self-reported rating, some missing |
| Patient Wait Time | Minutes from arrival to first contact |
| Patient CM | Case manager assigned (0/1) |

## Approach

Built a four-view Power BI solution on top of a single fact table and a custom date dimension:

1. **Monthly View** — KPIs and visuals filtered to a selected month (admission status, age bands, referrals, wait-time performance, gender/race mix, day-by-hour heatmap).
2. **Consolidated View** — Same KPI logic over a user-selected date range for flexible period analysis.
3. **Patient Details** — Row-level, filterable, exportable table for drill-down.
4. **Key Takeaways** — Narrative summary page of the main findings for non-technical stakeholders.

**Data modeling:**
- Built a dedicated Date table via DAX (`CALENDAR`, min/max of admission date) with Day Name, Week Day, Month Name, Year, Month & Year, and Month Number, related 1-to-many to the fact table on a cleaned, time-stripped admission date.
- Power Query cleanup: concatenated patient names, relabeled gender codes (M/F/NC → Male/Female/Not Confirmed), applied conditional formatting for readability, checked column profiles for missing values (e.g., satisfaction score gaps).
- Calculated columns: Admission Status (from a True/False admission flag), 10-year Age Group bands (via `SWITCH`), Waiting Time Status (30-minute target threshold), and a 2-hour Time Interval bucket (e.g., "12 PM - 2 PM") used to drive the day-by-hour traffic analysis.
- Core DAX measures: distinct patient count, average wait time, average satisfaction score, referred-patient count, all with daily-trend area sparklines for pattern spotting within a selected period.

**Environment:** Built on macOS, running Power BI Desktop inside a Windows 11 VM (VMware Fusion), since Power BI Desktop has no native Mac version.

## Challenges and how I solved them

| Challenge | How I solved it |
|---|---|
| No native Power BI Desktop for Mac; considered buying a Windows laptop | Researched alternatives, set up VMware Fusion with Windows 11, and tuned RAM/SSD allocation for acceptable performance without new hardware spend |
| DAX syntax was unfamiliar, especially `SWITCH` for age banding and wait-time logic | Worked through it step by step, used AI as a second pair of eyes to review and optimize DAX, and made sure I could explain each measure in plain English before moving on |
| Understanding why a separate Date table is necessary | Learned this is what enables correct time intelligence and consistent filtering across a data model, rather than relying on date columns embedded in the fact table |
| Power Query transformations (name concatenation, gender relabeling, conditional formatting) | Practiced each transformation individually and verified results against the source data before applying to the full model |
| Data model relationships (1-to-many join between Date and fact table) | Found this the most difficult but most valuable concept; solidified my understanding of dimensional modeling fundamentals |

## What I learned

- Why a separate date dimension is foundational to reliable time-based reporting, not just a nice-to-have.
- Practical DAX: `SWITCH` for banding, calculated columns vs. measures, and how to sanity-check DAX output.
- Data modeling fundamentals: 1-to-many relationships, fact/dimension structure, and why this scales better than flat, single-table reporting.
- Power Query as a real data-cleaning layer, not just a formatting step.
- How to troubleshoot and adapt a development environment (Mac + VM) when the standard tooling doesn't fit your setup.

## Tech stack

Power BI Desktop · DAX · Power Query (M) · Data Modeling · VMware Fusion (Windows 11 on macOS)

## Dashboards

Screenshots below (see `/images` folder).

**Monthly View**
![Monthly View](images/monthly-view.png)

**Consolidated View**
![Consolidated View](images/consolidated-view.png)

**Patient Details**
![Patient Details](images/patient-details.png)

**Key Takeaways**
![Key Takeaways](images/key-takeaways.png)

## Future improvements

- Add time intelligence: YTD, rolling averages.
- Parameterize the wait-time target (currently fixed at 30 minutes).
- Simulate row-level security (RLS) for different stakeholder groups.
- Integrate with a cloud data warehouse or lakehouse (Azure/Fabric) instead of a flat file source.

## About this project

Built independently as a structured learning and portfolio project using a publicly available hospital ER dataset, to practice real-world Power BI development: data modeling, DAX, and dashboard design end to end.

**Author:** Golam Munir
**GitHub:** [github.com/Golam-Munir](https://github.com/Golam-Munir)
