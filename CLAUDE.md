# Trail Advocacy Atlas: working notes for Claude

A single-file directory (`index.html`) of non-profit mountain bike and multi-use trail advocacy organizations in the United States. Part of Post Millennium Renaissance. The repo is **public** and the page is embedded on postmillenniumrenaissance.com and Pinkbike, so everything committed here is publicly readable.

## Who you are working with

Jon has no coding background and edits through GitHub's web UI. Deliver complete files, not diffs. Explain tradeoffs. Ask before restructuring.

## Hard rules

1. **One file.** `index.html` holds markup, CSS, JS and data. No build step, no dependencies.
2. **All data sits in one block** ending at the `END OF DATA` marker. Logic comes after it. Adding an organization is one new line in the organizations array.
3. **Never invent data.** A gap beats a guess. If a URL or fact cannot be confirmed, leave it out and say so.
4. See the PMR build standard (`pmr-build-standard` skill) for architecture and audit rules.

## Entry format

`{n:"Name",a:"ABBR",s:"ST",tier:"local",p:"PARENT_ABBR",c:"City City",r:"Short description",w:"https://site/"}`

- `tier`: `national`, `regional` (multi-state or broad footprint), `state` (statewide coalition or de facto statewide voice), `local` (everything else).
- `p` is the parent organization's abbreviation (for example `SORBA`, `CAMTB`) and is only set when membership is confirmed.
- `lat`/`lng` are optional and only present for the point-map states (CA, CO, VT, OR). A multi-state organization (for example Orogenesis Collective) has no pin on purpose.
- `status:"unconfirmed-active"` flags a real organization with no 2025 or later activity found. `conf:"draft"` is unused today.
- `s` is a two-letter code: `US` for national or multi-state umbrellas, `DC` (shown as "Washington DC") or `PR`.
- Abbreviation collisions are disambiguated in `a` only, with a suffix (`CIMBA-IL`, `WAMBA-AL`, `RCMBA-AZ`). Real names stay untouched.
- Renames and mergers are noted inside `r`, not kept as duplicate entries.
- Update the counts in `README.md` ("Current status", the `local` count in the tier list and the "Current split" line) whenever entries are added or removed. As of 2026-10-24: 449 total (2 national, 5 regional, 20 state, 422 local), 6 entries without a website.
- Abbreviations (`a`) and organization names must stay unique. Check this before every merge.

## Scope

In: pedal-powered MTB or multi-use trail advocacy, building and stewardship nonprofits. Hiking-first groups with MTB access are accepted only as flagged exceptions that say so in `r`.

Out (record the reason in the README exclusions list): motorized OHV clubs, NICA and high-school racing leagues, youth cycling programming, pure road or commuter advocacy, bike shops and for-profit businesses.

## Verifying entries

