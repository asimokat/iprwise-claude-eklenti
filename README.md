# IPRwise: Claude plugin

![IPRwise](assets/iprwise-512.png)

This plugin connects Claude to [IPRwise](https://www.iprwise.com), the global network of IP attorneys. Claude writes the answer; IPRwise supplies checked reference data with its sources: country IP facts quoted from official sources, international filing routes, the IPRwise attorney directory, IP office announcements, the Nice Classification 2027 changes, deadline and official fee calculations, and the Madrid, Hague and PCT data behind the IPRwise tools pages.

**Türkçe:** [aşağıda](#türkçe)

## What is in the plugin

| Part | What it does |
|---|---|
| `iprwise` connector | Connects to `https://www.iprwise.com/mcp`. 22 read-only tools (below). |
| `foreign-associate` skill | How to choose and instruct an attorney in another country: check the country's rules, shortlist on IPRwise, write the instruction letter. |
| `filing-strategy` skill | How to plan filings in several countries: Paris priority, Madrid, Hague, PCT and regional routes, country rules, Nice 2027 wording. |

## Tools

All 22 tools are read-only (`readOnlyHint: true`). None writes anything to IPRwise. Tools marked **member** show exactly what the signed-in member sees on iprwise.com: only the office of the country in their IPRwise profile, and for members not yet verified the first 10 rows with the total.

**Reference and directory**

| Tool | What it returns |
|---|---|
| `country_ip_facts` | Key IP facts for one country: IP office, main law, trademark term, opposition period, non-use cancellation, patent and utility model terms, design protection, local agent requirement, power of attorney formalities, filing language. Each fact has the official source link and a short verbatim quote from that source. |
| `filing_routes` | For up to 25 countries: Paris Convention, PCT, Madrid Protocol and Hague Agreement membership with the date it took effect, plus EU, Benelux, EPO (member, extension, validation), EAPO, OAPI and ARIPO (Harare, Banjul). Flags accessions not yet in force. Source: WIPO Lex and the official member lists. |
| `find_attorneys` | IP attorneys and firms in a country: IPRwise members whose identity and professional registration IPRwise checked come first, then directory records from public sources (such as the EPO list of professional representatives, EAPO and public business listings). Filters: practice, city. Each entry links to its iprwise.com profile. |
| `ip_news` | IPRwise IP Radar: official announcements of IP offices (fees, laws, accessions, practice, e-filing, decisions, events, warnings) with effective dates and source links. Summaries in nine languages. |
| `nice_2027_check` | Checks goods and services terms against the 2027 version of the Nice Classification (NCL 13-2027): added, deleted, reworded or moved, with the new wording. |
| `attorney_profile` | One public directory profile by its slug (from `find_attorneys`): name, firm, role, practice, city, country, website and whether it is a verified IPRwise member, with the profile link. Removed profiles are never returned. |

**Calculators** (from the same single fee table and office calendars as the website)

| Tool | What it returns |
|---|---|
| `ip_deadline` | Last day of a statutory period at 12 offices (USPTO, EPO, EUIPO, TÜRKPATENT, WIPO, UKIPO, DPMA, JPO, CNIPA, KIPO, CIPO, IP Australia): months added with the end-of-month rule, then weekends and official closure days skipped; with the legal rule and the calendar source. Paris priority, PCT national phase, office action responses, renewal grace or a custom period. |
| `official_fees` | Official trademark filing fees of 13 offices and the Madrid route total in CHF (basic fee, individual or complementary fee per designated office, supplementary class fee), each with its source page and tariff date. Attorney fees are not included. |

**WIPO data: Madrid, Hague and PCT**

| Tool | What it returns |
|---|---|
| `madrid_refusals` | Provisional refusals on international (Madrid) registrations: refusing office, total or partial, notification date, response deadline, holder, representative. Open refusals with a deadline still running by default; filters for office, Nice class, office of origin and type. |
| `madrid_office_scorecard` | How an office treats Madrid designations: refusals and how they ended, time to the first decision, refusal rates by Nice class. |
| `madrid_top_representatives` | Firms with the most new Madrid filings and subsequent designations in the last 30 days, with their main clients. |
| `goods_services_terms` | Goods and services wording an office cut or limited in Madrid designations, by Nice class. |
| `pct_national_phase_leads` | PCT applications whose national phase deadline at a target office falls in the coming days (WIPO PATENTSCOPE gazettes). |
| `non_use_candidates` | Madrid registrations old enough for a non-use attack in 17 jurisdictions, ranked, with the statute. A candidate is not proof of non-use. |
| `competitor_filings` | Recent Madrid filings and the Madrid portfolio of a company or brand, with the countries designated. |
| `brand_expansions` | New international registrations and subsequent designations: brands adding countries. |
| `pct_applicant_tracker` | Profile of a PCT applicant or inventor: number of PCT applications and Madrid marks, main IPC and Nice classes, recent filings. |
| `hague_designs` | International designs (Hague) that designate an office: recent, expiring within 90 days, without a representative, or refused. |
| `madrid_renewals` (member) | International registrations expiring in the next 6 or 12 months, or in the renewal grace period. |
| `madrid_office_leads` (member) | Daily lists for the member's office: lapsed marks, transformation leads, open Paris priority, US and Philippine use declarations, owner and representative changes, foreign firms sending marks into the office. |
| `madrid_portfolio` (member) | A representative's Madrid portfolio: open provisional refusals with response deadlines, renewals and use declarations due. |
| `mark_watch_matches` (member) | Newly published Madrid marks similar to the member's own watch list on iprwise.com, with opposition deadlines. The list is edited on the website. |

## Installation

### claude.ai and the Claude desktop app

1. **Customize → Plugins → Add → Add marketplace**: type `asimokat/iprwise-claude-eklenti`.
2. Install the **iprwise** plugin from the **Discover** list.
3. In the plugin's **Connectors** tab, connect. An iprwise.com page opens: sign in with your IPRwise account (Google, LinkedIn, Microsoft or e-mail) and click **Allow**.

Installing the plugin does not sign anyone in; each user connects with their own IPRwise account. A free account is enough.

### Claude Code

```bash
claude plugin marketplace add asimokat/iprwise-claude-eklenti
claude plugin install iprwise@iprwise
```

Then type `/mcp` in a session, choose `plugin:iprwise:iprwise` and sign in.

### ChatGPT

The same connector is published as a ChatGPT plugin: [IPRwise on ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6add4fdcfe108191b262e938b47ad7a1). Open the page, click **Install**, and sign in with your IPRwise account when asked. Two skills (filing strategy, foreign associate) and a get-started skill come with it. ChatGPT may list fewer tools until OpenAI has reviewed the newest ones.

### Connector only (any MCP client)

- Endpoint: `https://www.iprwise.com/mcp` (Streamable HTTP, JSON responses)
- Authorization: OAuth 2.1 with sign-in by IPRwise account. Dynamic Client Registration (RFC 7591) at `https://www.iprwise.com/api/oauth/register`; public clients only; authorization code with PKCE `S256`; scopes `iprwise` and `offline_access`.
- Metadata: `https://www.iprwise.com/.well-known/oauth-protected-resource/mcp` and `https://www.iprwise.com/.well-known/oauth-authorization-server`
- Claude Code without the plugin: `claude mcp add --transport http --scope user iprwise https://www.iprwise.com/mcp`

Use the `www.` address exactly as written.

## What it sends and keeps

- The plugin connects only to `https://www.iprwise.com/mcp`. It contains no hooks, no local server and no programs.
- The text of your questions and tool arguments is not stored. For each tool call the server log keeps the tool name, the duration, the size of the answer, whether an error occurred, the client application's name and a shortened account identifier.
- Member tools use the country and verification status in your IPRwise profile to show the same lists you see on the website, and `mark_watch_matches` reads the watch list you saved on IPRwise. The connector cannot change your profile or watch list and cannot see your other private data, such as your messages, tasks or payment details.
- IPRwise keeps a daily call counter for each account (total calls, calls per tool, calls per client application) and deletes it 12 months after the day it covers.
- Codes and tokens are stored only as SHA-256 hashes. Authorization codes expire after 5 minutes, access tokens after 1 hour, refresh tokens after 30 days; expired records are deleted within 30 days. Application registrations are deleted 12 months after their last use.
- Limits: 300 tool calls a day per account; within that, up to 60 calls each for `find_attorneys`, `competitor_filings`, `non_use_candidates`, `madrid_portfolio`, `pct_applicant_tracker`, `madrid_refusals` and `hague_designs`. They reset at 00:00 UTC.
- Privacy policy (section 4, "AI assistants connected to your account"): https://www.iprwise.com/privacy · Terms: https://www.iprwise.com/terms

## Check the connection

Ask Claude: "Is Türkiye a member of the Madrid Protocol, and since when?" Claude should call `filing_routes` and answer: member since 1 January 1999.

The tools return reference information, not legal advice. Rules and fees change; confirm with a local attorney before acting.

## Türkçe

Bu eklenti Claude'u IPRwise'a (dünya genelindeki fikri mülkiyet vekilleri ağı) bağlar. Cevabı Claude yazar; IPRwise kaynağıyla doğrulanmış veriyi verir: resmî kaynaktan birebir alıntılı ülke bilgileri, uluslararası başvuru yolları, IPRwise vekil rehberi, IP ofisi duyuruları, Nice 2027 değişiklikleri, süre ve resmî harç hesapları ile IPRwise araçlar sayfalarındaki Madrid, Lahey ve PCT verileri (22 alet).

- **Kurulum (claude.ai ve masaüstü):** Customize → Plugins → Add → Add marketplace → `asimokat/iprwise-claude-eklenti` → **iprwise** eklentisini kur → Connectors sekmesinde bağlan → açılan iprwise.com sayfasında IPRwise hesabınla gir ve **İzin ver**.
- **ChatGPT:** aynı bağlayıcı ChatGPT eklentisi olarak yayında: [IPRwise on ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6add4fdcfe108191b262e938b47ad7a1). Sayfada **Eklentiyi kur**, sonra IPRwise hesabınla gir.
- **Ücretsiz hesap yeter.** Her kullanıcı kendi hesabıyla bağlanır.
- **Aletler yalnız okur;** IPRwise'a hiçbir şey yazmaz. Soru metni saklanmaz.
- **Üye aletleri** sitedeki kuralla çalışır: üye yalnız profilindeki ülkenin ofisini görür, doğrulanmamış üye ilk 10 satırı ve toplamı görür. Bunun için profildeki ülke ve doğrulama bilgisi kullanılır, izleme aleti üyenin kaydettiği listeyi okur.
- **Saklama:** günlük sayaç 12 ay sonra, süresi dolan bağlantı kayıtları 30 gün içinde silinir. Ayrıntı: [gizlilik metni, 4. bölüm](https://www.iprwise.com/kvkk).
- **Sınır:** hesap başına günde 300 çağrı; vekil araması ve canlı arama yapan altı aletin her biri bunun içinde en çok 60.

## License

MIT, see [LICENSE](LICENSE). The license covers the plugin files in this repository only; the IPRwise service and its data are subject to the [IPRwise terms](https://www.iprwise.com/terms).
