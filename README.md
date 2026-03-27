# AezTrack

A lightweight, single-file web app for tracking IGCSE past paper attempts - with cloud sync, performance analytics, and a built-in past papers browser.

**Live:** [aeztrack.vercel.app](https://aeztrack.vercel.app)

---

## Features

- **Performance Overview** — per-subject average scores at a glance
- **Past Papers Browser** — browse question papers, mark schemes, examiner reports and grade thresholds for 7 subjects across all sessions and year ranges
- **Paper Logging** — log marks, time taken, wrong questions, and notes for any past paper
- **History & Filtering** — filter logged papers by subject, paper type, and score range
- **Cloud Sync** — passphrase-based sync across devices via the AezTrack API; no account required
- **Import / Export** — AES-encrypted `.aez` backup files
- **Dark Mode** — persisted across sessions

---

## Supported Subjects

| Subject | Code |
|---|---|
| Mathematics | 0580 |
| Physics | 0625 |
| Chemistry | 0620 |
| Biology | 0610 |
| Computer Science | 0478 |
| English | 0500 |
| Accounts | 0452 |

---

### Please note Ability to add other subjects will be added soon.

## Tech Stack

**Frontend**
- Vanilla HTML/CSS/JS — zero build step, single file
- CryptoJS — AES encryption for `.aez` exports and SHA-256 passphrase hashing
- Font Awesome — icons

**Backend** ([aeztrackapi](https://github.com/Aezdek/)) (Please note this backend is not public)
- Vercel Serverless Functions
- Supabase — cloud storage with Realtime sync
- SHA-256 passphrase auth — no user accounts or PII stored
- Rate limiting 

---

## Cloud Sync

Cloud sync is optional. Data is stored locally in `localStorage` by default.

To enable sync:
1. Click **Cloud Sync** in the Log tab
2. Upload your data — you'll receive a passphrase (e.g. `word-word-word-...`)
3. Use the same passphrase on any other device to pull your data

> Saves do not expire. There are no user accounts - your passphrase is the only identifier.

---

## Data & Privacy

- All data is stored locally in your browser by default
- Cloud saves are keyed by a SHA-256 hash of your passphrase — no email, name, or account required
- `.aez` export files are AES-encrypted

---

## License

Proprietary / All Rights Reserved

Copyright © 2026 Aezdek.

This source code is made available for portfolio review and educational purposes only. No part of this project (AezTrack) may be copied, modified, redistributed, or used for commercial purposes without explicit written permission from the author.
