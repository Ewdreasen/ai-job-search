# Target Organizations Watch-List

<!-- Consumed by the job-scraper skill (see SKILL.md, Step 1b: Target-Org Watch).
     Each org below is a named employer to check directly on every /scrape run,
     independent of the keyword WebSearch pass in search-queries.md.

     METHOD legend:
       json   - org exposes a clean public JSON feed; fetch + parse directly (preferred)
       fetch  - no JSON feed; WebFetch the careers/portal URL and extract openings from HTML
       search - no stable careers page; run a site/LinkedIn/Idealist WebSearch for the org

     STATUS legend (informational, refresh as you learn more):
       verified  - URL/feed confirmed reachable and parseable
       confirm   - URL is a best guess; confirm the exact careers path on first run
-->

## How the scraper uses this file

On each `/scrape`, after the keyword pass, iterate every org below:

1. If `METHOD: json`, fetch the `Feed` URL and parse positions from JSON.
2. If `METHOD: fetch`, WebFetch the `Careers` URL and extract current openings.
3. If `METHOD: search`, run a WebSearch like `"<Org>" jobs careers` plus `site:linkedin.com/jobs "<Org>"`.
4. Dedupe against `job_scraper/seen_jobs.json` and `job_search_tracker.csv` exactly as the keyword pass does.
5. Tag every result with `source: target-org` and the org name so highlights can group by employer.

Roles at these orgs should be surfaced even on a "narrow" run — they are the priority list, not the broad net.

## Priority watch-list

| Org | ATS / system | Method | Status |
|-----|--------------|--------|--------|
| Community Solutions (Built for Zero) | Breezy HR | json | verified |
| CSH (Corporation for Supportive Housing) | Paycom | fetch | verified |
| Chapin Hall | iCIMS (UChicago) | fetch | verified |
| AISP (Actionable Intelligence for Social Policy) | UPenn Workday | fetch | confirm |
| Community Technology Alliance (CTA) | none / ad hoc | search | verified |
| U.S. Digital Response (USDR) | careers page / Tech Jobs for Good | fetch | confirm |
| Mission Driven Data | none / ad hoc (LinkedIn) | search | verified |
| Coefficient Giving | Ashby | json | verified |
| 80,000 Hours job board (EA aggregator) | custom board | fetch | confirm |
| King County, WA | NEOGOV | fetch | confirm |
| City of Seattle | NEOGOV | fetch | confirm |

## Org details

### Community Solutions (Built for Zero)
- **Method:** json
- **Feed:** https://community-solutions.breezy.hr/json
- **Careers (human):** https://community-solutions.breezy.hr/
- **Notes:** Breezy HR exposes a public board JSON at `/json` (array of positions: name,
  location, type, url, and often a salary range). This is the one structured feed in the
  list — prefer the JSON over scraping the portal HTML. If `/json` ever 404s, fall back to
  fetching the portal URL. Mission-aligned (homelessness data/systems) — highest priority.

### CSH (Corporation for Supportive Housing)
- **Method:** fetch
- **Careers (human):** https://www.csh.org/about-csh/careers/
- **Portal (Paycom):** https://www.paycomonline.net/v4/ats/web.php/jobs?clientkey=459DACBECAB10D873B69AD0393B53F45
- **Notes:** Paycom has no clean public JSON API; fetch the portal URL and extract the job
  rows from HTML. National CDFI nonprofit, hybrid/remote-friendly.

### Chapin Hall
- **Method:** fetch
- **Careers (human):** https://www.chapinhall.org/why-work-at-chapin-hall/employment-opportunities/
- **Notes:** "Current Job Openings" button routes to an iCIMS / University of Chicago portal.
  No clean public JSON. Fetch the employment-opportunities page first; follow the openings
  link it exposes. Typically ~5-12 roles. Child/family policy research; data roles relevant.

### AISP (Actionable Intelligence for Social Policy)
- **Method:** fetch
- **Careers (human):** https://aisp.upenn.edu/  (no standalone jobs page observed)
- **Portal:** University of Pennsylvania Workday ("Careers@Penn"); filter to School of Social
  Policy & Practice (SP2).
- **Notes:** STATUS=confirm — AISP itself posts no separate board; roles appear in Penn's
  Workday among all university postings. On first run, locate the exact Penn external Workday
  careers URL and pin it here, then filter by department/keyword "AISP" or "SP2".

