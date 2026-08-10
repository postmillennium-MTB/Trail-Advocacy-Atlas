# US MTB Trail Advocacy Atlas

A single-file, zero-dependency directory of nonprofit mountain bike trail advocacy organizations across the United States. Searchable, filterable by tier and state, with a donut chart, a 50-state cartogram, real geocoded trailhead maps for four states, and a **Why** tab that models what one of these organizations is actually worth to a town in dollars. Built to the same architecture as PMR's other tools: one HTML file, no build step, deploy anywhere.

**Current status:** 373 organizations across all 50 states, Washington DC, and Puerto Rico.

---

## Opening / hosting it

It's one file: `trail-advocacy-atlas.html`. Double-click it to open locally, or push it to the `postmillennium-MTB` GitHub Pages repo like your other tools and iframe it into the PMR site or a Pinkbike post. No npm, no build, no server required.

## What's in the data

Everything lives in one JavaScript array near the top of the `<script>` block, called `ORGS`. Each organization is one object on one line:

```js
{n:"Colorado Mountain Bike Coalition",a:"COMTB",s:"CO",tier:"state",r:"Statewide coalition",w:"https://comtb.org/"},
```

| Field | Meaning | Required? |
|---|---|---|
| `n` | Full organization name | yes |
| `a` | Abbreviation shown next to the name (e.g. `COMTB`). Leave `""` if none. | no |
| `s` | Two-letter state code, `US` for national/regional umbrellas, or `PR` for Puerto Rico | yes |
| `tier` | `"national"`, `"regional"`, `"state"`, or `"local"` — see below | yes |
| `r` | One-line region/description (city, trail system, notable detail) | no but recommended |
| `c` | Extra city/town names folded into search only, not displayed (e.g. `"Naturita Nucla Norwood"`) | no |
| `w` | Website URL. Leave `""` if none exists (renders as plain italic text instead of a link) | no |
| `p` | Parent org's abbreviation, if this is a chapter (e.g. `p:"VMBA"`) | no |
| `lat`, `lng` | Real geocoded coordinates — only populated for CA/CO/VT/OR (see below) | no |
| `status` | `"unconfirmed-active"` — org is verified real but current activity is uncertain | no |
| `conf` | `"draft"` — not yet independently verified at all (rare; most drafts get resolved before merging) | no |

**Tiers:**
- `national` — umbrella bodies spanning the whole country (IMBA, NICA). Dark dot.
- `regional` — umbrella bodies or single orgs spanning multiple states or a broad multi-community footprint within one state (NEMBA, SORBA, MORE, Oregon Timber Trail Alliance). Neon/accent-colored dot.
- `state` — a statewide coalition of local orgs, or a single org that's the de facto statewide voice (JORBA in NJ, KYMBA in KY, CAMTB in CA). Brown/gold dot.
- `local` — everything else: town, county, or regional chapters. This is the vast majority of entries — 348 of 373. Green dot.

To add a new organization, find the right state section and insert a line following the same pattern. If in doubt, hand the file to Claude with "add these orgs to the atlas" — that's how this whole thing was built, incrementally, one state or one pasted list at a time.

## Confidence / sourcing discipline

Every entry has an independently verified, live URL — checked via web search at the time it was added, not copy-pasted from a list without confirmation. A handful of entries deliberately link to a Facebook page, Trailforks listing, or MTB Project club page instead of an official website, because no dedicated website could be found; 6 entries currently have no link at all rather than a guessed one.

Where a state has one authoritative source — a statewide coalition's own member directory — that source did most of the verification work in one fetch:

| Source | Chapters/members pulled in one shot |
|---|---|
| comtb.org/colorado-trail-organizations | Colorado (39 orgs) |
| vmba.org/chapters | Vermont (31 orgs) |
| ormtbcoalition.org/ombc-members | Oregon (26 orgs) |
| evergreenmtb.org/chapters | Washington (11 orgs) |
| camtb.org/members | California (44+ orgs) |
| sorba.org/chapters | 47 Southeast chapters across AL/FL/GA/LA/MS/NC/SC/TN |
| nemba.org/chapters | 35 New England chapters across ME/NH/MA/CT/RI |

