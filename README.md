# Temperature Shapes Multi-Trophic Aquatic Biodiversity Patterns Across Space and Time

This repository contains scripts and data for analyzing how **temperature and its variability shape aquatic biodiversity across trophic levels and spatial scales**. Using environmental DNA and morphological data from freshwater mesocosms distributed across a temperature gradient, the analyses test how mean temperature and variability jointly affect α-, β-, and γ-diversity. Results show that diversity peaks at intermediate temperatures, declines at thermal extremes, and responds nonlinearly to temperature variability—highlighting that both stable and fluctuating environments can sustain high richness.  

![Diversity Patterns](../figures/Figure_2_Regional climate and accumulated diversity across the Iberian Pond Network.png)

---

## Project Structure

```
├── data/                                  # Input data files (diversity metrics, temperature, site info)
├── figures/                               # Output plots and figures
├── outputs/                               # Model summaries and tables
├── 01.ms_diversities.Rmd                  # Data processing and summary statistics
├── 02.ms_diversities_reg_models.Rmd       # Regression models for diversity–temperature relationships
└── 03.ms_diversities_linear_mixed_models.Rmd  # Linear mixed-effects models including temporal interactions
```

---

## Requirements

All scripts are written in **R (≥4.2.0)** and rely on the following major packages:

### 🧰 General Data Handling
- `tidyverse` — data manipulation and visualization  
- `dplyr`, `tidyr`, `magrittr` — tidy workflows and data pipelines  
- `broom` — tidy summaries of model outputs  
- `tidyquant` — time and trend utilities  

### 📊 Visualization & Plotting
- `ggplot2` — core plotting  
- `ggpmisc`, `ggpubr`, `directlabels`, `gridExtra`, `viridis` — annotations, layouts, and color palettes  

### 📈 Statistical Modeling
- `lme4`, `lmerTest` — linear mixed-effects models  
- `ggeffects` — model predictions and marginal effects  
- `MuMIn` — model selection and R² estimation  

### 🧾 Reporting
- `knitr` — document rendering and figure export  

---

### 🔧 Setup

You can install all required packages using the following R code:

```r
required_packages <- c(
  "tidyverse", "dplyr", "tidyr", "magrittr", "broom", "tidyquant",
  "ggplot2", "ggpmisc", "ggpubr", "directlabels", "gridExtra", "viridis",
  "lme4", "lmerTest", "ggeffects", "MuMIn", "knitr"
)

installed <- required_packages %in% rownames(installed.packages())
if (any(!installed)) install.packages(required_packages[!installed])
```

---

# Data

Ensure the following datasets are available in the `data/` directory before running the scripts:

### 🧩 Diversity and Environmental Data
- `table_diversity_metrics.csv` — diversity estimates (local, beta, regional) per group and site  
- `table_environmental.csv` — temperature variables (mean, SD) and year  
- `table_trophic_groups.csv` — trophic group metadata  

All data files must be placed in the `data/` subfolder of the project root directory.

---

## Running the Analysis

Open and execute the scripts sequentially:

1. **`01.ms_diversities.Rmd`**  
   Data import, cleaning, and computation of diversity metrics.  

2. **`02.ms_diversities_reg_models.Rmd`**  
   Regression analyses assessing diversity–temperature relationships.  

3. **`03.ms_diversities_linear_mixed_models.Rmd`**  
   Mixed-effects models testing the interaction between temperature and time.  

Scripts can be run chunk by chunk in RStudio or knitted into full reports.

---

## Outputs

- Figures exported to the `figures/` folder  
- Model summaries and tables exported to the `outputs/` folder  

---

## Authors
Cátia Pereira, Museo Nacional de Ciencias Naturales (MNCN-CSIC)

Miguel Matias, Museo Nacional de Ciencias Naturales (MNCN-CSIC)
