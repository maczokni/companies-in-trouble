# Restricted data (not included)

The analysis uses aggregated county court judgment (CCJ) records against companies for 2015. We obtained them from the Consumer Data Research Centre (CDRC), which is now the [Geographic Data Service (GDS)](https://data.geods.ac.uk/dataset/county-court-judgments-ccjs). The licence does **not** permit redistribution, so the data are not in this repository. For the same reason, everything in `outputs/` (the rendered report, tables, figures and model caches) is excluded by `.gitignore`.

## How to get the data

Apply to the GDS for access to **County Court Judgments – Corporate Debt, 2015** (aggregated data). Save the workbook in this folder as:

```
data/restricted/2015 Aggregated Data Corporate.xlsx
```

## What the code expects

The workbook as supplied, with sheets named `GOR` (regions) and `MSOA`. On each sheet, rows 1–8 are metadata, row 7 holds the column names, and the data start in row 9.

| Sheet | Geography columns used | Measures (seven `DATA_VALUE` columns, in this order) |
|---|---|---|
| `GOR` | `GOR_NAME` | Total CCJs; total value of CCJs; average value of CCJs; CCJs under £250; under £500; under £1,000; over £1,000 |
| `MSOA` | `MSOA_CODE`, `LA_NAME` | as above |

Only the total and average values are used: the number of CCJs is total value divided by average value (see `analysis.Rmd`).

## Without the data

Set `synthetic_ccj: true` in the YAML header of `analysis.Rmd` to run every step with simulated CCJ counts. The results are then meaningless, and the rendered report says so.

**Do not commit the CCJ data, or any outputs derived from them, to this repository.**
