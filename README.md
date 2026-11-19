# Hurry Up Offense

**Do NFL offenses really surge late in each half, and if so, why?**

Status: Phase 0 complete (question and definitions locked). No data analysed yet.

---

## The idea

Watch enough NFL and you notice a pattern. Both offenses look stuck for most of a half, then in the last few minutes a team marches down the field for a touchdown that looks easy compared with everything before it. Broadcasts love to show it: the quarterback who started badly and then threw a perfect drive.

This project tests whether that pattern is real, and works out why it happens.

The obvious explanation is pace. In a hurry-up offense, the team skips the huddle, the defense can't substitute or communicate well, and the offense dictates the tempo. But pace is only one candidate. Prevent defense, play-calling changes, clock rules and game situation are all plausible too, and some of them have nothing to do with the offense getting better.

The second half of the project asks the strategic question: **if a mechanism is real and controllable, are teams using it enough?**

## Why this is harder than it looks

The pattern is easy to see and easy to fool yourself about.

- **Selection effect in what we notice.** TV only shows the "bad start, great drive" graphic when that contrast exists. Nobody shows "mediocre, then mediocre again."
- **Bad stretches fix themselves.** A team that has just struggled will, on average, do better on its next drive wherever it falls in the game. A late surge only counts if it beats that.
- **Not every yard is equal.** A 15-yard gain on 3rd and 22 against a soft defense is not a real gain. Metrics have to account for that.
- **The defense is not a bystander.** Prevent coverage can make an offense look better without the offense improving at all.

## The two claims

- **Claim A (average effect):** Efficiency and touchdown rate are higher in the late window of each half than in the rest of that half.
- **Claim B (surge after struggle):** Teams that have been struggling score at a higher rate on a late-half drive than on a similar drive at other points in the game.

Claim B is the one that matters most. Claim A is the foundation.

## Approach

| Phase | What | Status |
|---|---|---|
| 0 | Lock the question, definitions and hypothesis list before touching data | Done |
| 1 | Verify Claim A: efficiency by time bucket, both halves, all drives | Not started |
| 2 | Test Claim B: the struggle test, under two definitions of "struggled" | Not started |
| 3 | Test each mechanism (pace, prevent coverage, play-calling, etc.) and size each one | Not started |
| 4 | The "so what": which mechanisms can an offense control, and are teams underusing them? | Not started |
| 5 | Write-up and repo cleanup | Not started |

| 2026-11-18 | After seeing TD rate fall late, added any-score rate (TD or FG) as a co-reported drive metric; TD-rate result kept as originally specified |
| 2026-11-18 | Main focus is the 1st-half late surge; 2nd half end-of-game analysed separately as a different situation (decided after seeing notebook 02) |
2026-11-19: Def 2 tightened (doc v1.3) because the original flagged 55% of drives; decided before looking at any Claim B outcome. Original kept as a sensitivity check.

## Key decisions and why

The full definitions are in [`question-doc.md`](question-doc.md). The main decisions, and the reasoning behind them:

**Scope.** 2016-2025 regular seasons, using nflverse play-by-play data. Recent enough for the modern game, big enough for a lot of plays. Kneel-downs, spikes and overtime are excluded because they distort the numbers.

**1st and 2nd halves are analysed separately.** The end of the 1st half is about using the clock for a last score before the break. The end of the 2nd half is shaped by who is trailing and who is leading. Pooling them would let one hide the other.

**Time is bucketed, not a single cutoff.** Minutes left in the half: 15-8, 8-4, 4-3, 3-2, 2-1, 1-0. The curve shows where the shift starts instead of us guessing. The headline "late window" is the last 4 minutes of the half, as one summary number.

**Plays and drives are both used.** Plays (EPA per play, success rate) give a large sample and let us look at mechanisms. Drives (TD rate) match how the pattern is actually seen, and avoid a trap in EPA: it doesn't know what coverage the defense is in, so a soft underneath gain counts as a real gain. Touchdown rate is the primary drive outcome. Field goals are excluded from it because late in the 1st half teams often settle for them.

**Opening drives stay in the baseline.** Scripted opening drives are usually efficient, so keeping them makes the early half look better and the test harder. If the late window still wins, the result is stronger. Removing them is a robustness check.

**A drive must start with at least 30 seconds left to count.** A drive starting with 10 seconds left can't realistically score a touchdown, which would drag the late numbers down for reasons unrelated to the hypothesis. Starting field position and time left are controlled for, and results are split by starting zone, so a short field after a turnover is visible separately. 20 and 25 seconds are tested as robustness checks.

**"Struggled" is defined two ways, locked before looking at data.**
- *Definition 1 (scoring):* over all prior drives in the same game (minimum 3), no touchdowns and below-average EPA per drive.
- *Definition 2 (ball movement):* over the same drives, first downs per drive below league average, or at least half the drives ending in a three-and-out.

Touchdowns are rare (about 1 in 5 drives), so "no TD" alone is noisy. Running both definitions means the result has to hold whether struggle is measured by scoring or by moving the ball. Lookback is **always within the same game**, never into previous games. No more definitions are added after seeing data.

