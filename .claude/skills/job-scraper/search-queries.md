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
