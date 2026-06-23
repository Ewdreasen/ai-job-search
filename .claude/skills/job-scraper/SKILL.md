---
name: job-scraper
description: >
  Searches US job sites and a curated list of target employers for new positions matching
  your profile. Deduplicates across runs.
  Triggers on: job scrape, find jobs, search jobs, new jobs, job search, scrape jobs, /scrape
allowed-tools: Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, Agent, AskUserQuestion
---

# Job Scraper

---

## How It Works

This skill runs two passes: (a) a **keyword pass** over US job sites using targeted queries
from `search-queries.md`, and (b) a **target-org pass** that checks each named employer in
`target-orgs.md` directly. It deduplicates both against previously seen jobs and the
application tracker, then presents new matches with a quick fit assessment.

## Invocation

The user triggers this skill by saying things like:
- "Find new jobs"
- "Scrape for jobs"
- "Any new positions?"
- "/scrape"

Optional arguments:
- A focus area, e.g. "/scrape data science" or "/scrape analytics engineering"
- "broad" to run all search categories, e.g. "/scrape broad"

---

## Execution Steps

### Step 0: Load State

1. Read `job_scraper/seen_jobs.json` (create if missing - start with `{"seen": {}}`)
2. Read `job_search_tracker.csv` to extract already-applied companies+roles
3. Read `search-queries.md` (this directory) for the keyword search strategy
4. Read `target-orgs.md` (this directory) for the direct employer watch-list

### Step 1: Search

Run **WebSearch** queries from `search-queries.md`. By default, run the top 3 priority categories. If the user said "broad", run all categories.

If the user specified a focus area (e.g. "data science"), prioritize queries from that category.

For each search:
- Use `WebSearch` with site-specific queries (linkedin.com/jobs, idealist.org, builtin.com, techjobsforgood.com, usajobs.gov, etc.)
- Target your configured geographic area (Seattle metro / Washington / Remote)
- Look for postings from the last 14 days

### Step 1b: Target-Org Watch

After the keyword pass, iterate every org in `target-orgs.md`. For each:

- **`METHOD: json`** — WebFetch the `Feed` URL and parse positions directly from JSON
  (e.g. Community Solutions' Breezy board at `https://community-solutions.breezy.hr/json`).
  Prefer this over scraping the human portal. If the feed 404s, fall back to the `Careers` URL.
- **`METHOD: fetch`** — WebFetch the `Careers` (and, if present, `Portal`) URL and extract
  current openings from the HTML (Paycom, iCIMS, Workday portals have no clean JSON).
- **`METHOD: search`** — run `"<Org>" jobs careers` plus `site:linkedin.com/jobs "<Org>"`
  and any org-specific queries noted in its block (Idealist, etc.).

Tag every target-org result with `source: target-org` and the org name. These are the
priority list: surface them even on a narrow/focused run, not just on `broad`.

### Step 2: Fetch & Parse

For each promising result from Step 1:
- Use `WebFetch` to retrieve the job posting page
- Extract: **job title**, **company**, **location**, **posting date** (or "recent"), **URL**, **key requirements** (brief), **application deadline** (if listed)
- Skip if the URL or company+title combo already exists in `seen_jobs.json`
- Skip if the company+role already appears in `job_search_tracker.csv`

### Step 3: Quick Fit Assessment

For each new job, do a rapid fit check (NOT the full evaluation from `04-job-evaluation.md` - just a quick signal):

- **High match**: Role directly involves your core skills
- **Medium match**: Role is adjacent to your experience
- **Low match**: Role requires significant skills you lack

### Step 4: Deduplicate & Store

1. Add ALL fetched jobs (new and skipped) to `seen_jobs.json` with structure:
```json
{
  "seen": {
    "<url_or_company_title_key>": {
      "title": "...",
      "company": "...",
      "url": "...",
      "first_seen": "YYYY-MM-DD",
      "fit": "high/medium/low",
      "status": "new/skipped/evaluated"
    }
  }
}
```
2. Only present jobs NOT already in the seen list or tracker.

### Step 5: Present Results

Present new jobs in a table sorted by fit (high first):

```
## New Job Matches - YYYY-MM-DD

Found X new positions (Y high, Z medium, W low match).

| # | Fit | Title | Company | Location | Deadline | URL |
|---|-----|-------|---------|----------|----------|-----|
| 1 | High | ... | ... | ... | ... | [Link](...) |

### High-Match Highlights
For each high-match job, add 2-3 bullet points:
- Why it matches your profile
- Key requirements to check
- Any red flags
```

After presenting, ask:
> "Want me to evaluate any of these in detail? Just give me the number(s)."

If the user picks a number, invoke the **job-application-assistant** skill workflow (fit evaluation first, then CV + cover letter if approved).

### Step 6: Update Tracker (Optional)

If the user decides to apply to any job, add a row to `job_search_tracker.csv`.

---

## Important Rules

1. **Never fabricate job postings.** Only present jobs found via actual WebSearch/WebFetch results.
2. **Respect deduplication.** Always check seen_jobs.json AND job_search_tracker.csv before presenting.
3. **Focus on configured geographic area.** Honor `search-queries.md`: Seattle metro / Puget Sound (in-person or hybrid) and Remote (US). Skip onsite-only roles outside Puget Sound unless relocation is explicitly desired. Target-org roles that are Remote always qualify.
4. **Only open positions.** Skip postings with expired deadlines or those marked as closed.
5. **Be efficient with WebFetch.** Don't fetch every search result - use titles and snippets to pre-filter before fetching.
6. **Parallel searches.** Use the Agent tool or parallel WebSearch calls to speed up the search phase.
