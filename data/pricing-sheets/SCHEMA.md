# Pharmacy Pricing Sheets schema

File: `data/pricing-sheets.csv`. Migrated from Notion collection://427f9359-f112-4f6b-a258-617d2717be8e on 2026-10-12. One row per line; `Name` is the row key.

| Column | Type | Allowed values | Meaning |
|---|---|---|---|
| Name | title |  | File name (one row per file received). |
| Pharmacy | relation | Company names, `; `-separated | Linked Company Profile name(s). |
| Type | select | `Pricing`, `Formulary`, `Pricing + Formulary` | What the file contains. |
| Received | date | `YYYY-MM-DD` or ISO datetime; ranges as `start/end` | Date received. |
| Drive Link | url |  | Copy in the Pharmacy raw prices Drive folder. |
| Email Thread | url |  | Gmail thread it came in on. |
| Source | text |  | Where it came from. |
| Notes | text |  | Notes. |
| notion_url | url | | Original Notion page (archive reference, read-only). |

Row notes with page body content live in `data/pricing-sheets/<row-slug>.md` (frontmatter = row properties).
