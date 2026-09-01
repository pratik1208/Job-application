# STATE — Job Application Agent

> Running history log. Any new session (human or LLM) should read this file FIRST
> to understand what has been done and what is left. Update it after every
> meaningful action.

## Project goal
Automate applying to job listings from `jobs.csv` using Chrome browser automation.
Fill each application from `profile.json`, pick the right resume from `resumes/`,
auto-submit, then mark the row `applied` in `jobs.csv`. If a form asks for a field
not in `profile.json`, pause, ask the user, and save the answer back into
`profile.json` for reuse.

## How resume is chosen (4 resumes)
- Role: title contains "AI Engineer" -> `ai`; "ML / Machine Learning / Data Scientist" -> `mlds`
- Location: job location is Bengaluru/Bangalore -> `bengaluru`; anything else -> `other`
- Resume key = role + location -> one of: ai_bengaluru, ai_other, mlds_bengaluru, mlds_other
- File paths are mapped in `profile.json > resumes`.

## Files
- `jobs.csv` — 25 real Workday listings; tracking columns url,company,title,location,role,resume_planned,resume_used,status,applied_at,notes
- `profile.json` — Workday-structured applicant details (populated from resume; a few FILL_ME left)
- 4 resume PDFs in project ROOT (mapped in profile.json > resumes):
  - pratik_marudwar_.pdf = AI Engineer Bengaluru
  - pratik_marudwar.pdf  = AI Engineer non-Bengaluru
  - Pratik_.pdf          = ML/Data Scientist Bengaluru
  - Pratik.pdf           = ML/Data Scientist non-Bengaluru
- `README.md` — usage
- `STATE.md` — this file

## Log
### 2026-08-29
- Brainstormed design with user. Decisions: fully auto-submit; local CSV source;
  infer resume from job title + location; on missing field -> pause & ask user, save to profile.json.
- Created `jobs.csv` with 25 real myworkdayjobs.com listings (10 AI Engineer, 8 ML Engineer, 7 Data Scientist).
- Created `profile.json`, then populated it by reading pratik_marudwar_.pdf: phone, city/state,
  experience (WebMD, Rakuten), education (IIT Patna M.Tech, MIT Pune B.Tech), full skills list, languages.
- Found the 4 resume PDFs already in project root; mapped them in profile.json > resumes.
- Created `STATE.md` and `README.md`.
- Added LinkedIn + GitHub URLs. Restructured address -> profile.json > addresses with TWO location-dependent
  addresses (bengaluru = Bellandur; other = Nanded), picked by job location just like the resume.
- Filled CTC (16.5 LPA current / 27 LPA expected), notice period (60 days), willing_to_relocate (Yes),
  DOB (1998-08-14), earliest start (60 days from offer).
- profile.json now essentially complete. Only optional fields left: portfolio URL, gender voluntary disclosure.
- READY TO APPLY: all inputs present. Next session can run "apply to pending jobs in jobs.csv".
- NOT YET DONE: no applications submitted yet. All 25 rows status=pending.

## KEY FINDING (2026-08-29) — Workday requires account creation
Test run on Takeda Senior AI Engineer revealed the Workday apply flow is a 5-step wizard:
Create Account/Sign In -> My Information -> My Experience -> Application Questions -> Review.
STEP 1 forces creating an account (email + password) BEFORE any form filling.
The agent CANNOT create accounts or enter passwords (hard safety rule). Each company runs
its own separate Workday tenant, so it is a separate account per company.
=> "Fully auto-submit" is NOT possible unattended. Workflow must be:
   USER creates/signs into the account (password step), THEN the agent fills steps 2-5 and submits.
Cookie banner: agent clicks "Decline" (privacy-preserving). It can reappear after locale switch to en-GB.

## SECOND FINDING (2026-08-29) — scraped URLs go stale
Visa Lead AI Engineer (row 2) returned "The page you are looking for doesn't exist" — posting
removed/filled since it was scraped. Marked failed. Expect a fraction of the 25 sample URLs to be dead.
Verify a posting is live before investing in it.

## WORKING CHANNEL = LinkedIn Easy Apply (confirmed 2026-08-29)
User is logged into LinkedIn (Pratik Marudwar, Premium) in Chrome; the browser tools use that session.
Easy Apply needs NO per-company account -> agent CAN complete these end to end.
Filter URL pattern: https://www.linkedin.com/jobs/search/?keywords=<ROLE>&location=<CITY>&f_AL=true
Flow: open job -> Easy Apply -> contact info auto-fills from LinkedIn -> pick resume -> (screening
questions if any) -> Submit application. Some Easy Apply jobs are single-step (no questions).

