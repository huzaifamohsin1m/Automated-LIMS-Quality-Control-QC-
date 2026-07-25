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
