# AffinityRx Pharmacy SOPs

Maintained by the Pharmacy bot. Updated when Tim asks, plus one end-of-day pass.

| File | What it covers |
|---|---|
| [rules.md](rules.md) | Standing pharmacy rules: PMS/API, pricing, email style, approvals, holds, routines |
| [skills/contract-redline-cycle](skills/contract-redline-cycle/SKILL.md) | Contract redline loop |
| [skills/post-call-recap-email](skills/post-call-recap-email/SKILL.md) | Post-call recap and pharmacy onboarding email |
| [skills/pharmacy-pricing-sheet-intake](skills/pharmacy-pricing-sheet-intake/SKILL.md) | Logging pharmacy pricing and formulary sheets |
| [skills/clean-email-links](skills/clean-email-links/SKILL.md) | Link hygiene for every email |
| [skills/support-inbox-calendly-setter](skills/support-inbox-calendly-setter/SKILL.md) | Inbox rules relevant to pharmacy (excerpt) |
| [skills/setter-morning-follow-ups](skills/setter-morning-follow-ups/SKILL.md) | Pharmacy follow-up templates and compliance check (excerpt) |
| [notion/agent-memory-pharmacy-tab.md](notion/agent-memory-pharmacy-tab.md) | Notion Pharmacy-tab page text (archive) |
| [data/](data/) | Pharmacy databases (source of truth, replaces Notion) |

## Data folder

The four Pharmacy databases moved here from Notion on 2026-10-12. GitHub is now the source of truth; Notion copies are archive only.

| File | What it holds | Schema |
|---|---|---|
| [data/pharmacy-contacts.csv](data/pharmacy-contacts.csv) | One row per pharmacy contact: stage, threads, call dates, action items, follow-ups | [SCHEMA](data/pharmacy-contacts/SCHEMA.md) |
| [data/contract-tracker.csv](data/contract-tracker.csv) | One row per pharmacy contract negotiation | [SCHEMA](data/contract-tracker/SCHEMA.md) |
| [data/compliance-tracker.csv](data/compliance-tracker.csv) | One row per pharmacy: contract, compliance form, API status | [SCHEMA](data/compliance-tracker/SCHEMA.md) |
| [data/pricing-sheets.csv](data/pricing-sheets.csv) | One row per pricing/formulary file received | [SCHEMA](data/pricing-sheets/SCHEMA.md) |

Rows with longer notes also have `data/<db>/<row-slug>.md` (frontmatter = the row's columns, then the notes). Relations are stored as plain names (Company Profile names). `notion_url` points to the archived Notion page.

### Rules for bots editing data

1. One row per pharmacy (per contact in pharmacy-contacts, per file in pricing-sheets). Search for an existing row before adding one; never duplicate.
2. Update in place. Change the cells that changed; do not delete rows (set Stage/Status to `Declined`, `Stalled` or `On hold` instead).
3. Use only the allowed values listed in each SCHEMA.md. Dates are `YYYY-MM-DD`.
4. Notes: add new entries at the front, prefixed with the date (PT). Keep the CSV and any matching .md frontmatter in sync.
5. Keep valid CSV (quote cells containing commas, quotes or newlines). Edit with a CSV library, not string replace.
6. `git pull` before editing; one commit per change with the message `update <pharmacy>: <what>` (e.g. `update Next Gen: contract Signed`), then push.
