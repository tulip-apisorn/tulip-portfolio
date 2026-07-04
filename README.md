# Document Cabinet — Prototype

This project starts from a real problem: one adult's important documents are
scattered across both physical storage (folders/cabinets at home) and digital
storage (Google Drive, iCloud, iPhone Files, OneDrive — all separate, no
central index). When it's actually time to use them (termite service renewal,
mortgage/refinance, insurance, etc.), they're either impossible to find or the
renewal deadline sneaks up too late.

## Problem statement

- Important documents are scattered across multiple places, both physical and
  several different cloud providers.
- There's no single place that tells you what document is where, or what's
  coming up for renewal.
- Physical documents are the hardest to find, because there's no record of
  where they're actually stored.
- There's no advance reminder, so renewal/payment deadlines get missed
  regularly.

## Solution / Vision

The long-term flow we want:

1. Save the file/photo of the document in one place (wherever is already
   habitual — no need to change behavior).
2. An agent watches the log and records it in a central sheet (document name,
   type, due date, real file link, or storage location if physical).
3. Automatically create a due-date event in the calendar with the document
   link attached, so reminders go out ahead of time.
4. For physical documents, clearly state where it's stored, e.g. "2nd floor,
   green filing cabinet."

Budget is limited — the goal is to build this in-house. We'll decide later
whether to schedule the checking/reminding with a Claude Code routine or
another kind of cron (e.g. Codex). Right now the focus is proving the
workflow actually works.

## Current prototype (`index.html`)

A single static HTML/JS file — open it directly in a browser, no build step,
no server. Data is stored in the browser's `localStorage`, simulating a
"central sheet" as quickly as possible, before wiring up a real backend.

Simulates the full workflow:

- Summary dashboard: total / due soon / overdue / stored physically
- Document list sorted by due date, soonest first, with an urgency color
  stripe
- Each item shows its real storage location: a direct link if it's cloud, or
  the storage location (shelf/cabinet/color, etc.) if it's physical
- Add / edit / delete / mark as "renewed"
- Export/Import as JSON (simulating a future migration to a real Google
  Sheet)

What's *not* done in this version (intentionally cut to keep the prototype
fast): no real Google Sheet/Calendar/Drive integration yet, no agent reading
logs automatically yet, no real notifications yet (LINE/email) — all of that
is the next phase if the workflow holds up.

## Data model

| field | meaning |
|---|---|
| `name` | Item name, e.g. "Termite treatment renewal" |
| `category` | Category (termite, mortgage/refinance, insurance, other) |
| `dueDate` | Due date |
| `reminderDaysBefore` | Days of advance notice |
| `storageType` | `cloud` or `physical` |
| `cloudLink` | Real document link (if `storageType = cloud`) |
| `physicalLocation` | Storage location, e.g. "2nd floor, green filing cabinet" (if `storageType = physical`) |
| `note` | Free-form note |

## Feasibility — low-cost paths forward

Roughly in the order they're worth doing — not everything needs to happen:

1. **A real Google Sheet as the source of truth** instead of localStorage —
   use Google Apps Script as a free Web App. Apps Script already has a
   Calendar service built in, so one script can create Google Calendar
   events straight from sheet rows, no separate API key needed.
2. **Real reminders** via LINE Notify/LINE OA or a Telegram bot (free, and
   more practical than email for a Thai user) fired from a daily Apps Script
   trigger.
3. **Scheduling on the agent side**: use a Claude Code scheduled routine
   (this environment already has a tool for creating cron triggers) or
   another kind of cron (Codex, GitHub Actions cron — free for repos, etc.)
   to check due dates daily and update the sheet/send reminders — pick
   whichever fits later, it's not tied to this prototype.
4. **Real document links from Drive/iCloud** — pasting links manually is
   fine for now, since monthly document volume is low; not yet worth wiring
   up an API to fetch files automatically.
5. **OCR to read expiry dates from document photos automatically** (e.g.
   have Claude vision read a receipt/policy and fill it in) — a stretch
   goal saved for last, since volume is low enough that manual entry is
   still cheaper than building a pipeline for it.

Key point: nearly every option above fits comfortably in a free tier —
budget isn't the bottleneck. The real bottleneck is the time to write the
automation and the habit of actually logging documents consistently.

## Next steps

1. Try the prototype with 5-10 real documents and see if the fields cover
   what's needed.
2. Decide whether the source of truth becomes a Google Sheet or stays local.
3. Pick a scheduling approach for checks/reminders (Claude Code routine vs.
   another cron).
4. Wire up real Calendar + reminders as the next phase.
