---
name: Contract redline cycle
description: >-
  Use when a counterparty (e.g. a pharmacy) sends a redlined or updated version
  of our contract, emails contract change requests, or sends their own base
  contract: ingest it, recommend changes to the user, iterate until approved,
  then send back a redlined .docx. Repeats each round.
---
# Contract redline cycle

This is a loop: **ingest -> recommend -> back-and-forth with the user -> approved -> send redline -> wait for their next round**. The routine or the user names the folder, base contract, mailboxes, tracker doc, and log file.

## Triggers (any of these starts or restarts a cycle)
- **A contract file comes back from the counterparty.** It may be a .docx with tracked changes or comments, a PDF markup, or a renamed copy of our agreement ("v2", "redline", "revised", "_comments", "(TO Redline)", "Redline-<Name>", the counterparty's initials).
- **An email (or call notes) asking for contract changes with no file**, for example:
  - "can we change section X"
  - "our attorney/counsel reviewed"
  - "a few edits" or "request a few revisions before moving forward"
  - "too one-sided"
  - "we'd need" or "strike"
  - an inline numbered list of edits
  - questions about specific terms
- **The counterparty sends its OWN base contract or template instead of signing ours.** Phrase cues: "here's our standard agreement", "our standard SLA", "utilize our ... SLA", "just a template", "please complete the attached ... agreement". Filename cues: "[TEMPLATE]", "DRAFT", "Form_", "Provider Agreement", "SLA", "MSA", "outsourcing agreement", "purchase agreement".
- **The counterparty says an existing agreement should govern** ("master agreement in place", "already have an agreement"). Check whether that agreement is signed and in force, then compare it to ours.
- **A reply to our last redline** (the next round).
- **The routine's logged PATTERNS**, which are real examples of how counterparties send redlines. Use them as extra signals.

### Where it usually shows up
- Redlines usually arrive as replies on the existing sales thread, not new threads.
- They sometimes land on an unrelated thread from the same counterparty, such as an NDA e-signature thread, or come as a forward with a new subject ("Fwd: Contract Look Over").
- Cold inbound contracts tend to come through the support or contact-form inbox.
- Who sends:
  - Owners or CEOs forwarding counsel's file.
  - Sales or BD reps sending their own templates.
  - Lawyers rarely email directly.

### Not triggers
- **Status-only messages** ("being reviewed by our legal team", "forwarded to our lawyer", "legal counsel is busy"). Log them as a status update and don't start a cycle.
- **A fully signed contract with no changes.** Log it as Signed and tell the user.
- **A "no redlines" reply.** Log it as Ready to sign and tell the user.
- **Generic onboarding questions** with no contract terms.

## 1. Ingest
1. Dedupe against the log (thread id + message id). Skip anything already processed.
2. Save the original file into the redlines folder as `<Counterparty> - <Redline|Their Contract|Change Request> - <YYYY-MM-DD>.<ext>`. For email-only requests, save a short Google Doc that quotes the requested changes.
3. Extract every change:
   - For a .docx, parse tracked changes (`w:ins` / `w:del`) and comments with python-docx/lxml.
   - For a PDF, use pdftotext and diff it against the base. If it's a scanned PDF, OCR it.
   - For their own contract, compare it clause by clause to ours and list the material differences. Note any internal comments their team left in the file; they can be leverage.
4. Check which version they marked up. If it isn't our current standard, say so and list what that version already conceded.
5. For each change, record the section, the old text, their proposed text, and their reason.

## 2. Recommend
1. For each change, recommend Accept, Reject, or Counter. Give counter language and a one-line reason tied to the company's standing contract positions (from its memory or FAQ).
2. Flag anything legal-sensitive: indemnity, liability caps, exclusivity, non-circumvention, payment terms, fees, insurance, HIPAA/BAA, term and termination, governing law.
3. Never invent positions. If a position is unknown, ask the user.
4. Update the tracker (status: Awaiting review) and send the user a short list: the counterparty, then one line per change with your recommendation.

## 3. Back-and-forth
1. Apply the user's edits to the recommendation list and re-show only what changed.
2. Repeat until the user clearly approves the final set ("approved", "send it").
3. Record the approved decisions in the tracker.

## 4. Build the redline
Start from the counterparty's latest version if they sent one, so their accepted edits stay in. Otherwise start from our base contract. Produce a .docx that looks like a normal legal redline: the full document, with removed text struck through and new text inserted.
- **Preferred:** real Word tracked changes (`w:del` / `w:ins` runs, author = the user's name), so it opens in Word or Google Docs with suggestions showing as crossed out and inserted.
- **Fallback:** visible formatting, with removed text red and struck through, and new text blue and underlined.
- **For their own base contract,** either redline their document, or send ours with a short cover note listing what we kept from theirs. The user decides which.
- **Save both versions.** Store the redline in the folder as `<Counterparty> - AffinityRx Redline vN - <date>.docx`, plus a clean copy with all changes accepted.
- **Check the result.** Open it and confirm that every approved change appears and nothing else changed.

## 5. Send (only after the user approves)
1. Draft a short email:
   - One or two sentences saying our redline is attached.
   - The key changes in a few bullets.
   - The next step ("review and send back signed, or flag anything else").
   Follow the user's email voice and signature rules, and the `clean-email-links` skill.
2. Show the user the exact email and attachment name. Send only after an explicit yes.
3. Send in-thread with the .docx attached, through a mail path that supports attachments and keeps links intact. Reply-all to everyone on the thread. Apply the cc rules for the mailbox and lead type. Verify the sent copy has the attachment.
4. Update the log and tracker (status: Redline sent, round N, date).

## 6. Next round
Their reply restarts the cycle at step 1. A signed copy marks the tracker Signed: save it to the folder and tell the user.

## Rules
- Never send anything to a counterparty without the user's explicit approval of that exact email and file.
- Never quote fee percentages or make commitments the user hasn't approved.
- Keep every version in the folder; never overwrite an earlier round.
- Respect any hold the user put on a counterparty: analyze only, no drafts or replies.
- Banned phrases for pharmacy cover emails (Tim, 2026-10-12): never end with go-live or timeline closers — "so we can get you live", "get you live", "this week", "can you knock those out this week?", "start sending you scripts", "want to get you live this week", "want to start sending you scripts". End on the plain ask instead.
