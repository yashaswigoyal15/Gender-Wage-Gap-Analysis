Gender Wage Gap Analysis — Cross-Sector & Cross-Country Empirical Study

Author: Yashaswi Goyal  
BA Economics Honours| BS Data Science & Applications
Tools: R (tidyverse, ggplot2, lmtest) | Excel  
Data Source:ILOSTAT — ILO Mean Nominal Monthly Earnings by Sex and Economic Activity(https://ilostat.ilo.org/data/)

---

## Overview

This project examines the structural determinants of the gender wage gap across **6 industrial sectors** and **8 countries** over the period **2018–2023**. Using panel data from the International Labour Organization (ILO), I apply OLS regression to assess whether sector classification and regional context are statistically significant predictors of wage gap magnitude.

This is an undergraduate empirical project demonstrating a complete research workflow — from raw data cleaning to econometric analysis and policy interpretation.

---

## Key Findings

- **Overall mean wage gap: 13.76%** across 288 country-sector-year observations
- **Brazil** records the highest country-level gap at **21.20%**; **Philippines** the lowest at **1.63%**
- **Manufacturing** exhibits the largest sectoral gap at **17.97%**, followed by **Finance at 15.91%**
- OLS regression confirms both **sector** and **region** are statistically significant determinants (p < 0.05)
- The wage gap showed a modest declining trend from 2018–2022, with a partial reversal in 2023

---

## Charts

### Average Wage Gap by Sector
![Wage Gap by Sector](charts/wage_gap_by_sector.png)

### Average Wage Gap by Country
![Wage Gap by Country](charts/wage_gap_by_country.png)

### Wage Gap Trend Over Time (2018–2023)
![Wage Gap Over Time](charts/wage_gap_over_time.png)

### Regression Diagnostics — Residuals vs Fitted
![Residuals vs Fitted](charts/residuals_vs_fitted.png)

---

## Dataset

| Variable | Description |
|---|---|
| `Country` | 8 countries: Brazil, France, India, Philippines, Sri Lanka, Switzerland, Thailand, USA |
| `Year` | 2018–2023 |
| `Sector` | 6 ISIC sectors: Manufacturing (C), Retail (G), Finance (K), Professional (M), Education (P), Health (Q) |
| `Female_Earnings` | Mean monthly earnings — female workers (national currency) |
| `Male_Earnings` | Mean monthly earnings — male workers (national currency) |
| `Wage_Gap (%)` | (Male − Female) / Male × 100 |
| Sector dummies | Binary variables for each sector (Manufacturing = baseline) |
| Region dummies | Binary variables for each region (USA / Developed English = baseline) |

**Raw data source:** ILOSTAT — `EAR_EHRA_SEX_ECO_NB_A` dataset  
**Cleaned file:** `data/wage_clean.csv` (288 observations, 16 variables)

---

## Methodology

### 1. Data Cleaning
- Raw ILOSTAT CSV reshaped from long to wide format (Male/Female earnings as separate columns)
- Wage gap variable constructed as percentage of male earnings
- Sector and region dummy variables created for regression
- Performed in Excel (pivot tables) and verified in Python/pandas

### 2. OLS Regression
```r
model <- lm(WageGap ~ Sector_Education + Sector_Finance +
              Sector_Health + Sector_Professional + Sector_Retail +
              Region_Europe + Region_LatAm + Region_Asia_Dev +
              Region_Asia_Africa_Dev + Year,
            data = data)

summary(model)
```
- **Baseline sector:** Manufacturing (ISIC C)
- **Baseline region:** United States (Developed English)
- **Year** included as continuous control for time trend

### 3. Heteroskedasticity Testing
```r
# Breusch-Pagan test
bptest(model)

# Robust standard errors if heteroskedasticity detected
coeftest(model, vcov = vcovHC(model, type = "HC3"))
```

### 4. Visualisation
All charts produced using `ggplot2` in R — see `analysis.R` for full code.

---

## Repository Structure

```
Gender-Wage-Gap-Analysis/
│
├── data/
│   ├── EAR_EHRA_SEX_ECO_NB_A-filtered-2026-05-11.csv   # Raw ILOSTAT data
│   └── wage_clean.csv                                    # Cleaned panel dataset
│
├── charts/
│   ├── wage_gap_by_sector.png
│   ├── wage_gap_by_country.png
│   ├── wage_gap_over_time.png
│   └── residuals_vs_fitted.png
│
├── analysis.R          # Full R script (data loading, regression, charts)
├── report.pdf          # Full academic report with methodology and findings
└── README.md
```

---

## How to Reproduce

1. Clone the repository:
```bash
git clone https://github.com/yashaswigoyal15/Gender-Wage-Gap-Analysis.git
```

2. Open `analysis.R` in RStudio

3. Install required packages (run once):
```r
install.packages("tidyverse")
install.packages("lmtest")
install.packages("sandwich")
```

4. Set working directory to the repo folder and run the script

---

## Limitations

- Earnings reported in national currency — cross-country wage level comparisons are not PPP-adjusted
- Small country sample (n=8) limits statistical power for region dummies
- Model does not control for individual-level factors (education, experience, occupation) within sectors
- Estimates reflect total associations, not causal effects net of compositional differences

---

## References

- International Labour Organization. (2024). *ILOSTAT database*. Geneva: ILO.
- Blau, F. D., & Kahn, L. M. (2017). The gender wage gap: Extent, trends, and explanations. *Journal of Economic Literature*, 55(3), 789–865.
- Goldin, C. (2014). A grand gender convergence: Its last chapter. *American Economic Review*, 104(4), 1091–1119.
- R Core Team. (2024). *R: A language and environment for statistical computing*. Vienna: R Foundation.
- Wickham, H. et al. (2019). Welcome to the tidyverse. *Journal of Open Source Software*, 4(43), 1686.

---

## Contact

**Yashaswi Goyal**  
yashaswigoyal15@gmail.com  
[LinkedIn](https://www.linkedin.com/in/yashaswigoyal15) · [GitHub](https://github.com/yashaswigoyal15)
