# Job Application Agent

Semi-automated job applications driven by Claude Code + Chrome browser automation.

## What it does
1. Reads job URLs from `jobs.csv`.
2. For each `pending` row: opens the URL in Chrome, infers the right resume
   (AI vs ML/DS × Bengaluru vs other), fills the form from `profile.json`,
   uploads the resume, and submits.
3. If a form asks for something not in `profile.json`, it pauses, asks you,
   and saves your answer back into `profile.json` for next time.
4. Marks the row `applied` (with timestamp + which resume) in `jobs.csv`.

## Setup
1. The 4 resume PDFs are already in the project root and mapped in `profile.json > resumes`:
   - `pratik_marudwar_.pdf`  -> AI Engineer, Bengaluru
   - `pratik_marudwar.pdf`   -> AI Engineer, non-Bengaluru
   - `Pratik_.pdf`           -> ML / Data Scientist, Bengaluru
   - `Pratik.pdf`            -> ML / Data Scientist, non-Bengaluru
2. Fill the remaining `FILL_ME` fields in `profile.json` (LinkedIn/GitHub URLs, address, CTC, notice period, relocation).
3. Review/replace the sample rows in `jobs.csv` with your real job links.

## Running it
In a Claude Code session, ask the agent to "apply to the pending jobs in jobs.csv".
The agent drives your Chrome live during the session (it is not an unattended daemon).

## Files
| File | Purpose |
|------|---------|
| `jobs.csv` | Job links + status tracking |
| `profile.json` | Your details, Workday-structured |
| `*.pdf` (root) | Your 4 resume PDFs |
| `STATE.md` | Running history log for session continuity |

## Important notes
- **Chrome is driven live during a session** — not a background service.
- Some jobs have logins / CAPTCHAs / bot-blocking; those get marked `failed` with a note.
- US-based roles in the sample need work authorization you may not have — check the `notes` column.
- Resume inference is auditable: the chosen resume is recorded per row.
