# Pharmacy Contacts schema

File: `data/pharmacy-contacts.csv`. Migrated from Notion collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd on 2026-10-12. One row per line; `Name` is the row key.

| Column | Type | Allowed values | Meaning |
|---|---|---|---|
| Name | title |  | Person name (row key; one row per contact). |
| Company | relation | Company names, `; `-separated | Linked Company Profile name(s) = the pharmacy. |
| Role | text |  | Their title. |
| Email | email |  | Their email. |
| Phone | phone_number |  | Their phone. |
| LinkedIn | url |  | LinkedIn URL. |
| Type | select | `Pharmacy/503A`, `503B`, `Clinic/Telehealth`, `Supplier`, `Partner`, `Other` | Kind of organization. |
| Stage | select | `Lead`, `Calendly Sent`, `Booked`, `Call Done`, `Onboarding`, `Live`, `Stalled`, `Declined` | Pipeline stage. |
| Mailbox | select | `support@`, `timyoonco@` | Which inbox the thread lives in (legacy; after Oct 12 replies go from tim@affinityrx.com). |
| Time Zone | select | `ET`, `CT`, `MT`, `PT`, `Other` | Their time zone. |
| Source | text |  | How they came in. |
| Background | text |  | Who they are / company context. |
| Notes | text |  | Running notes, newest first. |
| Their Action Items | text |  | Open items they owe us. |
| Tim Action Items | text |  | Open items Tim owes them. |
| Last Email Thread | url |  | Most recent Gmail thread URL. |
| Last Contact | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date of last touch. |
| Last Call | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date of last call. |
| Next Call | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Scheduled next call. |
| Next Follow-up | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date the next follow-up is due. |
| FU Hotlist | checkbox | `true`, `false` | true = on the priority follow-up list. |
| Last Text | text |  | Last SMS content. |
| Last Text Date | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date of last SMS. |
| Text notes | text |  | SMS notes. |
| notion_url | url | | Original Notion page (archive reference, read-only). |

Dropped: `Reply From` (Notion formula; its value is not exportable). Its meaning: mailbox for all sends/replies after the Oct 12 move to tim@affinityrx.com.

Row notes with page body content live in `data/pharmacy-contacts/<row-slug>.md` (frontmatter = row properties).
