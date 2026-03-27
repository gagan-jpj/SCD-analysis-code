# Pipeline Workflow Overview

## SCD Multi-Modal Data Extraction — All of Us

---

## Data Flow

```
All of Us BigQuery CDR (Controlled Tier v8)
        │
        ▼
┌─────────────────────────────────────────────┐
│  Notebook 01: EHR Demographics Extraction   │
│  • Query person table                       │
│  • Extract age, gender, race, ethnicity     │
│  • Visualise distributions                  │
│  OUTPUT → SCD_Demographics.csv (476 pts)    │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│  Notebook 02: Phenotypic Extraction         │
│  • Query condition_occurrence table         │
│  • Map to SCD complication concept IDs      │
│  • Score severity (None/Mild/Mod/Severe)    │
│  • Extract Hydroxyurea drug exposure        │
│  OUTPUT → Complications + Severity CSVs     │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│  Notebook 03: Lab & Genotypic Extraction    │
│  • Query measurement table                  │
│  • Pivot to wide format (per analyte)       │
│  • Mann-Whitney U: labs vs severity         │
│  • KMeans clustering (k=3) + PCA            │
│  OUTPUT → Lab CSVs + Statistical Results    │
└─────────────────────────────────────────────┘
```

---

## Cohort Definition

- **Condition:** Sickle Cell Disease (SCD)
- **OMOP Concept IDs used:** condition_occurrence mapped via `cb_search_all_events`
- **Cohort size:** 476 participants
- **Dataset:** All of Us Controlled Tier v8

---

## Key Variables

### Demographics (Notebook 01)
- `person_id` — unique participant identifier
- `gender`, `race`, `ethnicity`
- `date_of_birth` → computed `age`
- `sex_at_birth`

### Complications (Notebook 02)
- Binary flags per complication (0 = absent, 1 = present)
- `complication_count` — total number of complications per participant
- `severity_group` — None / Mild / Moderate / Severe
- `HU_exposed` — Hydroxyurea treatment flag (0/1)

### Lab Measurements (Notebook 03)
- Haemoglobin (Hb)
- White Blood Cell count (WBC)
- Platelet count
- Lactate Dehydrogenase (LDH)
- Foetal Haemoglobin (HbF)
- Reticulocyte count
- Bilirubin (total)
- Creatinine

---

## Statistical Tests

| Test | Variable 1 | Variable 2 | Notebook |
|------|------------|------------|----------|
| Chi-squared | Gender | Complication presence | 02 |
| Mann-Whitney U | Lab value | Severity group | 03 |
| KMeans clustering | All lab values | — | 03 |
| PCA | All lab values | — | 03 |
