# Hurry Up Offense: Question Doc (v1.0)

Status: agreed. Written BEFORE touching data so we can't quietly change definitions to fit the result.

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

Time is `half_seconds_remaining`. Buckets (minutes left in the half):

| Bucket | Range |
|---|---|
| B1 | 15:00 to 8:00 |
| B2 | 8:00 to 4:00 |
| B3 | 4:00 to 3:00 |
| B4 | 3:00 to 2:00 |
| B5 | 2:00 to 1:00 |
| B6 | 1:00 to 0:00 |

- **"Late" (headline) = last 4 minutes of the half = B3 to B6.**
- The bucket curve shows where the shift actually starts. The headline is one summary number.
- A drive belongs to the bucket it **starts** in.
- **Minimum time for a drive to count: 30 seconds left at the start of the drive.** Starting field position and time left are both controlled for, and results are also reported split by starting zone (so short-field drives, like a turnover near the opponent's end zone, can be seen separately).

## 5. Outcome metrics

**Play level:** EPA/play, success rate (EPA > 0).
**Drive level:** TD rate (primary), points per drive, EPA per drive (secondary).

Why both: EPA doesn't know what coverage the defence is in, so a soft-zone underneath gain is credited as a real gain. TD rate doesn't have that problem. Field goals are excluded from the primary outcome because late in the 1st half teams often settle for them.

## 6. Defining "struggled" (Claim B)

**Definition 1 (scoring).** A team has "struggled" going into a drive if, over **all its prior drives in the game** (minimum 3 prior drives), it has:
- no touchdowns, AND
- below-average EPA per drive (average = league average for that season).

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

## 10. Robustness checks

Rerun the main result with one choice changed, to see if it survives:
- Opening drives removed from the baseline.
- Late window of 3 and 5 minutes instead of 4.
- Drive minimum at 20 and 25 seconds instead of 30.
- "Struggled" using only the last 4, 6, or 8 drives.
- Team-vs-itself comparison.

## 11. Game state

Not part of the headline, so Claims A and B stay purely about time. It is recorded for every play (score differential, timeouts, win probability) and used in the mechanism step (H7). If the late effect looks similar whether the offence is leading, tied or trailing, that supports "it's purely time."

## 12. Open items to decide

- Exact definition of "below-average EPA per drive" (league-wide vs team's own average).
- How to handle drives that span the end of a half or run into the final seconds.

## 13. Data

nflverse / nflfastR play-by-play, 2016-2025. Free, one row per play, includes EPA, win probability, `no_huddle`, `shotgun`, `half_seconds_remaining`, score differential, and drive identifiers. Some detail fields (formation, pass rushers) come from separate participation data and may be thinner. Gaps get flagged in the notebook, not hidden.

## 14. Plan

- **Phase 0:** this document.
- **Phase 1:** verify. Efficiency by time bucket, both halves, all drives (Claim A).
- **Phase 2:** the struggle test (Claim B).
- **Phase 3:** test each hypothesis, size each mechanism.
- **Phase 4:** the "so what" for coaches.
- **Phase 5:** README, repo cleanup, write-up.
