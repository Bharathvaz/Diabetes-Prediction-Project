# What Predicts Diabetes? — BRFSS 2024

**DATA-230 Group Project · Mid-presentation (9 Oct 2026)**

**Research question:** Which demographic, behavioural, and healthcare-access factors predict diabetes among US adults?

**Team:** <!-- TODO: add names -->
- Ayush Gajbhiye — Tableau dashboard (built + presenting)
- Syam Prasad Thota — EDA notebook
- Bharathvaz Reddy Narravula — verification & data prep

---

## Dataset

**CDC Behavioral Risk Factor Surveillance System (BRFSS), 2024 annual file** — the largest
continuous US health survey.

- Raw: **457,670 respondents × 301 columns** (SAS XPORT, `LLCP2024.XPT`)
- Cleaned: **456,587 rows × 30 columns** (`brfss2024_diabetes_eda_clean.csv`)
- Target: `diabetes_status` — **Yes (14.4%) / No diabetes (83.1%) / Pre-diabetes or borderline (2.5%)**
- Source: CDC BRFSS annual data, 2024 — <https://www.cdc.gov/brfss/annual_data/annual_data.html>

The cleaned CSV (~130 MB) is not stored in this repo — GitHub rejects files over 100 MB.
It is regenerated from the raw CDC file by the notebook (see `data/README.md`).

---

## Repository structure

```
├── README.md
├── Lab_Project_2_EDA.ipynb      # 89 cells, 29 figures — the full EDA, executed end-to-end
├── README.md                    # data provenance and regeneration notes
├── Lab_Project__1__1_jw83.twbx   # packaged dashboard (data bundled, opens anywhere)
└── DATA230_EDA_Study_Guide.pdf  # act-by-act walkthrough + Q&A prep
 
```

---

## Reproducibility

Requirements: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`.

The notebook reads `LLCP2024.XPT` from its own folder and writes
`brfss2024_diabetes_eda_clean.csv` beside it in the last cell — the same file the
Tableau dashboard was built on, so the numbers match throughout.

---

## Key findings (from the EDA)

- **Age:** diabetes prevalence climbs 1.4% (18–24) → 24.1% (75–79)
- **BMI:** median steps 27.4 → 29.2 → 29.6 across No / Pre-diabetes / Yes — a clean dose-response
- **Income × age compound:** at every age band, diabetes falls as income rises
  (e.g. ages 70–74: ~31% below $20k vs 14% at $200k+)
- **Education:** 26.6% (never attended) → 11.1% (college graduate)
- **Poor-health days:** means 2.8 → 4.35 → 5.0 across the three classes
- **Honest null:** smoking's link is real but tiny (χ² ≈ 2,199, Cramér's V ≈ 0.05) —
  former smokers look worst because a diagnosis is what makes people quit

## Cleaning highlights

- **POORHLTH rescue:** 42.6% "missing" was a skip pattern (question only asked after ≥1
  poor-health day) → filled with 0, recovering ~189k rows instead of dropping the column
- **Structural zeros:** never-smokers → 0 cigarettes/pack-years; non-drinkers → 0 drinks/binges
- **Unit fixes:** drinks/week and BMI stored ×100 (two implied decimals) → ÷100
- **SAS ghost values:** the bit pattern `5.4e-79` (a SAS special-missing that survives
  `read_sas`) mapped to NaN — ~204k + ~6k rows
- **Dropped:** soda questions, quit-smoking timing (~74–75% missing); pre-diabetes test
  recency (~67% missing *and* target leakage)
- Every fill was **audited**: diabetes % on observed-only rows vs all rows after filling —
  no distortion introduced

---

## Tableau dashboard

`tableau/Lab_Project__1__1_jw83.twbx` — packaged workbook (data bundled).

- Views: KPI tiles · BMI histogram · age bars · age × income heatmap · income bars · education line · BMI-category treemap
- Interactivity: `Select_Metric` parameter flips every prevalence chart between
  Diabetes / Pre-diabetes / No diabetes; filters apply to all sheets; hover-highlight
- Presented live by one team member (mid-presentation requirement)

## Preliminary ML direction

**Multiclass classification** of `diabetes_status` (Yes / No diabetes / Pre-diabetes),
with **class weights or resampling** for the 14.4% / 2.5% minority classes.
No model is fit at mid-presentation (per the brief) — the final (4 Dec 2026) is graded
on **interpretation, not accuracy**: confusion matrix, ROC, feature importance, SHAP.

---

## Roadmap

- [x] Dataset selected & cleaned (456,587 × 30)
- [x] EDA complete (89 cells, 29 figures)
- [x] Tableau dashboard built
- [x] Mid-presentation slides (9 Oct 2026)
- [ ] Final model + interpretation (4 Dec 2026)

---
*Course: DATA-230, San José State University · M.S. Applied Data Intelligence*
