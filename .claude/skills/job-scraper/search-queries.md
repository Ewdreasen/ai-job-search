# Search Queries for Job Scraper

<!-- US market configuration. The keyword pass below runs alongside the direct
     employer checks in target-orgs.md. Customize titles/skills/locations to taste. -->

## Search Sites

Primary (US job market):
- **linkedin.com/jobs** - largest reach; filter to US / your metro / Remote
- **builtin.com** - tech & mission-tech roles (Built In Seattle + national Remote)
- **idealist.org** - nonprofit / social-impact roles (strong for civic-tech orgs)
- **techjobsforgood.com** - public-interest / civic tech job board
- **usajobs.gov** - federal roles (official; has a free API if you want structured pulls)
- **workforcegps.org / npjobfinder.org** - nonprofit boards (supplemental)
- **governmentjobs.com / NEOGOV** - where WA public agencies actually post (Seattle Housing Authority, King County, City of Seattle); your local public-sector pipeline
- **foundationlist.org** - foundation/nonprofit roles; strong for grants & program ops
- **jobs.philanthropy.com** (Chronicle of Philanthropy) - philanthropy/grantmaking ops

Values-aligned / high-impact (EA-adjacent; your expanded remote-ops scope):
- **jobs.80000hours.org** - canonical EA/high-impact board; Coefficient Giving, CEA, Open Phil all post here
- **effectivealtruism.org/opportunities** - EA Opportunities Board
- **theimpactjob.com** - social-impact aggregator with good remote filtering
- **remoteimpact.org** - remote-only mission roles

Housing / homelessness sector-specific:
- **National Alliance to End Homelessness** & **National Housing Conference (NHC)** career centers

Aggregators with structured APIs (optional, if you wire them later):
- **Adzuna** - free API key, US listings across boards
- **USAJOBS** - official federal API
- **Greenhouse / Lever / Ashby board JSON** - per-company, when a target uses them

Direct employer career pages: see `target-orgs.md` (checked every run, not via `site:` here).

## Query Categories

Combine each query with location terms where the site supports it. Default locations:
`Seattle`, `Washington`, and `Remote` (Ewan is North Beacon Hill, Seattle; remote-open).

### Priority 1: Civic / public-interest data roles

Strongest, most-desired direction — homelessness/housing data, social-policy data.

```
site:linkedin.com/jobs "data analyst" homelessness OR housing Remote
site:linkedin.com/jobs "data analyst" "public sector" OR "social impact" Seattle OR Remote
site:idealist.org "data analyst" homelessness OR "supportive housing"
site:techjobsforgood.com data analyst
"HMIS" OR "Continuum of Care" data analyst remote
```

### Priority 2: Domain expertise (homelessness / housing / Medicaid systems)

Maps to a decade in the Seattle housing/homelessness sector and FCS/Medicaid 1115 work.

```
site:linkedin.com/jobs "systems analyst" Medicaid OR "supportive housing" Remote
site:linkedin.com/jobs "program analyst" homelessness Seattle OR Washington
site:idealist.org data OR analytics "continuum of care" OR HMIS
"data" "social policy" analyst remote nonprofit
```

### Priority 3: Adjacent technical roles (analytics engineering / BI)

Pivot-adjacent given self-taught Python/SQL/Power BI/Flask and the analyst bridge plan.

```
site:linkedin.com/jobs "analytics engineer" Remote nonprofit OR "public sector"
site:linkedin.com/jobs "business intelligence" analyst Seattle OR Remote healthcare OR housing
site:builtin.com "data analyst" OR "analytics engineer" Remote
"power bi" OR "tableau" analyst remote mission
```

### Priority 4: Broader technical / civic tech

Wider net for general technical roles with a values fit.

```
site:linkedin.com/jobs "civic technology" OR "govtech" data Remote
site:linkedin.com/jobs "data" engineer OR analyst government OR nonprofit Remote
site:usajobs.gov data analyst   (or use the USAJOBS keyword search directly)
"public interest technology" data role remote
```

### Priority 5: Remote operations roles at values-aligned orgs

Broadens beyond pure data roles. The strongest *remote + values* fits often surface as
operations/program roles at mission-driven orgs (effective-altruism funders, civic-tech,
housing/health nonprofits). These value analytical generalists, and ops work frequently
includes grants tracking, light data analysis, and reporting — a viable bridge.

```
site:jobs.ashbyhq.com "operations associate" OR "operations coordinator" remote
site:linkedin.com/jobs "operations associate" OR "operations manager" remote nonprofit OR "social impact"
"remote" "operations" associate OR coordinator "effective altruism" OR "mission-driven"
site:jobs.lever.co OR site:boards.greenhouse.io operations remote nonprofit
site:techjobsforgood.com operations
"program operations" OR "grants operations" analyst OR associate remote
```

Screen for: Remote (US) or Pacific-aligned, analytical/data-adjacent responsibilities, and
a mission you'd stand behind. De-prioritize pure admin/exec-assistant roles with no data hook.

### Priority 6: Board-specific sweeps

Run these against the boards added above when doing a broad sweep.

```
site:jobs.80000hours.org operations OR data OR grants remote
site:foundationlist.org grants OR program OR data analyst remote
site:jobs.philanthropy.com grants OR program operations remote
site:theimpactjob.com data OR operations remote
site:remoteimpact.org operations OR data
"National Alliance to End Homelessness" OR "National Housing Conference" jobs data OR analyst
```

## Location Filter

Acceptable locations when screening results:
- **Seattle metro** (North Beacon Hill base) — in-person or hybrid OK
- **Remote (US)** — preferred for national civic-tech orgs
- **Tacoma / Puget Sound** — acceptable hybrid
- **Hybrid requiring >~1x/week onsite outside Puget Sound** — borderline; flag
- **Onsite-only outside Puget Sound** — too far unless relocation is explicitly desired

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline not yet
passed. If the posting date is unknown, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area (e.g. `/scrape analytics engineering`), select queries
from the matching category and generate 2-3 custom queries for that focus.
