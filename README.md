# US MTB Trail Advocacy Atlas

A single-file, zero-dependency directory of nonprofit mountain bike trail advocacy organizations across the United States. Searchable, filterable by tier and state, with a donut chart, a 50-state cartogram, real geocoded trailhead maps for four states, and a **Why** tab that models what one of these organizations is actually worth to a town in dollars. Built to the same architecture as PMR's other tools: one HTML file, no build step, deploy anywhere.

**Current status:** 449 organizations across all 50 states, Washington DC, and Puerto Rico.

---

## Opening / hosting it

It's one file: `index.html`. Double-click it to open locally. The live copy is served by GitHub Pages from the `main` branch of this repo (`postmillennium-mtb.github.io/Trail-Advocacy-Atlas/`), and postmillenniumrenaissance.com/trail-advocacy-atlas/ and Pinkbike embed that page in an iframe, so merging to `main` is what publishes a change. No npm, no build, no server required.

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
- `local` — everything else: town, county, or regional chapters. This is the vast majority of entries — 422 of 449. Green dot.

Current split: 2 national, 5 regional, 20 state, 422 local (449 total). Update this line, the total in "Current status", and the local count above whenever entries are added or removed.

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
- Mount Shasta Trail Association (CA) — hiking trail association, not MTB advocacy (the MTB group there is Bike Shasta, already listed).
- Rich City Rides (CA) — Richmond bike-access and community programming, not MTB; removed after owner review.
- Removed after the K-S re-check (2026-10): Bend Trails (OR; site shut down June 2026), Red Rock Riders (NV; no legal entity or nonprofit record), Southern New Mexico Mountain Biking (youth programming only), Mt Hood Bicycle & Pedestrian Coalition (OR; no verifiable presence), Lower Umpqua Mountain Bike Riders (OR; no verifiable presence), and a duplicate Missouri listing of Urban Trail Co.
- Velomont Trail Collective (VT) removed in the T-Z re-check: it is a joint project of Vermont Huts & Trails and VMBA, not a separate organization.
- Wasatch Area Freeride Trails Association and Salt Lake Valley Trails Society (UT) removed after owner review: websites and email domain dead, last Facebook activity 2015 and 2021. Vermont Bikepackers (VT) excluded: route and event organization, not a trail builder or steward.
- All Bikes Welcome (AR) — 501(c)(3) outdoor/cycling access programming in Northwest Arkansas, not trail advocacy; same reasoning as Trips for Kids.
- Removed in the 2026-10 full state review: Ozark Highlands Trail Association (AR; hiking and backpacking only, no bike access documented), Arkansas Parks and Recreation Foundation (AR; parks funder, not a trail builder), Bicycle Advocacy of Central Arkansas (AR; general bicycle advocacy, and the link was a city page), Waco Bicycle Club (TX; road-cycling club with no trail work shown).

**A few orgs mentioned in source material but never added**, because no live site, social page, or third-party confirmation could be found: Susanville Area Bicycle Association (CA), Exchequer Riders Club (CA), South Orange County Trail Coalition (CA), LA Bike Park Collective (CA), SWIN (IN), Brainerd/Northfield Area MBA (MN), Central Mississippi Trail Alliance (MS), Hike NWA (AR; hiker education, no stable site confirmed), Women of Oz (AR; 501(c)(3) status unconfirmed). Better to leave a gap than invent a URL.

