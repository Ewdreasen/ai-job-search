Weekly scrape is scheduled and live.

  ✅ Routine created: "Weekly KC + Seattle HSD job scrape"

  ┌──────────┬──────────────────────────────────────────────────────────────┐
  │          │                                                              │
  ├──────────┼──────────────────────────────────────────────────────────────┤
  │ Schedule │ Every Monday 8:00am Pacific (0 15 * * 1 UTC)                 │
  ├──────────┼──────────────────────────────────────────────────────────────┤
  │ First    │ Mon, Jun 29, 2026 at 8:00am PT                               │
  │ run      │                                                              │
  ├──────────┼──────────────────────────────────────────────────────────────┤
  │ Repo     │ Ewdreasen/ai-job-search                                      │
  ├──────────┼──────────────────────────────────────────────────────────────┤
  │ Model    │ claude-sonnet-4-6                                            │
  ├──────────┼──────────────────────────────────────────────────────────────┤
  │ Output   │ Opens a PR titled "Weekly job scrape: <date>" with a         │
  │          │ findings table                                               │
  └──────────┴──────────────────────────────────────────────────────────────┘

  Manage it: https://claude.ai/code/routines/trig_019abh6iYrsJfTa2GEHrPH1d

  What it does each week

  - Runs the site: fallback for both King County and City of Seattle NEOGOV
  boards (since direct fetch returns "0 jobs").
  - Fetches each promising posting and checks the deadline against the actual 
  current date — so it won't repeat this session's trap of an "Open"-labeled but
  already-expired posting.
  - Watches for reposts of your two bullseye roles (APDE Cross Sector Data
  Analyst, HSD Temporary Senior Data Analyst) and flags them HIGH PRIORITY if a
  fresh, future-deadline instance appears.
  - Dedupes against and appends to seen_jobs.json, then opens a PR with a
  fit-sorted table. It's instructed to say plainly when nothing new is live (and
  never fabricate).
  
  A couple of notes:
  - It runs in the cloud (no access to your local machine) and delivers results
  as a GitHub PR on the repo — so you'll review findings there, not in this
  session.
  - It commits seen_jobs.json updates via that PR, so the dedupe store stays in
  sync once you merge.

  You can't delete routines from here — if you ever want it gone, use the link
  above.
