# AI-Readiness & Data Standardisation Pipeline

## Overview

This project implements a **domain-agnostic, pre-AI data readiness pipeline** for public datasets.  
It helps assess whether tabular datasets (CSV/XLSX) are suitable for AI and advanced analytics, standardises them safely, and generates clear governance artefacts **before** they are used in AI workflows.

The solution is designed primarily for **Indian public data**, where datasets are often syntactically clean but lack explicit AI-readiness indicators, standardised representations across releases, and documented limitations.

---

## Problem Statement

Public datasets published by Indian government portals are increasingly accessible and usable for basic analysis. However, they typically do not communicate:

- Whether the dataset is suitable for AI use  
- What data quality risks or coverage gaps exist  
- How consistent the data is across time or releases  
- What limitations should be considered before modelling  

As a result, AI practitioners, researchers, and policymakers rely on manual inspection and implicit assumptions, which can lead to wasted effort or misleading outcomes.

This project addresses the **AI-readiness and governance gap** by introducing a systematic, reproducible layer that evaluates, standardises, and documents public datasets before they are used in AI pipelines.

---

## Proposed Solution

The solution is implemented as a **batch data pipeline**, packaged as a **Python library with a CLI interface**.  
It operates on unknown tabular datasets without assuming schemas or domain semantics.

For each dataset (table), the pipeline:
1. Ingests the data safely
2. Profiles objective quality characteristics
3. Computes an explainable AI-readiness score
4. Applies safe, non-semantic standardisation
5. Generates machine-readable metadata
6. Produces human-readable readiness reports

The output consists of **AI-ready data files plus governance artefacts** that support responsible reuse.

---

## Key Design Principles

- Domain-agnostic and reusable  
- Deterministic and explainable  
- Non-destructive (raw data untouched)  
- No semantic inference or guessing  
- Configuration-driven behavior  
- Governance-first design  

---

## Architecture Overview
```txt
┌───────────────────────────────────────────┐
│               Input Layer                 │
│  Public Tabular Datasets (CSV / XLSX)     │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│            Ingestion Layer                │
│  Loads datasets, records basic structure, │
│  and preserves raw data without changes.  │ 
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│            Profiling Layer                │
│  Computes objective data characteristics  │
│  such as completeness, type stability,    │
│  and size-related statistics.             │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│        AI-Readiness Scoring Layer         │
│  Applies deterministic, configuration-    │
│  driven rules to generate table-level     │
│  AI-readiness scores and recommendations. │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│         Standardisation Layer             │
│  Performs safe, non-semantic              │
│  representation-level transformations     │
│  (naming, dates, types, nulls).           │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│        Metadata Generation Layer          │
│  Produces machine-readable descriptions   │
│  covering structure, coverage, readiness, │
│  and documented limitations.              │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│             Reporting Layer               │
│  Generates human-readable readiness       │
│  reports and dataset-level summaries.     │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│               Output Layer                │
│  Standardised data (CSV), metadata and    │
│  quality flags (JSON), and reports (MD).  │
└───────────────────────────────────────────┘
```
Each stage has a single responsibility and produces explicit output artefacts.  
The pipeline does not assume predefined schemas, perform semantic inference, or modify raw data.

---

## Output Artefacts

For each input dataset (table), the pipeline generates the following artefacts:
```txt
┌───────────────────────────────────────────┐
│        Standardised Data Outputs          │
│  • CSV files with consistent schema       │
│  • Normalised column names and types      │
│  • Safe, non-semantic transformations     │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│        Machine-Readable Metadata          │
│  • Dataset structure and schema           │
│  • Temporal and structural coverage       │
│  • AI-readiness score and grade           │
│  • Documented limitations                 │
│  Format: JSON                             │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│          Quality Flags Outputs            │
│  • Column-level data quality indicators   │
│  • Missing value and stability warnings   │
│  Format: JSON                             │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│        Human-Readable Reports             │
│  • AI-readiness assessment                │
│  • Usage recommendations and risks        │
│  • Dataset-level summary                  │
│  Format: Markdown                         │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│          Output Directory Layout          │
│  output/                                  │
│   ├── standardised_data/                  │
│   ├── metadata/                           │
│   ├── quality_flags/                      │
│   ├── reports/                            │
│   └── dataset_report.md                   │
└───────────────────────────────────────────┘
```
- **Standardised data**: Machine-consistent CSV suitable for AI pipelines  
- **Metadata**: Machine-readable description of structure, coverage, readiness, and limitations  
- **Quality flags**: Column-level data quality indicators  
- **Reports**: Human-readable AI-readiness assessment and recommendations  
- **Dataset summary**: Aggregate readiness overview across all tables  

---

## Configuration

All pipeline behavior is controlled through YAML configuration files.  
No code changes are required for policy-level adjustments.
```txt
┌───────────────────────────────────────────┐
│            Configuration Model            │
│  All pipeline behavior is controlled via  │
│  versioned YAML configuration files.      │
│  No code changes are required to adjust   │
│  policy or execution behavior.            │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│            pipeline.yaml                  │
│  • Enable or disable pipeline stages      │
│  • Control execution order                │
│  • Define input formats and overwrite     │
│    behavior                               │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│             scoring.yaml                  │
│  • Define AI-readiness metrics            │
│  • Configure metric weights               │
│  • Set thresholds and grading logic       │
│  • Control score scale                    │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│        standardisation.yaml               │
│  • Allowed column name normalization      │
│  • Date and numeric normalization rules   │
│  • Missing value tokens                   │
│  • Boolean value mappings                 │ 
│  • Explicitly restrict semantic changes   │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│            reporting.yaml                 │
│  • Readiness grading bands (A/B/C/D)      │
│  • Recommendation text                    │
│  • Report language and structure          │
└───────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────┐
│          Configuration Guarantees         │
│  • Deterministic execution                │
│  • Transparent decision logic             │
│  • Auditable policy changes               │
│  • Safe reuse across domains              │
└───────────────────────────────────────────┘
```
