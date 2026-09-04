# README

#HR People Analytics Dashboard: Employee Demographics

## Overview

An Excel-based HR analytics dashboard providing date-parameterized, live headcount calculation visibility across department, gender, age, tenure and recruitment source, with additional of an outcome analysis exploring performance rating relationships with satisfaction and tenure. Built using Power Query and PivotTables using rhuebner HR dataset (210 employee records).

## Positioning

Part of a broader pattern across this portfolio: the same core toolkit (Excel + Power Query) applied to distinct HR probles for data transformation and governance (Manpower Planning Tool), process/status monitoring (Performance Campaign Monitoring & Targeted Nitification Workflow), and this analysis (performance-satisfaction-tenure correlation). Each project applies the toolkit based on the problem it solves.

## Dataset

- Source: https://www.kaggle.com/datasets/rhuebner/human-resources-data-set by Rhuebner
- 210 employee records
- Fields use: HC, Department, Sex, Age Group, Tenure Group, Recruitment Source, Performance Score, Employee Satisfaction (1-5) Hire/Termination Date

## Dashboard Design

**Date-driven active headcount**

The dashboard is using ‘Date’ as the parameter to calculate headcount figures to reflect employee active as of the selected date, instead of a static snapshot. This lets the dashboard to answer the current headcount look like on any given date.

**Department cross-filter**

A single Department slicer to filter all charts and tables views on the dashboard.

**Views Included**

- Headcount overview (total, gender split)
- Department breakdown, with gender split
- Age Groyp distribution, with gender split
- Tenure Group distribution, with gender split
- Recruitment Source breakdown, with gender split
- Performance Score x Employee Satisfaction and x Tenure Group (See the **Findings** section below)

## Technical Decisions

**Power Query vs Raw Formula**

The ‘Demographic Data’ sheet includes formula-based headcount calculations, referencing the raw columns from the transformed HR dataset by Power Query.

This was a deliberated choice, not a redudancy to let reviewer verify the Power Query transformation logic against a transparent raw-formula baseline as the logic comparison between Power Query and cells formula.

**Why Power Query for the live dashboard:**

- Centralized transformation logic (categorization into Age Group, Tenure Group as a calculated columns) that can be used to create a data model with multiple data sources
- Support PivotTable for Dashboard design with chart and centralized data filter using Slicer

**File path configuration**

The workbook file or folder location references using a ‘Path’ field (e.g. ‘/Folder Name/people-analytics/’) used by Power Query’s data source connection. If you download this file, update this path to match local folder structure before refreshing to locate the source data.

- Excel for Mac note: Privacy Level settings are unavailable on Mac. Go to Power Query Editor → Home → Options → Project Options → enable “Allow Combining data from multiple sources.”

## Methodolgy (Performance Analysis)

**Why rank correlation:** Performance Score and the comparison data (Tenure Group and Employee Satisfaction) are already using a rank value, not true numeric intervals. Using rank-based correlation with Spearman-style ranked values via ‘CORREL()’ formula is the appropriate method for this data type.

Spearman/rank correlation capture monotonic trends only

**Steps:**

1. Ranked categorial fields: Performance Score (PIP=1 → Exceeds=4), Tenure Group (<1 Year = 1 → >5 Years = 5), Employee Satisfaction ( 1 → 5)
2. ‘CORREL()’ applied to ranked columns via HR dataset references
3. PivotTables with 100% stacked bar chart to visualize distribution shifts across performance categories

**Interpretation framework:**

Cohen (1988) rules: r > 0.1 very small, 0.1 ≤ r < 0.3 small, 0.3 ≤ r < 0.5 moderate, r ≥ 0.5 large.

## Findings (Performance Analysis)

| Relationship | Correlation (r) | Interpretation |
| --- | --- | --- |
| Performance vs Satisfaction | 0.30 | Small-to-moderate  |
| Performance vs Tenure | 0.05 | Very small |

**Performance vs Satisfaction**

Lower-performing employees (PIP) show the only representation with the lowest satisfaction score, meanwhile higher-performing categories (Exceed, Fully Meets) show almost no representation for satisfaction below level 3. There is correlation but it is weak as the dataset concentrated mostly in the higher-performing categories for data spreads trend.

**Performance vs Tenure:**

Very samll relationship between performance score with how long employee has been with the company. Tenure data distribution largely concentrated in long-tenured employees which dominate every performance categories, which makes it not affecting the performance rating directly.

## Repository Structure

- People Analytics.xlsx #main workbook (Dashboard, Demographic Data, HR Dataset sheets)
- README.md

## Tech Stack

- Excel for mac with Power Query and PivotTables (no Power Pivot/DAX support on Mac)
- Spearman-style rank correlation via ‘CORREL()’
- Date as parameter for live headcount calculation
- Chart and tables cross-filtering using Department Slicer
- Excel formula for Demgraphic data sheet