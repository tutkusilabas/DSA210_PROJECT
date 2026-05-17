# Climate or Currency? Turkey Tourism Analysis

**DSA210 Term Project / Spring 2025-2026**  
**Tutku Sıla Baş — Sabancı University**

---


🌐 **Project Website:** https://tutkusilabas.github.io/DSA210_PROJECT/

## 1. Project Overview

This project investigates what drives foreign tourist arrivals to Turkey between **2015 and 2025**: **climate and seasonality** or **economic conditions**.

The central research question is:

> **What drives tourism to Turkey more strongly: climate differences or economic factors?**

To answer this, the project combines monthly tourism arrivals, temperature data, inflation indicators, and exchange-rate information. The analysis is built around seven hypotheses, exploratory data analysis, and machine learning validation.

The project does not treat tourism as only a cultural topic. It treats each monthly arrival number as a measurable signal connected to weather, seasonality, price competitiveness, inflation, exchange rates, and global shocks such as COVID-19.

---

## 2. Motivation

This project was inspired by one of my favorite activities during high school: walking from Sultanahmet to Eminönü to buy coffee from Kurukahveci Mehmet Efendi. While walking through this highly touristic route, I often found myself wondering where the visitors around me came from and what motivated them to choose Istanbul.

Turkey is one of the most visited countries in the world, but the reasons behind tourism flows are not always obvious from raw arrival numbers. Some tourists may come because Turkey is warmer than their home country, while others may be influenced by exchange-rate movements that make Turkey relatively cheaper. Many also arrive during summer simply because holiday calendars and school breaks concentrate travel in July and August.

This project therefore asks whether foreign tourist arrivals to Turkey are driven more by **climate and seasonality** or by **economic factors** such as inflation, currency depreciation, and exchange-rate effects. This distinction matters because each explanation leads to different policy and business implications: climate-driven tourism points to season planning, while economics-driven tourism points to price competitiveness and source-market purchasing power.

---

## 3. Main Findings

| Finding Area | Result | Interpretation |
|---|---:|---|
| **Climate / seasonality** | Strongest single driver | Istanbul temperature explains a large share of monthly variation in arrivals. |
| **Economic indicators** | Supported but weaker | Turkey inflation and exchange-rate channels matter, but CPI alone explains less than temperature. |
| **Country-level differences** | Important | Some countries show escape-the-cold behavior, while others mainly follow the summer tourism calendar. |
| **COVID-19 shock** | Strong structural break | 2020–2021 must be treated separately because pandemic restrictions overwhelm normal tourism patterns. |
| **Machine learning validation** | Confirms climate importance | Temperature appears as the strongest individual feature, but exchange-rate variables remain meaningful. |

**Overall conclusion:**  
Climate and seasonality are the strongest single explanatory force in the dataset. However, economics should not be dismissed: once the exchange-rate channel is included, economic variables become a serious secondary explanation.

---

## 4. Dataset

### 4.1 Analysis Period

**January 2015 – December 2025**

The project works at monthly frequency. This allows the analysis to capture seasonality, COVID disruption, and month-to-month economic changes.

### 4.2 Data Sources

| Data Type | Source | Frequency | Usage |
|---|---|---:|---|
| Tourist arrivals by nationality | Republic of Türkiye Ministry of Culture and Tourism | Monthly | Main dependent variable |
| Turkey CPI inflation | TÜİK / TURKSTAT | Monthly | Domestic economic pressure |
| Source-country inflation | Eurostat HICP and World Bank | Monthly / annual | Purchasing-power comparison |
| EUR/TRY and cross exchange rates | Eurostat | Monthly | Exchange-rate competitiveness |
| Temperature data | Open-Meteo Historical Weather Archive / ERA5 | Daily aggregated to monthly | Climate and temperature-gap analysis |

### 4.3 Final Panel

The country-level analysis focuses on the top source countries by cumulative arrivals:

- Germany  
- Russia  
- United Kingdom  
- Bulgaria  
- Iran  
- Georgia  
- Netherlands  
- Iraq  
- Saudi Arabia  
- Poland  

The final processed data includes monthly arrivals, Turkey temperature, source-country temperature, Turkey inflation, source-country inflation, and exchange-rate variables where available.

---

## 5. Methodology

### 5.1 Data Cleaning and Integration

The raw data came from multiple sources with inconsistent naming and file structures. The pipeline therefore performs several cleaning steps:

- Standardizes country names across Turkish Ministry files, Eurostat tables, World Bank indicators, and weather locations.
- Removes aggregate rows such as total regions or continent-level summaries.
- Converts monthly files into a consistent long-format dataset.
- Joins all sources on `(year, month)` or `(year, month, country)`.
- Flags source-country inflation values that come from annual World Bank data rather than monthly Eurostat data.

### 5.2 Feature Engineering

Two variables are central to the project:

| Feature | Definition | Why it matters |
|---|---|---|
| `temperature_gap` | source-country temperature − Turkey temperature | Tests whether tourists are escaping colder home weather. |
| `inflation_difference` | Turkey inflation − source-country inflation | Measures relative price pressure between Turkey and the visitor’s home country. |

The project also uses seasonality measures such as the share of annual arrivals concentrated in June–September.

---

## 6. Hypothesis Tests

The project uses seven hypothesis tests. The H1–H7 labels are kept for academic clarity, but each hypothesis is written in plain language to make clear what is being measured.

