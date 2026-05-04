# 🏐 Explaining National Volleyball Success Through Economic and Development Indicators

**DSA210 — Introduction to Data Science — Term Project**
**Spring 2025–2026 | Sabancı University**

I am Nehir Eylül Balcı (33936), a student at Sabancı University. This is my DSA210 term project.

---

## 📌 Motivation

Volleyball is a sport I closely follow. While watching international competitions, I noticed that certain countries consistently dominate — but not always the wealthiest ones. Brazil and Turkey produce world-class teams despite having lower GDP per capita than many European nations, while massive populous countries are often absent from the top rankings. This sparked my central question: **what actually explains national volleyball success?**

---

## ❓ Research Questions

1. Are economic and development indicators (GDP per capita, HDI, population) associated with national volleyball success?
2. Is the association different for men's vs. women's national teams?
3. Does higher human development reduce the gender performance gap in volleyball?
4. Which countries over- or under-perform relative to their economic development?
5. Is European (CEV) dominance in volleyball statistically significant?
6. Can we build a reliable predictive model for national volleyball performance?

---

## 🗂️ Repository Structure

```
DSA210-TermProject/
│
├── notebooks/
│   ├── 01_data_loading.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_correlation_hypothesis.ipynb
│   ├── 04_regression.ipynb
│   ├── 05_ml_linear.ipynb
│   └── 06_ml_advanced.ipynb
│
├── data/
│   └── volleyball_economic_dataset.csv
│
├── Project Proposal .pdf
├── requirements.txt
└── README.md
```

> Raw data files (World Bank zips, HDI Excel, FIVB Excel) are not stored in the repository due to size.
> Download links are in the Dataset section below.

---

## 📊 Dataset

