# Sickle Cell Disease (SCD) Multi-Modal Data Extraction Pipeline
### NIH All of Us Research Program — Controlled Tier Dataset v8

---

## Overview

This repository contains a three-notebook pipeline for extracting and analysing
multi-modal data for Sickle Cell Disease (SCD) participants from the
[NIH All of Us Research Program](https://allofus.nih.gov/) Researcher Workbench.

The pipeline covers **476 SCD-diagnosed participants** and extracts:
- EHR demographic data
- Phenotypic complication data with severity scoring
- Laboratory measurements with statistical analysis and patient clustering

---

## Workflow

| Step | Notebook | Description | Key Outputs |
|------|----------|-------------|-------------|
| 1 | `01_EHR_demographics_extraction.ipynb` | Extracts demographic data (age, gender, race, ethnicity) for SCD participants | `SCD_Demographics.csv` |
| 2 | `02_phenotypic_complications_extraction.ipynb` | Extracts SCD complications, assigns severity categories, analyses Hydroxyurea exposure | `SCD_Demographics_with_Complications.csv`, `SCD_Complication_Severity_Only.csv`, `SCD_Demographics_with_HU.csv` |
| 3 | `03_lab_genotypic_extraction.ipynb` | Extracts lab measurements, runs Mann-Whitney U tests, performs KMeans clustering | `lab_measurements_wide_with_units.csv`, `ml_ready_latest.csv`, `complication_lab_mwu_results.csv` |

> ⚠️ Run notebooks **in order**: 01 → 02 → 03. Each notebook depends on outputs from the previous step.

---

## Repository Structure

```
scd-allofus-pipeline/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 01_EHR_demographics_extraction.ipynb
│   ├── 02_phenotypic_complications_extraction.ipynb
│   └── 03_lab_genotypic_extraction.ipynb
├── docs/
│   └── workflow_overview.md
└── data/
    └── README.md
```

---

## How to VIEW This Code (No Account Needed)

Anyone can read the notebooks directly on GitHub — no installation required.

1. Go to: https://github.com/gagan-jpj/SCD-analysis-code.git`
2. Click the `notebooks/` folder
3. Click any `.ipynb` file — GitHub renders it automatically in the browser

You will see all code, markdown explanations, and step-by-step instructions
exactly as they appear in Jupyter.

---

## How to RUN This Code

> ⚠️ This pipeline runs exclusively inside the NIH All of Us Researcher Workbench.
> The notebooks use environment variables (`WORKSPACE_CDR`, `WORKSPACE_BUCKET`)
> that only exist inside the secure All of Us enclave.

### If you have All of Us Researcher Workbench access:

**Option A — Clone from GitHub (Recommended)**

1. Log in at [workbench.researchallofus.org](https://workbench.researchallofus.org/)
2. Open your workspace → click **"Open Jupyter"**
3. Open a **Terminal** tab inside Jupyter
4. Run:
   ```bash
   git clone https://github.com/YOUR-USERNAME/scd-allofus-pipeline.git
   ```
5. Open `01_EHR_demographics_extraction.ipynb` in Jupyter
6. Select **Python 3** kernel
7. Click **Kernel → Restart & Run All**
8. Repeat for notebooks 02 and 03 in order

**Option B — Manual Upload**

1. Download notebooks from GitHub (click file → Download raw)
2. Upload to your All of Us Jupyter workspace via the Upload button
3. Run in order: 01 → 02 → 03

### If you do NOT have All of Us access:

- You can still read all code and methods on GitHub
- Apply at: [workbench.researchallofus.org](https://workbench.researchallofus.org/)
- Approval requires completing training modules (typically 1–2 weeks)

---

## SCD Complications Analysed

| Complication | Category |
|---|---|
| Haemolytic anaemia / Chronic haemolytic anaemia | Haematological |
| Aplastic crisis / Aplastic Anaemia | Haematological |
| Vaso-occlusive crisis (VOC) | Pain / Vascular |
| Dactylitis (Hand-foot syndrome) | Musculoskeletal |
| Acute chest syndrome | Pulmonary |
| Restrictive lung disease | Pulmonary |
| Stroke / Cerebrovascular accident | Neurological |
| Osteomyelitis / Avascular necrosis | Musculoskeletal |
| Preeclampsia | Obstetric |
| Liver cirrhosis / Hepatopathy | Hepatic |

---

## Statistical Methods

| Analysis | Method | Notebook |
|---|---|---|
| Gender vs complication | Chi-squared test | 02 |
| Lab values vs severity groups | Mann-Whitney U test | 03 |
| Patient stratification | KMeans clustering (k=3) | 03 |
| Dimensionality reduction | PCA | 03 |

---

## Important Compliance Notice

> ⚠️ **This repository contains code only — no participant data.**
>
> All notebooks have been cleared of outputs before upload, in compliance with
> NIH All of Us data sharing and publication policies. All data resides within
> the All of Us secure enclave and is accessible only to approved researchers.
>
> Do not commit any `.csv`, `.parquet`, or other data files to this repository.

---

## Citation

(Researchallofus.org, 2026)

---

## Author

Gagandeep Kaur   
King's college London  
2026