### FIRST SUCCESSFUL APPLICATION
- Evam Labs "Senior AI Scientist" (Bengaluru, on-site) -> APPLIED 2026-08-29, resume PratikMarudwar_.pdf.

### LinkedIn saved-resume mapping (CONFIRMED by user 2026-08-29)
- AI Engineer, Bengaluru      -> PratikMarudwar_.pdf  (saved on LinkedIn, select it)
- AI Engineer, non-Bengaluru  -> _pratik_marudwar.pdf (saved on LinkedIn, select it)
- ML/DS, Bengaluru            -> Pratik_.pdf          (saved on LinkedIn, select it)
- ML/DS, non-Bengaluru        -> local Pratik.pdf (NOT saved on LinkedIn -> must UPLOAD from
  /Users/pratikmarudwar/Documents/personal_code/job-application/Pratik.pdf)
(LinkedIn keeps upload history, so 5 rows show for 4 resumes; PratikMarudwar.pdf 109KB is an unused older dup.)
NOTE: file_upload to LinkedIn's visible "Upload resume" button failed (ref is a span, not the
<input type=file>). Need to resolve upload for the ML/DS-non-Bengaluru case.

## Test run results (Workday - blocked)
- Takeda Senior AI Engineer -> needs-account (account/password wall)
- Visa Lead AI Engineer -> failed (posting expired)

## RESUME UPLOAD TECHNIQUE (for ML/DS non-Bengaluru = local Pratik.pdf)
LinkedIn's "Upload resume" file input is hidden. To upload a local file:
1. javascript_tool: `document.querySelector('input[type=file][name="file"]')` -> remove 'hidden' class, set display/opacity
2. find "resume file upload input" -> get ref
3. file_upload with local absolute path (working-dir files ARE accepted)
Done once for Pratik.pdf; it is now saved on LinkedIn so future ML/DS-non-Beng jobs can just select it.

## BATCH RESULT — 10 applications submitted 2026-08-29 (all via LinkedIn Easy Apply)
| # | Company | Role | Loc | Resume |
|---|---------|------|-----|--------|
| 1 | Evam Labs | Senior AI Scientist | Beng | PratikMarudwar_.pdf |
| 2 | BTLITC | Anthropic Claude AI Engineer | Remote | _pratik_marudwar.pdf |
| 3 | Quantacus | AI Implementation Engineer | Beng | PratikMarudwar_.pdf |
| 4 | Lean IT Inc. | Machine Learning Engineer | Remote | Pratik.pdf (uploaded) |
| 5 | QuantumQuake | AI Prompt Engineer | Beng | PratikMarudwar_.pdf |
| 6 | ClearDemand | Machine Learning Engineer II | Remote | Pratik.pdf |
| 7 | Dimensionless | Senior Data Scientist | Remote | Pratik.pdf |
| 8 | ColigoMed | AI Engineer | Beng | PratikMarudwar_.pdf |
| 9 | Margn AI | Artificial Intelligence Engineer | Remote | _pratik_marudwar.pdf |
| 10 | Navi | AI Scientist 1 | Beng | PratikMarudwar_.pdf |

Skipped (mismatch/broken): TOCUMULUS (data-eng, broken numeric Location field), Platform Engineer AI Workflows
(backend dev), plus various data-eng/analytics-exec noise in searches.
Common screening answers used: experience=2, notice=60 days, CTC 16.5/27, current location Bengaluru,
willing to relocate=Yes, start immediately=No (60d notice), Claude/MCP built=Yes, Claude cert=No.
Unchecked "Follow company" on each where present.

## WORKDAY PENDING — cannot be automated (2026-08-31)
User asked to apply to all pending (Workday) jobs. Two hard blockers confirmed:
1. Account creation (email+password) required per company — agent not allowed to do this.
2. Workday pages (Rockwell, Convatec) hang 45s+ and never reach document_idle — automation tools
   time out, so the agent cannot even drive them to the account step right now.
=> Delivered `APPLY_WORKDAY_PREP.md`: ready-to-paste field values + per-job resume/address mapping
   so the user can self-apply fast. 19 India-live jobs listed; skip expired Visa + 2 US roles.

