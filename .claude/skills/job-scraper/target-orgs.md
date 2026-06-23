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

## Adding orgs

Append a row to the table and a matching `### Org` block. Resolve the ATS by opening the
careers page once: `boards.greenhouse.io/{slug}` or `api.greenhouse.io` -> Greenhouse (json),
`jobs.lever.co/{slug}` -> Lever (json), `jobs.ashbyhq.com/{slug}` -> Ashby (json),
`{slug}.breezy.hr` -> Breezy (json via `/json`), `paycomonline.net/...clientkey=` -> Paycom
(fetch), `*.icims.com` -> iCIMS (fetch), `myworkdaysite.com` / `myworkdayjobs.com` -> Workday
(fetch). Everything else: `search`.