Every other state was assembled org-by-org from web search — real, but not necessarily *exhaustive*. A state showing only 1-3 orgs almost certainly has more; it just means nobody has gone looking yet. The pie chart and cartogram reflect *research coverage*, not actual advocacy density.

**Deliberately excluded, on principle, not oversight:**
- Youth interscholastic racing leagues (NICA state chapters, high school cycling leagues) — not trail-*building* orgs. NICA itself stays at the national tier as a labeled exception (`r` field says so directly), everything downstream of it was left out.
- Motorized OHV/dirt-bike clubs (e.g. Reno Area Dirt Riders) — this atlas is pedal-powered MTB only.
- Trips for Kids — youth cycling programming, not trail advocacy; removed after initial inclusion.

**A few orgs mentioned in source material but never added**, because no live site, social page, or third-party confirmation could be found: Susanville Area Bicycle Association (CA), Exchequer Riders Club (CA), South Orange County Trail Coalition (CA), LA Bike Park Collective (CA), SWIN (IN), Brainerd/Northfield Area MBA (MN), Central Mississippi Trail Alliance (MS). Better to leave a gap than invent a URL.

**Renamed / merged orgs**, noted inline in `r` rather than as separate entries: Ride NoCo (formerly White Mountains NEMBA, went independent 2020 — though NEMBA has since re-chartered a separate active White Mountains chapter, so both now exist), OCMTBA (merged with SHARE Mountain Bike Club, 2022), Marin Trail Stewards (formerly Access4Bikes), Bike Shasta (formerly Mount Shasta MBA).

