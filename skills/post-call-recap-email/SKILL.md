---
name: Post-call recap email
description: >-
  Use when a call/meeting has ended and the external attendee(s) should get a
  short recap email with the summary, their action items, and the user's action
  items — from a Granola note or from bullets the user gives. Also logs both
  sides' action items to the contact's CRM row so follow-ups chase their items
  and the user hears about their own.
---
# Post-call recap email

## Inputs
- A finished meeting: a Granola note (summary, attendees, private notes; transcript only if a detail is missing), OR 2-5 bullets the user typed for an unrecorded call.
- A dedupe log of meetings already recapped (the routine names where it lives).

## Contacts databases (Tim, 2026-10-08)
AffinityRx contacts are split across three databases with one shared schema: Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd) for 503A/503B pharmacies, owned by the Pharmacy bot (agent id 6d0c3824-3840-4d2e-b714-a990356010a7); Supplier Contacts (collection://72656921-0e2b-41a2-817a-5d5a836b1388) for suppliers; and Setter Contacts (collection://b1cfa662-4dab-4817-b2d3-2bede1866f35) for clinics, telehealth, partners and other. Setter owns Supplier and Setter Contacts. Pharmacy recaps and the Pharmacy post-call email are run by the Pharmacy bot, never by Setter; if Setter finds a pharmacy call, it sends nothing and messages the Pharmacy bot with the meeting.

## Steps
1. **Skip check.** Skip if the meeting is already in the dedupe log, has no external attendee (everyone is on the user's own domains), is internal/personal, was a no-show/cancelled, or has no substantive notes yet (note still processing: leave it for the next run, don't log it).
2. **Recipients.** External attendees from the note/calendar invite. Use the real addresses; never guess one. If none can be found, log it as skipped and tell the user.
3. **Classify the call.**
   - **Pharmacy (503A compounding pharmacy) with onboarding still open → "Pharmacy post-call email" (section below).** The Pharmacy bot runs these emails, using Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd); Setter skips pharmacy calls and hands them to the Pharmacy bot. This applies after ANY call with a pharmacy, first call or not, as long as at least one onboarding item is still open: contract not Signed, compliance form not Done (Pharmacy Compliance Tracker), or their pricing not yet received. If ALL onboarding items are Done, don't use it: custom-write the recap (last bullet). Source of truth: Setter Memory row https://app.notion.com/p/3f2495351f5c81619b30eb154e8ef2a0.
   - Otherwise, is this the FIRST call with this counterparty? Check the contact's row (Last Call, Stage) in its database (Setter Contacts for clinic/telehealth/partner/other, Supplier Contacts for suppliers, Pharmacy Contacts for pharmacies), Granola history, and Gmail. A first call has no earlier call with them. If so, fetch the template row for the counterparty type (Type = Template, in Setter Memory `collection://9b6f313e-0c41-4293-8e61-f4da647e7aad`) and follow its page body exactly. The row is the source of truth for wording, links, and rules.
     - **Clinic / telehealth / telehealth platform**: https://app.notion.com/p/3f2495351f5c8191b3e6ea7ad963fab5
     - **Supplier** (wholesaler, API supplier, 503B, PPE, plastics, lab testing): https://app.notion.com/p/3f2495351f5c81ec93dddb58d8b78611
   - **Everything else** (non-pharmacy follow-up, negotiation, check-in; or a pharmacy with every onboarding item Done): don't use a template. Custom-write the whole email from the notes. Every other email (follow-ups, chases, etc.) is custom-written too.
4. **Extract** from the note:
   - Summary: 1-2 sentences, what was decided.
   - Their action items: only things THEY explicitly committed to. Name the person if several.
   - User's action items: only things the user explicitly committed to.
   Never invent commitments, dates, prices, or numbers. If a side has none, omit that section.
