---
name: ip-watch-and-leads
description: Use when auditing a company's or competitor's international IP filings and Madrid or PCT portfolio, checking non-use cancellation candidates, or running a practice digest of incoming refusals, renewals, domain gaps, watch matches and open work for an IP attorney's jurisdiction. Uses the IPRwise connector.
---

# Global IP portfolio audit and practice digest

This skill covers two recurring intelligence workflows on IPRwise: (A) auditing a company's, brand's or competitor's international trademark and patent activity, and (B) running a structured digest of incoming filings, deadlines, collisions and cross-border work for an IP practitioner's jurisdiction.

## If the connector is not connected

If the IPRwise tools (`competitor_filings`, `madrid_mark`, `pct_applicant_tracker`, `brand_expansions`, `non_use_candidates`, `madrid_refusals`, `open_tasks`, `domain_gaps`, `madrid_renewals`, `madrid_office_leads`, `madrid_portfolio`, `mark_watch_matches`) are not available, ask the user to connect the IPRwise connector: the plugin's **Connectors** tab, **Connect**, then sign in with an IPRwise account (a free account is enough). Without the connector, do not fabricate filing counts, registration numbers or watch results.

## Track A: Company, brand or competitor IP audit

1. **Define the target.** Confirm the company (holder/applicant), brand name, inventor name or international registration number (IRN), plus any target countries or Nice/IPC classes of interest.
2. **Map Madrid trademark activity.**
   - Call `competitor_filings` with the holder or brand name to pull new international registrations and subsequent designations from the last 60 days alongside the holder's wider Madrid portfolio and designated-country counts.
   - Call `brand_expansions` (filtering by `query` or `office`) to check recent territorial expansions, and call `madrid_mark` on priority IRNs to inspect full registration details and provisional refusal histories.
3. **Map PCT patent activity.**
   - Call `pct_applicant_tracker` with `query` (and follow up with `slug` if multiple matching entities are returned) to retrieve PCT application and Madrid counts, open refusals, top IPC and Nice classes, and recent filings.
   - Where upcoming national phase entries matter in a specific market, call `pct_national_phase_leads` with `target_office` and `query` or `ipc`.
4. **Assess non-use vulnerability (when clearing a mark).**
   - If the user is clearing a brand or evaluating older blocking Madrid marks in one of the 17 supported jurisdictions, call `non_use_candidates` with `office`, `classes` and `query`. State the statutory non-use period from `country_ip_facts` / `non_use_candidates` and note clearly that registration age makes a mark eligible for challenge, not proof of non-use.
5. **Deliver the audit.** Present a concise breakdown: portfolio size (Madrid marks and PCT filings), recent geographic expansions, core Nice/IPC classes, active provisional refusals, potential non-use candidates, and direct WIPO / iprwise.com links.

## Track B: Practice digest for an attorney's jurisdiction or firm

1. **Confirm scope and member visibility.** Identify the two-letter WIPO office code (for example `TR`, `EM`, `US`, `DE`, `GB`, `JP`, `CN`) or representative firm name. Note that member tools (`domain_gaps`, `madrid_renewals`, `madrid_office_leads`, `madrid_portfolio`, `mark_watch_matches`) follow iprwise.com rules: they return data for the office of the country in the signed-in member's IPRwise profile, showing the first 10 rows plus total for unverified members and the full list for verified members.
2. **Open cross-border requests and watch collisions.**
   - Call `open_tasks` to surface open cross-border IP work requests on IPRwise (NDA-safe public summaries with `https://www.iprwise.com/tasks/<id>` links).
   - Call `mark_watch_matches` to check newly published Madrid marks matching the member's saved watch list on iprwise.com, ordered by opposition deadline.
3. **Incoming refusals, unrepresented designs, PCT entries and domain gaps.**
   - Call `madrid_refusals` with `office` to list open provisional refusals in that jurisdiction with running response deadlines.
   - Call `domain_gaps` to spot brands newly designated to the member's country via Madrid whose corresponding local country-code domain appears unregistered.
   - Call `hague_designs` with `office` and `view: "unrepresented"` (or `"refused"` / `"expiring"`) for international designs needing local representation, and `pct_national_phase_leads` with `target_office` for PCT applications nearing the 30/31-month entry deadline.
4. **Renewals, daily office lists and firm portfolios.**
   - Call `madrid_renewals` (`next6`, `next12`, or `grace`) and `madrid_office_leads` (`lapsed`, `transformation`, `priority`, `declarations`, `ownership_changes`, or `inbound_firms`), or call `madrid_portfolio` with `representative` to audit a specific firm's open refusal deadlines and upcoming renewals.
   - Call `ip_news` for recent or upcoming fee and practice announcements at the office.
5. **Deliver the digest.** Group items by nearest statutory deadline first, citing WIPO registration numbers and iprwise.com links.

## Rules

- All IPRwise tools are read-only (`readOnlyHint: true`): never claim to edit a watch list, claim a task, or contact a holder; give the iprwise.com link so the user can act on the site.
- If a member tool reports that results are capped at 10 rows or that no profile country is set, explain briefly how to add the country (`https://www.iprwise.com/profile`) or complete verification (`https://www.iprwise.com/verify`).
- Never state a statutory deadline or non-use period from memory; rely on `ip_deadline`, `country_ip_facts` and the dates returned by the tools.
- Outputs are reference intelligence, not legal advice or proof of non-use or domain availability; confirm register status before acting.
