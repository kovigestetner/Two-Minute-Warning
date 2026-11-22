# The Two-Minute Warning

During an NFL game this season I saw a broadcast graphic that matched something I'd always believed: teams that struggle for most of a half suddenly come alive in the last few minutes. I tested it on 10 seasons of play-by-play data to see if it's actually true, or just confirmation bias from TV only showing the graphic when it fits.

## What I wanted to find out

1. Do offenses really play better at the end of halves?
2. Do teams that struggled all half get a bigger late boost than everyone else?
3. If either is true, why: no-huddle, tired defenses, prevent coverage, play-calling?

## What I found out

- **EPA says yes, the football says no.** EPA (expected points added, the go-to stat for grading a play) is higher in the last 4 minutes of the first half in all 10 seasons. But yards per play and success rate are flat, and sacks and turnovers are slightly worse.
- **EPA rises because late plays are worth less, not because they're better.** A failed play costs less with a minute left, since the other team has no time to use the field position. EPA prices that in, so the average goes up while the football stays the same.
- **Struggling teams don't wake up.** They get the same late bump as everyone else.
- **None of the usual explanations hold up.** No-huddle, pace, prevent defense, personnel, substitutions and timeouts were all ruled out.
- **What is real:** late in the second half, teams protecting a lead get worse, every season.
- **Case study:** the 2026 Browns (6-4, six game-winning drives) really have delivered late, but Watson plays like himself play to play, and history says starts like theirs regress to about .500.

There's also a plain-English version with charts for non-data people: [The Two-Minute Warning (PDF)](report/The-Two-Minute-Warning.pdf).

---

## How I tested it

- **Data:** every regular-season play from 2016 to 2025 from [nflverse](https://github.com/nflverse) (326,039 plays after dropping kneels, spikes, 2-point tries and rows without a down). The Browns section adds 2026 through Week 10. The coverage and personnel tests also use nflverse participation and FTN charting data.
- **Late vs rest:** the last 4 minutes of a half compared with the rest of that half. I also checked 3 and 5 minutes.
- **Never EPA on its own:** every EPA result is checked against plain stats that don't depend on the clock: yards per play, success rate (gaining 40% of the distance on 1st down, 60% on 2nd, all of it on 3rd/4th), first downs, sacks, turnovers and drive scoring.
- **Uncertainty:** 95% intervals from resampling whole games, because plays in the same game aren't independent.
- **Rules written first:** before looking at any results, I wrote down each phase's tests and what would count as "real" in [`question-doc.md`](question-doc.md). Every change after that is logged with a date and a reason, and anything added after seeing results is marked as such.

## The project in phases

Each phase has its own notebook, with the results and a written reading of them inside.

| Phase | Question | Short answer | Notebook |
|---|---|---|---|
| 1 | Do offenses play better late in halves? | In EPA, yes (+0.08 per play, 1st half). Per drive, no | `02_time_buckets` |
| 2 | Do struggling teams wake up? | No. Same bump as everyone else | `03_struggle_test` |
| 3 | Why does EPA go up? | Late plays are priced differently. Every football explanation ruled out | `04_mechanisms` |
| 4 | Are the 2026 Browns clutch? | They've delivered, but it shouldn't be expected to last | `05_browns_case_study` |

## The lesson about EPA

EPA judges every play against a baseline that depends on the clock, so comparing it across different points in a game can find "surges" that are really just plays being priced differently. Before believing a late-game or "clutch" claim built on EPA, check it on plain stats.

## Limitations

Regular season only, nflfastR's EPA model, and a 10-game Browns sample. The full list is in the notebooks and `question-doc.md`.

## What's in the repo

```
notebooks/
  01_load_and_clean.ipynb     downloads the play-by-play, builds play and drive tables
  02_time_buckets.ipynb       do offenses play better late?
  03_struggle_test.ipynb      do struggling teams wake up?
  04_mechanisms.ipynb         why EPA goes up, and every explanation tested
  05_browns_case_study.ipynb  the 2026 Browns and the clutch gene
  06_allen_hook.ipynb         the Bills vs Vikings game that started this
question-doc.md               definitions, rules written in advance, change log
results/                      result tables (CSV)
data/                         created by notebook 01, not committed
```

## Run it yourself

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Run the notebooks in order, 01 to 06. The first run of notebook 01 downloads 10 seasons of play-by-play into `data/`.

## Credits

- Data: [nflverse](https://github.com/nflverse), via [nflreadpy](https://github.com/nflverse/nflreadpy)
- EPA model: [nflfastR](https://www.nflfastr.com/)