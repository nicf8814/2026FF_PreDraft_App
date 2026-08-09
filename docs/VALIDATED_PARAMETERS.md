# Locked Parameters

These are hard-coded constants in `src/fwhffl_pipeline_original_reference.py`. They're
described in the code as "measured league parameters (from `FWHFFL_2026_Auction_
Database.xlsx`)" — that workbook is **not** in `data/` (see `KNOWN_GAPS.md`), so these
numbers currently can't be re-derived or audited from source in this repo. Treat them
as locked-but-unverifiable until that workbook is supplied.

## §2.1 — Replacement level (points per game, by position)

| Position | Replacement PG |
|---|---|
| QB | 18.98 |
| RB | 11.13 |
| WR | 9.15 |
| TE | 10.65 |

Note: TE replacement (10.65) exceeds WR replacement (9.15) — counterintuitive at first
glance, but reflects how the REC_FLEX slot is actually used in this league (drives more
TE scarcity at the replacement line than raw position depth would suggest).

## §2.2 — Mean absolute error (points per game, by position)

| Position | MAE |
|---|---|
| QB | 6.67 |
| RB | 6.54 |
| WR | 6.34 |
| TE | 5.84 |

Used for: Stage 4 tier-break threshold (half of MAE), Validation Pass 6 Monte Carlo
noise.

## §2.2 — Per-game shrink bands

| PG_raw band | Shrink |
|---|---|
| 0–5 | 1.345 (never actually applied — forced to 1.0, see framework doc caveat) |
| 5–10 | 0.960 |
| 10–15 | 0.965 |
| 15–20 | 0.954 |
| 20+ | 0.942 |

Logged as an ESCALATED finding (Pass 3): fitted to Sleeper weekly projections, applied
here as a directional prior against FantasyPros seasonal lines for ESPN scoring — not
a league-fitted number for the actual 2026 input source.

## §2.4 — Open-auction tier inflation (by projected-$ band)

| Projected $ band | Inflation |
|---|---|
| $40+ | +39.6% |
| $25–40 | +28.8% |
| $15–25 | −11.7% |
| $8–15 | −39.1% |
| $3–8 | −48.5% |
| $1–3 | −12.9% |

This is the "barbell, not flat" finding: the old flat 12.5% inflation assumption is
empirically wrong for this league. Studs and near-studs get bid well past model value;
the $8–15 tier gets crushed.

## §2.4 — Position inflation

| Position | Inflation |
|---|---|
| RB | +16.7% |
| QB | −20.3% |
| WR | −11.7% |
| TE | −23.1% |

Stage 6 blends tier inflation and position inflation as a simple average of the two
percentages for each non-kept player.

## §3.2 — Team-environment regression weight

`REG_WEIGHT_DEFAULT = 0.6` — for teams with a new 2026 play-caller, blend their 2025
efficiency multiplier 60% back toward league-average (1.0) before applying it.
`apply_weight = 0.5` — the resulting multiplier is then applied at half strength
league-wide, to avoid double-counting coaching-change signal FantasyPros' seasonal
projections may already reflect.

## Budget / roster constants

`BUDGET = 2400` ($200 × 12 teams), `TEAMS = 12`, `ROSTER = 13`, `POOL_SIZE = 156`
(13 × 12 — the full priced player pool).

## Findings from prior analysis sessions (context, not hard-coded constants)

- **$50+ players are reliable**: all eleven $50+ players in 2025 cleared replacement
  value; the $20–29 band was the worst-value tier that year.
- **Bench management is a bigger edge than draft optimization alone**: average team
  left ~14.6 pts/week on the bench in 2025 through suboptimal lineup decisions.
- **120 pts/week is a conservative target**: only the 62nd percentile of actual 2025
  outcomes; a top-3 finish needed closer to ~128+ actual (~133 projected once ~3.5%
  measured projection optimism at the lineup level is accounted for).
- **Fumble-lost coefficient**: confirmed effective −2 (the manifesto's raw text has two
  stacking −1 rules that net to this), reverse-engineered at a 99.6% match rate across
  roughly 2,957 player-weeks of actual league history.
- **RB committee snap threshold**: 28 snaps/game (corrected from an earlier buggy 40 —
  the original cutoff false-flagged nearly every back; ~38 snaps/game is genuine
  lead-back usage in a rotating backfield, not a platoon).
