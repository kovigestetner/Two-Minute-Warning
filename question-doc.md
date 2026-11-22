# The Two-Minute Warning: Question Doc

**Status: complete (v1.7, 2026-11-20).** Written before touching data, then added to phase by phase. Every version is kept below so you can see what was decided before and after each result. Nothing has been deleted; superseded parts are marked.

## Contents
- Version log (just below)
- Final answers
- §1-14: original question, definitions and plan (v1.0-v1.3)
- §15: Phase 3 pre-specification (v1.5)
- §16: Phase 3 results (v1.6)
- §17: Browns case study + clutch gene pre-specification (v1.7)
- Change log (after seeing data)

## Final answers

| Question | Answer | Where |
|---|---|---|
| Claim A: do offenses do better late in halves? | In EPA, yes in the 1st half (+0.079 per play). On raw outcomes, no: yards and success flat, sacks and turnovers slightly worse. 2nd half: slightly lower | Notebook 02, §16 |
| Claim B: do struggling teams surge more? | No. Same late bump as teams that weren't struggling | v1.4 below, notebook 03 |
| Why does EPA rise late? | Valuation: EPA prices the same result higher late (+0.096) while outcomes are slightly worse (−0.018). Every football mechanism tested was ruled out | §15-16, notebook 04 |
| What is real? | 2nd-half late dip comes from leading teams (every season); small first-down bump late in both halves | §16 |
| 2026 Browns / clutch gene | They converted go-ahead chances at about twice the typical rate, but clutch doesn't persist in the data and comeback-heavy starts regress to ~.500. Forecast ~.46 for the last 7 games | §17, notebook 05 |

## Version log

v1.0 (Phase 0): original question, definitions and plan (§1-14).

v1.1 (2026-11-18): added bucket B0 and the boundary rule (v1.0 buckets only covered the last 15 minutes of a 30-minute half), spelled out the drive-start rules, closed the open items, added the uncertainty method. No outcome data had been looked at.

v1.2 (2026-11-18): added after seeing notebook 02 results. (a) Any-score rate (TD or FG) is now reported alongside TD rate, because TD rate is biased against drives that start late with little clock. The TD-rate result stays reported as originally specified. (b) Main focus is the 1st-half late surge; the 2nd half is analysed separately as a different situation (clock management, leading vs trailing) and used as a comparison.

v1.3 (2026-11-19): Definition 2 tightened. The original (first downs per drive below the season average, OR at least half of earlier drives three-and-outs) flagged 55.5% of drives with 3+ earlier drives, too loose to isolate struggling teams. New Def 2: the team’s first downs per drive over its earlier drives in the game is at or below the 25th percentile of that season (among drives with 3+ earlier drives), OR at least half of earlier drives are three-and-outs. This flags 28.7%; 51.4% of those are also Def 1. The original version is kept as struggled_2_loose for a sensitivity check. Decided before any Claim B outcome was examined; only the share of drives flagged was checked.

v1.4 (2026-11-19): Claim B results. After a struggle (Def 1, Def 2 and the loose Def 2), late 1st-half drives show lower TD and any-score rates and higher drive EPA than mid-half drives, but the same pattern appears for drives not flagged. The struggled-minus-not-flagged difference is about 0 for all metrics with intervals spanning 0 (TD ±0.03, drive EPA ±0.23). At play level, late-vs-rest EPA/play is +0.059 to +0.067 after a struggle vs +0.074 to +0.087 otherwise (differences all spanning 0). Robust to lookback of 4/6/8 drives, 20/25 s minimum, cut-off drives removed, team-vs-itself, and prior-drive count. Conclusion: no evidence the late-half surge is a rebound from struggling. No new definitions were added after seeing outcomes.

v1.5 (2026-11-19): Phase 3 pre-specification added (§15), before any Phase 3 outcome was looked at.

v1.6 (2026-11-19): Step 1b added after seeing Step 1 (details and reframe in §16).

v1.7 (2026-11-20): Browns case study + clutch gene pre-specification added (§17), before any 2026 output was seen. Q6 added afterwards (see change log).
## 1. The question

Do NFL offenses score more (especially touchdowns) late in each half than earlier in that half? If so, why, and are teams leaving value on the table by not doing more of what causes it?

## 2. The claim (two parts)

- **Claim A (average effect):** Offensive efficiency and TD rate are higher in the late window of each half than in the rest of that half.
- **Claim B (surge after struggle):** Teams that have been struggling to score score at a higher rate on a late-half drive than they do on a similar drive at other points in the game.

