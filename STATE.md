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

## Applications submitted
(none yet)