### Community Technology Alliance (CTA)
- **Method:** search
- **Site:** https://ctagroup.org/
- **Notes:** Tiny HMIS nonprofit (~7-50 staff), no ATS. Posts ad hoc on its site, Idealist,
  and LinkedIn. Run `"Community Technology Alliance" jobs` + `site:idealist.org "Community
  Technology Alliance"` + `site:linkedin.com/jobs "Community Technology Alliance"`. Strong
  domain fit (HMIS, coordinated entry, data integration).

### U.S. Digital Response (USDR)
- **Method:** fetch
- **Careers (human):** https://www.usdigitalresponse.org/  (confirm exact /careers path on first run)
- **Aggregator mirror:** https://techjobsforgood.com/companies/us-digital-response
- **Notes:** STATUS=confirm — primarily a pro-bono volunteer network; paid openings are
  infrequent (none live at last check). Watch both the org careers page and the Tech Jobs
  for Good mirror.

### Mission Driven Data
- **Method:** search
- **Site:** https://missiondrivendata.com/
- **Notes:** Small behavioral-health / Credible-EHR / Power BI consultancy. No ATS; posts
  roles ad hoc via LinkedIn (e.g. a recent Solutions Consultant opening). Run
  `site:linkedin.com/company/mission-driven-data` and `"Mission Driven Data" hiring`.
  Warm-contact org (Nicole Morris, Katie) — flag any opening prominently.

### Coefficient Giving
- **Method:** json
- **Feed:** https://api.ashbyhq.com/posting-api/job-board/coefficientgiving
- **Careers (human):** https://jobs.ashbyhq.com/coefficientgiving
- **Notes:** Grantmaking/research nonprofit (Global Health & Wellbeing, Longtermism;
  formerly Open Philanthropy-adjacent EA funder). Ashby exposes a public posting API at
  `api.ashbyhq.com/posting-api/job-board/{slug}` — prefer the JSON over the portal HTML.
  Many roles are Remote USA or Remote Global; ops/program/research postings can include
  data-adjacent work. Uses compensated blind work-tests in lieu of resume-heavy screening.

### 80,000 Hours job board (EA aggregator)
- **Method:** fetch (org-filter URLs only — see below)
- **Board:** https://jobs.80000hours.org/
- **Org-filtered views:** e.g. https://jobs.80000hours.org/organisations/coefficient-giving
- **Notes:** Aggregates high-impact/EA roles across Coefficient Giving, CEA, Open Phil, and
  many mission-driven orgs. Not a single employer — an aggregator.
  **Verified 2026-06-24:** neither the main board HTML nor `site:jobs.80000hours.org` search
  exposes individual job postings — both only surface *organisation* pages. So do NOT rely on
  a `site:` keyword sweep here. Instead, WebFetch the per-org `/organisations/<slug>` URL for
  each EA org you care about (coefficient-giving, centre-for-effective-altruism, open-philanthropy,
  etc.) and read the roles listed there. Filter to Remote / US-eligible + analytical work.

### King County, WA
- **Method:** fetch (NEOGOV)
- **Careers:** https://www.governmentjobs.com/careers/kingcounty
- **Notes:** Local public-sector pipeline. Filter for analyst / data / program roles in
  DCHS (Dept of Community & Human Services), housing, and homelessness divisions. In-person
  or hybrid Puget Sound — clears the location filter. NEOGOV portals are JS-rendered; if the
  fetch returns 0 jobs, fall back to `site:governmentjobs.com/careers/kingcounty <keyword>`.

### City of Seattle
- **Method:** fetch (NEOGOV)
- **Careers:** https://www.governmentjobs.com/careers/seattle
- **Notes:** Local public-sector pipeline (Human Services Dept, Innovation & Performance team).
  Filter for analyst / data / research roles. Same NEOGOV JS-render caveat as King County —
  use the `site:` search fallback if the portal returns nothing.

## Adding orgs

Append a row to the table and a matching `### Org` block. Resolve the ATS by opening the
careers page once: `boards.greenhouse.io/{slug}` or `api.greenhouse.io` -> Greenhouse (json),
`jobs.lever.co/{slug}` -> Lever (json), `jobs.ashbyhq.com/{slug}` -> Ashby (json),
`{slug}.breezy.hr` -> Breezy (json via `/json`), `paycomonline.net/...clientkey=` -> Paycom
(fetch), `*.icims.com` -> iCIMS (fetch), `myworkdaysite.com` / `myworkdayjobs.com` -> Workday
(fetch). Everything else: `search`.
