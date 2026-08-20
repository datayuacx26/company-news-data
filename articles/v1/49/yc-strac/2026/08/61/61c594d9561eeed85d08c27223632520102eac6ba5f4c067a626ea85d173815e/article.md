---
schema_version: "1.0.0"
document_id: "61c594d9561eeed85d08c27223632520102eac6ba5f4c067a626ea85d173815e"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/is-airtable-hipaa-compliant"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T20:27:21.764316+00:00"
fetched_at: "2026-08-18T20:27:23.268263+00:00"
content_hash: "sha256:45e190231bb319f52f68f1f9242b79224917c4c3d08555c4c7c295079a0cb0e6"
---

# Is Airtable HIPAA Compliant?

- **Is Airtable HIPAA compliant?** Only on the Enterprise Scale plan, with a signed BAA — not on Team or Business plans.
- Airtable routinely holds patient trackers, care-coordination bases, intake records, and the attachments teams drop into fields — PHI the moment a covered entity uses it.
- The newest risk is **AI over MCP** : connecting Airtable to an assistant like Claude Cowork (which carries no BAA) can pull that PHI to a model outside your BAA.
- **Strac** keeps PHI safe either way — redacting it on the MCP path and stopping it from being entered where it should not be.


## ✨ Is Airtable HIPAA Compliant?


**Only on the Enterprise Scale plan, with a signed BAA — not on Team or Business plans.** HIPAA compliance is never automatic — it depends on a signed Business Associate Agreement (BAA) with the vendor plus how you configure and use the tool. Below is where Airtable stands, where PHI ends up, and the AI/MCP gap most teams miss.


Connect Airtable to an AI assistant over MCP and PHI can leave your BAA. Strac MCP DLP redacts it on the path.


## Does Airtable Sign a BAA?


Airtable offers a BAA on its Enterprise Scale plan only. On Team or Business plans there is no BAA, so putting PHI in a base is a HIPAA violation. Even on Enterprise you have to control shared views, extensions, and automations that can copy records out of the base.


## Where PHI Ends Up in Airtable


Even teams that "don’t use Airtable for health data" accumulate PHI in it: patient trackers, care-coordination bases, intake records, and the attachments teams drop into fields. Under HIPAA, a single identifier tied to health information is enough to trigger the rules — so the question isn’t whether PHI *could* land in Airtable, it’s what protects it when it does.


## ✨ The New Risk: Airtable + AI Agents via MCP


Here is the gap even a BAA doesn’t close. The moment someone connects Airtable to an AI assistant over the[Model Context Protocol (MCP)](https://www.strac.io/blog/mcp-dlp) , an agent can query it and pull PHI straight into the model’s context. If that model is **Claude Cowork** — which Anthropic will not sign a BAA for — the PHI now sits outside your BAA perimeter, a reportable exposure even though Airtable itself is configured correctly.


Strac protects PHI across every surface — SaaS, browser, endpoint, MCP/AI, cloud, and DSPM — with one content-aware engine.


## How Strac Protects PHI in Airtable


**Strac MCP DLP** sits on the MCP path, detects PHI in every request and response, and redacts, masks, or blocks it before it reaches the assistant — so an AI that won’t sign a BAA never sees regulated data. And **Strac browser and endpoint DLP** catch PHI *before* it is pasted or typed into Airtable in the first place. Every action is audit-logged for HIPAA. See[MCP integrations](https://www.strac.io/mcp-integrations) and[Airtable MCP server](https://www.strac.io/blog/airtable-mcp-server) .


Strac detects and redacts sensitive data in real time — the same engine that protects PHI around Airtable.


## How to Use Airtable with PHI Safely


1. Confirm you are on Enterprise Scale with a signed BAA, disable public shared views, and review every extension and automation that moves data out.
2. Put Strac browser/endpoint DLP in front so PHI is caught before it is entered.
3. If you connect to AI/MCP, run Strac MCP DLP so PHI is redacted before it reaches the model.
4. Keep an audit trail of what PHI was detected, redacted, and by whom.


## 🌶️ Spicy FAQs on Airtable HIPAA Compliance


**Is Airtable HIPAA compliant in 2026?** Only on the Enterprise Scale plan, with a signed BAA — not on Team or Business plans.


**Can I use Airtable with Claude or ChatGPT and stay HIPAA compliant?** Only if PHI never reaches a model that isn’t under a BAA. Claude Cowork does not sign one, so connecting Airtable over MCP can expose PHI. Strac MCP DLP redacts PHI on the MCP path before it reaches the assistant.


**Does Airtable store PHI?** It can — patient trackers, care-coordination bases, intake records, and the attachments teams drop into fields. Whether or not that’s intended, it needs to be protected.
