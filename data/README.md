# Data

## File

`arnavutkoy_hourly_2023-09-16_2026-09-16.xlsx` — raw hourly data, unchanged from the original download (only the file name was shortened).

| | |
|---|---|
| Source | Ministry of Environment, Urbanisation and Climate Change — SİM station data download: https://sim.csb.gov.tr/STN/STN_Report/StationDataDownloadNew |
| Station | İstanbul – Arnavutköy |
| Period | 16.09.2023 00:00 – 16.09.2026 00:00 (hourly; timestamps end in :00:56) |
| Pollutants | PM10, PM2.5, SO2, NO2 (µg/m³) |
| Downloaded | 16 October 2026 |
| Original file name | `İstanbul - Arnavutköy_16.09.2023-16.09.2026_Hava Kalitesi - İstasyon Veri İndirme_Detay.xlsx` |

## Format notes

- The first two rows are headers (station name, then pollutant names and units).
- Numbers use a decimal comma (e.g. `39,90`) and missing values are shown as `-`.
- Seven readings equal to `985` (3 in PM10, 4 in PM2.5) are treated as suspicious and set to missing in the analysis copy; the raw file is not modified.

## How to reproduce the download

On the SİM page, select station İstanbul – Arnavutköy, the period above, hourly data and the four pollutants, then download as Excel.
