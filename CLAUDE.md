# FWHFFL 2026 Auction Draft Kit — Claude Code Project Instructions

This file is read automatically by Claude Code at the start of every session in this
repo. It is the single source of truth for how to work on this project. Read this in
full before touching any code or data.

## What this project is

A data pipeline that produces auction dollar values and positional tiers for a 12-team,
0.5 PPR, $200-budget, 2-keeper-per-team fantasy football auction draft (league:
"Favre's Waffle House" / FWHFFL, moving from Sleeper to ESPN for 2026). The owner
("Nic") wants values grounded in real 2026 projections and seven seasons of this
league's own auction history — not generic rankings, and never placeholder/manufactured
numbers.

**Read `docs/LEAGUE_RULES.md` and `docs/DRAFT_KIT_FRAMEWORK.md` before making any
methodology decision.** They are not optional background — the scoring formula,
roster caps, and six-stage pipeline design directly determine what "correct" means
for every number this project produces.

## Hard rules — do not violate these

1. **Never manufacture data.** If a projection, salary, or stat is missing, say so and
   stop (or flag it and move on with a logged caveat) — never interpolate, guess, or
   backfill a number and present it as real.
2. **Never close a validation finding silently.** Every pipeline run logs findings as
   ACCEPTED / CORRECTED / ESCALATED. ESCALATED findings stay visible in output until
   the owner resolves them — don't downgrade a finding's severity to make output look
   cleaner.
3. **The league's own scoring formula is authoritative**, not generic site rankings or
   generic PPR values. See `docs/LEAGUE_RULES.md` §Scoring. A player's ESPN/Yahoo/
   FantasyPros consensus rank is not a substitute for scoring their raw stat line under
   this league's rules.
4. **Stage 6 (open-pool repricing) is gated on keepers.** Do not compute final inflation-
   adjusted dollar values until 2 confirmed keepers per team (24 total, all 12 teams)
   are supplied. Pre-keeper board output must say so explicitly, not silently show a
   number as if it were final.
5. **Flag data gaps, don't fill them.** If a required input file referenced in
   `src/fwhffl_pipeline.py` is missing (see "Known data gaps" below), say which file is
   missing and what it was needed for. Do not substitute a different file that happens
   to have similar columns without calling that out as a substitution.

## Repo layout

```
CLAUDE.md                          <- this file
README.md                          <- setup/run instructions
docs/
  LEAGUE_RULES.md                  <- extracted from Fantasy_MAnifesto.docx, plus the
                                       Article III/IV roster-size discrepancy noted
  DRAFT_KIT_FRAMEWORK.md           <- six-stage pipeline spec, reconstructed from the
                                       working code (no separate framework doc existed
                                       in the source project — see note in that file)
  VALIDATED_PARAMETERS.md          <- locked constants (replacement levels, shrink
                                       bands, inflation curves) with provenance
  KNOWN_GAPS.md                    <- missing input files and open decisions
src/
  fwhffl_pipeline.py                <- working pipeline, paths adjusted to ./data,
                                       missing-file guards added (see file header)
  fwhffl_pipeline_original_reference.py  <- untouched copy of the source script,
                                       for diffing if you change the adapted version
data/
  <22 real CSVs + Fantasy_MAnifesto.docx>   <- everything that existed in the
                                       original project's knowledge base
```

## Known data gaps (see docs/KNOWN_GAPS.md for full detail)

The pipeline code (`src/fwhffl_pipeline_original_reference.py`) references several
input files that were **not** present in the source project and are **not** in
`data/`. Do not proceed past Stage 2 in a way that presents output as final until
these are either supplied or the pipeline is deliberately run in degraded/partial mode
with that fact stated in the output:

- `NFL_Offensive_Analytics_2024_2025.xlsx` — team environment / play-caller continuity
  (Stage 2)
- `FWHFFL_2026_Auction_Database.xlsx` — the source-of-truth workbook the locked
  parameters in `VALIDATED_PARAMETERS.md` were measured from
- `Updated_2025_Sleeper_League_Results.csv` — held-out actuals used in Validation Pass 3
- `FantasyPros_Fantasy_Football_2025_Offense_Snap_Counts.csv` — used in Validation
  Pass 4 (RB committee detection)
- Confirmed 2026 keeper list — 2 players/team × 12 teams, with salary + market value
  for each, required to run Stage 6

Also note: the original script expects combined per-position projection files named
`cheatsheet_2_.csv`, `FantasyPros_Fantasy_Football_Projections_FLX.csv`, and
`FantasyPros_Fantasy_Football_Projections_QB_1_.csv`. What actually exists in `data/`
is `cheatsheet3.csv` and separate `..._QB2.csv` / `..._RB1.csv` / `..._WR1.csv` /
`..._TE1.csv` files. The adapted pipeline (`src/fwhffl_pipeline.py`) has been updated
to load from the files that actually exist — check its header comment for exactly what
changed versus the reference version before assuming column layouts match.

## Working style Claude Code should follow on this project

- Spec first, execution second. If asked to change methodology, update the relevant
  doc in `docs/` in the same change as the code.
- Validation is not optional scaffolding — it's the point. Keep the 8-pass validation
  log intact and legible in any pipeline output.
- When re-deriving or updating a constant in `VALIDATED_PARAMETERS.md`, log where the
  new number came from (which file, which seasons) in the same commit/response.
- Prefer flagging over guessing, every time, even if it means the task doesn't finish
  in one turn.