**Renamed / merged orgs**, noted inline in `r` rather than as separate entries: Ride NoCo (an independent nonprofit founded 2020, separate from White Mountains NEMBA, which is a long-running NEMBA chapter), OCMTBA (merged with SHARE Mountain Bike Club, 2022), Marin Trail Stewards (formerly Access4Bikes), Bike Shasta (formerly Mount Shasta MBA), Mad River Trails Alliance (merger of Mad River Riders and Mad River Path Association, effective Oct 1, 2026), Western Mass NEMBA (formerly Pioneer Valley NEMBA, 2023), Grand Valley Trails (formerly Grand Valley Canyons, per COPMOBA's chapter list), Oklahoma Mountain Bike Association (formerly Oklahoma Earthbike Fellowship), Kansas Trails Inc. (filed as Kansas Association of Trail Stewards).

**Entries kept but flagged "not confirmed active"** after the 2026-10 review, because the group is real but no 2025-26 activity could be found: Hawaii Mountain Bike Association, San Miguel Bike Alliance (CO), Cuchara Mountain Trails Alliance (CO), Lake Mountain Bicycle Club (UT).

**Abbreviation collisions** across unrelated orgs in different states are resolved with a state suffix on the `a` field only, so every abbreviation in the file is unique (checked 2026-10-24: no duplicate abbreviations or names among the 449 entries). Examples: ( `CIMBA-IL` vs. Idaho's `CIMBA`, `SMBA-NY` vs. Colorado's `SMBA`, `OMBA-OK` vs. Oregon/Colorado's `OMBA`; `MMBA-UT` for Moab vs. Michigan's `MMBA`, and likewise `HTA-AZ`, `MAMBA-CO`, `RAMBO-MN`, `KMBC-IL`, `SWMMBA-MT`, `CORBA-IL`, `CAMBA-OH`) — the real org names are untouched, this only disambiguates the abbreviation column.

## States with no coverage yet

As of this version, every US state, DC, and Puerto Rico has at least one entry. The next honest gap is **depth, not breadth** — states with only 1-2 orgs (North Dakota, Mississippi, Rhode Island, New Jersey) are probably close to complete for MTB-specific advocacy, but populous states like Michigan, Missouri, and the Rockies states almost certainly have more independent local clubs than are currently listed.

## Trailhead maps (California, Colorado, Vermont, Oregon)

These four states — the ones with the most entries — have **real geocoded coordinates**, not estimates. Two places use them:

1. **The Map tab's cartogram**: clicking CA/CO/VT/OR (bordered tiles) switches from the state-count grid to a real point map for that state.
2. **The dedicated "Trailhead map" tab**: a bigger, standalone card with its own CA/CO/VT/OR selector at the top, independent of whatever's in the search box or state dropdown. Click a dot to pin a full org card (name, tier, region, website link) above the map; hover for a quick preview first.

Both point maps list any regional, statewide or national group in that state that has no `lat`/`lng` under the dots as "Not pinned — covers more than one place" (today: Orogenesis Collective). This avoids a made-up pin for a group whose trail runs from Canada to Mexico.

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

### Making it personal (display only)

None of this touches `computeWhy()`, `WHY_INPUTS`, a slider range or a default. The wording relabel further down does need porting (see "Waiting to be ported to the ROI Calculator").

- **Your town (optional).** A text box at the top of the tab. The name labels the result ("Scenario for Castle Rock, Colorado"), the pinned bar, the printout and the shared link. It is a label only: nothing is looked up and no number changes. Because it can arrive in a link, it is cleaned (`cleanTown()`: letters, digits, spaces and `. , ' ’ ( ) & -` only, at most `WHY_TOWN_MAX` = 60 characters) and only ever written as text, never as HTML.
- **Your state (optional).** A state picker. It adds a card under the ledger, "Groups listed in Colorado that could be the partner", with the count listed in this atlas, any statewide group, the networks with groups there, and buttons into that state's Directory and scorecard (and its trailhead map for CA, CO, VT, OR). It does not change any number. Arriving from a one-state view (`#colorado`, then Why) starts the picker on that state.
- **Pinned result bar.** On a phone the first result sits about four screens below the sliders, and on a desktop it scrolls away as the lower sliders come up. While the result cards are off screen and the controls are on it, a slim bar fixed to the bottom of the screen shows the headline result with four key inputs, updated as a slider moves; tapping it scrolls to the full result. While it shows, the page gets bottom padding the bar's height, so it never covers the last slider or the footer, and the Share confirmation box sits above it. On phones the headline result is also repeated in the page flow at the top of each slider group (tap it to jump to the full result), which does not depend on the bar.

  **On the PMR page.** The wrapper at `postmillenniumrenaissance.com/trail-advocacy-atlas/` sets the iframe to `height: 100vh` with the outer page not scrolling, so the atlas scrolls inside the frame and the bar shows (tested at 390px and 1280px, 2026-11-11). One known catch: on iPhone Safari, `100vh` is the height with the browser toolbar collapsed, and because the outer page never scrolls, the toolbar stays expanded and covers the bottom of the frame, which can hide part of the bar and the last lines of the atlas. The in-flow copy above covers the bar; to fix the frame itself, change one line in the wrapper's `<style>`:
  ```css
  iframe { display: block; width: 100%; height: 100vh; height: 100dvh; border: none; }
  ```
  (`100dvh` is the currently visible height; browsers that do not know it keep using `100vh`.) If the frame is ever sized to the full content height instead, the result is always "on screen" from the frame's point of view and the bar will not appear; the in-flow copy still does.
- **Share this scenario.** Copies a link that restores every slider, the inflation pill, the town and the state (`#why?miles=14&grant=40&infl=2&town=Castle+Rock&state=co`), or opens the phone's share sheet. The link opens the full tab, so the number always arrives with its inputs and caveats.
- **The number never travels alone.** Under the three result cards a scenario line lists the key inputs (trail length, cost per mile, grant share, volunteer hours, private capital, horizon, and inflation when on), the "illustrative only" line and the model's caveat line. It prints on page 1 beside the number. Print / PDF from an iframe carries the town and state too (`w_town`, `w_state`).
- **All wording in one place.** `WHY_TEXT` in the data block holds the tab heading, the result labels ("Option 1 · Municipal only", "Option 2 estimated lower by" and so on), the "illustrative only" line and the caveat line. The heading, result cards, phone summary, in-flow phone result, pinned bar, scenario line, share sheet text and the "Data sources & methodology notes" section all read it, so changing the dollar language is one edit there. If it changes, make the same change in the ROI Calculator's Partnerships panel (see below).

### Illustrative cost comparison (labelling rule, November 2026)

Results on this tab are shareable by link and print, so every dollar figure is presented as an **illustrative cost comparison**, not a forecast, a promise of savings, or advice.

- The tab is headed "Illustrative cost comparison". The lead result reads "Option 2 estimated lower by $225K (33%)" (or "estimated higher by"; "Estimated difference" when within 1%), with "under these inputs" in its sub-line. It never says a town "saves".
- A fixed line goes beside every result: **"Illustrative only. Based on the inputs shown, which you can change. Not a forecast, an offer, or financial advice."** It sits in the scenario line (screen and print page 1) and in the methodology section. A shorter form, "Illustrative only, from the inputs shown. Not a forecast, offer or advice.", sits in the pinned bar, the phone's in-flow result and the phone's sticky summary, and goes with the share sheet text and the "link copied" message on the Why tab.
- Do not use "ROI", "return", "savings", "guaranteed" or any promise of what a town will get in result wording.
- This is wording only: `computeWhy()`, `WHY_INPUTS` ranges and defaults, and every number are unchanged (checked: default and all-maximum outputs are identical before and after).

### Waiting to be ported to the ROI Calculator

The November 2026 relabel above changes the result wording and adds a paragraph to the "Data sources & methodology notes" section, which the sync rule in `CLAUDE.md` says must match in both tools. The ROI Calculator (`postmillennium-MTB/mtb-economic-studies`, Partnerships panel) still needs these changes. Remove this list once they are made there.

1. Panel heading or title: add "Illustrative cost comparison".
2. Result labels: "Partnership saves" becomes "Option 2 estimated lower by"; "Partnership costs more" becomes "Option 2 estimated higher by"; "Difference" (within 1%) becomes "Estimated difference".
3. Result sub-line: "Cheaper / Costlier municipal cost over N years" becomes "Lower / Higher estimated town cost over N years, under these inputs"; "Within 1% — on these assumptions the two approaches are a wash." becomes "Within 1%: under these inputs the two estimates are about the same."; "Town's N-year cost:" becomes "Estimated town N-year cost:".
4. Beside every result (on screen and in its print report): "Illustrative only. Based on the inputs shown, which you can change. Not a forecast, an offer, or financial advice."
5. "Data sources & methodology notes": add a first section, **What this comparison is**: "An illustrative cost comparison for planning conversations. Every figure on this tab is an estimate computed from the inputs shown, all of which you can change. Illustrative only. Based on the inputs shown, which you can change. Not a forecast, an offer, or financial advice." and repeat the line after the closing caveat.
6. Wording in the panel's flags and chart notes: "Option 2 still comes out ahead here." becomes "Option 2 still comes out lower on these inputs."; "...is where the money actually is." becomes "On these inputs the gap widens every year the non-profit keeps its crews out there: the maintenance liability, not the construction discount, drives most of the difference."; the build-only flag's "volunteer adoption savings are included" becomes "volunteer adoption offset is included".
7. Slider hints (wording only, no figure): "The ceiling on volunteer savings." becomes "The ceiling on what volunteer labor can offset."; "...the partnership stops paying for itself." becomes "...Option 2 comes out more expensive."

No figure, slider range, default or line of `computeWhy()` changes, so the cost models stay identical.

## Color themes

Three palettes, switchable in the UI under the tier filter, persisted via `localStorage`:

- **Caribou** (default) — icy blue-teal base with a soft, abstract camo-blob background pattern (six low-opacity radial gradients, not a busy texture) and neon aurora accents: hot magenta, neon green, neon cyan.
- **Moose** — warm earth tones: cream paper, forest-green/rust accents, soft charcoal-gray ink (not pure black — see mobile note below on why near-black was a problem).
- **Elk** — warm gold parchment paper, mahogany-brown ink, amber/marigold accents.

Each theme is defined in **two places** that must stay in sync if you add a fourth:
1. CSS custom properties under `[data-theme="name"]{...}` — controls page chrome.
2. The `THEMES` registry in `<script>` — controls pie chart slice colors, the map's color-mix gradient, and the `paper` hex pushed to `<meta name="theme-color">`, since those are drawn in JS and can't read CSS variables automatically for computed effects like `color-mix()`.


### Background textures

Each color theme also has its own background texture (the colors themselves are unchanged). The textures are masks hosted at `postmillenniumrenaissance.com/textures/masks/` (served with open CORS, so they work from a github.io iframe) and tinted with `--tex-ink`; two empty `<div class="texture">` / `<div class="texture-2">` elements sit at the top of `<body>` and the CSS lives in the "PMR BACKGROUND TEXTURES" block at the end of the stylesheet. The generator's declarations are unchanged and only scoped per theme.

| Theme | Texture | Source patent(s) |
|---|---|---|
| Caribou (default) | `us-d268339` fading top to bottom, `crowe-1882-13106` fading bottom to top | US D268,339 (Inae and Fukuchi, Mitsuboshi Belting, 1983); US 13,106 (Crowe, 1882) |
| Moose | `day-1927-74141`, one seamless layer | US 74,141 (Day, 1927) |
| Elk | `fisher-1882-13110` fading left to right, `fisher-1882-13108` fading right to left | US 13,110 and 13,108 (Fisher, 1882) |

- `isolation:isolate` on `<body>` is load-bearing: without it the body's own background paints over the `z-index:-1` texture and it disappears.
- Textures are hidden in print. The credit lines for the active theme's texture are shown in the page footer (`.tex-credit`); keep them when editing.
- **To give a new theme a texture:** copy a block in that CSS section, change the theme selector, `--tex-ink` and the mask files, and add its credit to `.tex-credit`. Keep opacity low; the text sits on top.
- The mask files load from the web, so with the page opened as a local file the textures appear only while online.

### Changing the default theme

Four edits, and skipping any one of them leaves a visible bug:

1. **Move the bare `:root` selector** onto the new default's CSS block. It styles the first paint, before JS has stamped `data-theme` on `<html>`; leave it behind and every cold load flashes the old palette.
2. **Move that block to the top of the palette list.** `:root` and `[data-theme="elk"]` have *identical* specificity (0,1,0), so the one declared last wins. A default block sitting at the bottom silently overrides every other palette — swatches still flip `data-theme` and charts still recolor, but `--paper`/`--ink` stay stuck on the default. This one is easy to miss because it looks like it works until you click another swatch.
3. **Set `DEFAULT_THEME`** in the `THEMES` registry.
4. **Update the `<meta name="theme-color">` tag** in `<head>` to the new default's paper hex, so the mobile address bar matches before `applyTheme()` runs.

### Theme persistence

`applyTheme(name, persist)` only writes to `localStorage` when `persist` is true, which is exactly the swatch-click path. Applying the default on load deliberately does *not* write.

This mattered when Caribou became the default: the previous code saved on **every** load, default included, so every visitor who had ever opened the atlas carried an explicit `"moose"` they never chose — and would never have seen a new default. The storage key was retired (`mtbAtlasTheme` → `mtbAtlasTheme2`) to clear that once. A deliberate pick still survives reloads; a default no longer masquerades as one.

## Printing / Save as PDF

Everything for paper lives in one `@media print` block at the **end** of the `<style>` (search `PRINT / SAVE-AS-PDF`), plus a `beforeprint` hook near the bottom of the script. The screen layout is untouched. Open the tab you want, set filters if you want a subset, then press the **Print / PDF** button at the right end of the tab bar (or Ctrl/Cmd+P) and choose *Save as PDF*. "Background graphics" can be on or off — the print CSS forces data fills to print either way.

- **Only the open tab prints.** Search, filters, theme swatches, the tab bar, sliders, buttons and the banner are hidden. A "Filtered to: …" line appears when a state/tier/search filter is active, since the filter bar itself isn't printed.
- **One print palette for all three themes.** The screen accents fail on white (Caribou `--clay` 1.8:1, `--loam` 1.6:1; Elk `--clay` 2.7:1; Moose `--clay` 4.2:1 — WCAG text minimum is 4.5:1). Every theme prints with the same deep, ≥4.5:1 set, applied with `:root:root` (specificity 0,2,0) so it beats every theme block regardless of source order. Pie slices, the cartogram ramp and the per-capita tier bars still follow the chosen theme — they are data fills and every one carries a printed number or label.
- **Tier is shape as well as color on paper.** National = solid circle, regional = ring, statewide = square, local = small dot, with a one-line key under the masthead (built from `TIER_LABELS`). The point maps keep circles and use dot size.
- **Links print their URL** (Directory, Structure, Near me, map detail card) because paper can't be clicked.
- **Long lists print in full.** On screen the per-million chart and Structure's independent groups show their first 15 rows and a "Show all" button; on paper every row prints whether or not the button was pressed. (They used to scroll inside a fixed-height box, which on paper cut the per-capita chart to 31 of 52 rows.)
- **`beforeprint` opens every closed `<details>`** (the Structure tab's top-level groups) and restores them in `afterprint`.
- **Why tab prints results first**, assumptions on a fresh page after. Only the chosen inflation pill prints.
- **Cartogram label contrast** is chosen from each tile's actual blended luminance (`labelIsDark()`), not a fixed density threshold — this one change also applies on screen.
- **SVG text sizes are in chart units**, which scale with the chart; the print sizes for `.chart-axis-label` / `.why-axis-title` / `.maplabel` were measured to land at ~8pt+ printed. Re-measure if a chart's `viewBox` width changes.

**Known print limits:** on a black-and-white printer the stacked bars on the Why tab rely on their printed `$` labels and the legend order (two segments land on similar greys; narrow segments under 13% carry no inline label), the 52-slice pie relies on its legend, and the point maps distinguish national/regional/statewide by color only. Near me carries a typed town or picked state into the new tab (it rides in the hash); a location from the browser does not.

**The Print / PDF button and iframes.** Opened on its own, the button just calls `window.print()`. Embedded in an iframe (the PMR site, Pinkbike), the browser's print command prints the frame as a fixed-size box clipped to what is visible — not the full content — so the button instead opens the tool in a new tab with the current view in the URL (`?print=1&theme=…&q=…&state=…&tier=…&net=…`, plus `w_<slider key>=…` and `w_infl=…` on the Why tab, only for values that differ from defaults, and `w_town` / `w_state` when set). That tab restores the view, strips the `print` flag (so a reload doesn't reopen the dialog) and opens the print dialog after `PRINT_AUTO_DELAY_MS`. Every URL value is validated (known theme / state / tier, slider min–max, real inflation pill, search text capped at `PRINT_QUERY_MAX`), and the theme is applied for that tab only, not saved. Needs the iframe to allow popups — the wrapper template has no `sandbox` attribute, so it does; if one is ever added, include `allow-popups`. The button sits beside the tab list, not inside it (ARIA: a tablist may only contain tabs).

## Mobile dark-mode fix

Earlier versions of this file rendered correctly on desktop but showed a dark, hard-to-read background on some mobile browsers. This wasn't a bug in the page's own CSS — it was mobile browsers' **forced dark mode** (Android Chrome, Samsung Internet, some iOS dark-mode heuristics) auto-darkening a page that hadn't explicitly opted out. The fix has three parts, all now in place:

1. `<meta name="color-scheme" content="light">` in `<head>` — the actual signal that stops forced-dark reinterpretation. The CSS `color-scheme: light` property alone (which was tried first) only affects native form controls, not full-page darkening.
2. `html{background:var(--paper)}` — previously only `<body>` had an explicit background. On iOS Safari's rubber-band overscroll bounce (a mobile-only interaction), the `<html>` background briefly shows through; if undeclared, it can default dark under a dark system theme.
3. `<meta name="theme-color">`, updated dynamically per theme — tints the mobile browser's address bar to match the active palette instead of a mismatched default.

If a dark-background report ever comes back, check these three things first before assuming it's a new CSS bug.

## URL-hash deep links and the Share button

The address follows the visitor: switching tab, picking a state or tier, searching, opening an organization's profile, pinning a Trailhead map dot or moving a Why slider rewrites the hash, and opening that address restores the same view. The original links keep working exactly as before.

- **A tab id**: `trail-advocacy-atlas/#why` opens the Why tab. Works for any `TABS` id.
- **A state**: `trail-advocacy-atlas/#utah` opens the Directory filtered to Utah. The full name, the two-letter code and dashed or run-together spellings all work (`#utah`, `#ut`, `#new-hampshire`, `#newhampshire`). Covers every state plus DC (`#dc` or `#washington-dc`; the atlas spells it "Washington DC", so `#district-of-columbia` does not match), Puerto Rico and the national group (`#us` or `#national`). This is also the form the page writes for "Directory filtered to one state".
- **A tab with filters**: `#map?state=co&tier=local`, `#charts?q=springs`, `#list?net=SORBA`. `state`, `tier`, `q` (search) and `net` (a network: the parent organization and every group below it) are the keys.
- **One organization**: `#org/headwaters-trails-alliance` opens the Directory on that group's state with its profile open and highlighted. The key is made from the organization's **name** (120 entries have no abbreviation), lowercased with every run of other characters turned into a dash. Renaming an organization changes its link; an old link then falls back to the default tab.
- **Who speaks for my state**: `#map?mode=voice` opens the Map tab in that mode (see "State scorecards" below).
- **Trailhead map**: `#detail?state=vt&pin=<org key>` opens that state's map with that group pinned.
- **Near me**: `#near?q=austin` repeats a typed town or state search, `#near?state=tx` a state picked from the list. A location from the browser is never written into the link.
- **Why**: `#why?miles=12&grant=40&infl=2` restores sliders that differ from their defaults (keys are the `WHY_INPUTS` keys, `infl` is the inflation pill), and `town=` and `state=` restore the town label and state picker (see "Making it personal"). Display only: `computeWhy()` and `WHY_INPUTS` are not touched, so nothing needs porting to the ROI Calculator.

A hash that matches nothing is ignored and the default tab opens. Every value is checked against what the page offers (real state, tier, network, organization, slider range). `resolveHashRoute()` reads a hash and `viewHash()` writes one; both live in the "LINKS THAT FOLLOW THE VISITOR" block. All work on initial load, on a live `hashchange`, and through the `postMessage({type:"pmr-hash", hash:"#utah"})` fallback. The page rewrites its address with `history.replaceState`, so it never adds Back-button steps.

**Share button** (next to Print / PDF): copies a link to the current view, or opens the phone's share sheet. Each Directory profile also has "Copy link to this group". Links always point at `postmillenniumrenaissance.com/trail-advocacy-atlas/` plus the hash (`SHARE_BASE_URL`); the PMR wrapper already passes its hash on to the iframe, so shared links open the right view there. A box at the bottom of the screen confirms the copy and shows the link, so it can be copied by hand if the browser blocks automatic copying.

**Inside an iframe** (the PMR page, Pinkbike) two things are limited by the browser, not by this file:
1. The visitor's address bar shows the outer page's address, not the atlas view. The atlas sends `{type:"pmr-atlas-view", hash}` to the PMR wrapper (only when the wrapper is a PMR origin). To make the PMR address bar follow along, add to the wrapper's script:
   ```js
   window.addEventListener('message', e => {
     if (e.origin === 'https://postmillennium-mtb.github.io' && e.data && e.data.type === 'pmr-atlas-view')
       history.replaceState(null, '', e.data.hash || location.pathname);
   });
   ```
2. Copying and the share sheet need the iframe to allow them. Without it the button falls back to an older copy method, which worked in testing, or to the hand-copy box. To let it use the phone share sheet too, give the wrapper's `<iframe>` `allow="clipboard-write; web-share"`.

## Click-throughs

Anything that looks like data can be clicked: By state slices and legend rows (California, Colorado, Vermont and Oregon open their trailhead map), Map tiles, Charts points (on a phone, tap twice: the first tap shows the value) and per-million bars open that state in the Directory; Structure's "N below" and "N states" badges and the parent tag on each Directory row (`SORBA`, `CAMTB`) open the Directory filtered to that network, shown as a removable chip under the tab bar; Map tab dots open that group's profile; a pinned Trailhead map card links to the profile and the network. One delegated handler (`onClickThrough()`) reads `data-goto-state`, `data-net`, `data-open-org`, `data-toggle-org`, `data-pin-org`, `data-copy-org`, `data-clear`, `data-route`, `data-copy-hash`, `data-map-mode` and `data-show-all`, so a new panel only needs the attribute.

**Directory profiles.** "Details" on a row opens a short profile in place, built only from fields already in the data: tier, state, abbreviation, the parent chain (each step clickable), the number of member groups for a network, the website, a "See on the trailhead map" link when the group has a pin, "Copy link to this group" and "Suggest a correction" (see below).

**449 versus 445.** The header counts every entry (449). The By state ring counts the 445 tied to one state; the 4 national or multi-state entries (`s:"US"`) are not drawn, and a note under the ring now says so and links to them.

## Stat strip, state scorecards and "Who speaks for my state"

All three are computed from the data on every load (the "STATE FACTS" block: `stateFacts()`, `voiceOf()`), so adding an organization updates them. Every count reads "listed in this atlas", a missing statewide group reads "no statewide group listed", and ranks are "groups listed per resident", because counts reflect research coverage, not how many groups exist.

- **Stat strip** (above the tabs, replaces the old two counters): with no state picked it shows groups listed, the places with a group listed (52: the 50 states, DC and Puerto Rico; the 4 national or multi-state entries with `s:"US"` belong to no one state and are left out of the place count, the rank and the map, which the page says), the share that belong to a larger network, and how many states hold half of all listed groups. With a state picked it shows that state's count, its rank for groups listed per resident, its statewide group (or "None") and either its trailhead-map count (CA, CO, VT, OR) or how many belong to a larger network. Each fact clicks through to the matching view.
- **State scorecard**: a one-state Directory (`#colorado`, or picking a state) opens with a card: groups listed, rank and groups per million residents, who speaks for the state, the networks with groups there, the tier split, and how that state's list was built. Every number and tag opens the matching Directory view (`#list?state=co&net=COMTB`, `#list?state=co&tier=local`), and "Copy link to this state" copies `#colorado`. The card describes the whole state; tier and search filters apply only to the list below it.
- **How this list was built** comes from `LIST_SOURCES` in the data block, copied from the sourcing table above. When a state is transcribed from a coalition roster, add it to both.
- **Who speaks for my state** (Map tab, second button): every state is shaded by the first of four levels it meets (`VOICE_LEVELS`): a statewide (`state` tier) group listed; covered by a multi-state network (a network whose listed groups span two or more states: SORBA, NEMBA, MORE); local groups only (3 or more listed); thin coverage (`THIN_COVERAGE_MAX` = 2 or fewer). As of 2026-11-11: 17, 15, 14 and 6. Clicking a state opens its scorecard. Colors are four steps of the active theme's map ramp (darkest = statewide); neighbouring steps differ by at least about 1.45:1 in every theme, and each tile and legend entry also carries a symbol (★ ◆ ● ○), so the map does not rely on color alone.
- **Population** (`STATE_POPULATION`, used by the per-million chart, the strip and the scorecards) is the US Census Bureau's Vintage 2025 estimate for July 1, 2025 (`NST-EST2025-ALLDATA`, column `POPESTIMATE2025`, from www2.census.gov), public domain. It replaced a worldpopulationreview.com projection on 2026-11-11; the rank order barely moved (changes under 1.5% per state).

## Near me everywhere

Near me gives an answer in every state, and always says which kind it is giving:

- **Distance** (California, Colorado, Vermont, Oregon). Groups there have map pins, so "Use my location", or a town named in a pinned group's listing ("Durango", "Portland"), ranks the pinned groups by miles, measured from the location or from that group's pin (the page names which).
- **State level** (everywhere else). A typed town is matched as a whole word against each group's name, `c` towns and `r` description; the page lists the matching groups and links to the state scorecard, and says plainly that distance ranking exists only in the four pinned states. A state name or code ("Ohio", "TX") lists the groups listed there, statewide groups first, capped at `NEAR_ME_RESULTS` with a link to the full scorecard. "Austin, TX", "Austin TX" and "Austin Texas" restrict the town to that state; if no listing there names the town, the page says so and shows the state's list. A miss offers a pick-your-state list and the missing-group prompt.
- **Location outside the four states.** The page checks the browser's location against the hand-simplified `STATE_OUTLINES`. Outside them it cannot tell the state without sending the location to an outside service, which it does not do, so it asks the visitor to pick a state, with a "rank the mapped groups by distance anyway" button for people near a border. The location is never put in the link.

Nothing is geocoded: no town gets coordinates that are not already in the data. Town-level distance for every state is held for later; it needs a town for each entry from the group's own site and public-domain coordinates (US Census Gazetteer), as described in the 2026-11-11 data review.

## Corrections and missing groups

- **"Suggest a correction"** sits in every Directory profile. **"Know a group we're missing?"** appears under every empty result (Directory, Charts, Structure, By state, Map), under every Near me answer and at the foot of the Directory, naming the state when one is picked.
- Today each link opens a new GitHub issue on this repo, pre-filled with the group (or the search and state) and a link back to the atlas view, through the issue forms in `.github/ISSUE_TEMPLATE/` (`correction.yml`, `missing-group.yml`). Both ask for a source link and remind people the issue is public. The sender needs a free GitHub account. The forms only take effect once they are on `main`; before that GitHub shows a plain issue with the title filled in.
- **The destination is one setting**, `FEEDBACK`, at the top of the logic (just after `END OF DATA`). To switch to a Google Form, set `kind:"form"`, paste the form's link into `url`, reword `note`, and optionally copy the form's `entry.NNN` ids into `formFields` so the group, state and search are pre-filled (the comment there walks through it). No email address is ever put in the page.
- A suggestion is never published automatically. It goes through the normal verification in `CLAUDE.md` before an entry is added or changed.

## The eight tabs

Why (cost model, see below), Directory (searchable list grouped by state), By state (donut chart; the default landing tab), Map (50-state cartogram, point map for CA/CO/VT/OR), Trailhead map (standalone point map), Charts (per-capita and coverage charts), Structure (parent organization and chapter hierarchy, built from the `p` field) and Near me (distance ranking from your location or a town in CA/CO/VT/OR, and a state-level answer everywhere else; see "Near me everywhere"). The list lives in the `TABS` registry.

## Known structural quirks

- **The cartogram is a grid, not real geography.** One square per state in roughly correct relative position — good for reading relative density, not for tracing actual borders.
- **Search includes a hidden `c` field.** If a search misses an org you'd expect to find (e.g. searching a small town name), it's likely because that org's `c` field hasn't been populated yet — the city name needs to be manually added, it's not automatically extracted from anywhere.
- **Reset button** (dashed, next to "Local" in the tier bar) clears search text, state, tier and network filters back to defaults — it deliberately does *not* touch your color theme or active tab, which are treated as separate preferences.
- **Default tab on load is "By state"** (the pie chart), not the Directory — changed deliberately partway through development. "Why" sits first in the tab bar as the thesis of the whole atlas, but isn't the landing tab.
- **On phones the filters sit behind a "Filters & palette" button** (`#filter-toggle`, opens `#filter-panel`), with an "N active" badge when a filter is set, so the first screen shows content. The tab row fades at the edge that still has hidden tabs (`updateTabFade()`).
- **The Why tab hides the filter bar and counters.** Tabs registered with `filters:false` in the `TABS` registry don't read the search/state/tier controls, so `switchTab()` hides them rather than leaving dead controls on screen. Any future tab that ignores the filters gets the same treatment with one flag.
- **The counter row is the stat strip** (see "Stat strip, state scorecards" above), drawn by `renderCounts()` on every render. A small cartogram icon jumps to the Map tab. It exists as two buttons, both drawn by `renderMapJump()` from the full organization list (it ignores search and filters and is redrawn on every palette change): on screens wider than 760px it sits left of the subtitle in the masthead and shows on every tab, including Why; on phones it sits at the right end of the counter row and so disappears on the Why tab with the counters. Both are hidden in print.
- **The Why tab's sliders are built once, not re-rendered.** `renderWhy()` builds the panel shell on first open and everything after that goes through `updateWhy()`, which only replaces the results column. Rebuilding the slider markup on every `input` event would drop pointer capture mid-drag and the thumb would stop following the mouse.
- **No scroll boxes inside the page.** Long lists (the per-million chart, Structure's independent groups) show `SHOW_ALL_FIRST` = 15 rows and a "Show all" button, so a finger-swipe never gets trapped in a box. The CSS rule `.is-clipped ... :nth-child(n+16)` must match that number.
- **Touch screens get bigger targets.** On a touch screen (`pointer:coarse`) the small badge buttons (Details, network badges, Structure counts, per-million rows) grow to at least 34px tall and the Why slider thumbs to 26px; desktop keeps the compact look.
- **The tab bar scrolls sideways on narrow screens** instead of widening the page. Without `overflow-x:auto` on `.tabs`, eight tabs push the whole document into horizontal scroll on a phone.
- **The masthead title rotates** every 6 seconds between "Who builds the trails you ride" and "How trails get built," respecting `prefers-reduced-motion` for anyone with that OS setting on.

## Working with Claude on this file

The fastest way to extend this: paste a list of orgs (with state, city/region if known) into a new chat message along with the current `.html` file, and ask Claude to verify and add them. Claude will web-search each one, confirm it's a real active organization with a live site (or note honestly when it can't verify), and append correctly-formatted entries — following the same sourcing discipline as everything already in the file. That's exactly how this atlas grew from a 38-organization Colorado list to 449 entries covering the entire country.

## License

Copyright (c) 2026 Post Millennium Renaissance (https://www.postmillenniumrenaissance.com).

Released under the [PolyForm Noncommercial License 1.0.0](LICENSE). In short: free to use, copy, modify and share for any noncommercial purpose, including personal use, research, education, and use by charities, environmental-protection organizations and government institutions. Commercial use needs a separate license; ask through postmillenniumrenaissance.com. Copies must keep the `Required Notice:` line in `LICENSE`. The license text controls over this summary.

Facts, figures and quotations taken from third-party studies and sources remain the property of their authors. This license covers only this project's own code, selection and arrangement of material, and original text. Third-party components keep their own licenses.


to add later:
"What can non-profit trail advocacy organizations actually help with?" (from perspective of municipality) 
Planning & Data Analytics
Usage metrics: Provide access to Strava Metro and Trailforks heatmap data to prove existing rider volume and identify highly trafficked corridors.
Technical alignment: Utilize specialized GIS mapping software to design sustainable, terrain-specific trail alignments that minimize erosion before the town spends money on formal environmental assessments.
Feasibility screening: Identify potential roadblocks early, such as navigating legacy conservation easements or wildlife migration corridors, saving municipal planners time.
Coordination & Community Liaison
Single point of contact: Act as the unified voice for the off-road cycling community, filtering out noise and saving Parks & Rec staff from fielding hundreds of individual emails.
Cross-jurisdictional linking: Facilitate connections across fragmented land parcels by negotiating trail corridors between town open space, county land, and private HOAs.
Conflict mediation: Proactively address multi-use trail conflicts through user-group dialogue before they become town council grievances.
Ideation & Modernization
Alternative perspectives: Introduce modern, bike-optimized design concepts such as directional singletrack, progressive jump lines, or purpose-built rock armoring.
Economic ROI: Provide economic impact models showing how localized trails and specialized facilities (like pump tracks) retain local spending and boost adjacent property values.
Maintenance & Asset Protection
Deferred maintenance reduction: Schedule and coordinate routine drainage and erosion mitigation to directly reduce the town's Parks & Rec maintenance backlog.
Rapid response: Deploy certified volunteer sawyers to clear downed trees after storms faster than municipal crews can be mobilized.
Rogue trail prevention: Design and build high-quality, engaging trails that naturally eliminate the community's desire to build illegal, unsanctioned "spider trails."
Volunteer Coordination & Labor Management
Trained leadership: Provide certified Crew Leaders (e.g., Outdoor Stewardship Institute trained) to ensure volunteer workdays are safe, organized, and structurally sound.
Labor mobilization: Market and schedule public dig days, managing all waivers, tool logistics, and on-site training.
Quantifiable labor tracking: Meticulously log volunteer hours, transforming free community labor into a documented financial asset for the town.
Grant Writing & Financial Capacity
Expanded funding pools: Apply for private, foundation, or non-profit-specific recreation grants (e.g., IMBA Dig In, outdoor industry funds) that towns are ineligible to receive directly.
In-kind matching: Use the organization's tracked volunteer hours as the required "in-kind match" for massive municipal or state grants (like CPW State Trails grants), allowing the town to secure hundreds of thousands of dollars with minimal actual cash outlay.
Fiscal sponsorship: Serve as a fiscal sponsor to hold and distribute trail-specific fundraising dollars securely, bypassing municipal budgetary red tape.
