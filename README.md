# US MTB Trail Advocacy Atlas

A single-file, zero-dependency directory of nonprofit mountain bike trail advocacy organizations across the United States. Searchable, filterable by tier and state, with a donut chart and a grid-cartogram map. Built to the same architecture as PMR's other tools: one HTML file, no build step, deploy anywhere.

**Current status:** 266 organizations across 32 states plus a national/regional tier (IMBA, NEMBA, SORBA, NICA, Trips for Kids, CAMTB, CAMTB, JORBA, OMBA, HMBA are the umbrella-style entries).

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
| `s` | Two-letter state code, or `US` for national/regional umbrellas | yes |
| `tier` | `"national"`, `"state"`, or `"local"` — see below | yes |
| `r` | One-line region/description (city, trail system, notable detail) | no but recommended |
| `w` | Website URL. Leave `""` if none exists (renders as plain italic text instead of a link) | no |
| `p` | Parent org's abbreviation, if this is a chapter (e.g. `p:"VMBA"`) | no |

**Tiers:**
- `national` — umbrella bodies spanning multiple states (IMBA, NEMBA, SORBA, NICA, CAMTB, Trips for Kids). Displayed with a dark dot.
- `state` — a statewide coalition of local orgs, or a single org that is itself the de facto statewide voice (JORBA in NJ, OMBA in OK, HMBA in IN). Displayed with a brown/gold dot.
- `local` — everything else: town, county, or regional chapters. Displayed with a green dot. This is the vast majority of entries.

To add a new organization, find the right `/* ---------- STATE NAME ---------- */` comment block (or add a new one) and insert a line following the same pattern. The file will break if you forget a trailing comma or a closing quote — if in doubt, hand it to Claude with "add these orgs to the atlas."

## Confidence / sourcing discipline

Every entry in this file has an independently verified, live URL — checked via web search at the time it was added, not copy-pasted from a list without confirmation. A few entries deliberately link to a Facebook page, Trailforks listing, or MTB Project club page instead of an official website, because no dedicated website could be found. That's noted in the footer of the page itself.

Where a state has one authoritative source (a statewide coalition's own member directory), that source did most of the verification work in one shot:

| State | Primary source |
|---|---|
| Colorado | comtb.org/colorado-trail-organizations |
| Vermont | vmba.org/chapters |
| Oregon | ormtbcoalition.org/ombc-members |
| Washington | evergreenmtb.org/chapters |
| California | camtb.org/members |

Every other state was assembled org-by-org from web search — real, but not necessarily *exhaustive*. A state showing only 1-3 orgs almost certainly has more; it just means nobody has gone looking yet, not that only 1-3 orgs exist there. The pie chart and map reflect *research coverage*, not actual advocacy density — Colorado looks disproportionately large mostly because it has the cleanest published list.

A handful of orgs mentioned by name in source material could not be independently verified (no live site, no social page, no third-party confirmation) and were deliberately left out rather than guessed at: Susanville Area Bicycle Association (CA), Exchequer Riders Club (CA), South Orange County Trail Coalition (CA), LA Bike Park Collective (CA), Weaver Basin Trail Committee (WI — likely = Trinity Trail Alliance, not duplicated), SWIN (IN), Brainerd Area MBA (MN), Northfield Area MBA (MN), Central Mississippi Trail Alliance (MS).

Two deliberate exclusions on principle, not oversight: youth interscholastic racing leagues (NICA state chapters, high school cycling leagues) are not trail-*building* orgs and were left out even where mentioned in source material — except NICA itself at the national tier, which is the umbrella body, not a state league.

## States not yet covered

As of this version: Alaska, Connecticut, Delaware, Florida, Hawaii, Iowa, Kentucky, Louisiana, Maryland, Michigan, Missouri, Nebraska, Nevada, North Carolina, South Carolina, Tennessee, Virginia, West Virginia, Washington DC.

Highest-value next targets:
- **SORBA's remaining footprint** (FL, LA, NC, SC, TN) — SORBA already has 48 chapters across 8 states; only 5 are in the atlas (2 GA, 2 AL, 1 MS). `sorba.org/chapters` should work the same way CAMTB's member page did — one fetch, most of the verification done at once.
- **Michigan** — IMBA-assisted state coalition exists per IMBA's own "State of the States" post; not yet pulled in.
- **Nevada, North Carolina, Virginia** — no single coalition source identified yet; would need the org-by-org approach.

## Color themes

Three palettes, switchable in the UI (top of the page, under the tier filter), persisted via `localStorage`:

- **Moose** (default) — the original ink/loam/clay earth-tone palette.
- **Elk** — warm gold/amber/umber.
- **Caribou** — cool slate-blue/frost-teal.

Each theme is defined twice and both places need to stay in sync if you add a fourth:

1. CSS custom properties in `<style>`, under `[data-theme="name"]{...}` — controls the page chrome (background, text, buttons, tier dots).
2. The `THEME_PALETTES` object in `<script>` — controls the pie chart slice colors and the map's color-mix gradient, since those are drawn in JS and can't pick up CSS variables automatically.

To add a theme: copy both blocks, pick 5-6 new hex values, and add a fourth `<button class="theme-swatch" data-theme="...">` in the HTML markup next to the existing three.

## Known structural quirks

- **Abbreviation collisions.** Several real-world orgs share the same acronym (CIMBA = Central Illinois *and* Central Idaho; SMBA = San Miguel CO, Saratoga NY, Illinois; OMBA = Overland CO, Ocala FL, Oklahoma, Oregon Timber Trail). Where this happens within the atlas, a state suffix was added to the `a` field (e.g. `CIMBA-IL`, `SMBA-NY`, `OMBA-OK`) purely so the abbreviation column reads unambiguously — the actual org names are correct and unmodified.
- **Renamed / merged orgs.** A few entries note a rename or merger inline in the `r` field rather than as a separate object — e.g. Ride NoCo (formerly White Mountains NEMBA, went independent from NEMBA in 2020), OCMTBA (merged with SHARE Mountain Bike Club in 2022), Marin Trail Stewards (formerly Access4Bikes).
- **The map is a grid cartogram, not real geography.** One square per state in roughly correct relative position — accurate for reading relative density, not for tracing actual state borders. Real TopoJSON boundaries would add real weight to a file that's meant to stay a single lightweight HTML file.

## Working with Claude on this file

The fastest way to extend this: paste a list of orgs (with state, city/region if known) into a new chat message along with the current `.html` file, and ask Claude to verify and add them. Claude will web-search each one, confirm it's a real active organization with a live site (or note honestly when it can't verify), and append correctly-formatted entries to the right state section — following the same sourcing discipline as the entries already in the file.
