# AezTrack

A fast, offline-first web app for tracking IGCSE past paper attempts — with cloud sync, grade threshold lookup, performance analytics, AI-powered revision coaching, a planner, and a full past papers browser.

**Live:** [aeztrack.vercel.app](https://aeztrack.vercel.app)

> **Note:** This repo lacks full commit history as the host website uses a separate private repository. The public repo may not reflect the latest changes.
> Current Version: h4bfyrx08h
> Some features are to be added in the future! (but are still mentioned here)
---

## Features

### Core Tracking
- **Paper Logging** — log marks, time taken, wrong questions, and notes for any past paper
- **Paper Tracker Grid** — visual tracker showing every paper × session × year combination; click any cell to open the QP/MS directly or log marks
- **History & Filtering** — filter logged papers by subject, paper type, session, year range, score, and grade
- **Study Streak** — tracks consecutive days of logging with an animated streak counter
- **Raw Dump** — export all logged papers as a plain `.txt` file grouped by subject

### Analytics & Predicted Grades
- **Dashboard** — overview cards (avg score, streak, best paper, this-month count, exam countdown), sparkline charts per subject, recent papers, and wrong-questions review
- **Performance Tab** — detailed per-subject breakdowns with sparklines
- **Predicted Grade Engine** — exponentially-weighted rolling average (recent papers count more) combined with trend slope and consistency scoring to produce a projected grade and readiness % per subject
- **Next Grade Target** — shows exactly how many marks above your current average are needed to reach the next grade boundary

### AI Performance Analysis
- **Analyse My Performance** — sends aggregated performance data to Gemini 2.0 Flash Lite (free) or OpenAI and returns a structured breakdown: overall assessment, subject-by-subject weaknesses, pattern analysis, top 3 priority actions, and recommended papers
- **Regex Generator** — describe in plain English which papers to assign; AI writes the filter regex for you
- Supports Gemini 2.0 Flash Lite (free tier, no billing) and OpenAI
- One-time billing consent required; API key saved locally

### Grade Thresholds
- **Auto-fetch** — thresholds are pulled from a bulk JSON cache (~420 KB) on first load and stored in IndexedDB; zero API calls for subsequent lookups
- **Grade Calculation** — every logged paper shows its grade (A*–U) once thresholds are available
- **Manual Override** — thresholds can be set or overridden manually per paper

### Past Papers Browser
- Browse question papers, mark schemes, examiner reports, and grade threshold PDFs
- Covers 7 subjects across all sessions (Feb/March variant 2 only, May/June, Oct/Nov) and year ranges
- URL availability pre-checked and cached; 404s surfaced before you click
- Clicking a tracker grid cell shows quick-open buttons for QP and MS alongside the log option

### Planner
- Task manager for revision scheduling with five categories: Exam, Revision, Past Paper, Finals, Other
- Priority levels (Normal / High / Urgent), due dates, subject tagging, estimated time
- Overdue detection and colour-coded date labels
- **Assign Me Work** — randomly selects unlogged papers from your subjects using stratified sampling (no subject bias); filter by year range, session, or regex; results added directly to the planner
- **Topics to Review** — wrong questions from logged papers surfaced as a dismissible checklist at the top of the planner; check off a topic to permanently hide it
- **Daily Cleanup** — runs once per day at boot; removes completed tasks older than 7 days and stale no-due-date tasks older than 14 days
- Dashboard banner shows upcoming tasks as a horizontal scroll strip

### Cloud Sync
- Passphrase-based sync across devices via the AezTrack API — no account required
- Supabase Realtime — data updates pushed to other open tabs/devices instantly
- SHA-256 passphrase hashing — no email, name, or PII stored server-side
- **Deletion tombstone** — when a save is deleted, a hash of the passphrase is stored locally; any device that tries to load a deleted save is blocked with an explanation

### Settings & Personalisation
- **Profile** — display name and avatar photo (stored locally only)
- **Accent colour** — fully custom colour picker with preset swatches, a hue slider, and a hex input; updates the entire UI instantly
- **Theme** — dark / light mode, persisted across sessions
- **Analyse My Performance** — accessible from Settings; AI-powered revision coach
- **Reset Local Data** — wipes all IndexedDB stores and localStorage from the current device; cloud saves unaffected

### Storage & Portability
- **IndexedDB** — primary storage engine; automatically migrates existing `localStorage` data on first load
- **Import / Export** — AES-encrypted `.aez` backup files
- **Raw Dump** — plain `.txt` export of all papers grouped by subject with scores, grades, times, wrong questions, and notes
- **PWA** — installable on desktop and mobile, works offline after first load

### Navigation
- **SPA Routing** — clean URL paths (`/history`, `/papers`, `/planner`, etc.) via the History API with `vercel.json` rewrite; browser back/forward navigate correctly
- **Mobile bottom nav** — symmetric 2 + FAB + 2 layout; Planner accessible from the top header bar

---

## Supported Subjects

| Subject | Code |
|---|---|
| Mathematics | 0580 |
| Physics | 0625 |
| Chemistry | 0620 |
| Biology | 0610 |
| Computer Science | 0478 |
| English (ESL) | 0510 |
| Accounts | 0452 |

Custom subjects can be added with any subject code and year range.

For a guide on adding custom subjects (IGCSE / O Level / A Level), see [CustomSubject.md](https://github.com/Aezdek/AezTrack/blob/main/CustomSubject.md).

---

## Tech Stack

### Frontend
- Vanilla HTML / CSS / JS — zero build step, zero framework
- Six JS modules: `data.js`, `sync.js`, `app.js`, `router.js`, `views.js`, `ui.js`
- **CryptoJS** — AES encryption for `.aez` exports and SHA-256 passphrase hashing
- **Supabase JS** — Realtime subscriptions for live cross-device sync
- **Font Awesome 6** — icons
- **IndexedDB** — structured local storage with four object stores (`papers`, `meta`, `planner`, `thresholds`)

### Backend ([aeztrack-api](https://github.com/Aezdek/)) *(not public)*
- Vercel serverless functions (Node.js)
- Supabase Postgres for cloud saves
- Grade threshold scraping and bulk JSON generation

---

## Cloud Sync

Cloud sync is optional. Data lives in IndexedDB locally by default.

To enable sync:

1. Click **Cloud Sync** in the sidebar or Log drawer
2. Upload your data — you'll receive a passphrase (e.g. `word-word-word-...`)
3. Use the same passphrase on any other device to restore your data

Realtime sync keeps open tabs in sync automatically once a passphrase is set.

> Saves do not expire. Your passphrase is the only identifier — there are no user accounts.

---

## Threshold Caching

On first load, AezTrack downloads a bulk JSON file (~420 KB) containing all known grade thresholds and caches it to IndexedDB. A hash endpoint (`/api/hash`) is checked on subsequent loads — the bulk data is only re-downloaded if the hash changes. This means **threshold lookups are instant and offline** after the first visit, with zero per-paper API calls.

---

## Data & Privacy

- All data is stored locally in IndexedDB by default
- Cloud saves are keyed by a SHA-256 hash of your passphrase — no email, name, or account required
- Profile pictures and display names are stored locally only and never synced to the cloud
- `.aez` export files are AES-encrypted
- AI analysis requires explicit consent before any data leaves the device; only aggregated performance data (scores, subjects, wrong questions) is sent — no names, passphrases, or PII
- No analytics, no ads, no trackers

---

## License
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

Copyright (c) 2026 Aezdek

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
