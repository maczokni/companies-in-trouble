# Environment Agency enforcement actions against companies, England, 2015–2017

`ea_enforcement_actions_2015_2017.csv` contains **1,068 enforcement actions** taken by the Environment Agency (EA) against companies between 2015 and 2017. We compiled it for this study and geocoded it at postcode level. Of these, 1,056 fall inside an English MSOA and are used in the analysis. The rest are in Wales or Northern Ireland, or could not be located.

`manual_coordinate_corrections.csv` lists the six records whose coordinates were corrected by hand, with the reason for each.

## Sources

1. **The EA enforcement database:** enforcement notices and prosecutions (court cases) published by the Environment Agency for the study period (721 records in the final dataset).
2. **[Violation Tracker UK](https://violationtrackeruk.goodjobsfirst.org/):** EA actions in the same period, including civil penalties and enforcement undertakings that are not recorded in the EA database (347 records).

## How the dataset was compiled

1. We searched both sources for all EA enforcement actions against companies decided in 2015–2017.
2. We combined the records and removed duplicates.
3. We counted multiple outcomes decided on the same date against a single company as one action (for example, a conviction for five offences in a combined prosecution).
4. Where the source record did not include the company's address, we traced it through the company's Companies House entry at the time of the enforcement decision.
5. We geocoded addresses with OpenStreetMap Nominatim, falling back to ArcGIS where Nominatim found no match. We checked the results on a map and corrected six records by hand. The geocoding code is in Appendix A of `analysis.Rmd`.

## Codebook

| Variable | Description |
|---|---|
| `action_id` | Row identifier |
| `offender` | Name of the company subject to the action, as recorded in the source |
| `address` | Company address used for geocoding |
| `action_date` | Date of the enforcement decision (YYYY-MM-DD) |
| `relevant_legislation` | Legislation under which the action was taken, as recorded in the source. EA database abbreviations include EPR (Environmental Permitting Regulations), EPA (Environmental Protection Act 1990), WRA (Water Resources Act 1991) and HWR (Hazardous Waste Regulations). Multiple acts are separated by `\|`. Violation Tracker UK records are coded "environment-related offences". |
| `action_type` | `Enforcement Notice` or `Court Case` (EA database), or the Violation Tracker UK category (for example, `environmental violation`, `water pollution violation`) |
| `latitude`, `longitude` | Coordinates of the address (WGS84) |
| `source` | `EA enforcement database` or `Violation Tracker UK` |

## Things to know before using it

- **It records detected and sanctioned non-compliance.** Enforcement reflects regulatory activity as well as corporate behaviour.
- **Locations are company addresses, not offence sites.** For some companies the address is a registered office, which may be far from the site where the offence occurred.
- **Licence.** The EA records are published by the Environment Agency under the Open Government Licence. Records derived from Violation Tracker UK are subject to its terms of use; check them before redistributing those records.

## Citation

Please cite the paper if you use this dataset (full reference to be added on publication).
