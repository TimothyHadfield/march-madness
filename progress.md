# March Madness: progress

## START HERE
Last updated 2026-10-16. Read order: this file, then `CLAUDE.md` (file map, traps), then `index.html`.

## Goal right now
None active. The app is published and working; it was swept for bugs on 2026-10-16.

## Status
| Area | State |
|---|---|
| Historical charts + year brackets | Working. Data fixed 2026-10-16 (4 errors) |
| Build Your Own Bracket | Working. Re-tapping a pick no longer wipes later picks (2026-10-16) |
| Simulator + auto-fill | Working |
| Phone (390px) | Brackets scroll sideways instead of overlapping (2026-10-16) |
| Live site | https://timothyhadfield.github.io/march-madness/ (GitHub Pages, `main` root) |

## Authorized next steps
None.

## Standing instructions
- Tim owns visuals, wording, layout and names (global rules apply).

## Traps
- Data check before trusting a JSON edit (run from the repo root):
  `node -e 'const d=require("./ncaa_bracket_seeds_1985_2025.json"),M=[[1,16],[8,9],[5,12],[4,13],[6,11],[3,14],[7,10],[2,15]];for(const[y,t]of Object.entries(d.tournaments))for(const r of t.regions){const g=t[r];g.r64_winners.forEach((w,i)=>{if(!M[i].includes(w))console.log(y,r,"r64",i)})}'`
  It should print nothing. Also check r32/s16/e8 winners come from the previous round, and F4/title teams are region winners.
- No test suite; verify with phone-view `--eval` (see CLAUDE.md).

## Decisions
- 2026-10-16 sweep: fixed 4 data errors (1987 Southeast and 1989 East 6v11/3v14 winners swapped; 2005 Chicago 5v12/4v13 swapped; 1995 Final Four had the wrong semifinal pairings and title opponent), verified against Wikipedia. Re-tapping the current pick in the picker is now a no-op. Narrow-screen brackets scroll instead of overlapping. Added aria-labels / meta description (no visual change). CLAUDE.md written.

## NOT verified
- Only the 4 years the consistency check flagged were checked against real results; the other 36 years are internally consistent but not cross-checked against a source.
- Tooltip behaviour on a real iPhone (tap vs hover).

## Rejected / parked
(none)