- Load the organization's own site, check the final URL, and confirm 2025 or later activity.
- Check tax status on ProPublica Nonprofit Explorer (EIN, ruling year, subsection: 03 = 501(c)(3), 04 = 501(c)(4); say so in `r` when it is a (c)(4)).
- Label evidence honestly: loaded on the page, search snippet only, or unverified. Never upgrade a label.
- Prefer a primary source (the organization, its state coalition's member list, IMBA, SORBA, NEMBA) over an aggregator. Trailforks and MTB Project pages are acceptable fallbacks only when no site exists.

## Publishing

The live page is the GitHub Pages copy of `main` (`postmillennium-mtb.github.io/Trail-Advocacy-Atlas/`); the PMR site and Pinkbike iframe it. Merging to `main` publishes within minutes. The README is the long-form reference (themes, printing, URL-hash links, Why model); keep it in step when behavior changes.

## Checking a build before merge

Load `index.html` in a headless browser at about 390px and 1280px wide and confirm: no console errors; `#utah`, `#ut`, `#dc`, `#why` and a nonsense hash all behave (nonsense falls back to By state); the phone Filters button opens the panel; the Trailhead map draws one dot per entry with `lat`/`lng` for each of CA, CO, VT, OR; every tab renders; no duplicate abbreviations or names; every `p` value matches an existing `a`.

## Contact details: collect, but do not publish

For every organization, record an **email and/or mailing address (and phone if printed)** when it is readily available on the organization's own site. Many will be blank, because not every organization publishes an email. Blank is fine. Never guess or construct an address from a domain, and never take an address from a third-party aggregator.

- Store contacts in `private/contacts.csv`. That folder is git-ignored. **Never put emails, street addresses or phone numbers into `index.html`, `README.md`, `CLAUDE.md`, the research notes in any commit, or any other tracked file.**
- Columns: `organization, abbreviation, state, email, mailing_address, phone, source, date_added, notes`. Use `notes` for the EIN, a rename, or caveats.
- The container is temporary. After updating the CSV, send the file to Jon with `SendUserFile` so he holds the current copy.
- Contacts are not shown on the atlas for now. Do not add contact fields to the entry format or the UI unless Jon asks.

## Working conventions

- Work on the branch Jon names. Open a pull request only when asked. Merging to `main` publishes the live atlas, so only do it when Jon says to go live.
- `research_notes/` and `reports/` hold working files and stay out of git.
- When researching in batches, run state groups in parallel and have each researcher propose edits in a table rather than editing `index.html`. Review the proposals, then apply them in one pass.

## License and notice (do not remove)

This repo is licensed under PolyForm Noncommercial 1.0.0 (`LICENSE`). Every edit and rebuild must keep: the `Required Notice:` line at the top of `LICENSE`; the (c) / SPDX comment right after the doctype of each HTML page; and the visible `.pmr-license` footer on each page. Do not remove, reword or hide them. New pages get the same footer. Third-party material keeps its own license: never relicense it, and keep its notices. Jon holds the copyright and decides any license change.

## Kept in step with the other copy of the Why / Partnerships model

The Partnerships comparison (Option 1 municipal-only vs Option 2 with a non-profit) exists in two tools that are maintained separately and must not drift apart:

| Tool | Repo | Where it lives |
|---|---|---|
| Trail Investment ROI Calculator | `postmillennium-MTB/mtb-economic-studies` | the **Partnerships** button and panel |
| Trail Advocacy Atlas | `postmillennium-MTB/Trail-Advocacy-Atlas` | the **Why** tab (`#why`) |

**What must stay identical in both:**
- `computeWhy()`: the cost model. As of October 2026 the two copies are identical apart from comments and quote style.
- `WHY_INPUTS`: every slider's `min`, `max`, `step` and `value`. Also identical as of October 2026. Hint wording may differ, but any figure quoted in a hint may not.
- The funding-source and warning-flag rules (the ROI Calculator keeps them in `WHY_FUNDING` and `whyFlags`; the Atlas implements the same logic inside its own render code and has no such registries).
- Sourced figures: professional singletrack **$60,000–$80,000 per mile** (Singletracks.com contractor survey, April 2026), routine maintenance **$1,200–$2,525 per mile per year** for paved and mixed-surface trails (American Trails, quoting a 2007 Milwaukee County plan) and about **$3,000–$4,000** for singletrack (Singletracks' 5%-of-build-cost rule), the SLV GO toolkit's **$15,000–$40,000 per mile** typical singletrack construction as the lower published benchmark, volunteer time **$34.79/hr** (Independent Sector, 2024).
- The caveats section heading, **"Data sources & methodology notes"**, and the substance of its paragraphs (the ROI Calculator shows it as a fold, the Atlas as a section).

**What is allowed to differ:** layout and styling, the surrounding page chrome (the ROI Calculator's scenario buttons, currency and unit switcher and print report do not exist in the Atlas), and sentence-level wording that does not change a figure or a claim.

**Rule for every session:** if you change anything in the lists above in one repo, say so in your handover and either make the same change in the other repo in the same working session or tell Jon exactly what is left to port. Never fix a figure or a source in one copy and leave the other stale. Before a PR that touches the Why / Partnerships model, compare `computeWhy()` and the `WHY_INPUTS` numbers against the other repo.

