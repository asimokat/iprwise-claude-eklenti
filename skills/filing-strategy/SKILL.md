---
name: filing-strategy
description: Use when a user wants to protect a trademark, patent or design in several countries - choosing between national filings, Madrid, Hague, PCT and regional systems, computing priority and national phase deadlines, quoting official trademark fees, and laying out a country-by-country plan. Uses the IPRwise connector.
---

# International filing plan

This skill gives the method for turning "we want protection in these countries" into a filing plan. The IPRwise connector supplies treaty memberships, statutory deadlines, official trademark filing fees and country rules with their sources; this skill says how to combine them.

## If the connector is not connected

If the IPRwise tools (`filing_routes`, `country_ip_facts`, `ip_deadline`, `official_fees`, `nice_2027_check`, `ip_news`) are not available, ask the user to connect the IPRwise connector: the plugin's **Connectors** tab, **Connect**, then sign in with an IPRwise account (a free account is enough). Without it, mark every membership, deadline, fee and rule in the plan as unverified.

## Workflow

1. **Collect the facts.** The right; the first filing (country, date, number) or the home registration; the target countries; for trademarks the classes, specification and whether the mark is in colour; the business timeline; budget limits. Ask only for what is missing.
2. **Routes in one call.** Call `filing_routes` once with all target countries and the home country (up to 25 jurisdictions). Build a table: country, Paris, Madrid / Hague / PCT, regional option, note. A Madrid or Hague filing also needs the applicant to be connected to a member: a national of, domiciled in, or with an establishment in a contracting party (often the home country).
3. **Priority and statutory deadlines.** Never compute calendar dates from memory when `ip_deadline` covers the office. For the 12 supported offices (`USPTO`, `EPO`, `EUIPO`, `TR` for TÜRKPATENT, `WIPO`, `UKIPO`, `DPMA`, `JPO`, `CNIPA`, `KIPO`, `CIPO`, `IPAU`), call `ip_deadline` with `paris_tm` (6 months), `paris_pt` (12 months) or `pct` (30 or 31 months by office) from `start_date` so weekends and official closure days are skipped, and cite the legal rule and calendar source it returns. For offices outside those 12, state the Paris Convention period (Art. 4C: 12 months for patents and utility models, 6 months for trademarks and industrial designs) and note that the exact rollover day must be confirmed locally. If the deadline has passed, say so: later filings stand on their own dates.
4. **Choose the route for each country and give the reason.**
   - Trademarks: Madrid where both the applicant's office and the target are members; a national filing where the target is not a Madrid member or needs a local strategy; one EU trade mark for all 27 EU states; Benelux for Belgium, the Netherlands and Luxembourg; OAPI as the unitary route for its member states. Under Madrid the international registration depends on the basic mark for five years (Madrid Protocol Art. 6(3)): mention this when the basic mark is still exposed to refusal or opposition.
   - Patents: PCT to keep many countries open and defer costs; the EPO for Europe, including extension and validation states; EAPO for its member states; ARIPO (Harare Protocol) for its members. Confirm each office's 30- or 31-month PCT limit.
   - Designs: Hague where the target is a member; otherwise a national or regional filing (EU design, Benelux, OAPI).
5. **Official trademark filing fees.** For trademark plans that include any of the 13 offices covered by `official_fees` (`US`, `EM` for EUIPO, `GB`, `TR`, `DE`, `JP`, `CN`, `KR`, `CA`, `AU`, `BR`, `CH`, `IN`), call `official_fees` with `classes` and `madrid: true` (setting `color_mark` or `origin_agreement_state` if applicable). Report each national fee in the office's currency and the WIPO Madrid total in CHF (basic fee, individual or complementary fees under Art. 9sexies, supplementary class fees), citing the tariff date and official source link returned. State clearly that local attorney professional fees are separate. For other offices, or for patents and designs, do not guess fees; obtain quotes from local counsel.
6. **Country rules.** Call `country_ip_facts` for each target country. Flag early: a required local agent, power of attorney formalities (notarisation or legalisation can take weeks), the filing language and translations, opposition periods and non-use thresholds.
7. **Classification and specification wording.** For trademark specifications filed on or after 1 January 2027, run the terms through `nice_2027_check` (NCL 13-2027) and propose the new wording where a term was reworded, deleted or moved. For Madrid designations into offices with high partial-refusal rates, check `madrid_office_scorecard` and `goods_services_terms` to spot terms that office commonly cuts.
8. **Recent changes.** Call `ip_news` for the main target offices and WIPO. Mention items that affect the plan, such as fee adjustments or treaty accessions, with their effective dates.
9. **Deliver the plan.** A table with country, route, priority or national phase deadline, official trademark fee (where covered), local agent needed, power of attorney formality, key note and source links; then a dated timeline and next steps. To instruct local attorneys, use the `foreign-associate` skill and `find_attorneys`.

## Rules

- Quote government trademark fees only from `official_fees`, citing its tariff date and source page, and always note that attorney professional fees are not included. Never guess fees for unsupported offices or rights.
- Quote statutory rollover dates at the 12 supported offices only from `ip_deadline`, citing the legal rule and closure-calendar source.
- Every country rule in the plan cites the source link `country_ip_facts` returned, and puts in quotation marks only verbatim `Quote` text. No rule from memory.
- Membership dates matter: a country whose accession is "not yet in force" cannot be designated before that date.
- This is reference information, not legal advice; the user confirms the plan with counsel before filing.
