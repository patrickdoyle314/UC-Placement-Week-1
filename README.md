# Where Unaccompanied Children Are Placed: Week 1

Week 1 (scope and data assembly) of a project where unaccompanied children were released to sponsors
across U.S. states and counties, FY2015–FY2025, and what a future rise in national volume would mean for
local services (schools, legal aid, case management). Service planning only.

This repository holds the Week 1 work on its own. The full project, with later weeks, is
**[UC-Placement](https://github.com/patrickdoyle314/UC-Placement)**.

- Problem statement: [`docs/problem_statement.md`](docs/problem_statement.md)
- Plan of record: `docs/portfolio_plan_2026-10-20.docx` (kept local, not published)
- Changes: [`CHANGELOG.md`](CHANGELOG.md)


## Week 1 checklist

| Week 1 task | Status | Where |
|---|---|---|
| One-page problem statement, after FSO and contract-lead approval | [`docs/problem_statement.md`](docs/problem_statement.md) |
| Reshape the MPI file to one row per fiscal year and place, keyed by county FIPS code | Done: 389 of 393 counties keyed by FIPS (4 Connecticut counties unmatched) | [`src/build_dataset.py`](src/build_dataset.py) → `releases_by_fy_geo.csv` |
| Add an "other counties" row per state (state total minus listed counties); join Census county data | Done: Vintage 2024 total and school-age population. ACS language data pending | same |
| State and county totals reconcile to the MPI file for every year | Done: 648 of 648 checks pass | `reconciliation.csv` |

## Guardrails

These apply to every file, chart and write-up in this project:

1. **Public, aggregate data only.**
2. **Service planning only.** Never framed as, or usable as, a tool for locating children, sponsors or households.
3. **Respect ORR's threshold.** Counties with fewer than 50 releases are not published, and their counts are never backed out.
4. **Neutral language.** "Unaccompanied children", "released to sponsors"; describe patterns, not blame.
5. **Counts are service needs,** not characteristics of the communities.

## Layout

```
UC-Placement-Week-1/
  README.md, CHANGELOG.md, requirements.txt
  docs/problem_statement.md   one-page problem statement (draft for approval)
  src/build_dataset.py        reshape MPI data, add other-counties rows, join Census, check the totals
  exchange/                   contract and export for adjacent projects (see Exchange)
  data/raw/                   downloaded sources (not committed; see Data)
  data/processed/             generated tables (not committed)
```

## Reading the code

Every Python file opens with a **HOW TO READ THIS FILE** block: what the file does for the project
(business purpose) and the Python ideas it uses. Comments through the code explain *why* each step exists
(for example, why "other counties" are never split) and point out Python techniques where they first
appear. Start with `src/build_dataset.py` (openpyxl, regular expressions, `defaultdict`, dict
comprehensions, `csv.DictWriter`), then `exchange/export_exchange.py` (`csv.writer`, sets, `json`).

## Data

| Source | File | Notes |
|---|---|---|
| [Migration Policy Institute: Released to Sponsors by State and County, FY 2014–2025](https://www.migrationpolicy.org/data-tool/unaccompanied-children-state-county) | `data/raw/mpi/mpi-data-hub_uacsbystate-county-fy2014-fy2025.xlsx` (133,945 bytes, downloaded 2026-10-22) | Tabulation of ACF/ORR data. Counties listed only for years with ≥ 50 releases. First release location only. Includes FY2014, Puerto Rico and the U.S. Virgin Islands. |
| [Census Bureau Vintage 2024 county estimates by age and sex](https://www2.census.gov/programs-surveys/popest/datasets/2020-2024/counties/asrh/) | `data/raw/census/cc-est2024-agesex-all.csv` (8,723,627 bytes, downloaded 2026-10-22) | County FIPS codes; July 1, 2024 total and ages 5–17 population. Covers the 50 states and DC only. |
| Census ACS 5-year (language spoken at home) | — | Pending: needs a free Census API key, which you request yourself at api.census.gov. |

## Run

Save the two source files at the paths in the Data table, then:

```bash
pip install -r requirements.txt      # openpyxl, for reading the MPI spreadsheet
python src/build_dataset.py          # Week 1 table + reconciliation
python exchange/export_exchange.py   # aggregates for adjacent projects
```

## Week 1 results

`src/build_dataset.py` writes:

- **`data/processed/releases_by_fy_geo.csv`**: 3,673 rows, one per fiscal year × geography:
  12 national, 636 state (50 states, DC, PR, VI), 2,389 county, 636 "other counties".
  Columns: `fiscal_year, geo_level, state, state_name, county_fips, county_name, county_key, releases,
  listed_counties, pop_total_2024, pop_5_17_2024`.
- **`data/processed/reconciliation.csv`**: 648 checks, **all pass**. For every state and year, listed
  counties plus "other counties" equal the state total. For every year, states sum to the national total.
- **`data/processed/county_fips_unmatched.csv`**: 389 of 393 MPI counties matched to a FIPS code. The 4
  unmatched are Connecticut counties (Fairfield, Hartford, New Haven, New London): since 2022 the Census
  reports Connecticut by planning region, not county.

Findings for the plan:

- The MPI file **includes FY2014** (53,515 releases); the plan's data table lists it as unavailable.
- MPI's national totals include Puerto Rico and the U.S. Virgin Islands (together 0–9 releases a year, none
  in FY2014–FY2016 or FY2021); both are kept as state-level rows so the totals reconcile.
- 15 state-year cells are blank in MPI's States sheet (Alaska FY2018 and FY2020, Montana FY2019, Vermont
  FY2017, Puerto Rico in 4 years, the Virgin Islands in 7). They count as 0 releases, as in MPI's own
  FY 2014–2025 total column; read that way, the states add up to the national total in every year.
- Census population is one reference year (July 1, 2024) on every row. Puerto Rico and the Virgin Islands
  have no population in this file.
- The plan says to join ACS county data. Total and school-age population come from the Vintage 2024
  estimates above, which need no API key; the ACS join (language spoken at home) is pending (see Data).
