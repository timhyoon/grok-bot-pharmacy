---
name: Setter morning follow-ups
description: >-
  Use for Setter's Morning follow-ups routine: daily 8–9 AM (recipient's local
  time) nudges to non-pharmacy BD leads (suppliers, clinics, telehealth
  platforms, partners) whose last message is ours, post-call action-item chases,
  and the weekday 8:04 AM PT iMessage FU Hotlist texts. Also holds the archived
  pharmacy templates the Pharmacy bot owns.
---

> Pharmacy-only excerpt. Setter's non-pharmacy sections removed.

## Pharmacies are excluded (Tim, 2026-10-08)
Morning follow-ups never nudges anyone in Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd), any pharmacy sender, or any pharmacy in /workspace/pharmacy-postcall/state.json. The Pharmacy bot (agent id 6d0c3824-3840-4d2e-b714-a990356010a7) owns them, including their post-call email and daily nudges. If one needs action, message the Pharmacy bot instead.

## Pharmacy (Pharmacy bot only)
Archived reference moved from the Inbox replies skill on 2026-10-08. Setter never sends any of this; the Pharmacy bot owns all pharmacy email and follows its own send/draft rules. The Pharmacy post-call email lives in sand-workflow:post-call-recap-email.

**Pharmacy first-call Calendly:** https://calendly.com/timhyoon/affinity-pharmacy-demo (brand-new pharmacy leads only; after a call, only meet-with-tim-yoon).

**Pharmacy social proof.** Do NOT add this line to every reply. Only add it (right before the Calendly CTA) when a pharmacy seems to need a boost of conviction or their questions sound doubtful or skeptical (e.g. "who else is on this?", "is this legit?", hesitation about volume or trust). For plain, interested questions, leave it out (Tim, 2026-10-06): "We work with pharmacies like Empower, Strive, PerfectRx, and Promise Pharmacy." Naming pharmacies here is allowed; naming telehealth platforms never is.

**Pharmacy onboarding email (template, Tim 2026-10-08).** When a pharmacy agrees to move forward (asks for next steps or the contract), send ONE onboarding email on their existing thread. It carries a single link to the AffinityRx for Pharmacies guide, which holds the contract, the compliance form, pricing and the Book a call link:
- Guide URL (use exactly): https://docs.google.com/document/d/19s7XxjDJTE0n0FNlOeHuluc_7p8l6c2RHbCF_PJx49Q/edit?usp=sharing
- No attachment. Never attach the contract .docx or .pdf. No separate pricing-sheet, Google Form or contract links.
- Then short next-step bullets for only what isn't Done yet: `Sign the contract.` / `Send over your pricing.` / `Fill out the compliance form.` Check the Pharmacy Compliance Tracker, the thread and the pharmacy's Pharmacy Contacts row first, and drop any item that's already Done (contract Signed, form Done, their pricing already received).

Template (htmlBody; render the bullets as a `<ul>` list):
```
Hey [First],

Here's everything for [Pharmacy], including the contract: <a href="https://docs.google.com/document/d/19s7XxjDJTE0n0FNlOeHuluc_7p8l6c2RHbCF_PJx49Q/edit?usp=sharing">AffinityRx for Pharmacies</a>

- Sign the contract.
- Send over your pricing.
- Fill out the compliance form.

Tim Yoon
<a href="https://affinityrx.com">affinityrx.com</a>
```
**No go-live/timeline closers (Tim, 2026-10-12; pharmacy emails):** never end with go-live or timeline pressure. Banned: "so we can get you live", "get you live", "get live" as a closer, "this week", "can you knock those out this week?", "start sending you scripts", "want to get you live this week", "want to start sending you scripts". End on the plain ask instead.

Reply-all on the existing thread from the mailbox it's in. Cc tim+503a@affinityrx.com when it's sent from support@ or timyoonco@. Create and send it through Composio Gmail so the link isn't wrapped (clean-email-links), and read it back before sending. After it goes out, update the pharmacy's Pharmacy Contacts row (Their Action Items = the open bullets) and follow up daily until every item is back. Source of truth: the Setter Memory row "Pharmacy onboarding email template" (https://app.notion.com/p/3f1495351f5c81e4b43ad4f228f78920). After a CALL with a pharmacy, send the "Pharmacy post-call email" from post-call-recap-email instead.

**Pharmacy compliance tracker check (hard rule, Tim 2026-10-06).** Before ANY email to a pharmacy (new reply, follow-up, or onboarding), check the Pharmacy Compliance Tracker (Google Sheet 1Y_T80DGx1qnsRI03eFfw6h_o4ckqYEU9C-fO5UmS7QQ, tab "Untitled", columns Contract and Compliance Form; Notion mirror under Agent Memory) and the form responses (uploads in Drive "3 - Compliance > Compliance Form", named "<file> - <respondent name>"; Google Forms response emails; the thread itself, e.g. "this has been filled out").
- Never ask for a form that's already Done (or that the responses/thread show was submitted), and don't include the form link. Acknowledge it (e.g. "Got the compliance form.") and only mention what's still outstanding. If a submission shows up but the tracker isn't Done, tell Tim instead of chasing.
- If the latest message on the thread is already from Tim (timyoonco@ or support@, including his manual replies), don't reply.
- Check Pharmacy Contacts notes for holds first: Max Perryman (Striker) = Tim handling personally, never draft or send; Jessica Patrone (Pro Compounding) = no automated emails until Tim says.
Rule row: https://app.notion.com/p/3f1495351f5c81a99d91d82ddd2b7a6f
