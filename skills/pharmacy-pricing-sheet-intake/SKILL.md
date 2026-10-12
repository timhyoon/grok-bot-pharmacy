---
name: Pharmacy pricing sheet intake
description: >-
  Use when checking for, filing, or logging pricing sheets, price lists,
  formularies or catalogs that pharmacies (503A/503B) email to AffinityRx, or
  when a scheduled routine runs pharmacy pricing intake. It finds new sheets in
  the three inboxes, copies them into the Pharmacy raw prices Drive folder
  under Tim's naming convention, logs one Notion row per file, and reports the
  new links to Tim. It is safe to re-run.
---
# Pharmacy pricing sheet intake

Read-only on email. Never reply, send, trash, label or move anything in Gmail. Never edit, rename, move or delete files already in Drive. You only add new copies and new Notion rows.

## Where things live
- Drive folder: **Pharmacy raw prices**, id `1X-h1GEKmA40pM0i0begthrdX35XeZz6K`, owned by timyoonco@gmail.com. It has an **Archive** subfolder, id `1UXbRtHJaDfCka2pF3raJVuNGAHvoCEl1`, where Tim keeps superseded versions.
- Notion: **Pharmacy Pricing Sheets** database (💰, Pharmacy tab of Agent Memory; Tim moved it there 2026-10-07), https://app.notion.com/p/69097d0966504d72a9ec7bcc9a4b99f8, data source `collection://427f9359-f112-4f6b-a258-617d2717be8e`. Properties:
  - Name (title, = Drive file name)
  - Pharmacy (relation to Company Profiles)
  - Type: Pricing | Formulary | Pricing + Formulary
  - Received (date, PT)
  - Source: "Sender Name <email> to <inbox> (orig. <original filename>)"
  - Email Thread (url)
  - Drive Link (url)
  - Notes
- Company Profiles: `collection://f01170d7-e99a-4bd6-ad71-f460252908fc`. The Type is Pharmacy/503A or 503B for pharmacies.
- **Pharmacy Contacts** (📇, Pharmacy tab, owned by the Pharmacy bot since 2026-10-08): `collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd`. Every 503A/503B pharmacy contact lives here; Setter Contacts no longer holds pharmacies.
- Inboxes, via Composio session `fall`. Pass `account=`:
  - tim@affinityrx.com = `gmail_ammer-mot`
  - timyoonco@gmail.com = `timyoonco` (most older pharmacy threads)
  - support@joinaffinityai.com = `gmail_molal-obol` (most new pharmacy leads; plus-addresses like tim+503a@ land here)
- Composio Google Drive is connected as timyoonco@gmail.com, the folder owner.

## Access order
1. Composio first. In COMPOSIO_REMOTE_WORKBENCH, `run_composio_tool(slug, args, account=...)` works per inbox.
2. Then the native connectors (user-Gmail--support, user-Gmail, user-Gmail--tim-affinityrx-com, user-Google-drive, user-Notion).
3. Never sign in to Google in a browser.
- Big Composio calls can time out. Keep each call small:
  - list with `ids_only`
  - hydrate with a thread pool
  - avoid `verbose=True` on GMAIL_FETCH_EMAILS for 100+ messages

## 1. Find candidates
For each inbox, run GMAIL_FETCH_EMAILS (`ids_only: true`, `max_results: 500`, paginate) with:
- `newer_than:14d -from:me has:attachment (filename:xlsx OR filename:xls OR filename:csv OR filename:pdf OR filename:numbers)`. Routine runs use 14d; use 60–70d for a backfill.
- Also check Google Drive or Sheets links in the body: `newer_than:14d -from:me (has:drive OR has:spreadsheet OR "docs.google.com" OR "drive.google.com" OR dropbox) (pricing OR price OR formulary OR catalog OR menu)`

Then hydrate each message with GMAIL_FETCH_MESSAGE_BY_MESSAGE_ID (`format: full`). Collect sender, subject, timestamp, threadId and attachments by walking `payload.parts` recursively and keeping parts with `filename` + `body.attachmentId`. For body links, regex `docs.google.com/spreadsheets/d/<id>` and `drive.google.com/file/d/<id>` from the inbound message's own text. Ignore quoted replies, and skip any link that also appears in a message we sent.

### Keep a file only when all of these hold
- **The sender is a pharmacy.** Match the sender's domain or company to a Company Profiles row with Type Pharmacy/503A or 503B, or to a row in Pharmacy Contacts (`collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd`).
  - If there's no match, judge from the signature (e.g. "Compounding Pharmacy", "503B Outsourcing").
  - Flag unknown senders to Tim rather than guessing.
  - Clinics, telehealth companies, suppliers, labs, GPOs and research-peptide vendors don't count. Example: a clinic sending a "Pharmacy vs Affinity" price comparison.
  - One exception: a pharmacy contact writing from a personal Gmail still counts once the match is clear. Example: Vault's medical director, medicaldirector62@gmail.com.
- **The file is pricing or a formulary.** Look for words like price list, pricing, formulary, catalog, product list, menu, drug list or proposal in the filename, subject or body. Open the file to confirm it has products, and prices if you call it Pricing.
  - Not these: invoices, receipts, contracts/MSAs/BAAs/NDAs, onboarding or new-account forms, licenses, COAs, SOPs, API docs, SLAs, brochures for equipment.
- **It isn't ours.** Never take AffinityRx's own sheets, such as `1ARrW1cPZjqJ_o3Lx-bx8hTuizc4JqHhdeKdkjpGquag` (pricing sheet we send), or any file only we sent.
  - A pharmacy returning OUR template filled with THEIR prices does count (e.g. "Affinity.xlsx", "new affinity.xlsx").