### Volleyball Performance
| Source | Description | Link |
|--------|-------------|------|
| FIVB / Volleyball World | Senior Men's & Women's World Rankings (2025) | [volleyballworld.com](https://en.volleyballworld.com/volleyball/world-ranking/) |
| Wikipedia | Exact WR point values | [FIVB Senior World Rankings](https://en.wikipedia.org/wiki/FIVB_Senior_World_Rankings) |

> FIVB's website renders tables via JavaScript and doesn't support direct download. Point values were collected from Wikipedia which mirrors the official data.

### Socioeconomic Data
| Source | Indicator | Download |
|--------|-----------|----------|
| World Bank | GDP per capita (USD) | [NY.GDP.PCAP.CD](https://data.worldbank.org/indicator/NY.GDP.PCAP.CD) |
| World Bank | Total population | [SP.POP.TOTL](https://data.worldbank.org/indicator/SP.POP.TOTL) |
| UNDP | Human Development Index | [HDR Data Center](https://hdr.undp.org/data-center/human-development-index) |

> Download `API_NY.GDP.PCAP.CD` zip (per capita, not MKTP). HDI file: `HDR25_Statistical_Annex_HDI_Table.xlsx`.

### Final Dataset — `data/volleyball_economic_dataset.csv`

38 countries, merged from all four sources.

| Column | Description |
|--------|-------------|
| `country_std` | Standardised country name (World Bank format) |
| `confederation` | FIVB confederation (CEV, AVC, CSV, NORCECA, CAVB) |
| `pts_men` | Men's Senior FIVB WR points |
| `pts_women` | Women's Senior FIVB WR points |
| `avg_senior_pts` | Average of men's and women's points |
| `gender_gap` | Women's points − Men's points |
| `gdp_per_capita` | GDP per capita USD (World Bank 2023) |
| `log_gdp` | Log₁₀(GDP per capita) |
| `population` | Total population (World Bank 2023) |
| `log_population` | Log₁₀(population) |
| `hdi` | Human Development Index 0–1 (UNDP 2023) |

---

## 🔬 Methodology

The analysis is split across 6 notebooks, each building on the previous.

| # | Notebook | Contents |
|---|----------|----------|
| 01 | Data Loading | Load 4 sources, standardise country names, merge, apply log transforms, export CSV |
| 02 | EDA | Distributions, top 15 bar charts, GDP/HDI scatter plots, gender gap, population paradox, confederation boxplots |
| 03 | Correlation & Hypothesis Testing | Spearman heatmap, 8 hypothesis tests (H1–H8), Mann-Whitney U test for European dominance |
| 04 | Regression | Simple regression, multiple regression (men vs women), residual analysis |
| 05 | ML — Linear Models | StandardScaler, 80/20 train/test split, 4-model comparison (M1–M4), 5-fold cross-validation, feature importance, learning curve |
| 06 | ML — Advanced | kNN regression (k tuning), Decision Tree (depth tuning + tree visualisation), model leaderboard, K-Means clustering (elbow + silhouette), hierarchical clustering dendrogram |

---

## 📈 Key Findings

| Question | Finding | Evidence |
|---|---|---|
| GDP → volleyball? | Moderate positive association | Spearman r ≈ 0.40, p < 0.05 |
| HDI → volleyball? | **Strongest predictor** | Spearman r ≈ 0.50, p < 0.05 |
| Population → volleyball? | **No association — population paradox** | Spearman r ≈ 0.10, p > 0.05 |
| Men vs women? | Effect is stronger for women | Higher HDI coefficient in women's model |
| Europe dominant? | Yes, statistically significant | Mann-Whitney U p < 0.05 |
| Over-performers? | Brazil, Turkey | Largest positive residuals across all models |
| Best ML model? | M3: Log GDP + HDI | Highest CV R² |
| kNN vs Linear? | Comparable — relationship is approximately linear | Similar test R² across all models |
| Country clusters? | Distinct archetypes found | K-Means + hierarchical clustering agree |

### Conclusions 

**1. Development Over Pure Wealth.** The most significant finding is that HDI consistently outperforms GDP per capita as a predictor of success. This suggests that volleyball excellence is more closely linked to broad societal well-being (health, education, and equality) than to raw economic output alone. In the multiple regression models, the impact of HDI was notably higher for women’s teams, indicating that gender-specific sports performance is highly sensitive to overall national development.

**2. The Population Paradox.** We found no significant statistical evidence that a larger population leads to better volleyball results (Spearman r ≈ 0.10, p > 0.05). Nations like Slovenia (approx. 2.1M people) consistently outrank countries with populations 50 to 100 times larger. This confirms our hypothesis that the "quality" of the athletic pipeline and specialized infrastructure is far more important than the "quantity" of potential athletes.

**3. The "Volleyball Culture" Factor.** Our residual analysis across all ML and regression models highlighted Brazil and Turkey as the most significant "positive outliers." These nations produce world-class results that far exceed what their economic and development indicators predict. This leads to the conclusion that qualitative factors—such as institutional federation support, historical tradition, and high national interest—can overcome economic limitations.

**Predictive Feasibility.** While linear and kNN models showed comparable performance, the M3 model (Log GDP + HDI) provided the most reliable predictions. However, the moderate R² values suggest that while socioeconomic data provides a strong foundation, roughly 40-50% of volleyball success is determined by variables outside traditional economic datasets, such as tactical innovation and coaching quality.

---

## ⚙️ How to Run

Install dependencies:
```
pip install -r requirements.txt
```

Open notebooks in Google Colab in order (01 → 06).

- **Notebook 01** requires the 4 raw data files and exports `volleyball_economic_dataset.csv`
- **Notebooks 02–06** only need `volleyball_economic_dataset.csv`

Run all cells: `Runtime → Run all` or `Ctrl+F9`

---

## ⚠️ Limitations

- Small sample (n ≈ 38) limits statistical power — learning curve confirms more data would help
- Correlation ≠ causation — sports culture and federation quality are unmeasured confounders
- Single time point (2024–2025 snapshot) — no longitudinal trends
- FIVB youth category data not directly downloadable

---

## 🤖 AI Usage Disclosure

AI assistance (Claude, Anthropic) was used for code debugging, visualisation suggestions, and documentation. All analysis decisions, interpretations, and conclusions were reviewed and verified by me.

---

**Nehir Eylül Balcı · 33936 · DSA210 Spring 2025–2026 · Sabancı University**
