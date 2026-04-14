# 🏐 Explaining National Volleyball Success Through Economic and Development Indicators

**DSA 210 — Introduction to Data Science - Term Project **
**Spring 2025–2026 | Sabancı University**

I am Nehir Eylül Balcı a student from Sabancı University, and this is my term project for DSA210 Introduction to Data Science course.

## 📌 Project Overview

This project investigates whether **national success in volleyball** can be explained by country-level **economic and development factors**. Using official FIVB World Rankings merged with World Bank and UNDP data, I apply statistical analysis and linear regression to quantify the relationships between GDP per capita, population size, and the Human Development Index (HDI) and volleyball performance.

The analysis covers **Senior Men's and Senior Women's** national team rankings for ~30 countries each, enabling both a cross-country comparison and a gender-specific breakdown.

---

## ❓ Research Questions

1. Are economic and development indicators (GDP per capita, HDI, population) associated with national volleyball success?
2. Is the association different for men's vs. women's national teams?
3. Does higher human development reduce the gender performance gap in volleyball?
4. Which countries over- or under-perform relative to their economic development?
5. Is European (CEV) dominance in volleyball statistically significant?

---

## 🗂️ Repository Structure

```
DSA210-Volleyball-Project/
│
├── data/
│   ├── FIVB_Volleyball_Rankings.xlsx   # Senior Men/Women + Youth rankings (FIVB)
│   ├── GDP_WorldBank.zip               # GDP per capita — World Bank
│   ├── Population_WorldBank.zip        # Population — World Bank
│   └── HDI_UNDP.xlsx                   # Human Development Index — UNDP
│
├── dsa210_volleyball_analysis.py       # Main analysis script (runs in Google Colab)
├── volleyball_economic_dataset.csv     # Final merged dataset (output)
├── requirements.txt
└── README.md
```

---

## 📊 Dataset

### Volleyball Performance Data
| Source | Description | URL |
|--------|-------------|-----|
| FIVB / Volleyball World | Senior Men's & Women's World Rankings (Jan–Jun 2025) | https://en.volleyballworld.com/volleyball/world-ranking/ |
| Wikipedia | Historical ranking snapshots | https://en.wikipedia.org/wiki/FIVB_Senior_World_Rankings |

### Socioeconomic Data
| Source | Indicator | URL |
|--------|-----------|-----|
| World Bank Open Data | GDP per capita (current US$) | https://data.worldbank.org/indicator/NY.GDP.PCAP.CD |
| World Bank Open Data | Total population | https://data.worldbank.org/indicator/SP.POP.TOTL |
| UNDP Human Development Reports | Human Development Index (HDI) | https://hdr.undp.org/data-center/human-development-index |

### Final Dataset Structure
Each row represents one country. Key columns:

| Column | Description |
|--------|-------------|
| `country_std` | Standardised country name |
| `pts_men` | Men's Senior FIVB WR Points |
| `pts_women` | Women's Senior FIVB WR Points |
| `avg_senior_pts` | Average of men's and women's points |
| `gender_gap` | Women's points − Men's points |
| `gdp_per_capita` | GDP per capita in USD |
| `log_gdp` | Log₁₀(GDP per capita) |
| `population` | Total population |
| `log_population` | Log₁₀(population) |
| `hdi` | Human Development Index (0–1) |
| `confederation` | FIVB volleyball confederation (CEV, AVC, CSV, …) |

---

## 🔬 Methodology

### 1. Data Collection & Preprocessing
- FIVB Senior Men's and Women's ranking data scraped from Wikipedia (exact WR point values as of Jan 2025 and Jun 2025 respectively)
- World Bank and UNDP data downloaded as CSV/Excel via official portals
- Country names standardised across all datasets (e.g. "Turkey" → "Turkiye", "South Korea" → "Korea, Rep.")
- Missing values handled via forward-fill on the most recent available year

### 2. Feature Engineering
- **Log transformation** applied to GDP and population (both heavily right-skewed) to correct for heteroskedasticity before regression — as per course Recitation 7
- **Gender gap** computed as Women's points minus Men's points

### 3. Exploratory Data Analysis (EDA)
- Histograms: distributions of all key variables (before/after log transformation)
- Bar charts: top 15 countries by men's and women's rankings
- Scatter plots: GDP/HDI/population vs volleyball points, with regression lines
- Boxplots: performance distribution by confederation (continental grouping)
- Gender gap bar chart and HDI scatter

### 4. Correlation Analysis
- **Spearman rank correlation** used (as volleyball distributions are non-normal and the relationship may be monotonic but non-linear — discussed in course Week 2b)
- Hypothesis tests for all 8 predictor–outcome pairs with p-values and significance levels
- **Mann-Whitney U test** to test whether European nations (CEV) are significantly stronger than non-European nations

### 5. Linear Regression
- **Simple linear regression**: Log GDP → Women's points (with residual plot)
- **Multiple linear regression**: Log GDP + HDI + Log Population → Men's / Women's points
- Residual analysis to identify over- and under-performing countries relative to economic expectations

---

## 📈 Key Findings

| Finding | Evidence |
|---------|----------|
| HDI is the strongest predictor of volleyball success | Spearman r ≈ 0.50, p < 0.05 for both genders |
| GDP per capita has a moderate positive association | Spearman r ≈ 0.40, p < 0.05 |
| Population has no meaningful association ("population paradox") | Spearman r ≈ 0.10, p > 0.05 |
| HDI is a slightly stronger predictor for women's volleyball | Higher β₂ coefficient in women's model |
| European confederation (CEV) is significantly dominant | Mann-Whitney U test, p < 0.05 |
| Brazil and Turkey over-perform relative to their GDP | Largest positive residuals in women's model |

**The most surprising finding** is the *population paradox*: countries like Poland, Italy, and Slovenia (populations of 5–38 million) outperform much more populous nations. This suggests that volleyball success depends on the quality of sports infrastructure and cultural investment, not the raw size of the talent pool.

---

## ⚙️ How to Run

### Prerequisites
```
numpy
pandas
matplotlib
seaborn
scipy
scikit-learn
openpyxl
```
Install with:
```bash
pip install -r requirements.txt
```

### Google Colab (Recommended)
1. Open [Google Colab](https://colab.research.google.com/)
2. Upload `DSA_210_Term_Project.ipynb`
3. Run the script — it will prompt you to upload the 4 data files
4. All plots and results will render inline

### Local (Jupyter Notebook)
Replace the `files.upload()` section with direct file paths:
```python
ranking_file         = "data/FIVB_Volleyball_Rankings.xlsx"
gdp_zip_file         = "data/GDP_WorldBank.zip"
population_zip_file  = "data/Population_WorldBank.zip"
hdi_file             = "data/HDI_UNDP.xlsx"
```

---

## Limitations

- **Sample size:** ~30 countries per gender category — a larger dataset would increase statistical power
- **Causality:** Correlations do not imply causation; a third variable (sports culture, historical tradition) may drive both development and volleyball success
- **Single time point:** All data represents a 2024–2025 snapshot; a longitudinal study would better capture dynamic relationships
- **Youth categories:** FIVB U21/U19/U17 ranking points are approximate estimates (the FIVB website renders these via JavaScript and cannot be scraped directly)

---


**Nehir Eylül Balcı**
Student ID: 33936
DSA 210 — Introduction to Data Science, Spring 2025–2026
Sabancı University
