# London Residential Electricity Demand Analysis

End-to-end time series analysis of hourly electricity consumption across 4,400+ households in London, using smart meter data from a large-scale energy pricing experiment (2020-2021).

## Key Findings

| Metric | Value |
|---|---|
| Households analyzed | 4,429 |
| Observations | 13.5M rows |
| Peak demand hour | 6:00 PM |
| Minimum demand hour | 3:00 AM |
| Temperature correlation | r = -0.49 (inverse) |
| Recommended model | SARIMA(p,1,q)(P,1,Q)_24 |

## Analysis Pipeline

1. **Data Cleaning & Validation** -- Type conversion, integrity checks, handling 88.7% missing values in experimental pricing columns
2. **Panel Data Aggregation** -- Aggregated 4,429 individual smart meters into a single normalized time series, accounting for a 5.9x sample size change mid-experiment
3. **Exploratory Analysis** -- Hourly, daily, weekly, and monthly demand patterns; weekday vs. weekend comparison; temperature-demand relationship
4. **Seasonal Decomposition** -- Additive decomposition (period=24h) isolating trend, seasonal, and residual components
5. **Stationarity Testing** -- ADF and KPSS tests on original and differenced series; ACF/PACF analysis to identify model parameters

## Selected Visualizations

### Daily Demand Pattern
Peak consumption at 6 PM (cooking, heating, lighting), trough at 3 AM. Peak-to-trough ratio of 1.4x.

### Temperature vs. Demand
Strong inverse relationship -- colder weather drives higher electricity consumption, consistent with electric heating load.

### Seasonal Decomposition
Additive model with 24-hour period successfully separates trend, daily seasonality, and white-noise residuals.

### Stationarity Analysis
Original series is non-stationary (slow ACF decay, failed ADF/KPSS). Seasonal differencing (d=1, D=1, s=24) achieves stationarity confirmed by both tests.

## Technical Highlights

- **Normalization strategy**: Per-household demand metric eliminates bias from 5.9x sample size change (742 to 4,375 households), producing a stable series suitable for modeling
- **Data gap handling**: Identified and documented an 8-month gap (Apr-Nov 2020) inherent to the experimental design, scoping conclusions to winter demand patterns
- **Model readiness**: Determined differencing orders (d=1, D=1) and seasonal period (s=24) for SARIMA, with temperature as a candidate exogenous regressor (SARIMAX)

## Tech Stack

- **Python** -- pandas, NumPy
- **Visualization** -- matplotlib, seaborn
- **Statistical Analysis** -- statsmodels (seasonal decompose, ADF, KPSS, ACF/PACF), SciPy
- **Data Format** -- Apache Parquet

## Dataset

Hourly smart meter readings from a residential electricity pricing experiment in London. Contains demand (kWh), temperature, and 24/48/72-hour weather forecasts for 4,429 households over two winter periods.

**Source**: [London Smart Meter Energy Consumption Data](https://data.london.gov.uk/dataset/smartmeter-energy-use-data-in-london-households)

## Future Work

- SARIMA / SARIMAX modeling with temperature as exogenous variable
- Machine learning approaches (XGBoost, LightGBM, LSTM)
- Cross-validation with time series split
- Forecast evaluation (RMSE, MAE, MAPE)

## Getting Started

```bash
git clone https://github.com/aaliusama/london-electricity-demand-analysis.git
cd london-electricity-demand-analysis
pip install pandas numpy matplotlib seaborn statsmodels scipy pyarrow
jupyter notebook London_TimeSeries_Analysis.ipynb
```

## License

MIT
