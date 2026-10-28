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

Station location (from the SİM station map, matched in Google Maps): **41.220937° N, 28.707547° E** (Öztürk Cd., Adnan Menderes Mah., Arnavutköy). The station is operated by İstanbul Büyükşehir Belediyesi.

---

# Processed data (`processed/`)

Built by [`notebooks/03_external_data.ipynb`](../notebooks/03_external_data.ipynb) on 28 October 2026. Station data are merged with Open-Meteo weather and Copernicus CAMS forecasts for the station point. All times are local (Europe/Istanbul); SİM timestamps (`:00:56`) are floored to the hour. The analysis copy rules of the EDA apply (`985` readings set to missing).

## `arnavutkoy_hourly_merged.csv` (one row per hour)

| Column | Meaning |
|---|---|
| `tarih` | Hour (local time) |
| `PM10`, `PM2_5`, `SO2`, `NO2` | Station measurements, µg/m³ |
| `met_temperature_2m` | Air temperature, °C |
| `met_relative_humidity_2m` | Relative humidity, % |
| `met_precipitation` | Precipitation, mm |
| `met_surface_pressure` | Surface pressure, hPa |
| `met_wind_speed_10m`, `met_wind_gusts_10m` | Wind speed and gusts at 10 m, m/s |
| `met_wind_direction_10m` | Direction the wind comes **from**, degrees (0 = north) |
| `met_wind_u`, `met_wind_v` | Wind components, m/s (u > 0: towards east, v > 0: towards north) |
| `cams_PM10`, `cams_PM2_5`, `cams_NO2`, `cams_SO2` | CAMS European forecast at the station point, µg/m³ |
| `cams_dust` | CAMS Saharan dust, µg/m³ |

The file starts one week before the station data (2023-09-09) so that lagged inputs exist for the first days.

## `arnavutkoy_daily_merged.csv` (one row per day)

| Column | Meaning |
|---|---|
| `<pollutant>_medyan` | Daily median (forecast target); empty if fewer than 18 hourly values |
| `<pollutant>_ortalama` | Daily mean (for WHO comparison); same 18-hour rule |
| `<pollutant>_saat` | Number of valid hourly values |
| `cams_<x>_ort` | Daily mean of the CAMS value |
| `cams_dust_maks`, `cams_PM10_maks` | Daily maximum of CAMS dust and PM10 |
| `met_*` | Daily mean (temperature, humidity, pressure, wind speed, u, v), daily sum (precipitation), daily maximum (gusts) |
| `met_wind_dir_vektorel` | Daily vector-mean wind direction, degrees |

## Sources and attribution

- Weather: Open-Meteo Historical Weather API (ERA5-based reanalysis), https://open-meteo.com, CC BY 4.0.
- Air quality forecasts: Open-Meteo Air Quality API, CAMS European air quality forecast (`cams_europe` domain; the notebook falls back to `cams_global` for any variable with < 80% coverage and prints which domain was used). Contains modified Copernicus Atmosphere Monitoring Service information.
- The Open-Meteo archive keeps the most recent forecast for each hour; see the limitations in the main README.
