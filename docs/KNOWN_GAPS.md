# Known Gaps

## Missing input files

The reference pipeline (`src/fwhffl_pipeline_original_reference.py`) reads from these
paths, none of which exist in `data/`:

| File referenced in code | Used for | Status |
|---|---|---|
| `NFL_Offensive_Analytics_2024_2025.xlsx` | Stage 2 team environment (Coach Map + Advanced Analytics sheets) | **Missing.** Stage 2 cannot run without it. |
| `FWHFFL_2026_Auction_Database.xlsx` | Source workbook the constants in `VALIDATED_PARAMETERS.md` were measured from | **Missing.** Constants are usable as locked values but not re-auditable. |
| `Updated_2025_Sleeper_League_Results.csv` | Validation Pass 3 (calibration bias check against held-out actuals) | **Missing.** Pass 3 cannot run. |
| `FantasyPros_Fantasy_Football_2025_Offense_Snap_Counts.csv` | Validation Pass 4 (RB committee detection) | **Missing.** Pass 4 cannot run. |

## File-naming mismatches (fixed in `src/fwhffl_pipeline.py`, flagged here for awareness)

The reference script expects:
- `cheatsheet_2_.csv` → what actually exists is `cheatsheet3.csv`
- `FantasyPros_Fantasy_Football_Projections_FLX.csv` (combined RB/WR/TE) → what
  actually exists is three separate files: `..._RB1.csv`, `..._WR1.csv`, `..._TE1.csv`
- `FantasyPros_Fantasy_Football_Projections_QB_1_.csv` → what actually exists is
  `..._QB2.csv`

`src/fwhffl_pipeline.py` has been adapted to load from the RB/WR/TE/QB files that
actually exist instead of a combined FLX file. **Verify column layout before trusting
output** — the adaptation assumes the per-position files share the same stat columns
the original FLX file would have had; this has not been independently re-validated
against a held-out season since the source's Validation Pass 3/4 inputs are also
missing.

## `cheatsheet3.csv` is not a market-value sheet

The reference pipeline's `load_projections()` expects a cheat sheet with `$` auction
values in columns like `VALUES.1`/`VALUES.2`/etc. **`cheatsheet3.csv` has no dollar
values at all** — it's an ADP/ECR ranking export (columns: Overall/Bye Week/Tags/Expert
Notes/My Notes/ADP/ECR, repeated per position block for QB/RB/WR only — no TE block
either). This means Stage 5's "fit the compressive exponent to match the market's own
top-3 average" step has no real market anchor to fit against in this repo as-is.

`src/fwhffl_pipeline.py` handles this by falling back to a default exponent (see the
script's header) and printing a clear warning rather than silently fitting to garbage.
**If Nic has an actual $-valued cheat sheet/market sheet, add it to `data/` and update
`load_projections()` — do not try to reverse-engineer dollar values out of ADP/ECR
ranks.**

## Outstanding decision: keeper list

Stage 6 needs 2 confirmed keepers × 12 teams = 24 players, each with prior salary and
current market value. Not yet supplied. Until it is, any pipeline run should present
its Stage 5 pre-keeper board only, clearly labeled as pending Stage 6.

## Files present in `data/` that don't map to any pipeline stage

Several CSVs (season-by-season `2019`–`2023` files, `Sheet2/3/5`, `Transformed_Data`,
`Heist_RB_Values`, `Position_Rankings_2021/2022`) are historical league-analysis
artifacts, not pipeline inputs. Per prior session notes on this data:

- `Heist_RB_Values-Table_1.csv` is 2023 **preseason projections**, not actual
  production — don't treat it as historical ground truth.
- `Position_Rankings_2021-Table_1.csv` and `Position_Rankings_2022-Table_1.csv`
  contain only QB data despite the generic filenames.
- `2026_draft_results.csv` is actually **2025's** auction results, misnamed.
- `2024_Inflation_Adjusted_Values.csv` is the authoritative source for 2024 actual
  production + draft cost (production in column 11, cost in column 10, PPG in
  column 12).

Confirm these against the raw file before using any of them in analysis — don't trust
filenames at face value in this dataset.
