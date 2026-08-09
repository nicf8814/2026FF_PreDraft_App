# FWHFFL 2026 Auction Draft Kit

Auction-value pipeline for a 12-team, 0.5 PPR, $200-budget, 2-keeper fantasy football
league. Built for use with **Claude Code** — see `CLAUDE.md` for the full project brief
Claude Code reads automatically.

## Quick start

```bash
cd fwhffl-kit
pip install pandas numpy openpyxl
python3 src/fwhffl_pipeline.py
```

This runs Stages 1–6 and the 8-pass validation log against the real data in `./data`,
prints the top 30 priced players, writes the full 156-player board to
`data/output_board_prekeeper.csv`, and prints every validation finding — including the
ones that are currently `ESCALATED` because a source file is missing (see below).

**This produces a pre-keeper board only.** Stage 6's actual inflation math needs 24
confirmed keepers (2/team × 12 teams); until then `FinalValue` = `Value` with `base =
1.0`. Don't present pre-keeper `Value`/`FinalValue` numbers as the final draft board.

## What's here and what's missing

This kit is a straight repackaging of everything that actually existed in the source
Claude.ai project, not a fresh build. Read `docs/KNOWN_GAPS.md` first — it lists four
source files the pipeline logic depends on that are **not** in `data/`
(`NFL_Offensive_Analytics_2024_2025.xlsx`, `FWHFFL_2026_Auction_Database.xlsx`,
`Updated_2025_Sleeper_League_Results.csv`, `FantasyPros_Fantasy_Football_2025_
Offense_Snap_Counts.csv`), plus the fact that `cheatsheet3.csv` — the only cheat-sheet
file present — has no dollar values, so it can't serve as the market-shape anchor the
pipeline was designed around.

The pipeline runs and produces real output anyway: Stage 2 (team environment) and
Validation Passes 3–4 no-op with a printed warning instead of crashing, and Stage 5
falls back to a fixed exponent instead of fitting to a market anchor. Every one of
these degradations is logged in the validation output, not hidden.

**If you have those missing files, drop them straight into `data/`** — the pipeline
already looks for them by name and will use them automatically.

## Reading order for a new Claude Code session

1. `CLAUDE.md` — hard rules and repo map (read automatically, but worth knowing what's
   in it)
2. `docs/LEAGUE_RULES.md` — scoring, roster, keeper, auction mechanics
3. `docs/DRAFT_KIT_FRAMEWORK.md` — the six-stage pipeline design
4. `docs/VALIDATED_PARAMETERS.md` — the locked constants and where they came from
5. `docs/KNOWN_GAPS.md` — what's missing and what to do about it

## Next steps (in priority order)

1. Supply the 24-player keeper list (player, prior salary, market value per player) to
   unlock Stage 6.
2. Supply the four missing source files if available, to unlock Stage 2 and Validation
   Passes 3–4, and to give Stage 5 a real market-shape anchor.
3. Decide whether to rebuild 2023/2025 actual production from box scores (flagged as an
   open decision in prior work on this project — not started).
4. Build "Path C": an interactive web app for keeper flagging sitting on top of this
   pipeline (deferred goal from prior sessions, not started in this kit).