| Hypothesis | What it Measures | Result |
|---|---|---|
| **H1 — Istanbul temperature and total tourist arrivals** | Tests whether warmer months in Turkey are associated with more total foreign arrivals. | ✅ Strongly supported |
| **H2 — Turkey inflation and tourism demand** | Tests whether Turkey becoming more price-competitive is associated with higher arrivals. | ✅ Supported, but weaker than climate |
| **H3 — Source-country inflation and outbound tourism** | Tests whether inflation in tourists’ home countries changes travel behavior toward Turkey. | ⚠️ Mixed |
| **H4 — Temperature gap and escape-the-cold behavior** | Tests whether tourists come when Turkey is warmer than their home country. | ⚠️ Country-specific |
| **H5 — Climate versus economics** | Directly compares whether climate explains more variation than economics. | ✅ Climate stronger |
| **H6 — Lagged economic effects** | Tests whether economic signals affect tourism after a delay. | ⚠️ Inconclusive |
| **H7 — COVID-19 structural break** | Tests whether pandemic years are statistically different from normal years. | ✅ Confirmed |

---

## 7. Key Visualizations

### 7.1 Annual Foreign Arrivals

![Annual foreign arrivals](figures/eda_figures/yearly_foreigners.png)

*Figure 1. Annual foreign arrivals to Turkey, 2015–2025. The chart shows growth before 2019, a sharp COVID-19 collapse, and a strong recovery after 2021.*

### 7.2 Monthly Seasonality

![Monthly arrivals by year](figures/eda_figures/monthly_line.png)

*Figure 2. Monthly arrivals by year. The repeated July–August peak shows that tourism to Turkey has a strong seasonal structure.*

### 7.3 Climate versus Economics

![Climate vs economics](figures/hypothesis_figures/H5_climate_vs_economics.png)

*Figure 3. The central comparison of the project. Climate explains more variance in monthly arrivals than Turkey CPI alone.*

### 7.4 Machine Learning Feature Importance

![Feature importance](figures/ml_figures/ml_feature_importance.png)

*Figure 4. Random Forest feature importance. Temperature is the strongest single predictor, while exchange rate and inflation remain important secondary signals.*

---

## 8. Machine Learning Analysis

The machine learning section is used as validation rather than as a replacement for hypothesis testing.

### 8.1 Regression

Linear Regression and Decision Tree Regression are used to predict monthly arrivals from climate and economic variables. The Decision Tree learns a first split around Istanbul temperature, which supports the statistical finding that temperature is the strongest first-order signal.

### 8.2 Classification

A Logistic Regression model classifies months into high-season and low-season categories. The high accuracy confirms that the tourism calendar is strongly separable using climate and economic features.

### 8.3 Random Forest Feature Importance

Random Forest feature importance is used to compare the relative contribution of temperature, exchange rate, and inflation. The result supports the main conclusion: temperature is the strongest individual feature, but exchange-rate competitiveness is also meaningful.

### 8.4 K-Means Clustering

K-Means clustering is used to see whether the algorithm can rediscover tourism seasons without being directly given month labels. The clusters align with low season, shoulder season, and peak season, supporting the idea that seasonality is embedded in the feature space.

---

## 9. Web Application

🌐 **Project Website:** https://tutkusilabas.github.io/DSA210_PROJECT/

The web application is an interactive extension of the report. It allows users to select a source country and view the country’s tourism pattern, including total-arrival rank, strongest year, peak months, seasonality, temperature-gap behavior, and inflation sensitivity.

The website is built as a static HTML/CSS/JavaScript page and can be opened directly through the GitHub Pages link above.

## 10. Repository Structure

```text
.
├── README.md
├── Report.md
├── climate_currency_country_explorer.html
├── index.html
├── requirements.txt
├── country_profiles_summary.csv
├── country_profiles_data.json
├── eda_analysis_and_hypotheses_testing.ipynb
├── Machine_Learning_Analysis.ipynb
├── datasets/
│   ├── raw_data/
│   └── processed_data/
├── figures/
│   ├── eda_figures/
│   ├── hypothesis_figures/
│   └── ml_figures/
└── TutkuSilaBas_DSA210_TermProject_Proposal.pdf
```

---

## 11. Limitations and Future Work

- **Seasonality confounding:** Temperature and summer holidays are strongly connected, so temperature may partly represent the holiday calendar.  
- **Annual inflation values for some countries:** World Bank inflation is annual for several non-Eurostat countries, which reduces monthly precision.  
- **No causal identification:** Correlations and machine learning feature importance do not prove causality.  
- **Missing tourism confounders:** Visa changes, flight availability, geopolitical shocks, and hotel prices are not fully modeled.  
- **Future work:** Add country fixed effects, include hotel and flight prices, expand the country panel, and test causal models or panel regressions.

---

## 13. AI Usage Disclaimer

AI tools were used for writing assistance, code debugging, wording refinement, and formatting support. The statistical analysis, interpretation of results, dataset construction, and final conclusions were checked manually by the project author.

---

## 14. References and Tools

**Data Sources**

- Republic of Türkiye Ministry of Culture and Tourism  
- TÜİK / TURKSTAT  
- Eurostat  
- World Bank Indicators  
- Open-Meteo Historical Weather Archive / ERA5  

**Python Libraries**

- `pandas`, `numpy`  
- `scipy`, `statsmodels`  
- `matplotlib`, `seaborn`, `plotly`, `altair`  
- `scikit-learn`  
- `jupyter`, `openpyxl`, `xlrd`  

---

## 15. Author

**Tutku Sıla Baş**  
Sabancı University  
DSA210 — Introduction to Data Science  
Spring 2025-2026
