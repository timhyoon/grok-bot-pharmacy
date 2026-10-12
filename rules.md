# Standing pharmacy rules (AffinityRx)

## Partner qualification
- The pharmacy's PMS must have a working API. If it doesn't, we can't work together unless they move to a system that has one (Tim, 2026-10-09).
- Liberty (PharmacyOne) has no API. Never list Liberty as an integration option.
- PrimeRx, Pharmetika and similar systems with an API qualify.

## Pricing model
- Clinics save about 10% versus going direct.
- Pharmacy pricing is stepped (tiered by volume).

## Email style
- Keep it very simple: a few lines with the core point and one next step. Start with `Hey [First],`. Use a peer tone, with no "!" and no emoji.
- Initial (cold or first-touch) emails can be assertive.
- With established partners we've already talked to, frame asks as questions ("Can he…?", "Can you send…?"). Never use directive or status-lowering wording.
- Banned: "please", "if you could", "thanks for the time", "just", "happy to", "feel free", "no rush", "let me know if you have any questions", apologies, asking permission.
- No go-live, timeline or scripts closers (Tim, 2026-10-12). That covers "so we can get you live", "get you live", "this week", "can you knock those out this week?", "start sending you scripts", "want to get you live this week" and "want to start sending you scripts". End on the plain ask.
- Footer, exactly: `Tim Yoon` / `<a href="https://affinityrx.com">affinityrx.com</a>`. No title, no LinkedIn.
- Cc tim+503a@affinityrx.com on pharmacy emails sent from support@ or timyoonco@ (any non-affinityrx address). Cc tim+supplier@ on supplier emails. Never cc plain tim@.
- Run clean-email-links on every link. Send through Composio Gmail with is_html. Keep everything above the quoted history.
- Calendly: a pharmacy with no call yet gets https://calendly.com/timhyoon/affinity-pharmacy-demo. Anyone we've already called gets only https://calendly.com/timhyoon/meet-with-tim-yoon, and only when a call is offered or they asked for one.

## Approvals and holds
- Never email a pharmacy without Tim's approval of the exact email. Exception: the routines have standing permission for post-call emails and instant inbox replies.
- Don't share files externally without Tim's OK. Deleting drafts needs approval.
- Holds: Epiq (paused). Striker / Max Perryman (Tim handles). Jessica Patrone / Pro Compounding (no automated email). Emerald counter (Tim reviews before any send).
- Check the Pharmacy Compliance Tracker before any pharmacy email. Never ask for an item that's already Done.

## Routines (summary)
- **Post-call recap (pharmacy):** after a Granola-recorded call, sends the pharmacy post-call/onboarding email (post-call-recap-email) and writes action items to Pharmacy Contacts. Currently scheduled at 8:02. A move to hourly is pending Tim's approval.
- **Pharmacy daily follow-ups:** one nudge a day per pharmacy on its existing thread, naming the open item from Their Action Items. Stops on reply or decline.
- **Pharmacy inbox replies:** Setter hands pharmacy threads to the Pharmacy bot. Instant replies only where the pharmacy wrote last. Contracts go through contract-redline-cycle.
- **Redline watch:** metadata-only searches for returned contracts, then runs contract-redline-cycle and logs to the Contract Tracker.
- **Pricing sheet intake:** logs pharmacy pricing and formulary files to Pharmacy Pricing Sheets and the Drive raw prices folder (pharmacy-pricing-sheet-intake).
- **SOP repo upkeep:** update this repo when Tim asks, plus one end-of-day pass.
- **All runs:** stay small. No whole-DB, whole-thread or 30-day Granola pulls.
