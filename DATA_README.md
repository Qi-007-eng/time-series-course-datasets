# Time Series Datasets for ARIMA/SARIMA Course Design

## Overview
All datasets are from the Monash Time Series Forecasting Repository (https://forecastingdata.org/)
and OikoLab (https://oikolab.com), available under CC BY 4.0 license.

---

## Dataset 1: Oikolab Weather (**RECOMMENDED**)
**Source**: https://zenodo.org/records/5184708

**Description**: Hourly climate data near Monash University, Clayton, Australia, 
from 2010-01-01 to 2021-06-01 (11+ years).

**Files**:
- `temperature.csv` - Air temperature in Celsius (°C) ★★★ BEST
- `dewpoint_temperature.csv` - Dewpoint temperature (°C)
- `wind_speed.csv` - Wind speed (m/s)
- `mean_sea_level_pressure.csv` - Mean sea level pressure (Pa)
- `relative_humidity.csv` - Relative humidity (0-1)
- `surface_solar_radiation.csv` - Solar radiation (W/m²) ★★★
- `surface_thermal_radiation.csv` - Thermal radiation (W/m²)
- `total_cloud_cover.csv` - Cloud cover (0-1)

**Statistics**:
- 8 time series (univariate)
- Each has **100,057 observations** (hourly)
- Time range: 2010-01-01 to 2021-06-01

**ARIMA/SARIMA Suitability**:
✅ **Data size**: 100,057 observations (exceeds 10,000 requirement)
✅ **Non-stationarity**: Temperature shows clear yearly trend
✅ **Seasonality**: 
   - Daily cycle (ACF ≈ 0.78 at lag 24)
   - Weekly cycle (ACF ≈ 0.63 at lag 168) 
   - Yearly cycle (ACF ≈ 0.53 at lag 8760)
✅ **Multi-seasonal**: Perfect for SARIMA with seasonal periods 24, 168, 8760
✅ **No missing values**: Clean data, no preprocessing needed
✅ **Perfect for**: Differencing (d=1, D=1, D_s=1), ARIMA/SARIMA modeling, forecasting

**Recommended SARIMA model**: ARIMA(p,d,q)(P,D,Q)_24 with additional yearly seasonal component

---

## Dataset 2: Electricity Hourly
**Source**: https://zenodo.org/records/3889830

**Description**: Hourly electricity consumption of 235 clients from 2012-01-01 to 2014-12-31.

**Statistics**:
- 235 time series (each client is one series)
- Each has **26,304 observations** (hourly, ~3 years)
- Time range: 2012-01-01 to 2014-12-31

**ARIMA/SARIMA Suitability**:
✅ **Data size**: 26,304 observations per series (exceeds 10,000)
✅ **Seasonality**:
   - Daily cycle (ACF ≈ 0.75-0.90 at lag 24)
   - Weekly cycle (ACF ≈ 0.58-0.87 at lag 168)
   - Yearly cycle (ACF ≈ 0.54 at lag 8760)
✅ **Multi-client**: Can test model across multiple series
✅ **Non-constant variance**: Some clients show changing variance over time
✅ **Perfect for**: SARIMA with double seasonality (daily + weekly)

**Recommended SARIMA model**: ARIMA(p,d,q)(P,D,Q)_24 with weekly seasonal period 168

---

## Best for Your Course Design

### Top 1 Choice: Weather Temperature
- 100K+ observations, extreme data volume
- Clear yearly cycle (perfect for seasonal differencing)
- Clean data, no missing values
- Can use `pd.date_range(start='2010-01-01', periods=100057, freq='h')`
- Try ARIMA with seasonal periods: 24 (daily), 168 (weekly), 8760 (yearly)

### Top 2 Choice: Solar Radiation
- 100K+ observations
- **Perfect** daily cycle (ACF=0.904 at lag 24!)
- Strong yearly cycle
- Clear zero values at night (beautiful seasonal pattern)

### Top 3 Choice: Electricity T50
- 26K observations
- Triple seasonality (daily + weekly + yearly)
- Classic SARIMA benchmark dataset

---

## SARIMA Modeling Tips

For hourly data with daily+weekly seasonality:
```
from statsmodels.tsa.statespace.sarimax import SARIMAX

# Single seasonal period (daily)
model = SARIMAX(y, order=(p,d,q), seasonal_order=(P,D,Q,24))

# Double seasonal (use DHR-ARIMA or TBATS instead)
# Or fit one seasonality at a time
```

## References
- Monash Repository: https://forecastingdata.org/
- Zenodo Records: https://zenodo.org/communities/forecasting
- CC BY 4.0 License: https://creativecommons.org/licenses/by/4.0/
