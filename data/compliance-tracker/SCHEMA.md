# Pharmacy Compliance Tracker schema

File: `data/compliance-tracker.csv`. Migrated from Notion collection://9dca4cc0-e467-4a7c-9d2b-03216221f29e on 2026-10-12. One row per line; `Pharmacy` is the row key.

| Column | Type | Allowed values | Meaning |
|---|---|---|---|
| Pharmacy | title |  | Pharmacy name (row key). |
| Primary Contact | text |  | Main contact name. |
| Contact Email | email |  | Main contact email. |
| Status | select | `In progress`, `Live`, `Stalled`, `On hold` | Overall onboarding status. |
| Contract | select | `Not signed`, `Updating`, `Signed` | Contract signature status. |
| Compliance Form | select | `Not Sent`, `Sent`, `Done` | Compliance form status. |
| API Type | select | `Pharmetika`, `LifeFile`, `Custom` | Pharmacy management system / API. |
| API Progress | select | `Receiving`, `To integrate`, `Integrated` | Integration progress. |
| Email Thread | url |  | Gmail thread URL. |
| notion_url | url | | Original Notion page (archive reference, read-only). |

Row notes with page body content live in `data/compliance-tracker/<row-slug>.md` (frontmatter = row properties).
