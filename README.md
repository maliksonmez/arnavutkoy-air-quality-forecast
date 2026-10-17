# Next-Day Air Quality Forecasting for Arnavutköy, Istanbul

**MTH4800 Innovation Campus AI Applications — Capstone Project, Group 9**

Harun Bazancir · Ata Ali Sarımaden · Ömer Malik Sönmez · Khadıja Huseynova · Aykut Coşkun

---

## Overview

Air pollution is one of the most important environmental health risks in large cities. This project builds a machine-learning tool that forecasts **tomorrow's daily concentrations of PM10, PM2.5, SO2 and NO2** at the Arnavutköy (Istanbul) air quality monitoring station, and compares them with the **WHO 24-hour air quality guideline levels**.

- **SDG link:** SDG 11 — Sustainable Cities and Communities
- **Target users:** residents of Arnavutköy/Istanbul and sensitive groups (children, the elderly, people with respiratory or heart conditions), and local decision-makers
- **Final product (planned):** a Streamlit web app showing tomorrow's forecast, an alert level, historical trends and an explanation of each forecast

## Data

| | |
|---|---|
| Source | Ministry of Environment, Urbanisation and Climate Change — SİM station data download ([sim.csb.gov.tr](https://sim.csb.gov.tr/STN/STN_Report/StationDataDownloadNew)) |
| Station | İstanbul – Arnavutköy |
| Period | 16.09.2023 – 16.09.2026 (hourly) |
| Records | 26,305 hourly rows · 1,097 calendar days |
| Pollutants | PM10, PM2.5, SO2, NO2 (µg/m³) |

The raw data file is not stored in this repository. See [`data/README.md`](data/README.md) for how to download it.

## Methodology

![Project flowchart](docs/flowchart.png)

1. **Cleaning and quality control.** Text-to-number conversion and missing-value audit (3.9–5.8% missing per pollutant). Seven suspicious `985` readings are set to missing in the analysis copy; the raw data is kept unchanged. A sensitivity check shows that this choice has a negligible effect on the results.
2. **Exploratory data analysis.** Distributions, hourly/weekly/monthly patterns, correlations and high-pollution episodes.
3. **Daily aggregation.** A day is valid if it has at least 18 hourly readings. The daily **median** is used because it is robust to single-hour spikes and suspicious values.
4. **Forecasting (Assignment 1 — first experiment).**
   - Target: next day's daily median for each pollutant
   - Inputs: the previous 7 days of all 4 pollutants (28 lag features)
   - Chronological split: ~60% training · ~15% validation · 25% test (test period not used yet; no shuffling, to avoid leakage)
   - Models: **baseline** ("tomorrow = today") vs. **Ridge regression**
   - Metric: **MAE** (mean absolute error, µg/m³)

## Assignment 1 results (validation period, 07.07–17.12.2025)

| Pollutant | Baseline MAE | Ridge MAE |
|---|---:|---:|
| PM10 | **7.57** | 7.67 |
| PM2.5 | **2.63** | 3.40 |
| SO2 | **1.19** | 1.51 |
| NO2 | **9.58** | 11.42 |

In this first experiment, Ridge regression did not beat the simple baseline for any pollutant. Only 48–49 of the 164 validation days could be evaluated, because the model required all 28 inputs to be available.

## Limitations

- Single station: results cannot be generalised to all of Istanbul.
- No meteorological variables (wind, temperature, humidity) yet.
- The time zone of the source data has not been verified.
- `985` readings and hours with PM2.5 > PM10 could not be confirmed as sensor errors without device information.

## Roadmap

- **Assignment 2:** improve the model — lag selection, one model per pollutant using its own history, filling short gaps, additional daily summaries (mean, P90), meteorological data, walk-forward validation, Random Forest and XGBoost.
- **Assignment 3:** final model, Streamlit web app with alerts based on WHO guideline levels and SHAP explanations, Docker container and cloud deployment.

## Repository structure

```
├── data/        how to obtain the raw data (data files are not committed)
├── docs/        flowchart, proposal and presentation
├── notebooks/   Colab notebooks (EDA, forecasting)
└── requirements.txt
```

## How to run

Open the notebooks in Google Colab, then upload the Excel file from the SİM portal when the notebook asks for it.

## References

- Jalali, M. W., Saidi, B., Farahmand, H., Rezvan Panah, M. A., & Saruhan, E. N. (2025). Scalable AI-driven air quality forecasting and classification for public health applications. *Discover Atmosphere, 3*, 25. https://doi.org/10.1007/s44292-025-00052-8
- World Health Organization. (2021). *WHO global air quality guidelines: Particulate matter (PM2.5 and PM10), ozone, nitrogen dioxide, sulfur dioxide and carbon monoxide.* Geneva: WHO. https://iris.who.int/handle/10665/345329
