# Companies in Trouble: replication materials

Code and data for the paper *Companies in Trouble: Does Place Matter for Corporate Strain? * (submitted to the *Journal of Criminal Justice*).

The paper asks whether place matters for corporate strain. It maps county court judgments (CCJs) for unpaid debt against companies across England's 6,791 neighbourhoods (Middle Layer Super Output Areas, MSOAs) in 2015, and uses Bayesian surprise maps to account for the uneven distribution of businesses. It then tests whether neighbourhoods with more companies in trouble than expected are more deprived, and whether they are more likely to see Environment Agency enforcement action.

Everything needed to reproduce the analysis is here, **except the CCJ data**. These are licensed and cannot be redistributed (see [Restricted data](#restricted-data)). Every step, from the raw files to every table and figure, is in a single R Markdown document, [`analysis.Rmd`](analysis.Rmd).

## What is in this repository

```
.
├── analysis.Rmd                 data preparation and full analysis (knit this)
├── companies_in_trouble.Rproj   RStudio project
├── data/
│   ├── enforcement/             NEW DATASET: Environment Agency enforcement actions, 2015–2017
│   ├── open/                    open data used in the analysis (denominators, deprivation, lookups, boundaries)
│   ├── boundaries/              MSOA boundaries: download instructions (too large for GitHub)
│   └── restricted/              CCJ data: NOT included; how to apply for access
└── outputs/                     created when you knit: rendered report, tables (CSV), figures (PDF)
```

| Data | Included? | Where |
|---|---|---|
| Environment Agency enforcement actions, 2015–2017 (compiled for this study) | Yes | [`data/enforcement/`](data/enforcement/README.md) |
| Business counts by MSOA (ONS UK Business Counts, 2016) | Yes | [`data/open/`](data/open/README.md) |
| Companies by region (ONS UK Business Entities, 2015) | Yes | [`data/open/`](data/open/README.md) |
| Index of Multiple Deprivation 2015 | Yes | [`data/open/`](data/open/README.md) |
| Geographic lookups and region and country boundaries | Yes | [`data/open/`](data/open/README.md) |
| MSOA boundaries, 2011 (about 245 MB) | No: free download | [`data/boundaries/`](data/boundaries/README.md) |
| County court judgments against companies, 2015 | **No: licensed** | [`data/restricted/`](data/restricted/README.md) |

## The Environment Agency enforcement dataset

As part of this study we compiled a new dataset of **1,056 enforcement actions taken by the Environment Agency against companies in 2015–2017**, from the EA enforcement database and Violation Tracker UK, geocoded at postcode level. 
To our knowledge it is the first geocoded dataset of corporate environmental enforcement in England. We publish it for reuse:

- [`data/enforcement/ea_enforcement_actions_2015_2017.csv`](data/enforcement/ea_enforcement_actions_2015_2017.csv): the dataset
- [`data/enforcement/README.md`](data/enforcement/README.md): sources, how it was compiled, and a codebook
- Appendix A of `analysis.Rmd`: the geocoding code

## Restricted data

The CCJ data are the *County Court Judgments – Corporate Debt, 2015* aggregated data, which we obtained from the Consumer Data Research Centre (CDRC). The CDRC is now the Geographic Data Service (GDS). The licence does not allow redistribution, so the data are not included here. For the same reason, `outputs/` (which contains results derived from them) is excluded by `.gitignore`. Researchers can apply to the GDS for the same data; see [`data/restricted/README.md`](data/restricted/README.md).

**Running the code without the CCJ data.** Set `synthetic_ccj: true` in the YAML header of `analysis.Rmd`. This replaces the CCJs with simulated counts, so you can check that every step runs and inspect the code's outputs. The results are then meaningless, and the rendered report says so prominently.

## How to reproduce the analysis

1. **Get the CCJ data** from the GDS and save the workbook as `data/restricted/2015 Aggregated Data Corporate.xlsx`.
2. **Download the MSOA boundaries** as described in [`data/boundaries/README.md`](data/boundaries/README.md).
3. **Install the R packages:**

   ```r
   install.packages(c("rmarkdown", "knitr", "readxl", "dplyr", "tidyr", "sf", "ggplot2",
                      "patchwork", "bayesiansurpriser", "lme4", "lmerTest", "broom",
                      "broom.mixed", "spdep", "spatialreg"))
   ```

4. **Knit `analysis.Rmd`.** Open `companies_in_trouble.Rproj` in RStudio and click *Knit*, or run:

   ```r
   rmarkdown::render("analysis.Rmd", output_dir = "outputs")
   ```

The rendered report is written to `outputs/analysis.html`, tables to `outputs/tables/` (CSV) and figures to `outputs/figures/` (PDF). The first run takes about 30 minutes, most of it building the spatial neighbours and fitting the spatial error model. Both are cached in `outputs/cache/`, so later runs are quick. To re-estimate the spatial model, set `refit_spatial_model: true`.

### Options (YAML header of `analysis.Rmd`)

| Parameter | Default | Effect |
|---|---|---|
| `refit_spatial_model` | `false` | Re-estimate the spatial error model rather than use the cached fit |
| `ea_location_filter` | `"msoa"` | How enforcement actions are located in England (see [Known issues](#known-issues)) |
| `synthetic_ccj` | `false` | Use simulated CCJ counts so that the code runs without the restricted data |

### Where each result in the paper comes from

| Paper | Section of `analysis.Rmd` | Output file (`outputs/tables/`) |
|---|---|---|
| Descriptive statistics (Data and methods) | Descriptive statistics | `descriptives.csv` |
| Table 1, Figure 1 | RQ1 | `table1_ccjs_by_region.csv`, `figures/figure1_*.pdf` |
| Table 2, Figure 2, Kruskal–Wallis test | RQ1 | `table2_msoa_surprise_by_region.csv`, `rq1_kruskal_wallis.csv`, `figures/figure2_*.pdf` |
| Table 3 | RQ2 | `table3_deprivation_models.csv` |
| RQ2 checks: mixed model, logistic model, ICC, Moran's I | RQ2 | `rq2_checks.csv` |
| Table 4 | RQ3 | `table4_enforcement_model.csv` |
| RQ3 checks: per-SD odds ratio, predicted probabilities, mixed model, Moran's I | RQ3 | `rq3_checks.csv`, `rq3_enforcement_by_surprise.csv` |

## Key analytical decisions

- **Unit of analysis.** 2011 MSOAs (6,791 in England): the smallest geography at which both aggregated corporate CCJs and business counts are available.
- **Counting CCJs.** CDRC rounds counts up to multiples of five for disclosure control. We calculate the number of CCJs in each area as total value divided by average value, neither of which is rounded.
- **Duplicate MSOA.** E02001166 appears twice in the CDRC file (under Manchester and Salford). ONS assigns it to Salford, so we keep only the Salford record.
- **Denominators.** For MSOAs: ONS UK Business Counts for 2016, the earliest year available at that level. For regions: enterprises with the legal status of a company in 2015 (ONS UK Business Entities).
- **Bayesian surprise.** Calculated with [`bayesiansurpriser`](https://CRAN.R-project.org/package=bayesiansurpriser), with expected CCJs proportional to businesses and the package's default model space (uniform, base rate and de Moivre funnel), following Correll and Heer (2017).
- **Deprivation.** The percentage of each MSOA's LSOAs in the 20% most deprived in England (IMD 2015), entered per 10 percentage points.
- **RQ2 models.** The main model is a spatial error model with region fixed effects. Region is fixed because nine regions are too few to estimate a between-region variance reliably. The spatial term is included because the residuals are spatially autocorrelated. The checks are a linear mixed model and a logistic mixed model, each with a random intercept for region.
- **RQ3 model.** Enforcement is too sparse for count models, so we model whether an MSOA had any enforcement action, with a logistic regression with region fixed effects (as in RQ2). As a check, we fit a logistic mixed model with a random intercept for region. The surprise coefficient is reported per unit in Table 4; because surprise spans only about one unit, the text also gives the odds ratio per standard deviation.

## Licences

- **Code:** MIT licence (see [`LICENSE`](LICENSE)).
- **Open data in `data/open/`:** © Crown copyright, used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Contains National Statistics data © Crown copyright and database right, and OS data © Crown copyright and database right.
- **Enforcement dataset:** see [`data/enforcement/README.md`](data/enforcement/README.md). Environment Agency records are published under the Open Government Licence; records from Violation Tracker UK are subject to its terms of use.

## Citation

If you use this code or the enforcement dataset, please cite the paper (full reference to be added on publication). See [`CITATION.cff`](CITATION.cff).
