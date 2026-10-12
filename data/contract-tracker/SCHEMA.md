# Contract Tracker schema

File: `data/contract-tracker.csv`. Migrated from Notion collection://1e46e33c-848c-4451-96de-373e81219327 on 2026-10-12. One row per line; `Page` is the row key.

| Column | Type | Allowed values | Meaning |
|---|---|---|---|
| Page | title |  | Pharmacy name (row key). |
| Pharmacy | relation | Company names, `; `-separated | Linked Company Profile name(s). |
| Contact | text |  | Main person negotiating the contract. |
| Stage | select | `Awaiting contract send`, `Sent`, `Redlines received`, `Reviewing with Tim`, `Redline sent`, `Signed`, `On hold` | Where the contract is in the redline cycle. |
| Round | number |  | Redline round number (1 = first counter). |
| Contract type | select | `Our agreement`, `Their agreement` | Whose paper: our Pharmacy Platform Agreement or their own. |
| Tag | select | `Redline`, `Base contract`, `Change request` | Kind of document in play. |
| Tab | select | `Idea`, `Project`, `To-do` | Legacy Agent Memory tab bucket; usually Project. |
| Latest file | url |  | Link to the newest contract file (Drive or local path). |
| Email thread | url |  | Gmail thread for the contract back-and-forth. |
| Last activity | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date of the last contract action (YYYY-MM-DD). |
| Notes | text |  | Running notes, newest first. |
| Created time | created_time |  | When the row was created (UTC, read-only history). |
| notion_url | url | | Original Notion page (archive reference, read-only). |

Row notes with page body content live in `data/contract-tracker/<row-slug>.md` (frontmatter = row properties).
