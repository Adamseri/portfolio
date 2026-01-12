# Predicting Dental Cleaning Uptake: A Two-Part Hurdle Model Analysis

## Overview
This project analyzes dental cleaning utilization among U.S. adults using data from the Medical Expenditure Panel Survey (MEPS) 2022-2023. Using a two-part hurdle model, I identify key predictors of preventative dental care access and utilization patterns.

## Key Findings
- **63.2%** of adults receive no dental cleanings annually, highlighting substantial gaps in preventative care
- Dental insurance coverage provides **greatest benefit to low-income populations** (significant negative interaction effect)
- Lacking a usual source of care reduces odds of dental cleaning by **51%** (OR = 0.49, p<0.001)
- **Racial disparities persist** even after controlling for income, education, and insurance (40% lower odds for non-white individuals, OR = 0.61, p<0.001)
- Among users, utilization patterns are **remarkably homogeneous** across socioeconomic groups—disparities exist at the point of initial access, not in intensity of care

## Policy Implications
Counterfactual policy simulations reveal:
- **Universal dental insurance** would increase utilization by 6.2 percentage points (from 36.8% to 43.1%)
- **Targeted insurance for low-income individuals** achieves 58% of universal coverage's impact while affecting only 37% of the population—highly cost-effective
- **Universal usual source of care** would increase utilization by 2.3 percentage points
- **Income support programs** (50% increase for bottom quartile) show minimal impact (0.3 percentage points) due to diminishing returns

**Key Takeaway**: Interventions should focus on **reducing barriers to initial access** (insurance coverage, care coordination) rather than increasing utilization among existing users.

## Methodology

### Data Source
- **Survey**: Medical Expenditure Panel Survey (MEPS) 2022-2023
- **Sample**: 11,633 person-year observations from 6,038 individuals
- **Population**: Adults aged 26+ (independent dental care decision-makers)
- **Files**: Linked person-level consolidated files and dental visit files

### Statistical Approach
**Two-Part Hurdle Model**:

**Part 1: Access to Care (Logistic Regression)**
- Outcome: Any dental cleaning (binary)
- Method: Logistic regression with clustered standard errors by individual
- Model development: Baseline → Added polynomial terms (age², poverty²) → Added interaction terms (insurance × poverty, insurance × usual source of care)
- Validation: Split-sample cross-validation (AUC = 0.7672), Hosmer-Lemeshow test, calibration plots

**Part 2: Intensity Among Users (Zero-Truncated Negative Binomial)**
- Outcome: Number of cleanings (conditional on ≥1 cleaning)
- Method: ZTNB regression with clustered standard errors
- Validation: Split-sample cross-validation (MAE = 0.62, RMSE = 0.75), calibration analysis

### Key Variables
- **Outcome**: Dental cleanings (0-14 range)
- **Primary predictors**: Dental insurance, poverty level (% of FPL), usual source of care
- **Demographics**: Age, sex, race/ethnicity, marital status, education, employment, veteran status
- **Interactions**: Insurance × poverty, insurance × usual source of care

## Repository Structure
```
dental_visits/
├── final_analysis.ipynb          # Main analysis notebook (complete workflow)
├── data/                          # MEPS data files
│   ├── dental_meps_2022.dta      # Dental visit records 2022
│   ├── dental_meps_2023.dta      # Dental visit records 2023
│   ├── meps_2022.dta             # Person-level data 2022
│   └── meps_2023.dta             # Person-level data 2023
├── README.md                      # This file
└── requirements.txt               # Python dependencies
```

## Requirements
- Python 3.8+
- pandas
- numpy
- statsmodels
- matplotlib
- seaborn
- scipy

Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/dental_visits.git
   cd dental_visits
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Open and run the analysis:
   ```bash
   jupyter notebook final_analysis.ipynb
   ```

4. Run all cells to reproduce the complete analysis

## Notebook Contents
The `final_analysis.ipynb` notebook includes:
1. **Data Loading & Cleaning**: Merging MEPS files, handling missing data
2. **Exploratory Data Analysis**: Distribution of cleanings, insurance coverage patterns
3. **Part 1 Model**: Logistic regression for any cleaning (access)
4. **Part 2 Model**: ZTNB regression for cleaning frequency (intensity)
5. **Model Validation**: Cross-validation, calibration, diagnostics
6. **Policy Simulations**: Counterfactual scenarios for policy interventions
7. **Visualizations**: Distribution plots, regression tables, calibration plots

## Data Source
Data from the Agency for Healthcare Research & Quality's Medical Expenditure Panel Survey (MEPS):
- [MEPS Homepage](https://meps.ahrq.gov/mepsweb/)
- Public use files for 2022 and 2023
- Nationally representative survey of U.S. healthcare utilization and expenditures

## Key Results Summary

### Part 1: Predictors of Any Dental Cleaning
| Predictor | Odds Ratio | 95% CI | Interpretation |
|-----------|------------|--------|----------------|
| Dental Insurance | 2.09*** | [1.60, 2.73] | 109% higher odds with insurance |
| No Usual Source of Care | 0.49*** | [0.42, 0.57] | 51% lower odds without usual care |
| Non-White | 0.61*** | [0.53, 0.70] | 40% lower odds for non-white individuals |
| Poverty Level (scaled) | 1.22*** | [1.15, 1.30] | Positive effect with diminishing returns |
| Insurance × Poverty | 0.98** | [0.96, 0.99] | Insurance helps more at lower incomes |

### Part 2: Frequency Among Users
- **Limited variation** in cleaning frequency among users
- Only **age** showed significant (modest) association (quadratic relationship, turning point ~age 39)
- **No significant effects** for insurance, income, education, race, or gender
- **Interpretation**: Clinical guidelines drive utilization among users, not socioeconomic factors

## Author
Adam Serilevi  
[GitHub](https://github.com/adamseriwarp) | a.seri@wearewarp.com

## License
MIT License

## Citation
If you use this analysis, please cite:
```
Serilevi, A. (2026). Predicting Dental Cleaning Uptake: A Two-Part Hurdle Model Analysis. 
GitHub repository: https://github.com/yourusername/dental_visits
```

## Acknowledgments
- Data provided by the Agency for Healthcare Research & Quality (AHRQ)
- Medical Expenditure Panel Survey (MEPS) respondents

