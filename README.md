# Temperature Shapes Multi-Trophic Aquatic Biodiversity Patterns Across Space and Time

This repository contains scripts and data for analyzing how **temperature and its variability shape aquatic biodiversity across trophic levels and spatial scales**. Using environmental DNA and morphological data from freshwater mesocosms distributed across a temperature gradient, the analyses test how mean temperature and variability jointly affect α-, β-, and γ-diversity. Results show that diversity peaks at intermediate temperatures, declines at thermal extremes, and responds nonlinearly to temperature variability—highlighting that both stable and fluctuating environments can sustain high richness.

![Diversity Patterns](figures/Figure_2_Regional_climate_accumulated_diversity.png)

---

## Project Structure

```
└── inputs/                                     # Input data files
│   ├── data_enve_all.Rdata                    # Environmental variables
│   ├── all_macrophytes.Rdata                  # Macrophyte community data
│   ├── 0_hybrid_all_geo_fit.RData             # Hybrid eDNA + morphological species data
│   ├── data_eDNA_bacteria_v2.0.RData         # Bacterial eDNA data
│   └── 3b_diversity_env_df_TEST.RData        # Consolidated diversity-environment dataset (generated)
│
└── scripts/
│   ├── 00_prepare_datasets.Rmd               # Template/preparation script
│   ├── 01_metrics.Rmd                        # Compute diversity metrics (α, β, γ) using MOBR
│   ├── 02_plots.Rmd                          # Generate publication figures
│   ├── 03_reg_models.Rmd                     # Simple regression models (linear/quadratic)
│   └── 04_linear_mixed_models.Rmd            # Linear mixed-effects models with spatial/temporal structure
│
└── figures/                                   # Output plots and figures
└── outputs/                                   # Model summaries and tables
    ├── 02.linear_models_evaluation.csv
    └── 02.quadratic_models_evaluation.csv
```

---

## Script Workflow

The analysis pipeline consists of five sequential scripts that should be executed in order:

### 1. `00_prepare_datasets.Rmd`
Template/preparation script (currently minimal).

### 2. `01_metrics.Rmd` — Diversity Metrics Computation
**Purpose:** Compute α-, β-, and γ-diversity metrics using the MOBR (Multifaceted Biodiversity) framework.

**Key functions:**
- Loads environmental, macrophytes, hybrid species, and bacterial eDNA data
- Prepares community matrices for five trophic groups: Whole-community, Bacteria, Phytoplankton, Zooplankton, Macroinvertebrates
- Computes diversity metrics at multiple spatial scales using `mobr::calc_comm_div()`
- Generates sample-based rarefaction curves for each trophic group
- Merges diversity metrics with environmental covariates
- **Output:** Saves `3b_diversity_env_df_TEST.RData` containing `div_env_df` (diversity-environment data frame) and `rarefaction_df` (rarefaction curves)

**Key packages:** `mobr`, `vegan`, `tidyverse`, `factoextra`, `ggcorrplot`, `viridis`

### 3. `02_plots.Rmd` — Visualization
**Purpose:** Generate publication-quality figures for the manuscript.

**Key outputs:**
- **Figure 2:** Regional climate and accumulated diversity across the Iberian Pond Network
- Temperature–diversity relationships across spatial scales (regional γ-diversity and local α-diversity)
- Plots for mean temperature and temperature variability (SD) relationships
- Year-specific and temporal average visualizations
- Supplementary figures

**Key packages:** `tidyverse`, `ggplot2`, `ggpmisc`, `ggpubr`, `gridExtra`, `viridis`, `tidyquant`, `directlabels`, `broom`

### 4. `03_reg_models.Rmd` — Simple Regression Models
**Purpose:** Fit and compare linear and quadratic regression models to test diversity–temperature relationships.

**Approach:**
- Separate models for each trophic group and diversity scale (α, β, γ)
- Tests linear vs. quadratic relationships with mean temperature (TMean) and temperature standard deviation (TSD)
- Model comparison using AIC and Residual Standard Error (RSE)
- Year-specific models (2016, 2017, 2018) and temporal averages

**Outputs:**
- `02.linear_models_evaluation.csv` — Linear model summaries
- `02.quadratic_models_evaluation.csv` — Quadratic model summaries

**Key packages:** `tidyverse`, `broom`, `knitr`

### 5. `04_linear_mixed_models.Rmd` — Linear Mixed-Effects Models
**Purpose:** Fit LMMs to assess how temperature drives diversity across space and time, accounting for hierarchical structure.

**Model structure:**
- **Regional (γ) diversity:** Random effects = Site
- **Local (α) and β diversity:** Random effects = Site + Pond (nested in Site)
- Fixed effects: Polynomial temperature terms, TSD, Year, and their interactions
- Model selection via AIC, AIC weights, and likelihood ratio tests
- Diagnostic checks: residual plots, Q–Q plots, R²m (marginal) and R²c (conditional)

**Key packages:** `lme4`, `lmerTest`, `ggeffects`, `MuMIn`, `tidyverse`

