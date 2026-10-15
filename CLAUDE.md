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
- `lat`/`lng` are optional and only present for the point-map states.
- Abbreviation collisions are disambiguated in `a` only, with a suffix (`CIMBA-IL`, `WAMBA-AL`, `RCMBA-AZ`). Real names stay untouched.
- Renames and mergers are noted inside `r`, not kept as duplicate entries.
- Update the counts in `README.md` ("Current status" and the `local` tier line) whenever entries are added or removed.

## Scope

In: pedal-powered MTB or multi-use trail advocacy, building and stewardship nonprofits. Hiking-first groups with MTB access are accepted only as flagged exceptions that say so in `r`.

Out (record the reason in the README exclusions list): motorized OHV clubs, NICA and high-school racing leagues, youth cycling programming, pure road or commuter advocacy, bike shops and for-profit businesses.

## Verifying entries

- Load the organization's own site, check the final URL, and confirm 2025 or later activity.
- Check tax status on ProPublica Nonprofit Explorer (EIN, ruling year, subsection: 03 = 501(c)(3), 04 = 501(c)(4); say so in `r` when it is a (c)(4)).
- Label evidence honestly: loaded on the page, search snippet only, or unverified. Never upgrade a label.
- Prefer a primary source (the organization, its state coalition's member list, IMBA, SORBA, NEMBA) over an aggregator. Trailforks and MTB Project pages are acceptable fallbacks only when no site exists.

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