Claim B is the one we care about most. Claim A is the foundation.

## 3. Scope

- **Seasons:** 2016-2025 regular season.
- **Halves analysed separately:** 1st half (Q1+Q2) and 2nd half (Q3+Q4). Not pooled.
- **Excluded plays:** kneel-downs, spikes, overtime.
- **Unit of analysis:** plays (the engine) and drives (the headline). Run separately first, then together.

## 4. Time definitions

Time is `half_seconds_remaining`, measured at the start of the play. Buckets (minutes left in the half):

| Bucket | Range |
|---|---|
| B0 | 30:00 to 15:00 |
| B1 | 15:00 to 8:00 |
| B2 | 8:00 to 4:00 |
| B3 | 4:00 to 3:00 |
| B4 | 3:00 to 2:00 |
| B5 | 2:00 to 1:00 |
| B6 | 1:00 to 0:00 |

- **Boundaries:** a play sitting exactly on a boundary goes in the later bucket (closer to the end of the half). So 4:00 exactly is B3, 3:00 exactly is B4, and 15:00 exactly is B1.
- **"Late" (headline) = last 4 minutes of the half = B3 to B6.**
- The bucket curve shows where the shift actually starts. The headline is one summary number.
- A drive belongs to the bucket it **starts** in.
- **Drive start rule:** a drive must start with at least 30 seconds left in the half to count. Drives starting with under 30 seconds are excluded.
- **Drives that start with 30+ seconds but get cut off by the end of the half stay IN the main run** (they count as not scoring). This works against the hypothesis, so a surge that survives is stronger. A rerun without them is a robustness check.
- Starting field position and time left are both controlled for, and results are also reported split by starting zone (so short-field drives, like a turnover near the opponent's end zone, can be seen separately).

## 5. Outcome metrics

**Play level:** EPA/play, success rate (EPA > 0).
**Drive level:** TD rate (primary), points per drive, EPA per drive (secondary).

- **Added v1.2:** any-score rate (TD or FG made), reported alongside TD rate.

Why both: EPA doesn't know what coverage the defence is in, so a soft-zone underneath gain is credited as a real gain. TD rate doesn't have that problem. Field goals are excluded from the primary outcome because late in the 1st half teams often settle for them.

## 6. Defining "struggled" (Claim B)

**Definition 1 (scoring).** A team has "struggled" going into a drive if, over **all its prior drives in the game** (minimum 3 prior drives), it has:
- no touchdowns, AND
- below-average EPA per drive (average = league average EPA per drive for that season).

This crosses halftime on purpose, so the first drive of the 2nd half can be assessed too. **It never looks back into previous games. Every lookback is confined to the current game only.** We report results split by how many prior drives the team had, since a "no TD in 3 drives" team and a "no TD in 9 drives" team are not the same group.

Robustness: the same analysis using only the last 4, 6, or 8 prior drives **in that game**. If a team has had fewer drives than the cap, it uses all it has (still minimum 3).

**Definition 2 (ball movement).** TDs are rare (roughly 1 in 5 drives), so "no TD" alone is noisy: a team can move the ball well and still end up with field goals or a turnover near the goal line. Definition 2 measures whether the team moved the ball at all. Over the same prior drives (same game only, minimum 3), a team has "struggled" if EITHER:
- its first downs per drive are below the league average for that season, OR
- at least half of its prior drives ended in a three-and-out.

Standalone punt counts are not used, because a punt depends heavily on field position and coaching decisions, and most of the useful signal is already in three-and-outs.

Claim B is run under BOTH definitions. If the surge appears whether struggle is defined by scoring or by ball movement, the result is much harder to dismiss. If it appears under only one, that is a finding too. **No further definitions get added after seeing data.** This is locked now to avoid searching for a definition that flatters the result.

## 7. Baselines and comparison groups

**For Claim A (baseline):** all other plays/drives in the same half. The scripted opening drives stay IN for the main analysis (this makes the test harder, not easier). The version without them is a robustness check.

**For Claim B (struggle comparison group):**
We select teams because they struggled, never because of what happened next. (Selecting only the struggling teams that then scored would guarantee a "surge" by construction.)

1. Find every drive that starts after a "struggled" stretch.
2. Label each by where it falls: first drive of the 2nd half, mid-half, or late window.
3. Compare TD rate and EPA/drive across those labels.
4. If only the late drives jump above the rest, timing is doing something. If they all match, the "surge" is just a team's results bouncing back (regression to the mean), which happens at every point in a game, not just late.

The first drive of the 2nd half is a deliberately hard comparison because halftime adjustments could also "reset" a team. We also compare against mid-quarter drives.

## 8. Hypothesis list (written before testing)

*→ Tested in §15-16. Result: all ruled out as performance mechanisms; pass/run is a contributor that feeds the valuation effect.*

| # | Hypothesis | How we'd test it |
|---|---|---|
| H1 | Pace / no-huddle | `no_huddle` flag; late rate vs efficiency with and without it |
| H2 | Prevent / soft coverage | Depth of target, air yards, yards per completion, completion rate; like-for-like down/distance/field position |
| H3 | Play-calling shift | Pass rate, early-down pass rate, short vs deep targets |
| H4 | Clock and timeout effects | Incomplete-pass stoppages, timeouts left, plays per drive |
| H5 | Personnel / formation | Shotgun, formation, number of receivers (where available) |
| H6 | Defensive substitution / fatigue limits | Compare no-huddle plays vs huddle plays in the same situation |
| H7 | Game state / urgency | Score differential, trailing vs leading, win probability; time x game state interaction |
| H8 | Composition (who has the ball) | Check the score differential of the team with the ball in each time bucket; team-vs-self comparison (early vs late within the same team) if needed |

Null: results bouncing back after a bad stretch, plus sample composition, explain the pattern.

Each mechanism that turns out to be real gets a second question: can an offence control it on purpose, and are teams underusing it?

## 9. What counts as "real"

Rough rule set up front:
- The late effect is visible in both halves (or we explain why only one).
- It holds across most seasons, not just a couple.
- It survives like-for-like comparison (same down, distance, field position).
- For Claim B: late drives after a struggle beat drives at other points after a struggle.

## 10. Robustness checks and uncertainty

Rerun the main result with one choice changed, to see if it survives:
- Opening drives removed from the baseline.
- Drives cut off by the end of the half removed.
- Late window of 3 and 5 minutes instead of 4.
- Drive minimum at 20 and 25 seconds instead of 30.
- "Struggled" using only the last 4, 6, or 8 drives.
- Team-vs-itself comparison.

**Uncertainty:** for the headline number and the bucket curve, bootstrap confidence intervals, resampling whole games (not single drives), since drives in one game aren't independent. Used in notebook 02. With this much data almost any gap will be "significant", so we read the interval for effect size, not just for whether it exists.

## 11. Game state

Not part of the headline, so Claims A and B stay purely about time. It is recorded for every play (score differential, timeouts, win probability) and used in the mechanism step (H7). If the late effect looks similar whether the offence is leading, tied or trailing, that supports "it's purely time."

## 12. Resolved items

- "Below-average EPA per drive" = league average EPA per drive for that season (not the team's own average).
- Drives that span the end of the half: kept in the main run (see section 4), removed as a robustness check.

## 13. Data

nflverse / nflfastR play-by-play, 2016-2025. Free, one row per play, includes EPA, win probability, `no_huddle`, `shotgun`, `half_seconds_remaining`, score differential, and drive identifiers. Some detail fields (formation, pass rushers) come from separate participation data and may be thinner. Gaps get flagged in the notebook, not hidden.

## 14. Plan

- **Phase 0:** this document.
- **Phase 1:** verify. Efficiency by time bucket, both halves, all drives (Claim A).
- **Phase 2:** the struggle test (Claim B).
- **Phase 3:** test each hypothesis, size each mechanism.
- **Phase 4:** the "so what" for coaches. *→ Replaced by the v1.6 reframe (valuation, not performance) and the Browns case study (§17).*
- **Phase 5:** README, repo cleanup, write-up. *→ Done: README and the story page.*

## 15. Phase 3 pre-specification (v1.5, 2026-11-19)

Written before any Phase 3 outcome was looked at. Anything changed after this point gets logged with date and reason.

### Framing
Phase 2 showed the 1st-half surge is not a rebound: struggling and non-struggling teams get the same boost. A boost that goes to everyone is still a finding, and arguably a bigger one. So Phase 3 asks two things: **(1) why does a surge show up for every team state**, and **(2) who gets more or less of it** (heterogeneity), not only whether some mechanism exists.

### Fixed rules
- Unit is the play. 1st half is primary, 2nd half is a comparison where it's cheap. Late = 240 s or less left, rest = the half's B0-B2 plays (unchanged from Phase 1).
- Headline quantity: late minus rest in EPA/play, 1st half (Phase 1: +0.079). Notebook 04 re-derives it first as a check on the new code.
- Intervals: 95% percentile bootstrap, resampling whole games, 1,000 draws.
- **Mix vs within.** For a play characteristic X (no-huddle, pass/run, score state...), late-minus-rest = *mix* + *within*. Mix = what the gap would be if the only change late were a shift in how many plays of each X-type there are, valued at rest-of-half performance. Within = like-for-like gap with X held fixed. **Explained share = mix / total gap.** This is accounting, not causation.
- **Verdict rule** (on the marginal share, one variable at a time): *main driver* if share >= 50% and its lower bound > 25%; *contributor* if lower bound > 10%; *ruled out* if upper bound < 20%; otherwise *unclear*. Incremental share in the ladder (Step 12) is reported next to it, because correlated variables (e.g. no-huddle and trailing) share credit.
- Order: Step 1 first. If the surge disappears on raw outcomes, stop the mechanism chase and reframe.
- Hypotheses that need participation/FTN data (H2, H5 personnel, H6) are only tested on seasons where those fields exist. If a field isn't there, the test is reported as "cannot test", not guessed.

### Hypotheses, tests, rules
| # | Idea | Primary test | Secondary | Evidence rule |
|---|------|--------------|-----------|---------------|
| Step 1 | Measurement: late EPA has a different baseline (EPA model knows the clock) | Late-minus-rest on raw outcomes: yards/play, first-down-or-TD rate, down-and-distance success (no EPA), explosive rate, turnover, sack; raw and like-for-like on down x distance | By bucket | Surge "survives" if first-down rate, raw success and yards/play gaps are all positive with lower bound > 0. If none are, it's an EPA artifact: stop and reframe. Mixed = partly artifact. |
| H8 | Composition: better offenses / weaker defenses / better game states have the ball late | (a) team-quality composition: gap in offense quality + opposing-defense quality (season EPA, built without 1st-half late plays); (b) gap within the same team in the same game-half (fixed effects) | Home share | Supported if composition-predicted share >= 50% or the fixed-effects gap <= half the raw gap. Ruled out if both < 20%. |
| H1 | No-huddle / pace | Marginal mix share by `no_huddle` flag | Surge among huddle-only plays; no-huddle vs huddle like-for-like (down x distance x score state); clean snap-to-snap tempo late vs rest; mix share by tempo tercile; fast vs slow tempo like-for-like | Verdict rule on the no-huddle share. If the surge is as big on huddle-only plays and tempo shows no late speed-up, pace is out. |
| H7 | Game state (leader vs trailer, urgency) | Marginal mix share by score-state (5 bins) | Late gap within each state, both halves; 2nd-half state x season table for the 2024-25 flip | Verdict rule. Also: if the gap is similar in every state, state is not the engine. |
| H2 | Prevent / soft coverage | Marginal mix share by defensive shell (DB count x pass rushers) on pass plays | Coverage type, man/zone, surge on non-soft plays only (<= 5 DBs and >= 4 rushers), pbp proxies (completion %, air yards, sacks) | Verdict rule. Without participation data: "cannot test", pbp proxies reported as indirect evidence only. |
| H5 | Personnel / formation | Marginal mix share by personnel (RB/TE counts) x shotgun | WR count, shotgun alone (pbp only) | Verdict rule. |
| H3 | Play-calling | Marginal mix share by pass vs run | Pass-depth mix (air yards bins), play-action/screen/motion/RPO rates (FTN, 2022+) | Verdict rule. |
| H4 | Clock / timeouts / two-minute warning | Within the late window, plays with >= 2 timeouts vs <= 1, like-for-like (down x distance x score state) | 30-second slice profile of EPA vs rest, to see whether the jump sits at 3:00 or at the 2:00 warning | Supported if the >= 2 timeouts group is higher by >= 0.03 EPA/play with lower bound > 0. Slice profile is descriptive. |
| H6 | Defensive substitution limits | Two links: (i) defense subs less often late (any_sub rate, clean-clock plays), (ii) plays where the defense didn't sub have higher EPA like-for-like | Mix share by any_sub; any_sub by tempo tercile | Supported only if both links hold (i: late any_sub rate lower with CI excluding 0; ii: difference > 0 with lower bound > 0) and mix share lower bound > 10%. |

### Heterogeneity (question 2)
Late-minus-rest gap by: offense-quality third, defense-quality third, pass-leaning third, home/away. Difference between extreme levels with a joint bootstrap. Descriptive; no verdict rule.

### Joint accounting
Ladder of regressions, EPA ~ late + controls, controls added in this order: down/distance/field position, score state, pass/run, no-huddle, shotgun, (personnel where available), then team-game-half fixed effects. Coefficient on `late` after each step, with the share of the raw gap each step removes. Order matters: this is a way to see how much is left, not a ranking.

### Known limits (written now so they aren't discovered later)
- Tempo is endogenous (teams going well play faster), so tempo and no-huddle results are associations. Snap-to-snap clock time is only used on clean intervals (prior play completed in bounds, no penalty, no timeout, not across the 2:00 warning, 8-45 s); unseen no-plays can still slip in.
- Participation and FTN charting cover only some seasons. FTN data requires attribution (FTN Data via nflverse, CC-BY-SA 4.0) in the write-up.
- "Explained share" is accounting and can exceed 100% or go negative; controlling for a mediator (e.g. pass/run) removes a mechanism's effect rather than a confound.

## 16. Phase 3 results (v1.6, 2026-11-19)

**v1.6 note.** Added Step 1b (outcome-priced decomposition) after seeing the Step 1 result, to explain it. Diagnostic only: it doesn't add a new outcome or change any Phase 3 test. Result: of the +0.079 1st-half EPA gap, +0.096 comes from how EPA prices the same result late and −0.018 from what actually happened. Reframe: the project question moves from "why do offenses surge late in the 1st half" to "the end-of-half surge is a valuation effect, not a performance one: what does that mean?" The reframe came from the data and is logged here openly. The original question and all pre-specified tests stay as written.

- Step 1: raw outcomes don't surge (yards +0.01, success +0.001; turnovers, sacks and explosives worse). First-down rate +1.4 pts, rising late in both halves. Pre-spec verdict: partly artifact.
- Step 1b: the gap is valuation (+0.096 [0.090, 0.102]), not outcomes (−0.018 [−0.031, −0.002]).
- H8 composition: ruled out (0%). H1 no-huddle/pace: ruled out (1%, 3%). H7 game state, 1st half: ruled out (6%). H2 soft coverage: ruled out (−22%). H5 personnel/formation: ruled out (−8%). H6 defensive subs: link (i) holds, link (ii) fails, not supported. H3 pass/run: contributor (23%), read as feeding the valuation effect. H4 timeouts: opposite sign (−0.044).
- Heterogeneity: none clear. The effect is universal.
- Ladder: 90% of the gap survives all football controls.
- 2nd half: the late dip comes from leading teams (−0.06 to −0.15 every season); trailing teams are ~0 to slightly positive.

## 17. Case study: 2026 Cleveland Browns + clutch gene (v1.7, 2026-11-20)

Pre-specified before any 2026 output was seen. Notebook: notebooks/05_browns_case_study.ipynb.

**Data.** nflverse pbp 2016-2026, regular season. 2026 cut-off: through Week 10 (CUTOFF_WEEK; Week 11 is CLE's bye). Plays = the notebook-01 filter (no kneels, spikes, 2-pt, rows without a down). Checked by reproducing Claim A (+0.079). OT is left out of every by-quarter number.

**Metrics, clock-neutral first.** succ_raw (yardage success: 40% of distance on 1st down, 60% on 2nd, 100% on 3rd/4th, no turnover), fd (first-down rate: gained the distance, no turnover), yds (yards/play), drive scoring rate (TD or FG), points per drive (TD = 7, FG = 3). EPA is shown alongside, never alone (§16 lesson: its baseline moves with the clock).

**Game flags** (built from the pbp score timeline):

- 4th-quarter comeback win (cb): won, and trailed at some point in the 4th quarter (entering it or after any 4th-quarter play). Stricter cb_strict: also trailed by 1-8 at some point in the 4th (PFR/Kacsmar style). Ties don't count as wins.
- Game-winning drive (gwd): won, and took the lead for good in the 4th quarter or OT. This counts any go-ahead score, including the rare defensive or special-teams one (Kacsmar requires an offensive drive).
- Sanity gate: the definitions must give CLE 2026 a 6-4 record and 6 GWD wins before anything uses them on history. If they don't, fix the definition first and log it.

**Q1.** Good 4th quarter, or bad quarters 1-3? CLE 2026 by quarter vs the 2026 league (other teams) and vs CLE 2025. Tests: CLE minus league in Q1-3 and in Q4, plus the difference between the two, with intervals from resampling games. Reading: Q1-3 below the league and Q4 ≈ league means "slow starters, average finishers". Q4 above the league with an interval clear of 0 means a genuine late edge, which goes to Q2.

**Q2.** Better than a normal team in the same spot? CLE plays and drives in the 4th quarter when trailing or tied, vs league teams in the same cells (margin bins tied / 1-3 / 4-8 / 9-16 / 17+ down × time bins 15:00-10:00 / 10:00-5:00 / 5:00-2:00 / 2:00-0:00 of the 4th). The matched gap asks what the league would do with CLE's exact mix of situations. Reading: a matched gap ≈ 0 means game state explains it; a gap clear of 0 means CLE really was better than typical comeback teams.

**Q3.** How much is the opponent? Opponents' offense when leading CLE in the 4th quarter vs league leaders in the same cells (lead bins 1-3 / 4-8 / 9-16 / 17+ × the same time bins). Measures: run rate, clock seconds per play, success, first-down rate, yds/play, EPA; drive score rate, three-and-out rate, points per drive. The defensive side of this question is Q2 by construction (CLE offense vs league trailing offenses in the same cells = opponents' defense vs league leading defenses), so it isn't repeated. Reading: opponents below matched leaders means part of the comeback is the opponent stalling. Points attribution: CLE excess points (Q2 gap × CLE drives) vs points the opponents left on the table (Q3 gap × their drives).

**Q4.** Will it last? (team level, 2016-2025) Split each team-season into games 1-10 ("early", matching CLE's 10 games) and 11+ ("rest"). Unusual = 4 or more GWD wins in the first 10 games (sensitivity: top 5% of the distribution). For flagged team-seasons: early vs rest win %, GWD and comeback rate per game, one-score win %, plus next-season win %. Also across all team-seasons: does early one-score win % predict rest one-score win % (min 3 early and 2 rest one-score games)? Compared with early point differential per game. Reading: a big drop from early to rest, and one-score record not predicting, means comeback runs are mostly luck that regresses. CLE 2026 gets a rest-of-season forecast from point differential + one-score record fitted on history.

**Q5.** Clutch gene (persistence test). Clutch window = 4th quarter, last 5:00 (gsr ≤ 300), offense trailing or tied by 0-8 (needs to score). Sensitivity: |margin| ≤ 8 (adds one-score leads). Unit = QB (dropbacks, QB id = passer id, or rusher id on scrambles; ids, not names), and team (all plays). Residual per unit-season = clutch mean − non-clutch mean − that season's league clutch-minus-normal gap. Metrics: succ_raw (primary), fd, yds; EPA as a secondary check.

Persistence: (a) split-half: odd vs even team games in the same season, min 12 clutch and 100 other plays in each half; (b) season t vs t+1: min 25 clutch and 200 other plays in each season.
Noise baseline: reshuffle which plays count as clutch within each unit-season (same counts), 100 times. This gives the band of correlations noise alone produces. Detectability reference: on the same reshuffles, the persistence of the unit's raw level on clutch-sized samples. This shows how much real, stable skill shows through on samples this small.
Reading: r above the noise band means clutch is real-ish, and the next-season forecast for a clutch residual = r × this season's residual. r inside the band means it's mostly luck (judged against the detectability reference: if even true skill barely shows on samples this small, the test is weak, and that's said too). Watson 2026 and CLE 2026 are then placed on the distribution.

**Sample-size warning.** 10 games is tiny. Q1-Q3 are descriptive, with intervals that resample CLE's games (and the league's). Q4-Q5 are where the real inference is (hundreds of team-seasons and QB-seasons). Intervals: NBOOT = 1000 game (or unit) resamples, percentile 95%.

## Change log (after seeing data)

Any change gets a date and a reason here.

- 2026-11-20: **Q6 added after seeing Q1-Q5** (post hoc, exploratory), to test the "Watson has been clutch" claim on its own terms. A go-ahead chance = any drive starting in the 4th quarter or OT with the offense tied or down 1-8; converted = the drive put them ahead. Each chance gets the league's 2016-2025 conversion rate for the same margin × time cell. Result: 10 of 22 converted vs 5.4 expected (p = 0.024). Caveat: the Browns were chosen because they won close games, so a low p is partly built in.