## 2. Dedupe (idempotent, check before every upload)
Skip a candidate if any of these is true:
1. A Notion row in Pharmacy Pricing Sheets has an **Email Thread** containing the threadId AND a **Source** containing `orig. <original filename>`.
2. A file in the folder or in Archive has the same **md5Checksum** as the attachment. Use GOOGLEDRIVE_FIND_FILE with `q: "trashed = false and ('1X-h1GEKmA40pM0i0begthrdX35XeZz6K' in parents or '1UXbRtHJaDfCka2pF3raJVuNGAHvoCEl1' in parents)"` and `fields: files(id,name,md5Checksum,size)`, then md5 the downloaded bytes.
   - Native Google Sheets have no md5. For a linked Sheet, compare its file id against the Drive Link of the Notion rows.
3. The same attachment (same filename + md5) already went through earlier in this run. Pharmacies often re-attach the same file down a thread, so log only the first message that carried it.

If the file is already in the folder but has no Notion row, add the row only, pointing at the existing file. Note "Already in folder (identical file)".

## 3. Copy into the folder
- **Attachments:** download with GMAIL_GET_ATTACHMENT (message_id, attachment_id, file_name; returns `file.s3url`). Save the bytes in the workbench, then use `upload_local_file(path)` to get an s3key.
  - For files ≤5 MB, upload with GOOGLEDRIVE_UPLOAD_FILE (`file_to_upload: {name, mimetype, s3key}`, `folder_to_upload_to: FOLDER`).
  - For bigger files, use GOOGLEDRIVE_RESUMABLE_UPLOAD (same file_to_upload, plus `metadata: {name}`, `folder_to_upload_to`, chunkSize as a multiple of 262144).
  - Keep the original format: xlsx stays xlsx and pdf stays pdf. Don't convert to Google Sheets.
- **Linked Google Sheets or Drive files the pharmacy owns:** copy them into the folder with the conventional name (GOOGLEDRIVE_COPY_FILE_ADVANCED or native copy_file with parentId). Never move or rename the original.
- **Verify:** list the folder again and confirm each new file's name, size, md5 and parent.

## Naming convention (Tim's, inferred from the folder)
- The name is the pharmacy's short brand name plus the original extension. No dates, no "pricing", no sender names. Existing examples:
  - `Strive.xlsx`, `Emerald.pdf`, `Vault.xlsx`, `Restorative.xlsx`, `Valiant.xlsx`, `HealthWealth.xlsx`, `Empower.pdf`, `Epiq.pdf`, `ProCompounding.xlsx`
  - Native Sheets and PDFs with no extension: `Perfect Rx`, `Pharmacy Hub`, `RxRoute`, `Striker`
- Drop generic words like "Pharmacy", "Compounding Pharmacy", "Pharmaceuticals" or "LLC", unless the result would be unclear ("Hybrid Pharma", "Pharmacy Hub").
- When a pharmacy has more than one file, add one short word after the name:
  - `Formulary` for a product list without prices
  - `Catalog` for a full catalog that has prices
  - otherwise a short descriptor from the file (`Promise All`, `Promise GLP`, `Promise Combined`)
- Names already used, so a new file from the same pharmacy needs a descriptor:
  - Perfect Rx (+ Perfect Rx Formulary.pdf), Promise All / GLP / Combined, Vault, Pharmacy Hub, Strive, ProCompounding, Striker, RxRoute, Emerald, Restorative, Valiant, HealthWealth, Empower, Epiq
  - National Health Rx (+ Catalog), NGCPRx, Hybrid Pharma, AnazaoHealth, Olympia (+ Formulary), HEWA (+ Formulary), Pure Path (+ Formulary), BoomRx, Center City, BNDS
- **Updated versions:** when a pharmacy sends a newer version of a file that's already there, upload it as `<Name> <Mon YYYY>.<ext>` (e.g. `HEWA Nov 2026.xlsx`). Leave the old file alone and tell Tim it can go to Archive. Never move it yourself.

## 4. Log in Notion
Make one row per new file in the Pharmacy Pricing Sheets data source (`notion-create-pages`, parent `data_source_id: 427f9359-f112-4f6b-a258-617d2717be8e`).
- **Name** = Drive file name.
- **Pharmacy** = the Company Profiles page URL. If there's no profile, leave it empty and say so in Notes.
- **Type:** Pricing (has prices), Formulary (no prices), or Pricing + Formulary (full product list or catalog with prices).
- **Received** = the email's date in PT (`date:Received:start`, `is_datetime: 0`).
- **Source** = `Sender Name <email> to <inbox short> (orig. <original filename>)`.
- **Email Thread** URL:
  - support: `https://mail.google.com/mail/?authuser=support@joinaffinityai.com#all/<threadId>`
  - timyoonco: `https://mail.google.com/mail/?authuser=timyoonco@gmail.com#all/<threadId>`
  - tim@: `https://mail.google.com/mail/?authuser=tim@affinityrx.com#all/<threadId>`
- **Drive Link:**
  - Office/PDF: `https://drive.google.com/file/d/<id>/view`
  - xlsx or Sheets: `https://docs.google.com/spreadsheets/d/<id>/edit`
- **Notes** = one line on what's inside (tiers, all-in-one shipping, no prices, damaged file, declined partner, etc.).

Company Profiles has no pricing-sheet property, so don't edit profiles. The relation on the row is the link.

## 5. Report to Tim
- When there are new files, send one short message listing each one as pharmacy, type, received date (PT) and the Drive link. Include the Notion database link and any flags:
  - unknown sender
  - file you couldn't open
  - an updated version that may supersede an older file
  - a non-pharmacy sheet you skipped
- When nothing is new, follow the routine's quiet rule. Default: say nothing.
- Never send email to the pharmacy as part of this skill.
