# Data Download Instructions

The four CSV files required to run the notebooks are **not included** in this repository due to data licensing terms. All datasets are freely available from official sources. Download each file, rename it exactly as shown below, and place it in this `data/` folder.

---

## File 1 — Canadian Zero-Coupon Bond Yields

**Filename:** `yield_curves.csv`  
**Source:** Bank of Canada  
**URL:** https://www.bankofcanada.ca/rates/interest-rates/bond-yield-curves/  
**Steps:**
1. Go to the URL above
2. Scroll to "Yield Curve Data"
3. Select "Retrieve all data"
4. Click Submit
5. Download the CSV file
6. Rename to `yield_curves.csv`

**What it contains:** Daily zero-coupon bond yields for Government of Canada bonds, maturities 0.25 to 30 years, expressed as decimals (0.05 = 5%). Goes back to 1986 but data before 1994 has gaps.

---

## File 2 — CORRA Overnight Rate

**Filename:** `CORRA.csv`  
**Source:** Bank of Canada  
**URL:** https://www.bankofcanada.ca/rates/interest-rates/canadian-interest-rates/  
**Steps:**
1. Go to the URL above
2. Click "Canadian Overnight Repo Rate Average"
3. Select full date range
4. Download CSV
5. Rename to `CORRA.csv`

**What it contains:** Canadian Overnight Repo Rate Average (CORRA) — Canada's risk-free overnight rate. Used in Method 2 calibration (Phillips-Yu estimation). Available from August 1997.

---

## File 3 — US Treasury Yields (FRED)

**Filename:** `FRED_treasury_rates_merged.csv`  
**Source:** Federal Reserve Economic Data (FRED)  
**URL:** https://fred.stlouisfed.org  

Download each of the following series separately by searching the series code on FRED, setting start date to 1997-01-01, and downloading as CSV:

| Series Code | Maturity |
|-------------|----------|
| DGS3MO | 3-Month |
| DGS6MO | 6-Month |
| DGS1 | 1-Year |
| DGS2 | 2-Year |
| DGS3 | 3-Year |
| DGS5 | 5-Year |
| DGS7 | 7-Year |
| DGS10 | 10-Year |
| DGS20 | 20-Year |
| DGS30 | 30-Year |

Then merge them into one file using this Python snippet:

```python
import pandas as pd
import glob

files = {
    'DGS3MO': '3-Month', 'DGS6MO': '6-Month',
    'DGS1': '1-Year',    'DGS2': '2-Year',
    'DGS3': '3-Year',    'DGS5': '5-Year',
    'DGS7': '7-Year',    'DGS10': '10-Year',
    'DGS20': '20-Year',  'DGS30': '30-Year',
}
frames = []
for code, label in files.items():
    df = pd.read_csv(f'{code}.csv', index_col=0, parse_dates=True)
    df.columns = [label]
    df[label] = pd.to_numeric(df[label], errors='coerce')
    frames.append(df)

merged = pd.concat(frames, axis=1).sort_index()
merged.to_csv('FRED_treasury_rates_merged.csv')
```

---

## File 4 — ECB EUR AAA Yield Curve

**Filename:** `ECB_Data_Portal_*.csv` (keep original filename)  
**Source:** ECB Statistical Data Warehouse  
**URL:** https://data.ecb.europa.eu  
**Steps:**
1. Go to the URL above
2. Navigate to Financial Markets → Interest Rates → Euro Area Yield Curves
3. Expand "Euro area yield curve" → "AAA-rated government bonds yield curve" → "Spot curve"
4. Select all available maturities (tick all checkboxes)
5. Set date range: 2004-09-06 to present
6. Download as CSV
7. Keep the original filename (starts with ECB_Data_Portal_)

**What it contains:** Daily AAA-rated Euro area government bond spot rates, maturities from 3 months to 30 years. Rates go negative from 2014–2022. Available from September 2004.

---

## Folder contents after download

```
data/
    README_data.md                      ← this file
    yield_curves.csv                    ← Bank of Canada ZCB
    CORRA.csv                           ← overnight rate
    FRED_treasury_rates_merged.csv      ← US Treasuries merged
    ECB_Data_Portal_[date].csv          ← EUR yield curve
```
