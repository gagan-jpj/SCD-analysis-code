# Sickle Cell Disease (SCD) Multi-Modal Data Extraction Pipeline
### NIH All of Us Research Program — Controlled Tier Dataset v8

---

## Overview

This repository contains a **four-notebook pipeline** for extracting and analysing
multi-modal data for Sickle Cell Disease (SCD) participants from the
[NIH All of Us Research Program](https://allofus.nih.gov/) Researcher Workbench.

The pipeline covers **476 SCD-diagnosed participants** and extracts:
- EHR demographic data
- Phenotypic complication data with severity scoring
- Laboratory measurements with statistical analysis and patient clustering
- Phased short-read WGS genotypes for 16 disease-relevant SNPs

---

## Workflow

| Step | Notebook | Description | Key Outputs |
|------|----------|-------------|-------------|
| 1 | `01_EHR_demographics_extraction.ipynb` | Extracts demographic data (age, gender, race, ethnicity) for SCD participants | `SCD_Demographics.csv` |
| 2 | `02_phenotypic_complications_extraction.ipynb` | Extracts SCD complications, assigns severity categories, analyses Hydroxyurea exposure | `SCD_Demographics_with_Complications.csv`, `SCD_Complication_Severity_Only.csv`, `SCD_Demographics_with_HU.csv` |
| 3 | `03_lab_extraction.ipynb` | Extracts lab measurements, runs Mann-Whitney U tests, performs KMeans clustering | `lab_measurements_wide_with_units.csv`, `ml_ready_latest.csv`, `complication_lab_mwu_results.csv` |
| 4 | `04_genotypic_WGS_extraction.ipynb` | Extracts phased genotypes for 16 SNPs across 8 SCD-relevant genes from srWGS data | `FINAL_MERGED_ALL_476_16pos.phased.vcf.gz`, `ALL_476_genotypes_long.csv` |

> ⚠️ Run notebooks **in order**: 01 → 02 → 03 → 04. Each notebook depends on outputs from the previous step.

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
│   ├── 03_lab_extraction.ipynb
│   └── 04_genotypic_WGS_extraction.ipynb
├── docs/
│   └── workflow_overview.md
└── data/
    └── README.md
```

---

## How to VIEW This Code (No Account Needed)

Anyone can read the notebooks directly on GitHub — no installation required.

1. Go to: `https://github.com/gagan-jpj/SCD-analysis-code`
2. Click the `notebooks/` folder
3. Click any `.ipynb` file — GitHub renders it automatically in the browser

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
   git clone https://github.com/gagan-jpj/SCD-analysis-code.git
   ```
5. Open `01_EHR_demographics_extraction.ipynb` in Jupyter
6. Select **Python 3** kernel
7. Click **Kernel → Restart & Run All**
8. Repeat for notebooks 02, 03, and 04 in order

**Option B — Manual Upload**

1. Download notebooks from GitHub (click file → Download raw)
2. Upload to your All of Us Jupyter workspace via the Upload button
3. Run in order: 01 → 02 → 03 → 04

### If you do NOT have All of Us access:

- You can still read all code and methods on GitHub
- Apply at: [workbench.researchallofus.org](https://workbench.researchallofus.org/)
- Approval requires completing training modules (typically 1–2 weeks)

---

## Full SCD Complications List (Notebook 02)

All 51 complications mapped using OMOP concept IDs from the `condition_occurrence` table:

### Haematological
| Complication | OMOP Concept ID |
|---|---|
| Haemolytic anaemia / Chronic haemolytic anaemia | 435503 |
| Aplastic crisis / Aplastic Anaemia | 137829 |
| Iron overload | 4246084 |

### Pain & Vascular
| Complication | OMOP Concept ID |
|---|---|
| Vaso-occlusive crisis (VOC) | 35626024 |
| Dactylitis | 4131942 |
| Hand and foot syndrome | 4159748 |
| Chronic pain | 436096 |
| Avascular necrosis (AVN) | 4287786 |
| Leg ulcers / Ulcer of lower extremity | 197304 |

### Neurological
| Complication | OMOP Concept ID |
|---|---|
| Ischaemic stroke | 4310996 |
| Transient ischaemic attacks (TIAs) | 4043734 |
| Cognitive impairment / disorder | 40480615 |
| Seizures | 4106574 |
| Cognitive delay | 4137543 |

### Pulmonary
| Complication | OMOP Concept ID |
|---|---|
| Acute chest syndrome | 254062 |
| Pulmonary hypertension | 4322024 |
| Restrictive lung disease | 4266931 |
| Obstructive lung disease | 255573 |
| Sleep-disordered breathing | 44783631 |

### Cardiac
| Complication | OMOP Concept ID |
|---|---|
| Cardiomyopathy | 321319 |

### Renal
| Complication | OMOP Concept ID |
|---|---|
| Haematuria | 79864 |
| Proteinuria | 75650 |
| Albuminuria | 4168705 |
| Chronic kidney disease (CKD) | 46271022 |
| End-stage kidney disease (ESKD) | 193782 |
| Focal segmental glomerulosclerosis (FSGS) | 4030513 |
| Renal papillary necrosis | 4152839 |
| Nocturia | 40304526 |
| Enuresis | 193874 |
| Renal transplant | 4324887 |

### Hepatic / Gastrointestinal
| Complication | OMOP Concept ID |
|---|---|
| Cholestasis | 4143915 |
| Cholelithiasis | 444367 |
| Gallstones | 196456 |
| Liver cirrhosis | 4064161 |

### Musculoskeletal
| Complication | OMOP Concept ID |
|---|---|
| Osteomyelitis | 141663 |

### Ophthalmological
| Complication | OMOP Concept ID |
|---|---|
| Sickle retinopathy | 4021365 |
| Vitreous haemorrhage | 315276 |

### Infectious
| Complication | OMOP Concept ID |
|---|---|
| Sepsis | 132797 |
| Meningitis | 435785 |
| Parvovirus B19 infection | 134569 |

### Psychosocial
| Complication | OMOP Concept ID |
|---|---|
| Depression | 440383 |
| Anxiety | 441542 |
| Social isolation | 4309238 |
| Chronic stress | 4138454 |

### Reproductive / Developmental
| Complication | OMOP Concept ID |
|---|---|
| Growth delay | 4031877 |
| Delayed puberty | 4266651 |
| Infertility (Female) | 201909 |
| Infertility (Male) | 198197 |
| Priapism | 315586 |
| Miscarriage | 4067106 |
| Preeclampsia | 439393 |

### Surgical / Procedural
| Complication | OMOP Concept ID |
|---|---|
| Splenectomy | 4120626 |
| Bone marrow transplant | 42537745 |

---

## Full Lab Measurements List (Notebook 03)

All 21 lab analytes extracted from the All of Us BigQuery `measurement` table:

### Complete Blood Count (CBC)
| Lab Test | Clinical Relevance in SCD |
|---|---|
| Haemoglobin | Primary anaemia marker — typically low in SCD |
| Foetal haemoglobin (HbF) | Protective modifier — higher HbF = milder disease |
| Red blood cell count | Reflects chronic haemolysis |
| Hematocrit | Proportion of red blood cells in blood |
| Mean corpuscular volume (MCV) | Red blood cell size — affected by HbF level |
| Mean corpuscular haemoglobin concentration (MCHC) | Haemoglobin concentration per cell |
| Platelets | Often elevated in SCD due to chronic inflammation |
| Reticulocyte count | Marker of bone marrow response to haemolysis |

### White Blood Cell Differential
| Lab Test | Clinical Relevance in SCD |
|---|---|
| WBC (Leukocytes) | Elevated in infection and inflammation |
| Neutrophils | Key innate immune cell — elevated in VOC |
| Lymphocytes | Adaptive immune response marker |
| Monocytes | Inflammatory cell — elevated in chronic SCD |
| Eosinophils | Associated with allergic and parasitic responses |
| Basophils | Rare — marker of bone marrow activity |

### Organ Function
| Lab Test | Clinical Relevance in SCD |
|---|---|
| Lactate dehydrogenase (LDH) | Haemolysis marker — elevated during crises |
| Bilirubin, total | Haemolysis byproduct — elevated in SCD |
| Bilirubin, direct | Liver function marker |
| Creatinine | Renal function — SCD nephropathy marker |

### Vitals / Cardiovascular
| Lab Test | Clinical Relevance in SCD |
|---|---|
| Systolic blood pressure | Cardiovascular risk marker |
| Diastolic blood pressure | Cardiovascular risk marker |
| Heart rate | Elevated in anaemia and VOC |

---

## Target Genes — Notebook 04 (WGS)

| Gene | Chr | SNPs | Relevance to SCD |
|------|-----|------|-----------------|
| HBB-cluster | chr11 | 3 | Direct SCD-causal variants (HbS) |
| HBG2 | chr11 | 1 | Foetal haemoglobin (HbF) regulation |
| BCL11A | chr2 | 3 | Primary HbF repressor — major modifier |
| HBS1L-MYB (HMIP) | chr6 | 3 | HbF quantitative trait locus |
| TNF | chr6 | 1 | Inflammatory severity modifier |
| APOE | chr19 | 2 | Stroke / lipid risk modifier |
| APOL1 | chr22 | 2 | Kidney disease risk in SCD |
| VCAM1 | chr1 | 1 | Vascular adhesion / VOC mechanism |

---

## Statistical Methods

| Analysis | Method | Notebook |
|---|---|---|
| Gender vs complication | Chi-squared test | 02 |
| Lab values vs severity groups | Mann-Whitney U test | 03 |
| Patient stratification | KMeans clustering (k=3) | 03 |
| Dimensionality reduction | PCA | 03 |
| WGS genotype extraction | Hail + bcftools | 04 |
| Allele frequency summary | Haplotype counting | 04 |

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

The "All of Us" Research Program. (2019). *New England Journal of Medicine*, 381(7), 668–676.
https://doi.org/10.1056/NEJMsr1809937

---

## Author

**Gagandeep Kaur**  
King's College London  
2026