5. **Write** in the user's voice: short, confident, no fluff. Base format (templates add their own blocks):
   - `Hey <first name>,`
   - `Quick recap:` + 1-2 sentence summary. Never open with or include "thanks for the time" (or any thank-you for the call); Tim, 2026-10-07.
   - Template block: pharmacy calls use the Pharmacy post-call email below (guide link + open next-step bullets); clinic/telehealth and supplier first calls use their row's block (website + precall page / partnership summary)
   - `On your end:` bullets (only if they exist)
   - `On my end:` bullets (only if they exist)
   - Booking link, only if a next call was agreed or they asked for one: `Book here:` linked to https://calendly.com/timhyoon/meet-with-tim-yoon. Never a demo link or the follow-up event (see "Post-call Calendly" below).
   - Signature (Tim, 2026-10-08): exactly two lines, `Tim Yoon` then `affinityrx.com` linked to https://affinityrx.com (plain text: "Tim Yoon\naffinityrx.com").
   **Maintain status (Tim, 2026-10-07):** peer-to-peer, never deferential. No thank-yous for their time, no "let me know if you have any questions", "hope this helps", "just", "happy to", "feel free", "whenever you get a chance", "no rush", apologies, or asking permission. State next steps as plain directives ("Sign the contract."). Nudges name the item, not "any updates?".
   **No go-live/timeline closers (Tim, 2026-10-12):** pharmacy emails must never end with go-live or timeline pressure. Banned: "so we can get you live", "get you live", "get live" as a closer, "this week", "can you knock those out this week?", "start sending you scripts", "want to get you live this week", "want to start sending you scripts". End on the plain ask instead.
   Subject for fresh emails: one of Tim's approved net-new subjects only: empty, "Potential partnership", "<FirstName>?", or a short contextual subject (never all lowercase). Reply on the existing thread when one is active.
