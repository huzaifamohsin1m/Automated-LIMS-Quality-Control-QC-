# Digital LIMS: Automated Quality Control & Statistical Process Control (SPC) Analytics Pipeline

> **An end-to-end Automated Laboratory Information Management System (LIMS) designed for high-throughput fertilizer manufacturing (Urea & Ammonia Synthesis).**  
> *Bridging wet-lab analytical chemistry, Python-driven Statistical Process Control (SPC), and executive Power BI monitoring.*

---

## Main Purpose: Why This Project Was Built

In continuous chemical synthesis complexes (such as Urea and Ammonia production units operated by industrial giants like FFC, Engro, and Fatima), the Quality Control (QC) laboratory is the central operational checkpoint. Chemical analysts continuously run shift tests on critical process streams.

When plant quality control relies on **manual paper logs or basic spreadsheets**, three severe operational risks occur:
1. **Delayed Anomaly Detection:** Out-of-Specification (OOS) trends go unnoticed for hours while production keeps running.
2. **High Re-Processing & Scrap Costs:** If an off-spec batch reaches a main storage silo, thousands of tons of final product get contaminated, costing millions of rupees in re-processing.
3. **Human Error:** Manual entry and hand-calculated statistical process control metrics lead to errors during shift handovers.

### The Digital Solution
This repository presents a **Digital LIMS Pipeline** that automates the entire QC workflow. It ingests raw multi-parameter shift data, programmatically checks specifications, calculates real-time **Process Capability ($C_{pk}$)**, plots **3-Sigma Shewhart Control Charts**, and feeds an interactive **Power BI Executive Dashboard** for instant plant-wide visibility.

---

## How to Use the System (Operational Workflow)
The complete end-to-end architecture flows seamlessly across four integrated layers:

```text
┌────────────────────────────────┐
│   Industrial Laboratory Data   │  Step 1: Ingest Shift Logs
│ (3 Shifts/Day, 1,095 Records) │  (Biuret, Moisture, pH, Prill Size, Ammonia)
└───────────────┬────────────────┘
                │
                ▼
┌────────────────────────────────┐
│  Python Data Engine (Jupyter)  │  Step 2: Automated Quality & SPC Engine
│  - Multi-Parameter Validation  │  - Programmatic threshold checks
│  - 3-Sigma Control Limits      │  - Shewhart control charts & Cpk calculations
│  - Cpk Process Capability      │
└───────────────┬────────────────┘
                │
                ▼
┌────────────────────────────────┐
│ Enriched Export Dataset (.csv) │  Step 3: Export Enriched Data
│  (lims_processed_powerbi_data) │  - PASS/FAIL flags & violation tags
└───────────────┬────────────────┘
                │
                ▼
┌────────────────────────────────┐
│    Power BI Control Dashboard  │  Step 4: Executive Monitoring
│  - Real-time Pass Rate KPIs    │  - Interactive shift/analyst slicers
│  - Failure Mode Analysis       │  - Parameter drift trendlines
└────────────────────────────────┘
---

## Technology Stack & Operational Rationale

| Tool / Technology | Primary Purpose | Why It Was Chosen / Industrial Value |
|:------------------|:----------------|:--------------------------------------|
| **Python (Pandas, NumPy)** | Automated Data Pipeline | Fast programmatic validation of multi-parameter chemical threshold checks, replacing manual data entry error. |
| **SciPy & Matplotlib** | Statistical Quality Control (SQC) | Enables calculation of Cpk, process capability indices and generates publication-grade 3-Sigma X-bar Shewhart control charts. |
| **Power BI Desktop** | Executive Visual Dashboard | Translates complex laboratory data into an interactive control-room interface with dynamic slicers for Plant Managers and QC Heads. |
| **DAX Measures** | Dynamic KPI Aggregation | Ensures mathematically precise KPI calculations (e.g., distinguishing simple parameter averages from weighted batch compliance rates). |

---

## Analytical Chemistry & Quality Methodology

The pipeline evaluates five critical laboratory parameters mapped directly to industrial fertilizer synthesis standard operating procedures (SOPs):

| # | Parameter | Specification | Rationale |
|:--|:----------|:--------------|:----------|
| 1 | **Urea Biuret Content** | `< 1.00%` USL | Monitors toxic side-product dimerization (C₂H₅N₃O₂) resulting from high reactor synthesis temperatures. High biuret damages crop foliage. |
| 2 | **Moisture Content** | `< 0.30%` USL | Prevents prill caking, lumping, and mechanical degradation during storage silo transfers and bagging operations. |
| 3 | **Prill Size Distribution** | `> 90.0%` in 1.0mm – 2.8mm | Ensures uniform physical application characteristics for broad-caster fertilizer application. |
| 4 | **Ammonia Stream Purity** | `> 99.5%` LSL | Guarantees stoichiometric efficiency in the carbamate formation stage. |
| 5 | **Boiler Water pH** | 8.5 – 9.5 Target Window | Strictly controlled in high-pressure steam generation boilers to prevent acidic line corrosion (`< 8.5`) or alkaline scaling (`> 9.5`). |

---

## Statistical Process Control (SPC) Formulas Applied

### Process Capability Index (Cpk)

\[
C_{pk} = \min \left( \frac{\text{USL} - \mu}{3\sigma}, \frac{\mu - \text{LSL}}{3\sigma} \right)
\]

### 3-Sigma Control Limits

\[
\text{UCL} = \mu + 3\sigma, \quad \text{LCL} = \mu - 3\sigma
\]

Where:
- **USL** = Upper Specification Limit
- **LSL** = Lower Specification Limit
- **μ** = Process Mean
- **σ** = Process Standard Deviation
- **UCL** = Upper Control Limit
- **LCL** = Lower Control Limit

---

## Key Performance Indicators (KPIs) & Audit Results

Analysis of **1,095** historical shift records (3 shifts/day across 365 days) yielded the following operational metrics:

| KPI | Value | Insight |
|:----|:------|:--------|
| **Total Shift Tests Audited** | 1,095 | Full year of 3-shift plant data [DOCX](#) |
| **Multi-Parameter Pass Rate** | 81.74% | Identified cross-parameter compounding failure events across all 5 criteria simultaneously |
| **Total Out-of-Spec (OOS) Batches** | 200 | Batches failing at least one critical parameter |
| **Average Urea Biuret Content** | 0.75% | Process mean remains healthy and well below the strict 1.00% USL limit |
| **Primary Defect Driver** | Moisture spikes (54 events) | Caused by seasonal monsoon ambient humidity in July/August, alongside periodic boiler pH fluctuations |

---

## Detailed Execution Steps

### 1. Raw Lab Ingestion
Raw analytical test logs are loaded from shift sheets.  
`scripts/generate_lab_data.py` simulates 1 year of continuous 3-shift plant data.

### 2. Python SPC Analysis
Run the Jupyter Notebook:  
`notebooks/01_quality_control_analysis.ipynb`  
Python validates every record against standard operational limits, calculates (Cpk) indices, generates X-bar control charts, and flags anomalous batches.

### 3. Clean Data Export
The notebook outputs `lims_processed_powerbi_data.csv` containing enriched status tags (*PASS* or *FAIL*) and specific violation labels.

### 4. Executive Power BI Dashboard
Open `dashboard/FFC_LIMS_Quality_Dashboard.pbi` in Power BI Desktop. Plant managers can view real-time compliance rates, filter by shift/analyst, and identify root-cause failure drivers.

---
Author: Muhammad Huzaifa Mohsin
Co-Author: Marryam Rashid
