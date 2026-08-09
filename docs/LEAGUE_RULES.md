# FWHFFL League Rules (2026)

Source: `data/Fantasy_MAnifesto.docx` ("Favre Waffle House Fantasy Football League
Manifesto," created 2011, last updated August 2024), cross-checked against the
2026-specific rules Nic has confirmed directly. **Where the two disagree, the
2026-confirmed rules below win** — the manifesto document itself contains an internal
contradiction (see "Known discrepancy" below) and hasn't been fully reconciled.

## League structure

- 12 teams, single division
- $200 auction budget per team ($2,400 total league pool)
- Platform: Sleeper (through 2025) → **migrating to ESPN for 2026**

## Roster (2026-confirmed — see discrepancy note)

- **13-man roster**: 7 starters + 6 bench, plus 1 separate IR spot
- **Starting lineup**: QB, RB, RB, WR, WR, FLEX (RB/WR/TE), REC_FLEX (WR/TE)
- **Position caps**: max 2 QB, max 5 RB, max 8 WR
- No kicker, no D/ST (both removed by league vote; see manifesto Appendix I/II/III
  petition history for the votes that got the league here)

### Known discrepancy — roster size

The manifesto's **Article III** says a team is composed of a maximum of **13** NFL
players. **Article IV** (Auction Draft) says "each team must acquire **15** players
during the Auction Draft" and Article I still says lineups are "15 players." This is
an artifact of the manifesto not being fully updated after later roster-size votes
(Appendix III petition #8, prior to 2024 season, lowered bench from 7 to 6 — consistent
with 13 total — but the older 15-player language in Articles I and IV was never
corrected). **Treat 13 as authoritative for 2026** per Nic's direct confirmation; flag
this to Nic if it ever matters for a rules dispute rather than silently picking one.

## Scoring (2026, decimal system — Article IX, as amended)

| Category | Rate |
|---|---|
| Passing yards | 0.05 pts/yard |
| Passing TD | 4 pts |
| Interception thrown | −2 pts |
| Rushing yards | 0.15 pts/yard |
| Rushing TD | 6 pts |
| Receiving yards | 0.15 pts/yard |
| Receiving TD | 6 pts |
| Reception | 0.5 pts (half-PPR, added by 2024 petition) |
| Fumble lost | effective **−2** pts (two stacking −1 rules in the raw manifesto text;
  reverse-engineered and confirmed at 99.6% match rate across ~2,957 player-weeks —
  see `VALIDATED_PARAMETERS.md`) |
| 2-pt conversions (pass/rush/rec) | 2 pts each |

No kicker or D/ST scoring — both positions are removed from the league entirely.

## Keepers

- **Exactly 2 keepers per team**, any position, any prior salary
- Keeper cost: prior salary **+$10** for a 1-year "get-locked-cheap" (GLC) contract, or
  **+$15 then +$10** the following year for a 2-year GLC
- Keeper salaries are removed from the $200/team pool before the open auction; this is
  what drives Stage 6 (open-pool repricing / inflation) in the pipeline — **cannot run
  without a confirmed 24-player keeper list**

## Auction mechanics (Article IV)

- Snake-order-determined nomination order: previous year's reverse final standings
- $1 minimum opening bid, $1 minimum increment
- A team cannot bid more than (remaining budget − $1 per remaining *other* roster
  spot to fill) for any player
- A team cannot bid on a position it has already filled to its max roster cap
- Players on NFL injury/suspension lists to start the season are still draft-eligible

## Trades

- Trade window: end of Auction Draft → Wednesday before Thanksgiving
- 24-hour "trade inception" review window (petition-approved system replacing the old
  veto process): competing offers can be submitted for any 1 player in an accepted
  trade during the window; a successful interception restarts the 24-hour clock
- Trades do not change a player's salary or keeper/contract status
- Rosters must meet Article III position limits immediately after any trade

## Governance

- "Commissioner for Life" model with an optional 2-3-owner Executive Committee for
  disputed rulings; league-wide vote can veto committee decisions
- Rule changes: majority vote, off-season only except 3/4 supermajority for in-season
  emergency changes
- Full petition history (2011–2024) is preserved in the manifesto's Appendices I–III
  if historical context on *why* a rule exists is ever needed — useful for
  understanding, e.g., why kickers/D-ST are gone or why scoring moved to decimals