6. **Apply all standing email rules**: signature rule, banned phrases/claims, and run the `clean-email-links` skill on every link. Keep everything above any quoted history.
7. **Send** as a fresh email (or reply on the existing thread with that person if one is active and it reads naturally), reply-all to every external attendee. Use the mailbox the routine specifies.
8. **Log** meeting id, recipients, time sent, message id in the dedupe log.
9. **Write action items to the CRM row** (the routine names the contacts database; for AffinityRx it's the contact's own database: Setter Contacts (collection://b1cfa662-4dab-4817-b2d3-2bede1866f35) for clinics/telehealth/partners/other, Supplier Contacts (collection://72656921-0e2b-41a2-817a-5d5a836b1388) for suppliers, and Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd) for pharmacies, written only by the Pharmacy bot). For each external attendee's contact row:
   - `Their Action Items`: what THEY committed to (step 4), plus any onboarding steps the template email asked for (pharmacy: send pricing, sign the contract, fill out the compliance form; skip any already Done). `Tim Action Items` (the user's items): what the user committed to.
   - Format: a `Call <M/D>:` header, then one item per line, `[ ]` open / `[x]` done. Keep earlier calls' lines below; don't delete history. If a side has nothing, write `Call <M/D>: none captured.`
   - Mark an item `[x]` right away if it's already done (e.g. the recap itself delivered the recording or the pricing sheet).
   - Also set Last Call to the meeting start and Stage to Call Done if it was earlier than that (never move a contact backwards from Onboarding/Live).
   - Never email anyone about the user's own items; they're surfaced to the user in the routine's summary.
10. **Report** one line per recap sent (who, subject) to the user, plus the user's own action items from each call; say nothing when nothing was sent.

## Pharmacy post-call email
Run by the Pharmacy bot (agent id 6d0c3824-3840-4d2e-b714-a990356010a7), which uses Pharmacy Contacts (collection://367e4d14-c8aa-4018-8dc7-4ac22950cfcd) for every read and write below. Setter never sends it.
Tim, 2026-10-08. Pharmacy calls get their OWN post-call email, separate from the generic recap. Send it after any call with a pharmacy (503A), first call or not, whenever at least one onboarding item is still open. If ALL onboarding items are Done (contract Signed, compliance form Done, pricing received), fall back to the custom-written recap. Setter Memory row (keep in sync): https://app.notion.com/p/3f2495351f5c81619b30eb154e8ef2a0

Before writing:
- Check the Pharmacy Compliance Tracker (Sheet 1Y_T80DGx1qnsRI03eFfw6h_o4ckqYEU9C-fO5UmS7QQ: Contract = Signed means done; Compliance Form = Done means done), the thread, and the pharmacy's Pharmacy Contacts row (pricing received?). Drop every bullet that's already Done.
- Respect holds (Max Perryman/Striker: Tim handles; Jessica Patrone/Pro Compounding: no automated emails; Epiq paused). Contract redlines and term questions from the call go to the Pharmacy bot, never into this email.

Format (htmlBody; bullets as a `<ul>` list):
```
Hey <first name>,

Quick recap: <1-2 sentence summary of what we discussed/decided>.

Everything you need is in our <a href="https://docs.google.com/document/d/19s7XxjDJTE0n0FNlOeHuluc_7p8l6c2RHbCF_PJx49Q/edit?usp=sharing">AffinityRx for Pharmacies</a> guide. Next steps:
- Send over your pricing.
- Sign the contract.
- Fill out the compliance form.

On your end:        (only if they made real commitments beyond the bullets above)
- <their action item>

On my end:          (only if Tim made real commitments)
- <Tim's action item>

Tim Yoon
<a href="https://affinityrx.com">affinityrx.com</a>
```
Rules:
- Guide URL, exactly: https://docs.google.com/document/d/19s7XxjDJTE0n0FNlOeHuluc_7p8l6c2RHbCF_PJx49Q/edit?usp=sharing. It's the single place for the contract, compliance form, pricing and booking link. No contract attachment, and no separate pricing-sheet, form or contract links.
- No thank-you for the call and none of the banned phrases (step 5), including go-live/timeline closers (Tim, 2026-10-12). Don't repeat a bullet item under "On your end". End on the plain ask; never close with "so we can get you live", "this week", "knock those out this week", or "start sending you scripts".
- No booking link unless a next call was agreed or they asked for one; then only `Book here:` linked to https://calendly.com/timhyoon/meet-with-tim-yoon.
- Reply-all on the active thread with the pharmacy if one exists, otherwise send fresh with an approved net-new subject (step 5). Cc tim+503a@affinityrx.com when it's sent from support@ or timyoonco@.
- Create/send through Composio Gmail with is_html and run clean-email-links on every href (read the sent copy back).
- Then do steps 8-9: log it, and write the open bullets plus any real commitments into `Their Action Items` and Tim's into `Tim Action Items` on the pharmacy's Pharmacy Contacts row.

## After the recap (follow-up behavior for routines)
Once a call has happened, follow-ups chase the other party's open `Their Action Items` (one item-focused nudge a day max, on the existing thread, naming exactly what they said they'd do) instead of asking for another call or sending a booking link, unless booking a call was itself their commitment. When all their items are done, nudge only on remaining onboarding steps, or send nothing. Open `Tim Action Items` are listed for the user in each routine summary.

**Post-call Calendly (hard rule, Tim 2026-10-07):** once we've had at least one call with a contact (pharmacy, supplier, clinic or telehealth), every later email that offers or asks them to book a call, including this recap, uses exactly one link: Meet with Tim Yoon, CEO @ AffinityRx, https://calendly.com/timhyoon/meet-with-tim-yoon. Never send them affinity-pharmacy-demo, affinity-clinic-telehealth-demo, or follow-up-with-tim-yoon-ceo-affinityrx. Only add the link when a call is actually offered, they asked for one, or booking it was their commitment; otherwise chase their items with no link. First-call outreach to brand-new leads (no call yet) keeps its lead-type Calendly. Rule row: https://app.notion.com/p/3f3495351f5c812c83a7d601937c4e51

## CC rule (Tim, 2026-10-08)
Any recap sent from timyoonco@gmail.com or support@joinaffinityai.com must cc the matching tim+tag@affinityrx.com for the lead type (tim+tp@ telehealth platform, tim+tb@ telehealth brand, tim+clinic@, tim+503a@ pharmacy, tim+supplier@, tim+investor@, tim+telehealth@). Never cc plain tim@affinityrx.com in its place.