## WORKDAY LINK VERIFICATION (2026-08-31) — most are dead
Re-checked every pending Workday link via get_page_text (fresh tab; old tab got stuck).
Result of ~20 checks: 15 EXPIRED ("page doesn't exist"), 2 LIVE but senior-mismatched
(HP Agentic AI = 10+ yrs; Veralto Sr ML = 5+ yrs) AND still need account creation,
3 UNVERIFIED (extension lacks domain permission: condenast x2, comcast).
Takeaway: scraped Workday links go stale within days; even live ones are gated by account creation
the agent cannot do. => Workday route is effectively closed. CSV statuses updated accordingly.

## PAST-WEEK BATCH (2026-08-31) — 3 fresh applications
Filter: keywords AI Engineer, Bengaluru, Easy Apply, Past week (f_TPR=r604800), Entry/Associate (f_E=2,3).
Applied: Control One (Computer Vision Engineer, Beng, 0-2yr GREAT fit), TECEZE (Generative AI Engineer,
Beng, RAG/MCP match), Data Eminence (ML Engineer, Remote, Pratik.pdf). Skipped TELUS (robotics/middleware
mismatch). Running total applied = 13.

### IMPORTANT LinkedIn UI note (learned this session)
- LinkedIn now sometimes opens Easy Apply via a NEW "SDUI" flow (openSDUIApplyFlow=true) rendered in an
  isolated frame that find/read_page/javascript CANNOT introspect, AND a short (625px) window hides its footer.
  This happens when navigating directly to /jobs/view/<id>.
- WORKING METHOD: use the SEARCH SPLIT-VIEW (navigate to /jobs/search/?...), click the job in the left list,
  click its Easy Apply button, then drive the CLASSIC modal via refs: find dialog -> read_page(ref_id=dialog)
  -> click "Continue to next step" refs -> fill via form_input (dropdowns) + javascript native-setter+events
  (text inputs, esp. numeric CTC/notice fields which reject non-numbers) -> "Review" -> "Submit application".
- The modal is off-screen in the short window but ref-based clicks (computer left_click with ref) still work.

## PAST-WEEK BATCH #2 (2026-08-31) — 10 more applications (running total = 20)
Control One, TECEZE, Data Eminence, Lifesight, EdgeVerve, Impetus, Jobgether, Cloud202, Talentgigs, Bandhan.
Skipped: TELUS (robotics/middleware mismatch), TalentQuell (required Bachelor's from IIT = No + asked 10th/12th % not on file).
LESSON: switching resume must be a REAL click (computer left_click on the radio), NOT javascript .click()/setter
— JS check-only leaves LinkedIn's "A resume is required" error and won't advance. After a real click it registers.
Numeric fields (CTC, notice period, sometimes "current location") reject text — use plain numbers.
Missing from profile.json for some forms: 10th/12th class percentages (add if user has them).

## HARVEST (2026-08-31) — 16 fresh LinkedIn jobs added as pending
User asked to FIRST collect jobs (last week, India, 0-3yr = f_E=2,3, AI/ML/DS profiles, Easy Apply) into the CSV,
THEN apply as a separate step. Harvested via LinkedIn search + JS extraction from the virtualized results list
(localStorage accumulator window.__C / __h / __out). AI & ML searches overlap heavily (same promoted cards);
DS search added the distinct Data Scientist roles. Filtered out generic SWE/Python/Backend/aggregator noise.
16 new rows are status=pending, applied=No (companies: Nekko Tech, People Tech, Sony Research, C3iHub IIT Kanpur,
FMR, OATI, Glance, Kuku, Aditi, noon, Applied Data Finance, Taggd, BDO, Crossing Hurdles, Chryselys, Sonatype).
NEXT STEP (per user) = apply to these pending LinkedIn jobs (use the split-view + ref/JS method; real-click to switch resume).
Note: a few are 'Senior'/'II' titled (may want >3yr despite filter) — skip poor fits at apply time.

## NEXT SESSION TODO
- Workday is a dead end here (expired links + account-creation wall). If user still wants those 2 live
  ones (HP Agentic, Veralto), they must self-apply via APPLY_WORKDAY_PREP.md (both are 5-10+ yr roles though).
- BEST PATH for more applications = LinkedIn Easy Apply (works hands-off; 10 already done). Plenty left.
- Grant extension permission for condenast/comcast domains if their liveness must be confirmed.
