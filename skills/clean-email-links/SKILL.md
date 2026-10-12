---
name: Clean email links
description: >-
  Use EVERY time you write, reply to, forward, or send an email (or draft card)
  that contains any link, including signature links. Makes sure every URL is the
  real destination, never a Google redirect wrapper.
---
# Clean email links: never send Google redirect URLs

## The failure this prevents
When an agent copies a link out of a Gmail message it read (a quoted thread, a prior sent email, a get_thread or get_message HTML body), Gmail has already rewritten that link into a tracking redirect like:
`https://www.google.com/url?q=https://calendly.com/timhyoon/meet-with-tim-yoon&source=gmail&ust=1791312516554000&sa=E`
Pasting that into a new email sends the recipient a broken, ugly, suspicious link. This happened on real sends: Calendly links to suppliers and pharmacies, the compliance form link, the signature website link, and LifeFile webhook endpoints sent to Vault, Valiant and Empower (the engineer had to resend corrected URLs).

## Verified 2026-10-07: the native Gmail connectors wrap links ON SEND
Tested with an internal self-email: the built-in Gmail connectors (`user-Gmail` for timyoonco@ and `user-Gmail--support` for support@) rewrite every href, and even bare URLs in the plain-text body, into `google.com/url?q=...&source=gmail&ust=...` at send time, even when the href you pass is clean. The stored sent message (read back raw through Composio) contains the wrapped link.
The same test sent through Composio Gmail (`GMAIL_SEND_EMAIL` / `GMAIL_REPLY_TO_THREAD` with `is_html: true`) kept the href exactly as written.
- For ANY outbound email that contains a link, send through Composio Gmail on the matching account (support@, timyoonco@, or tim@affinityrx.com), not the native `send_message` / `reply` / `forward` tools.
- Composio `GMAIL_REPLY_TO_THREAD` takes a threadId (not a messageId) and explicit `recipient_email` / `extra_recipients` / `cc`; it does not add Tim's own addresses automatically.
- Native Gmail tools are still fine for reading, searching, labels, and link-free emails.
- After the first send in a run, read the sent message back raw (Composio `GMAIL_FETCH_EMAILS` with include_payload) and confirm the href is unchanged.

## Hard rules (anti-prompt)
- NEVER put any URL containing `google.com/url`, `www.google.com/url?q=`, `&source=gmail`, `&ust=`, or `&sa=E` into an email body, htmlBody, href, signature, or draft card.
- NEVER copy a URL out of a Gmail message body, quoted history, snippet, or a previous email you sent. Gmail-read content is presumed wrapped.
- NEVER copy the signature from a fetched email. Rebuild it from the stored rule.
- NEVER wrap links in any other tracker either (`safelinks.protection.outlook.com`, `urldefense.com`, click-tracking domains).
- Do not "fix" a wrapped link by guessing. Unwrap it (below) or pull it from the source of truth.

## Where clean URLs come from (in order)
1. The canonical value in a skill, Notion memory/settings page, routine prompt, or other source of truth (Calendly links, form links, the company website).
2. The system that owns the link: Drive/Docs/Sheets file metadata, the Google Forms URL, the API or database that issued a webhook endpoint.
3. Only as a last resort, unwrap a Gmail redirect: take the `q=` parameter value, URL-decode it, and drop everything from the first `&source=` / `&ust=` / `&sa=` onward. The result must start with the real domain (`https://calendly.com/...`, `https://forms.gle/...`, `https://docs.google.com/...`).

## Pre-send check (every time, no exceptions)
1. Collect every URL in subject, body, htmlBody (including every `href=`), and signature.
2. Scan for: `google.com/url`, `source=gmail`, `ust=`, `sa=E`, `safelinks`, `urldefense`.
3. If any hit: replace with the clean URL from the sources above, then re-scan.
4. Confirm visible link text and href point at the same clean destination.
5. Send through a path that keeps links intact (Composio Gmail, see above), then verify the sent copy.

## If a wrapped link already went out
Tell the user which emails and recipients got it, and offer a short correction reply with the clean link (draft it; don't auto-send unless they told you to).
