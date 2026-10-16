# March Madness Bracket Simulator

Tim's single-page NCAA tournament explorer: real seed-vs-seed history 1985–2025 (no 2020), a "build your own bracket" picker, and a tunable bracket simulator. A personal/showcase app; no accounts, no backend, no user data.

- **Live:** https://timothyhadfield.github.io/march-madness/
- **Repo:** TimothyHadfield/march-madness (public)
- Handoff notes: `progress.md` (read START HERE first).

## File map
- `index.html`: the whole app (CSS, HTML, JS inline). Sections top to bottom:
  - Historical section: year buttons + presets, 3 Chart.js charts (`hLsC`, `hWpC`, `hUdC`), "View Bracket for Year" (`buildHistBracket`).
  - Build Your Own Bracket (`hypoData`, `hypoPick*`, `propagateHypo*`, `renderHypoBracket`). Tooltip odds come from `getHistOdds` over the selected years.
  - Simulator: skill-gap exponent + curve-shape sliders → `compSkills`, exact title/perfect-bracket odds by DP in `calcTheoretical`, 5000-run Monte Carlo upset histogram in `calcUpsetDist`, `simulateBracket` auto-fill.
  - Shared bracket renderer: `renderBracketInto` / `mkRC` / `mkCC` / `mkPT` / `mkMU` (and `mkHypo*` copies for the picker).
- `ncaa_bracket_seeds_1985_2025.json`: seeds-only results. Per year: `regions` (4 names, vary by year), per region `r64_winners[8]` (in order 1v16, 8v9, 5v12, 4v13, 6v11, 3v14, 7v10, 2v15), `r32_winners[4]`, `s16_winners[2]`, `e8_winner`, then `f4[2]` and `championship` with `{seed, region}`.
- `docs/screenshots/`: README images only.
- `.gitignore` ignores `reference/` (a saved third-party page, not used) and `*(hobbies copy)*`.
- Libraries: Chart.js 4.4.1 + chartjs-plugin-datalabels 2.2.0 from jsDelivr (in `<head>`, blocking; the inline script calls `Chart` immediately, so they can't simply be `defer`red).

## Run locally
`python -m http.server <port>` in the repo root, open `http://localhost:<port>/`. Opening the file directly won't load the JSON (fetch).

## Test
No test suite. Verify with `node ~/.claude/tools/phone-view.mjs <url> --eval "<js>"`; all app functions are globals (e.g. `simulateBracket()`, `showHypoBracket(); hypoNoUpsets()`, `buildHistBracket(1995)`, `extractMatchups()` after setting `selectedYears`).
Data sanity check (every winner must come from its own matchup, F4/title teams must be the region winners): see the node one-liner pattern in `progress.md` Traps.

## Deploy
GitHub Pages, legacy build from `main` branch root. Push to `main` = live in about a minute.

## Traps
- **Data positions matter.** `r64_winners[i]` must be one of the two seeds of matchup `i`; a swapped pair silently corrupts the historical counts and that year's bracket (the code picks `teamB` whenever the seed doesn't match `teamA`). Four such errors were fixed 2026-10-16 (1987 Southeast, 1989 East, 2005 Chicago, 1995 Final Four pairings), checked against Wikipedia.
- `championship.matchup` order doesn't always match `f4` order; the app only uses `f4[i].winner` + `championship.winner.region`, so that's fine.
- "Upset" means the higher seed number won (historical) or the lower-skill team won (simulator). Same-seed games are never upsets.
- Brackets are 884px of fixed-width columns. Under 900px wide a media query stops columns shrinking so they scroll sideways inside their box instead of overlapping (`.bsec`, `#histBc`, `#hypoBc` are the scroll boxes). Keep that rule *after* the base `.bracket` rule or it loses the cascade.
- Hover tooltips (`#tip`) are mouse events; on iPhone they appear on tap.
- `.ctrl .cg` (control group) and `.cg` (bracket connector group) share a class name.

## Standing instructions
- Tim owns looks, wording, layout and names; fix defects without restyling.
