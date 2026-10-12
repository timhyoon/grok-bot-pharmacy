---
name: support-inbox-calendly-setter
description: >-
  Use for Setter's Inbox replies routine: triage and answer new inbound BD email
  (suppliers, clinics, telehealth platforms, partners) in support@, timyoonco@
  and tim@affinityrx.com, send the right Calendly, hand pharmacies to the
  Pharmacy bot, and escalate anything needing Tim without waiting.
---
# Inbox replies (Setter)

> Pharmacy-relevant excerpt: scope/handoff, mailboxes, Calendly rule, hard rules, run-size guardrails. Setter-only triage, CRM and approved-answer sections removed.

## 1. Purpose and scope
Setter answers BD threads from suppliers, clinics, telehealth platforms, partners and other BD leads in the three mailboxes below. **Pharmacies belong to the Pharmacy bot (Tim, 2026-10-08; hard rule):** Inbox replies and Morning follow-ups never reply to or nudge a pharmacy (503A or 503B). That covers anyone in Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd), any pharmacy in /workspace/pharmacy-postcall/state.json, and any thread where the sender is a pharmacy or writes from a pharmacy domain (matched in Pharmacy Contacts or Company Profiles collection://f01170d7-e99a-4bd6-ad71-f460252908fc, or clearly a pharmacy from the signature, e.g. "Compounding Pharmacy", "503B Outsourcing"), even when they aren't in any database yet. Setter never creates, updates, emails, nudges or texts anyone there. If a pharmacy thread slips through (it reached Setter, is unanswered, or needs action), send no email: only message the Pharmacy bot (agent id 6d0c3824-3840-4d2e-b714-a990356010a7) with the pharmacy, the contact, the mailbox, the thread link and what they asked. Classify a sender by email and domain in Pharmacy Contacts first, then Setter Contacts, then Supplier Contacts, then the company's Type in Company Profiles (Pharmacy/503A/503B = pharmacy).
Morning follow-ups: see sand-workflow:setter-morning-follow-ups

## 2. Mailboxes and sending
| Mailbox | Composio account |
|---|---|
| support@joinaffinityai.com | gmail_molal-obol |
| timyoonco@gmail.com | timyoonco |
| tim@affinityrx.com | gmail_ammer-mot |

Reply from the mailbox that received the email. Mail to tim@ often also lands in support@: handle each thread once. Send only with Composio GMAIL_REPLY_TO_THREAD or GMAIL_SEND_EMAIL with is_html (htmlBody); never the built-in Gmail send tools (they rewrite links). Delete any stale draft on the thread before replying, because Composio quotes it.

**Which Calendly (hard rule, Tim 2026-10-07).** Already had a call (Granola note exists, or the row has Last Call set or Stage = Call Done / Onboarding / Live; check Gmail/Calendar if unclear) → only https://calendly.com/timhyoon/meet-with-tim-yoon; never affinity-clinic-telehealth-demo, affinity-pharmacy-demo or follow-up-with-tim-yoon-ceo-affinityrx. No call yet → clinic / telehealth: https://calendly.com/timhyoon/affinity-clinic-telehealth-demo; supplier / other BD: https://calendly.com/timhyoon/meet-with-tim-yoon (the pharmacy demo link is the Pharmacy bot's). After a call, include a link only when a call is offered, they ask, or booking was their commitment. Rule row: https://app.notion.com/p/3f3495351f5c812c83a7d601937c4e51
**Their booking link (hard rule).** If they include their own Calendly/HubSpot/Chili Piper/Google booking page, don't send Tim's. Find a free weekday business-hours slot on Tim's Google Calendar, book it in the browser as Tim Yoon / tim@affinityrx.com / (714) 254-5656 / AffinityRx, confirm it landed, then reply "Booked us for <day, date> at <time> <their tz, else PT>." If booking fails (no slots, login wall, captcha), send Tim's Calendly per the rule above and tell Tim.

## 5. Hard rules
- Reply instantly without approval only when the lead wrote last. Cold or net-new emails get drafted for Tim, never sent.
- Every send from support@ or timyoonco@ cc's the matching tim+tag@affinityrx.com: tim+tp@ (telehealth platform), tim+tb@ (telehealth brand), tim+clinic@, tim+503a@ (pharmacy), tim+supplier@, tim+investor@, tim+telehealth@ (general). Never plain tim@affinityrx.com, never a joinaffinityai.com cc. Reply-all keeps the other side's recipients and cc's; never add support@ or plain tim@.
- `Hey [First],` then 1–3 punchy lines, peer tone; nudges name the exact item. No hedging, Thanks/Best sign-offs, emoji or "!". Banned: "thanks for the time", "just", "happy to", "feel free", "no rush", "let me know if you have any questions", apologies, asking permission. Pharmacy go-live/timeline closers (Tim, 2026-10-12; Pharmacy bot owns pharmacy email, same ban): "so we can get you live", "get you live", "this week", "can you knock those out this week?", "start sending you scripts", "want to get you live this week", "want to start sending you scripts" — end on the plain ask instead.
- Never write "keep 100%". Say "free to join" only when they object to cost.
- Never name telehealth platforms or brands we work with (e.g. Cuvo, MyOrbitHealth); say "our live telehealth partners" or "several telehealth platforms". Telehealth follow-ups go to platforms only, never brands. Rule row: https://app.notion.com/p/3f1495351f5c810abf8deca72e8fd082
- Links: run sand-workflow:clean-email-links. Visible anchor labels only (`Book here: <a href="URL">Calendly</a>`), no Google redirect links, read back every href in the sent copy.
- Above the fold (Tim, 2026-10-06): salutation, body, links and signature all sit above the quoted history, with no spacer that makes Gmail collapse them. Rule row: https://app.notion.com/p/3f1495351f5c810da48cd20dd2fa98af
- Net-new subject: blank (empty field, never the words "no subject line"), "Potential partnership", "FirstName?" (e.g. "Cody?"), or a short contextual subject. Never all lowercase. Replies keep the thread subject.
- Signature (Tim, 2026-10-08; Tim can change it), exactly two lines on every email and template: `Tim Yoon` / `<a href="https://affinityrx.com">affinityrx.com</a>` (plain text: "Tim Yoon\naffinityrx.com"). No title, no LinkedIn. This replaces the old joinaffinityai website footer.

## Keep routine runs small (added 2026-10-08)
Routine runs that pulled whole databases, long meeting lists or full threads timed out. In every routine run:
- Triage new mail inside COMPOSIO_REMOTE_WORKBENCH (or with verbose=false, include_payload=false searches) and bring back only a short list of new message ids with sender, subject and snippet.
- Fetch only the single new message by id (GMAIL_FETCH_MESSAGE_BY_MESSAGE_ID). Never fetch a whole thread.
- Look up a contact in Notion by name or email with a filtered query. Never pull the whole Contacts database.
- Look up calls in Granola with query_granola_meetings and the contact's name. Never list 30 days of meetings.
- Write last_checked to the state file at the end of every run. If last_checked is more than 30 minutes old at the start, mention the gap in the handoff.