---

## Requirements

All scripts are written in **R (≥4.2.0)** and rely on the following packages:

### 🧰 Core Data Handling
- `tidyverse` — data manipulation and visualization
- `dplyr`, `tidyr`, `magrittr` — tidy workflows
- `broom` — tidy model summaries

### 📊 Biodiversity Metrics
- `mobr` — Multifaceted Biodiversity framework for α, β, γ diversity
- `vegan` — community ecology analyses

### 📈 Statistical Modeling
- `lme4`, `lmerTest` — linear mixed-effects models
- `ggeffects` — model predictions and marginal effects
- `MuMIn` — model selection and R² estimation

### 📊 Visualization & Plotting
- `ggplot2` — core plotting
- `ggpmisc`, `ggpubr`, `directlabels`, `gridExtra` — annotations and layouts
- `viridis`, `viridisLite` — color palettes
- `tidyquant` — time and trend utilities

### 🔧 Utilities
- `factoextra`, `ggcorrplot` — PCA and correlation visualization
- `knitr` — document rendering

### 🔧 Setup

You can install all required packages using the following R code:

```r
required_packages <- c(
  "tidyverse", "dplyr", "tidyr", "magrittr", "broom",
  "mobr", "vegan",
  "lme4", "lmerTest", "ggeffects", "MuMIn",
  "ggplot2", "ggpmisc", "ggpubr", "directlabels", "gridExtra", 
  "viridis", "viridisLite", "tidyquant",
  "factoextra", "ggcorrplot", "knitr"
)

installed <- required_packages %in% rownames(installed.packages())
if (any(!installed)) install.packages(required_packages[!installed])
```

---

## Data

Ensure the following datasets are available in the `inputs/` directory before running the scripts:

### Required Input Files
- `data_enve_all.Rdata` — Environmental variables (temperature, nutrients, etc.)
- `all_macrophytes.Rdata` — Macrophyte community data
- `0_hybrid_all_geo_fit.RData` — Hybrid eDNA + morphological species assignments
- `data_eDNA_bacteria_v2.0.RData` — Bacterial eDNA metabarcoding data

### Generated Files
- `3b_diversity_env_df_TEST.RData` — Consolidated diversity–environment dataset (generated by `01_metrics.Rmd`)

### Data Sources

The dataset used in this manuscript was published as part of a methods paper:

**Pereira, Cátia Lúcio; Gilbert, M. Thomas P.; Araújo, Miguel Bastos; Matias, Miguel Graça (2021).** Fine‐tuning biodiversity assessments: A framework to pair eDNA metabarcoding and morphological approaches. *Methods in Ecology and Evolution*. https://doi.org/10.1111/2041-210x.13718

**Original code repository:**  
Pereira, C. L., Gilbert, M. T. P., Araújo, M. B., & Matias, M. G. (2021). Data from: Fine-tuning biodiversity assessments: A framework to pair eDNA metabarcoding and morphological approaches. Zenodo. https://doi.org/10.5281/zenodo.5336961

**Dataset repository:**  
Pereira, Cátia Lúcio; Gilbert, M. Thomas P.; Araújo, Miguel Bastos; Matias, Miguel Graça (2021). Data from: Fine-tuning biodiversity assessments: A framework to pair eDNA metabarcoding and morphological approaches [Dataset]. Dryad. https://doi.org/10.5061/dryad.k6djh9w71

All data files must be placed in the `inputs/` subfolder of the project root directory.

---

## Running the Analysis

Open and execute the scripts sequentially in RStudio or R:

1. **`01_metrics.Rmd`**  
   Computes diversity metrics (α, β, γ) for all trophic groups using MOBR. Generates the consolidated diversity–environment dataset required by subsequent scripts.

2. **`02_plots.Rmd`**  
   Generates all publication figures showing temperature–diversity relationships across spatial scales and trophic groups.

3. **`03_reg_models.Rmd`**  
   Fits simple regression models (linear and quadratic) to test diversity–temperature relationships. Exports model comparison tables to `outputs/`.

4. **`04_linear_mixed_models.Rmd`**  
   Fits linear mixed-effects models accounting for spatial (Site, Pond) and temporal (Year) structure. Includes model diagnostics and significance testing.

Scripts can be run chunk by chunk in RStudio or knitted into full PDF reports. Note that `01_metrics.Rmd` must be run first to generate the required data file for scripts 2–4.

---

## Outputs

- **Figures:** Exported to the `figures/` folder (PNG format, high resolution)
- **Model summaries:** Exported to the `outputs/` folder (CSV format)
  - `02.linear_models_evaluation.csv` — Linear regression model comparisons
  - `02.quadratic_models_evaluation.csv` — Quadratic regression model comparisons

---

## Authors

**Cátia Lúcio Pereira**  
Museo Nacional de Ciencias Naturales (MNCN-CSIC)

**Miguel G. Matias**  
Museo Nacional de Ciencias Naturales (MNCN-CSIC)
