# API Data Recipes

Python code patterns for fetching economic data from major APIs. Used by the Data-engineer agent.

---

## FRED (Federal Reserve Economic Data)

```python
import pandas as pd
import requests
from datetime import datetime

def fetch_fred_series(series_id: str, api_key: str,
                      start_date: str = "2000-01-01",
                      end_date: str = None) -> pd.DataFrame:
    """Fetch a single FRED series."""
    if end_date is None:
        end_date = datetime.now().strftime("%Y-%m-%d")
    
    url = "https://api.stlouisfed.org/fred/series/observations"
    params = {
        "series_id": series_id,
        "api_key": api_key,
        "file_type": "json",
        "observation_start": start_date,
        "observation_end": end_date,
    }
    
    response = requests.get(url, params=params)
    response.raise_for_status()
    data = response.json()["observations"]
    
    df = pd.DataFrame(data)
    df["date"] = pd.to_datetime(df["date"])
    df["value"] = pd.to_numeric(df["value"], errors="coerce")
    return df[["date", "value"]].rename(columns={"value": series_id.lower()})
```

### Common FRED Series

| Series ID | Description |
|-----------|-------------|
| `UNRATE` | Unemployment Rate |
| `CPIAUCSL` | CPI (All Urban Consumers) |
| `FEDFUNDS` | Federal Funds Rate |
| `GDP` | Gross Domestic Product |
| `GDPC1` | Real GDP |
| `DFF` | Daily Federal Funds Rate |
| `T10Y2Y` | 10-Year minus 2-Year Treasury Spread |
| `PAYEMS` | Total Nonfarm Payrolls |
| `UMCSENT` | Consumer Sentiment (Michigan) |
| `HOUST` | Housing Starts |
| `M2SL` | M2 Money Supply |

---

## World Bank

```python
def fetch_world_bank_data(indicator: str, country: str = "US",
                          start_year: int = 2000,
                          end_year: int = 2023) -> pd.DataFrame:
    """Fetch a World Bank indicator for a country."""
    url = f"https://api.worldbank.org/v2/country/{country}/indicator/{indicator}"
    params = {
        "format": "json",
        "date": f"{start_year}:{end_year}",
        "per_page": 500,
    }
    
    response = requests.get(url, params=params)
    response.raise_for_status()
    data = response.json()[1]
    
    df = pd.DataFrame(data)
    df = df[["date", "value"]].copy()
    df["date"] = pd.to_numeric(df["date"])
    df["value"] = pd.to_numeric(df["value"], errors="coerce")
    return df.sort_values("date").reset_index(drop=True)
```

### Common World Bank Indicators

| Indicator | Description |
|-----------|-------------|
| `NY.GDP.PCAP.CD` | GDP per capita (current US$) |
| `SI.POV.GINI` | Gini Index |
| `SL.UEM.TOTL.ZS` | Unemployment (% of labor force) |
| `SE.XPD.TOTL.GD.ZS` | Education expenditure (% of GDP) |
| `SP.DYN.LE00.IN` | Life expectancy at birth |
| `FP.CPI.TOTL.ZG` | Inflation (CPI, annual %) |
| `NE.TRD.GNFS.ZS` | Trade (% of GDP) |

---

## Usage Example

```python
# Fetch multiple FRED series and merge
api_key = "YOUR_FRED_API_KEY"  # Get from https://fred.stlouisfed.org/docs/api/api_key.html

series_list = ["UNRATE", "CPIAUCSL", "FEDFUNDS"]
dfs = [fetch_fred_series(s, api_key) for s in series_list]

from functools import reduce
merged = reduce(lambda left, right: pd.merge(left, right, on="date", how="outer"), dfs)
merged = merged.sort_values("date").reset_index(drop=True)
merged.to_csv("data/raw/macro_indicators.csv", index=False)
```
