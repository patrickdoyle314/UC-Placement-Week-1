# Changelog

All notable changes to this repository. Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
dates are when the change was made.

## [Unreleased]

Nothing yet.

## [0.1.0] - 2026-10-27 (Week 1 published on its own)

### Added
- The Week 1 files from [UC-Placement](https://github.com/patrickdoyle314/UC-Placement) v0.5.1, as a separate
  public repository at the project owner's direction: `src/build_dataset.py`, `docs/problem_statement.md`,
  `exchange/` (contract v1.0 and export script) and `.gitignore`. These are the files of UC-Placement's Week 1
  commit, at their reviewed versions.
- `requirements.txt` with only what Week 1 needs (openpyxl; everything else is Python's standard library).
- README and CHANGELOG cut down to Week 1, with a Week 1 checklist.

### Changed
- Problem statement status line names this repository's release date.

### History (from UC-Placement)
- 2026-10-22, v0.1.0: Week 1 built. `src/build_dataset.py` writes one row per fiscal year × geography keyed by
  county FIPS, an "other counties" row per state, and Census total and ages 5–17 population. Found: 648 of 648
  reconciliation checks pass once Puerto Rico and the Virgin Islands are included; 4 Connecticut counties have
  no FIPS match (the Census uses planning regions since 2022); the MPI file includes FY2014.
- 2026-10-26, v0.4.2: comments for readers learning Python ("HOW TO READ THIS FILE" blocks and notes on why
  each step exists).
- 2026-10-27, v0.5.1: Week 1 review. Comments in `src/build_dataset.py` explain its layout, why a blank MPI
  state cell counts as 0, and why Connecticut's "other counties" population stays blank. README corrections:
  the Puerto Rico and Virgin Islands range (0–9 a year, not 1–9), the blank state cells, and the Census file's
  size and date. Rerun: 3,673 rows and 648 of 648 checks pass.

### Pending
- FSO and contract-lead approval of the problem statement.
- ACS language-spoken-at-home join (needs a Census API key).
- Connecticut county-to-planning-region crosswalk.
