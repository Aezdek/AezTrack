# AezTrack

A fast, offline-first web app for tracking IGCSE past paper attempts — with cloud sync, grade threshold lookup, performance analytics, a revision planner, and a full past papers browser.

**Live:** [aeztrack.vercel.app](https://aeztrack.vercel.app)

> **Note:** This repo lacks full commit history as the host website uses a separate private repository. The public repo may not reflect the latest changes.
> Current Version: h4bfyrx08h

---

## Features

### Core Tracking
- **Paper Logging** — log marks, time taken, wrong questions, and notes for any past paper
- **Paper Tracker Grid** — visual tracker showing every paper × session × year combination; click any cell to log or view
- **History & Filtering** — filter logged papers by subject, paper type, session, year range, score, and grade
- **Study Streak** — tracks consecutive days of logging with an animated streak counter

### Analytics
- **Dashboard** — overview cards (avg score, streak, best paper, this-month count, exam countdown), sparkline charts per subject, recent papers, and wrong-questions review
- **Performance Tab** — detailed per-subject breakdowns with sparklines

### Grade Thresholds
- **Auto-fetch** — thresholds are pulled from a bulk JSON cache (~420 KB) on first load and stored in IndexedDB; zero API calls for subsequent lookups
- **Grade Calculation** — every logged paper shows its grade (A*–U) once thresholds are available
- **Manual Override** — thresholds can be set or overridden manually per paper

### Past Papers Browser
- Browse question papers, mark schemes, examiner reports, and grade threshold PDFs
- Covers 7 subjects across all sessions (Feb/March, May/June, Oct/Nov) and year ranges
- URL availability pre-checked and cached; 404s surfaced before you click

### Planner
- Task manager for revision scheduling with five categories: Exam, Revision, Past Paper, Finals, Other
- Priority levels (Normal / High / Urgent), due dates, subject tagging, estimated time
- Overdue detection and colour-coded date labels
- Dashboard banner shows upcoming tasks as a horizontal scroll strip

### Cloud Sync
- Passphrase-based sync across devices via the AezTrack API — no account required
- Supabase Realtime — data updates pushed to other open tabs/devices instantly
- SHA-256 passphrase hashing — no email, name, or PII stored server-side

### Settings & Personalisation
- **Profile** — display name and avatar photo (stored locally only)
- **Accent colour** — pick any colour; updates the entire UI instantly via CSS custom property injection
- **Theme** — dark / light mode, persisted across sessions

### Storage & Portability
- **IndexedDB** — primary storage engine; automatically migrates existing `localStorage` data on first load
- **Import / Export** — AES-encrypted `.aez` backup files
- **PWA** — installable on desktop and mobile, works offline after first load

### Navigation
- **SPA Routing** — clean URL paths (`/history`, `/papers`, `/planner`, etc.) via the History API; 
- Browser back/forward navigate between views correctly

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
- No analytics, no ads, no trackers

---

## License

**Proprietary / All Rights Reserved**

Copyright © 2026 Aezdek.

This source code is made available for portfolio review and educational purposes only. No part of this project may be copied, modified, redistributed, or used for commercial purposes without explicit written permission from the author.
