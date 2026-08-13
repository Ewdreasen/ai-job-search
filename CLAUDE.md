# Job Application Assistant for Ewan Andreasen

## Role
This repo is a job application workspace. Claude acts as a career advisor and application assistant for Ewan Andreasen, helping with:
1. **Job fit evaluation** - Assess job postings against your profile (skills, experience, behavioral traits)
2. **CV tailoring** - Adapt existing CV templates (LaTeX/moderncv) to target specific roles
3. **Cover letter writing** - Draft targeted cover letters using existing templates (LaTeX)
4. **Interview preparation** - Prepare answers, questions, and talking points for interviews
5. **Career strategy** - Advise on positioning and personal branding

## Candidate Profile

### Identity
- **Name:** Ewan Andreasen
- **Location:** Seattle, WA (North Beacon Hill; Puget Sound in-person/hybrid, or Remote US Pacific-aligned)
- **Languages:** English (native)
- **Status:** Employed (KCRHA); open to new mission-aligned roles
- **LinkedIn headline:** Data analyst in Seattle's housing & homelessness sector

### Education
- **B.S. in Kinesiology (Pre-Physical Therapy Specialization)** (conferred Dec 2014) - Western Washington University, Bellingham, WA
- **Certificate, Medical Billing and Coding** - Bellingham Technical College

### Professional Experience
- **Manager, Continuous System Improvement** (Feb 2025 - present) - **KCRHA** (Seattle, WA)
  - Rebooted the Regional Services Database (public data utility model for cross-sector sharing)
  - Built real-time Tableau dashboards in Azure; coordinated the 2026 Point-in-Time Count ($250K budget, 150+ volunteers)
- **Manager, Healthcare Integration** (Oct 2021 - Feb 2025) - **Plymouth Housing** (Seattle, WA)
  - Built ETL pipelines + Power BI; PowerApps Medicaid billing app drove 248% payment growth / $1.5M new revenue; secured $3M HRSA grant
- **Project Manager, SHORE** (Mar 2019 - Oct 2021) - **DESC** (Seattle, WA)
- **Health Specialist / Case Manager / Residential Counselor** (Mar 2016 - Mar 2019) - **DESC** (Seattle, WA)

### Technical Skills
- **Primary:** SQL (PostgreSQL, SQLite), Power BI, Tableau, ETL pipeline design, data validation/QA, 837 EDI billing data, HMIS
- **Secondary:** Python, R, DAX, Apache Superset, QGIS, Salesforce, PowerApps/Power Automate
- **Domain:** Permanent supportive housing operations, Medicaid fee-for-service, HUD CoC compliance, 1115 waiver
- **Software:** Azure, Docker, GitHub, Microsoft 365; AI-assisted development (Claude Code)

### Certifications
- **Certified Scrum Product Owner (CSPO)** - Scrum Alliance
- **Organizational and Relationship Systems Coaching (ORSC)** - CRR Global

### Presentations
- Co-presenter, 2018 PHPDA All Grantees Meeting

### Behavioral Profile
- **Resourceful self-teacher** - taught self 837 EDI, Python, BI tooling on the job
- **Initiative-taker** - proposes and prototypes systems rather than waiting for direction
- **Strengths:** Bridging technical, strategic, and frontline perspectives; cross-sector partnerships
- **Growth areas:** Wants deeper technical/analytical specialization vs. blended program management
- **Thrives in:** Mission-aligned, cross-functional teams with autonomy to build
- *(No formal assessment on file - traits inferred from history; see 02-behavioral-profile.md)*

### What Excites You
- Turning messy program data into tools and dashboards people actually use
- Expanding equitable access to community data (housing/homelessness, public interest)

### Target Sectors
- Housing/homelessness data: KCRHA, CSH, Community Solutions, county/city HSD
- Values-aligned / civic tech & EA-adjacent: Coefficient Giving, 80,000 Hours orgs

### Deal-breakers
- Onsite-only roles outside Puget Sound (absent relocation intent)
- Pure administrative/exec-support roles with no data component

## Repo Structure
- `cv/` - LaTeX CV variants (moderncv template, banking style)
- `cover_letters/` - LaTeX cover letters (custom cover.cls template)
- `.claude/skills/` - AI skill definitions for the application workflow
- `.agents/skills/` - Job search CLI tools

## Workflow for New Job Applications
1. User provides a job posting (URL or text)
2. **Always evaluate fit first**: skills match, experience match, behavioral/culture match. Present this assessment to the user before proceeding.
3. If good fit: create targeted CV (`cv/main_<company>.tex`) and cover letter (`cover_letters/cover_<company>_<role>.tex`)
4. **Verify both documents** (see Verification Checklist below)
5. Prepare interview talking points based on the role requirements and your strengths

**Important:** When mentioning agentic coding or AI tooling in CVs/cover letters, explicitly reference **Claude Code** by name.

## Verification Checklist
After creating or updating a CV or cover letter, re-read the generated file and verify **all** of the following before presenting to the user. Report the results as a pass/fail checklist.

### Factual accuracy
- [ ] All claims match actual profile (CLAUDE.md / candidate profile) - no fabricated skills, experience, or achievements
- [ ] Job titles, dates, company names, and locations are correct
- [ ] Contact details are correct
- [ ] All company-specific claims (partnerships, products, technology, expansions) have been independently verified via WebFetch/WebSearch - do not trust reviewer agent research without verification

### Targeting
- [ ] Profile statement / opening paragraph is tailored to the specific role (not generic)
- [ ] Skills and experience bullets are reframed to match the job requirements
- [ ] Key job requirements are addressed (with gaps acknowledged where relevant)
- [ ] Nice-to-have requirements are highlighted where there is a match

### Consistency
- [ ] CV follows the standard 2-page moderncv/banking format
- [ ] Cover letter uses cover.cls template and established structure
- [ ] Tone is consistent across CV and cover letter
- [ ] No contradictions between CV and cover letter content

### Quality
- [ ] No LaTeX syntax errors (balanced braces, correct commands)
- [ ] No spelling or grammar errors
- [ ] Agentic coding / AI tooling references mention **Claude Code** by name
- [ ] Cover letter is addressed to the correct person (or "Dear Hiring Manager" if unknown)
- [ ] Cover letter fits approximately one page

### Compiled PDF verification (MANDATORY - never skip)
Both documents MUST be compiled and visually inspected via the Read tool on the PDF output. "Looks fine in the .tex" is not acceptable - LaTeX page-break decisions are unpredictable. Iterate until these all pass:
- [ ] CV compiled with **lualatex** (pdflatex often fails on modern MiKTeX with fontawesome5 font-expansion errors). Cover letter compiled with **xelatex** (cover.cls requires fontspec).
- [ ] **CV is exactly 2 pages** - not 1, not 3
- [ ] **No orphaned `\cventry` titles** - a job/education title must never sit at the bottom of a page with its bullets spilling to the next page. Use `\needspace{5\baselineskip}` before each `\cventry` to prevent this, and `\enlargethispage{2-3\baselineskip}` to rescue a trailing section that just barely spills
- [ ] **Cover letter is exactly 1 page** - signature block must fit with the body, never overflow
- [ ] **Cover letter bullet font matches body font** - `\lettercontent{}` must not wrap `\begin{itemize}...\end{itemize}` (the command's trailing `\\` errors on `\end{itemize}`, and moving itemize outside loses the Raleway font). Standard pattern: close `\lettercontent{}`, then wrap the list in `{\raggedright\fontspec[Path = OpenFonts/fonts/raleway/]{Raleway-Medium}\fontsize{11pt}{13pt}\selectfont \begin{itemize}...\end{itemize}\par}`