**Claim B has a proper comparison group.** We select teams because they struggled, not because of what happened next. Then we compare their next drive across three situations: the first drive of the 2nd half, mid-half, and the late window. If only the late drives jump above the rest, timing is doing something. If all of them match, it's just a bad stretch bouncing back. The first drive of the 2nd half is a deliberately hard comparison, because halftime adjustments could also reset a team.

**Game state is not in the headline.** The claims are about time. Score, timeouts and win probability are recorded and used in the mechanism step. If the late effect looks similar whether the offense is leading, tied or trailing, that supports "it's the time itself." If it only shows up when trailing, it's urgency.

## Hypotheses (written before testing)

| # | Hypothesis | How it's tested |
|---|---|---|
| H1 | Pace / no-huddle | `no_huddle` flag; late performance with and without it |
| H2 | Prevent / soft coverage | Depth of target, air yards, yards per completion; like-for-like down, distance and field position |
| H3 | Play-calling shift | Pass rate, early-down pass rate, short vs deep targets |
| H4 | Clock and timeout effects | Incomplete-pass stoppages, timeouts left, plays per drive |
| H5 | Personnel / formation | Shotgun, formation, number of receivers (where available) |
| H6 | Defensive substitution / fatigue limits | No-huddle vs huddle plays in the same situation |
| H7 | Game state / urgency | Score differential, trailing vs leading, win probability |
| H8 | Composition (who has the ball) | Score differential of the team with the ball in each bucket; team-vs-itself comparison if needed |

Any mechanism that turns out to be real gets a second question: can an offense control it on purpose, and are teams underusing it?

## What counts as "real"

- The late effect appears in both halves, or we explain why only one.
- It holds across most seasons, not just a couple.
- It survives a like-for-like comparison (same down, distance and field position).
- For Claim B: late drives after a struggle beat drives at other points after a struggle.

## Robustness checks

Rerun the main result with one choice changed:
- Opening drives removed from the baseline.
- Late window of 3 and 5 minutes instead of 4.
- Drive minimum of 20 and 25 seconds.
- "Struggled" looking back only 4, 6 or 8 drives (same game).
- Team-vs-itself comparison.

## Data

[nflverse / nflfastR](https://github.com/nflverse) play-by-play, 2016-2025. One row per play, with EPA, win probability, `no_huddle`, `shotgun`, time remaining, score differential and drive identifiers. Formation and pass-rusher detail comes from separate participation data and may be thinner, so H5 may be weaker than the others. Gaps are flagged, not hidden.

### A note on EPA

Expected Points (EP) is a model's estimate of how many points an offense will average from a given situation (down, distance, field position, time, score). EPA is the change in EP from one play to the next, so a gain that doesn't improve the situation, like 15 yards on 3rd and 22 that still ends in a punt, scores near zero. Success rate is the share of plays with EPA above zero.

## Repo structure (planned)

```
hurry-up-offense/
├── README.md
├── question-doc.md
├── data/
├── notebooks/
│   ├── 01_load_and_clean.ipynb
│   ├── 02_time_buckets.ipynb
│   ├── 03_struggle_test.ipynb
│   ├── 04_mechanisms.ipynb
│   └── 05_findings.ipynb
```

## Findings

Phase 2: the 1st-half late surge (+0.06 to +0.09 EPA/play) appears equally for teams that had struggled earlier in the game and teams that hadn’t, so it isn’t a rebound from a bad stretch. Robust to 10 variations.

## Limitations (so far)

- EPA knows nothing about the coverage the defense is in.
- Formation and pass-rush data is thinner than the main play-by-play data.
- Observational data can show what goes with a late surge, not prove it causes one. Findings about "teams should do this more" are suggestions, not proven.

## Decision log

| Date | Decision |
|---|---|
| 2026-11-10 | Project defined as two claims (A and B), not just a pace theory |
| 2026-11-10 | Halves analysed separately |
| 2026-11-10 | Time bucketed; last 4 minutes as the headline window |
| 2026-11-10 | Opening drives stay in the baseline |
| 2026-11-10 | Drive minimum of 30 seconds with field position controlled |
| 2026-11-10 | "Struggled" defined two ways; lookback within the same game only |
| 2026-11-10 | Game state kept out of the headline, used in mechanism step |
Phase 3: mechanisms (notebook 04)

The 1st-half "surge" is a valuation effect, not a performance one. EPA/play rises +0.079 in the last 4 minutes of the 1st half, but the football doesn't improve: yards and success rate are flat, and turnovers, sacks and explosive plays are slightly worse. Comparing plays with identical results (same down, distance, field position and outcome) shows why. Late in the half, EPA squeezes outcomes toward zero. An incompletion costs −0.45 instead of −0.78, and a sack −0.89 instead of −1.37, because with little time left a failed play really does cost fewer expected points. That repricing accounts for the whole gap (+0.096); the actual outcomes contribute −0.018.

Everything else fits:

the effect is the same for every team, game state and opponent
no football factor explains it: no-huddle, pace, soft coverage, personnel, defensive substitutions and timeouts were all ruled out or went the opposite way
90% of it survives every control

Offenses and defenses really do change behaviour late (faster snaps, less play action, more and deeper passing; more DBs, fewer rushers, fewer subs), but it nets out to no raw gain.

In the 2nd half, the late dip comes from leading teams running the clock, in every season since 2016.

Scoreboard: results/phase3_scoreboard.csv.