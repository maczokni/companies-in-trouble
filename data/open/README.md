# Open data

All files in this folder are published by UK public bodies under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Some file names have been shortened.

| File | Contents | Used for | Source |
|---|---|---|---|
| `uk_business_counts_msoa_nomis.csv` | Businesses by 2011 MSOA, 2016–2025 (column `2016` is used) | MSOA denominator | ONS UK Business Counts, via [Nomis](https://www.nomisweb.co.uk/) |
| `companies_by_region_2015.csv` | Enterprises with the legal status of a company, by region, 2015 | Region denominator (Table 1, Figure 1) | ONS, *UK business: activity, size and location: 2015* (UK Business Entities) |
| `imd2015_file1_index_of_multiple_deprivation.xlsx` | English Index of Multiple Deprivation 2015, File 1 (LSOA ranks and deciles) | Deprivation | [Ministry of Housing, Communities and Local Government](https://www.gov.uk/government/statistics/english-indices-of-deprivation-2015) |
| `lsoa11_msoa11_lookup_ew.csv` | 2011 LSOA to 2011 MSOA (distinct pairs from the ONS *Output Area to LSOA to MSOA to LAD (December 2011) Lookup in England and Wales*) | Aggregating deprivation to MSOAs | [ONS Open Geography Portal](https://geoportal.statistics.gov.uk/) |
| `msoa11_msoa21_lad22_lookup_ew.csv` | MSOA (2011) to MSOA (2021) to LAD (2022) Exact Fit Lookup for E&W (V2) | Assigning MSOAs to local authorities | [ONS Open Geography Portal](https://geoportal.statistics.gov.uk/) |
| `lad22_rgn22_lookup_england.csv` | Local Authority District to Region (December 2022) Lookup in England | Assigning MSOAs to regions | [ONS Open Geography Portal](https://geoportal.statistics.gov.uk/) |
| `regions_2015/` | Regions (December 2015) Ultra Generalised Clipped Boundaries in England | Figure 1 | [ONS Open Geography Portal](https://geoportal.statistics.gov.uk/) |
| `countries_dec2021_uk_bgc.geojson` | Countries (December 2021) UK BGC boundaries | Only with `ea_location_filter: "simplified_england"` | [ONS Open Geography Portal](https://geoportal.statistics.gov.uk/) |

Attribution: Source: Office for National Statistics, licensed under the Open Government Licence v3.0. Contains OS data © Crown copyright and database right.
