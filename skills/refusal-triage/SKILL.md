---
name: refusal-triage
description: Use when a user receives a WIPO Madrid provisional refusal or a national IP office action - inspecting the registration and refusal record, calculating the statutory response deadline, checking office refusal patterns and specification wording, verifying local agent and power of attorney rules, and shortlisting local counsel with an instruction brief. Uses the IPRwise connector.
---

# Madrid provisional refusal and office action triage

This skill gives a structured workflow for assessing a WIPO Madrid provisional refusal or a national trademark or patent office action, verifying deadlines and formalities from official sources, and preparing a clean instruction brief for local counsel.

## If the connector is not connected

If the IPRwise tools (`madrid_mark`, `madrid_refusals`, `ip_deadline`, `madrid_office_scorecard`, `goods_services_terms`, `nice_2027_check`, `country_ip_facts`, `find_attorneys`) are not available, ask the user to connect the IPRwise connector: the plugin's **Connectors** tab, **Connect**, then sign in with an IPRwise account (a free account is enough). Without the connector, do not guess refusal dates, response deadlines, office statistics or power of attorney formalities: mark them as unverified.

## Workflow

1. **Inspect the registration and refusal record.**
   - For a WIPO Madrid mark, call `madrid_mark` using the international registration number (IRN) or mark name to inspect the registration details and its refusal history across designated offices.
   - Call `madrid_refusals` (filtering by `office`, `class`, or `type` as needed) to check open provisional refusals, whether the refusal is total or partial, the WIPO notification date, the recorded response deadline, the holder and any recorded representative.
   - If the user has not pasted the examiner's refusal notice, note what WIPO's register confirms (scope, classes, dates) and ask for the notice text if specific grounds (cited prior marks or absolute grounds) need analysis.

2. **Calculate the exact statutory response deadline.**
   - Never do calendar math from memory.
   - When the refusing office is one of the 12 offices supported by `ip_deadline` (`USPTO`, `EPO`, `EUIPO`, `TR` for TÜRKPATENT, `WIPO`, `UKIPO`, `DPMA`, `JPO`, `CNIPA`, `KIPO`, `CIPO`, `IPAU`), call `ip_deadline` with the applicable event (`madrid_ref_2` for a 2-month Madrid refusal period, `uspto_oa_3` for a 3-month USPTO office action, `epo_r71_4`, or `custom` with `months`) and `start_date`. State the rollover date, skipped weekends or closure days, legal rule and calendar source returned.
   - State clearly which date triggers the period at that office (WIPO notification date vs. office issue date or deemed receipt date), and for offices outside those 12, report the deadline from `madrid_refusals` and flag the exact trigger rule for local counsel to confirm immediately.

3. **Check office refusal patterns and acceptable wording.**
   - Call `madrid_office_scorecard` for the refusing office to show context: total vs. partial refusal counts, historical outcomes (protection granted, partial protection, final refusal), median days from refusal to closure, and refusal rates in the relevant Nice classes.
   - When goods or services wording is objected to (or in a partial refusal), call `goods_services_terms` for that office and Nice class to see specific terms the office has cut or limited in Madrid designations.
   - Run any proposed narrowed or replacement goods and services terms through `nice_2027_check` so amendments align with NCL 13-2027 wording.

4. **Check representation and power of attorney formalities.**
   - Call `country_ip_facts` for the refusing country. Check whether foreign holders must act through a local agent to file a response, what power of attorney formalities apply (simple signature, notarisation, apostille or consular legalisation, and whether post-filing submission is allowed), and the office's language.
   - Cite the official source URL returned by `country_ip_facts` and put in quotation marks only verbatim text from its `Quote` lines. Flag notarisation, apostille or legalisation immediately when the deadline is tight.

5. **Shortlist local counsel and draft the instruction brief.**
   - Call `find_attorneys` for the refusing country with `practice` (`trademark` or `patent`), and `attorney_profile` by `slug` when checking a candidate's details. Present verified IPRwise members first, followed by public-source entries, with their iprwise.com profile links.
   - Deliver a concise triage report (IRN/application number, mark, refusing office, total vs. partial scope, statutory response deadline with source, local agent and POA requirements, specification amendment options, and 2 to 3 shortlisted attorneys) followed by a ready-to-send instruction brief setting an internal target 7 to 14 days before the statutory deadline.

## Rules

- Never compute a response deadline from memory; rely on `ip_deadline` and `madrid_refusals`, citing the legal rule and calendar source.
- Never invent cited marks, examiner objections, e-mail addresses, phone numbers or attorney fees.
- If a partial refusal covers only certain classes or goods, explain the option of responding vs. allowing uncontested classes to proceed where the office's practice permits, subject to local counsel confirmation.
- This is reference information, not legal advice; a qualified local attorney must review the official refusal notice before acting.
