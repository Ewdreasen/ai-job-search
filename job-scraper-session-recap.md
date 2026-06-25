# Job Scraper — Session Recap (2026-06-24)

Summary of changes made to the job-search tooling in this session.

## 1. Scope expansion: remote operations roles

Search scope was widened beyond civic/housing **data-analyst** roles to also include
**remote operations roles at values-aligned / mission-driven orgs** (effective-altruism
funders, civic-tech, housing/health nonprofits). Rationale: the strongest *remote + values*
matches currently surface as ops/program roles, and ops work at these orgs often includes
grants tracking, light data analysis, and reporting — a viable bridge for an analyst track.

- Added **Priority 5: Remote operations roles at values-aligned orgs** to `search-queries.md`.
- Screen rule: Remote (US) or Pacific-aligned + analytical/data-adjacent responsibilities;
  de-prioritize pure admin/exec-assistant roles with no data hook.

## 2. New job boards added (`search-queries.md`)

- **governmentjobs.com / NEOGOV** — where WA public agencies actually post (King County,
  City of Seattle, Seattle Housing Authority). Primary local public-sector pipeline.
- **foundationlist.org**, **jobs.philanthropy.com** — foundation / grantmaking ops.
- Values-aligned cluster: **jobs.80000hours.org**, **effectivealtruism.org/opportunities**,
  **theimpactjob.com**, **remoteimpact.org**.
- Housing-sector: **National Alliance to End Homelessness**, **National Housing Conference**.
- Added **Priority 6: Board-specific sweeps** with `site:`-scoped query lines.

## 3. New target-org watch-list entries (`target-orgs.md`)

| Org | ATS / system | Method | Notes |
|-----|--------------|--------|-------|
| Coefficient Giving | Ashby (JSON) | json | EA grantmaking funder (Open Phil rebrand); feed verified |
| 80,000 Hours job board | custom board | fetch | EA aggregator — use per-org `/organisations/<slug>` URLs only |
| King County, WA | NEOGOV | fetch | DCHS / housing / homelessness analyst roles |
| City of Seattle | NEOGOV | fetch | HSD + Innovation & Performance analyst roles |

### Operational learnings recorded in the notes

- **NEOGOV portals are JS-rendered** — a direct WebFetch of the careers landing page returns
  "0 jobs found". Use the `site:governmentjobs.com/careers/<org>` search fallback instead.
- **80,000 Hours** exposes only *organisation* pages via board HTML and `site:` search — not
  individual postings. Fetch per-org `/organisations/<slug>` URLs to read roles.
- **Deadline trap:** NEOGOV pages stay indexed (and can still show an "Open" label) long after
  the application deadline passes. Always compare the deadline to today's actual date.

## 4. Roles to watch (recurring exact-fit, closed at last check)

- **Cross Sector Data Analyst — APDE** (King County Public Health) — cross-sector housing /
  criminal-legal / Community & Human Services data linkages.
- **Temporary Senior Data Analyst** (City of Seattle HSD, Data/Performance/Evaluation) —
  HMIS / GIS / Navigation App homelessness data. Prior instance: job #2026-00633.

Both are recurring hires; the limiting factor observed this session was **timing, not fit**.

## 5. Automation: weekly cloud scrape

A scheduled cloud routine ("Weekly KC + Seattle HSD job scrape") now runs every **Monday
8:00am PT**. It runs the `site:` fallback for the King County + City of Seattle boards,
deadline-checks each posting against the current date, watches for reposts of the two roles
above, dedupes against `seen_jobs.json`, and opens a PR with a fit-sorted findings table.