**Abbreviation collisions** across unrelated orgs in different states are resolved with a state suffix on the `a` field only (e.g. `CIMBA-IL` vs. Idaho's `CIMBA`, `SMBA-NY` vs. Colorado's `SMBA`, `OMBA-OK` vs. Oregon/Colorado's `OMBA`) — the real org names are untouched, this only disambiguates the abbreviation column.

## States with no coverage yet

As of this version, every US state, DC, and Puerto Rico has at least one entry. The next honest gap is **depth, not breadth** — states with only 1-2 orgs (North Dakota, Mississippi, Rhode Island, New Jersey) are probably close to complete for MTB-specific advocacy, but populous states like Michigan, Missouri, and the Rockies states almost certainly have more independent local clubs than are currently listed.

## Trailhead maps (California, Colorado, Vermont, Oregon)

These four states — the ones with the most entries — have **real geocoded coordinates**, not estimates. Two places use them:

1. **The Map tab's cartogram**: clicking CA/CO/VT/OR (bordered tiles) switches from the state-count grid to a real point map for that state.
2. **The dedicated "Trailhead map" tab**: a bigger, standalone card with its own CA/CO/VT/OR selector at the top, independent of whatever's in the search box or state dropdown. Click a dot to pin a full org card (name, tier, region, website link) above the map; hover for a quick preview first.

Coordinates were sourced by looking up each org's name + city via Google Places — most resolved to the org's actual address or a specific trailhead/shop; where nothing specific existed, a plain town-center point was used as fallback. This was a one-time lookup pass, done state by state, not an automated geocoding pipeline.

**State outlines** on both maps are hand-simplified boundary polygons (a handful of vertices each, stored in `STATE_OUTLINES`) — good enough to anchor the dots visually, not survey-accurate at the county-border level. Colorado's is literally a rectangle, because Colorado actually is one.

**To extend this to a 5th state:** geocode that state's orgs the same way (org name + city → Google Places lookup), add `lat`/`lng` to each entry, add a simplified outline to `STATE_OUTLINES`, and add the state code to the `POINT_MAP_STATES` array. The rendering code (`buildProjection()`, `renderPointMap()`, `renderDetailMap()`) is already generic and will pick up any state added to that list automatically.

## The "Why" tab — cost model

The atlas answers *who* builds the trails. This tab answers *why it matters that they exist*, by comparing two ways of delivering the same trail:

- **Option 1 — Traditional municipal.** The town hires a for-profit contractor for the whole job and owns the finished asset, and its maintenance, forever.
- **Option 2 — Leveraged partnership.** Same contractor for the machine work, but a non-profit brings volunteer hand-finishing labor, private capital, donated planning, and an adopt-a-trail maintenance agreement.

Everything is computed by a single function, **`computeWhy()`**, which is meant to be read top to bottom. Fourteen sliders defined in the **`WHY_INPUTS`** registry feed it; adding an input is one line there plus one line in the model.

**The three deliberate honesty constraints** — these are the whole reason the tab is defensible, so don't quietly remove them:

1. **Both options get the same public grant rate.** Municipalities apply for RTP, RAISE, CMAQ and state trail money directly and win it routinely. The partnership's real funding advantage is the *private* pool — donations, corporate sponsorship, outdoor-industry and foundation grants — which towns generally can't access. The model also surfaces the awkward corollary: because volunteer labor shrinks the contract, the same grant *percentage* draws fewer grant dollars under Option 2.
2. **Volunteer labor is discounted and capped.** Hours are priced at Independent Sector's national value of volunteer time ($34.79/hr, 2024), multiplied by a productivity factor (volunteers move less dirt per hour than a paid crew), then capped at the hand-finishing share of construction. Machine time — excavator, operator, mobilization — can't be volunteered away, so no amount of volunteer labor drives cost below the machine-only floor.
3. **The partnership is allowed to lose.** Adoption caps at 95% (storm damage, drainage, liability inspections and machine work stay municipal), and coordination carries a real annual cost for staff time, training, insurance and tools. Set coordination high and volunteer hours low and Option 2 comes out behind — the time chart then names the crossover year. A model that can't produce that result isn't a model.

**Not modeled, deliberately:** volunteer hours often qualify as in-kind local match on public grants, which would make Option 2 look *better* than shown — match rules vary too much by program to model responsibly, so the tab is conservative there and says so. Also excluded: schedule risk, build-quality variance, and every downstream economic return (visitor spending, property values, health). This tab compares what a trail *costs*; the [ROI calculator](https://www.postmillenniumrenaissance.com/trail-roi/) compares what it returns.

The "who pays to build it" bars are deliberately the same length in both options — the trail costs what it costs, and the entire argument is about who covers each slice.

## Color themes

Three palettes, switchable in the UI under the tier filter, persisted via `localStorage`:

- **Caribou** (default) — icy blue-teal base with a soft, abstract camo-blob background pattern (six low-opacity radial gradients, not a busy texture) and neon aurora accents: hot magenta, neon green, neon cyan.
- **Moose** — warm earth tones: cream paper, forest-green/rust accents, soft charcoal-gray ink (not pure black — see mobile note below on why near-black was a problem).
- **Elk** — warm gold parchment paper, mahogany-brown ink, amber/marigold accents.

Each theme is defined in **two places** that must stay in sync if you add a fourth:
1. CSS custom properties under `[data-theme="name"]{...}` — controls page chrome.
2. The `THEMES` registry in `<script>` — controls pie chart slice colors, the map's color-mix gradient, and the `paper` hex pushed to `<meta name="theme-color">`, since those are drawn in JS and can't read CSS variables automatically for computed effects like `color-mix()`.

### Changing the default theme

Four edits, and skipping any one of them leaves a visible bug:

1. **Move the bare `:root` selector** onto the new default's CSS block. It styles the first paint, before JS has stamped `data-theme` on `<html>`; leave it behind and every cold load flashes the old palette.
2. **Move that block to the top of the palette list.** `:root` and `[data-theme="elk"]` have *identical* specificity (0,1,0), so the one declared last wins. A default block sitting at the bottom silently overrides every other palette — swatches still flip `data-theme` and charts still recolor, but `--paper`/`--ink` stay stuck on the default. This one is easy to miss because it looks like it works until you click another swatch.
3. **Set `DEFAULT_THEME`** in the `THEMES` registry.
4. **Update the `<meta name="theme-color">` tag** in `<head>` to the new default's paper hex, so the mobile address bar matches before `applyTheme()` runs.

### Theme persistence

`applyTheme(name, persist)` only writes to `localStorage` when `persist` is true, which is exactly the swatch-click path. Applying the default on load deliberately does *not* write.

This mattered when Caribou became the default: the previous code saved on **every** load, default included, so every visitor who had ever opened the atlas carried an explicit `"moose"` they never chose — and would never have seen a new default. The storage key was retired (`mtbAtlasTheme` → `mtbAtlasTheme2`) to clear that once. A deliberate pick still survives reloads; a default no longer masquerades as one.

## Mobile dark-mode fix

Earlier versions of this file rendered correctly on desktop but showed a dark, hard-to-read background on some mobile browsers. This wasn't a bug in the page's own CSS — it was mobile browsers' **forced dark mode** (Android Chrome, Samsung Internet, some iOS dark-mode heuristics) auto-darkening a page that hadn't explicitly opted out. The fix has three parts, all now in place:

1. `<meta name="color-scheme" content="light">` in `<head>` — the actual signal that stops forced-dark reinterpretation. The CSS `color-scheme: light` property alone (which was tried first) only affects native form controls, not full-page darkening.
2. `html{background:var(--paper)}` — previously only `<body>` had an explicit background. On iOS Safari's rubber-band overscroll bounce (a mobile-only interaction), the `<html>` background briefly shows through; if undeclared, it can default dark under a dark system theme.
3. `<meta name="theme-color">`, updated dynamically per theme — tints the mobile browser's address bar to match the active palette instead of a mismatched default.

If a dark-background report ever comes back, check these three things first before assuming it's a new CSS bug.

## Known structural quirks

- **The cartogram is a grid, not real geography.** One square per state in roughly correct relative position — good for reading relative density, not for tracing actual borders.
- **Search includes a hidden `c` field.** If a search misses an org you'd expect to find (e.g. searching a small town name), it's likely because that org's `c` field hasn't been populated yet — the city name needs to be manually added, it's not automatically extracted from anywhere.
- **Reset button** (dashed, next to "Local" in the tier bar) clears search text, state, and tier filters back to defaults — it deliberately does *not* touch your color theme or active tab, which are treated as separate preferences.
- **Default tab on load is "By state"** (the pie chart), not the Directory — changed deliberately partway through development. "Why" sits first in the tab bar as the thesis of the whole atlas, but isn't the landing tab.
- **The Why tab hides the filter bar and counters.** Tabs registered with `filters:false` in the `TABS` registry don't read the search/state/tier controls, so `switchTab()` hides them rather than leaving dead controls on screen. Any future tab that ignores the filters gets the same treatment with one flag.
- **The Why tab's sliders are built once, not re-rendered.** `renderWhy()` builds the panel shell on first open and everything after that goes through `updateWhy()`, which only replaces the results column. Rebuilding the slider markup on every `input` event would drop pointer capture mid-drag and the thumb would stop following the mouse.
- **The tab bar scrolls sideways on narrow screens** instead of widening the page. Without `overflow-x:auto` on `.tabs`, eight tabs push the whole document into horizontal scroll on a phone.
- **The masthead title rotates** every 6 seconds between "Who builds the trails you ride" and "How trails get built," respecting `prefers-reduced-motion` for anyone with that OS setting on.

## Working with Claude on this file

The fastest way to extend this: paste a list of orgs (with state, city/region if known) into a new chat message along with the current `.html` file, and ask Claude to verify and add them. Claude will web-search each one, confirm it's a real active organization with a live site (or note honestly when it can't verify), and append correctly-formatted entries — following the same sourcing discipline as everything already in the file. That's exactly how this atlas grew from a 38-organization Colorado list to 369 entries covering the entire country.
