---
schema_version: "1.0.0"
document_id: "e5838f149290970a36d9c005dd1f1b1d1b5c3b5ab5c881983f03a8aac53b881b"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/is-docusign-hipaa-compliant"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T20:27:21.764316+00:00"
fetched_at: "2026-08-18T20:27:23.268263+00:00"
content_hash: "sha256:7de0949a4206f954a419b8f928af921777fb726976d1899ec275c9eb1a99d474"
---

# Is DocuSign HIPAA Compliant?

- **Is DocuSign HIPAA compliant?** Yes — DocuSign signs a BAA, so it can be used for PHI when configured correctly.
- DocuSign routinely holds signed consent forms, HIPAA authorizations, intake paperwork, and the envelope fields that carry patient identifiers — PHI the moment a covered entity uses it.
- The newest risk is **AI over MCP** : connecting DocuSign to an assistant like Claude Cowork (which carries no BAA) can pull that PHI to a model outside your BAA.
- **Strac** keeps PHI safe either way — redacting it on the MCP path and stopping it from being entered where it should not be.


## ✨ Is DocuSign HIPAA Compliant?


**Yes — DocuSign signs a BAA, so it can be used for PHI when configured correctly.** HIPAA compliance is never automatic — it depends on a signed Business Associate Agreement (BAA) with the vendor plus how you configure and use the tool. Below is where DocuSign stands, where PHI ends up, and the AI/MCP gap most teams miss.


Connect DocuSign to an AI assistant over MCP and PHI can leave your BAA. Strac MCP DLP redacts it on the path.


## Does DocuSign Sign a BAA?


DocuSign will sign a Business Associate Agreement and publishes HIPAA guidance, which makes it one of the few e-signature platforms you can legitimately use for PHI — patient consent forms, HIPAA authorizations, and release-of-information documents — provided the BAA is active on your plan and you lock down access and audit logging.


## Where PHI Ends Up in DocuSign


Even teams that "don’t use DocuSign for health data" accumulate PHI in it: signed consent forms, HIPAA authorizations, intake paperwork, and the envelope fields that carry patient identifiers. Under HIPAA, a single identifier tied to health information is enough to trigger the rules — so the question isn’t whether PHI *could* land in DocuSign, it’s what protects it when it does.


## ✨ The New Risk: DocuSign + AI Agents via MCP


Here is the gap even a BAA doesn’t close. The moment someone connects DocuSign to an AI assistant over the[Model Context Protocol (MCP)](https://www.strac.io/blog/mcp-dlp) , an agent can query it and pull PHI straight into the model’s context. If that model is **Claude Cowork** — which Anthropic will not sign a BAA for — the PHI now sits outside your BAA perimeter, a reportable exposure even though DocuSign itself is configured correctly.


Strac protects PHI across every surface — SaaS, browser, endpoint, MCP/AI, cloud, and DSPM — with one content-aware engine.


## How Strac Protects PHI in DocuSign


**Strac MCP DLP** sits on the MCP path, detects PHI in every request and response, and redacts, masks, or blocks it before it reaches the assistant — so an AI that won’t sign a BAA never sees regulated data. And **Strac browser and endpoint DLP** catch PHI *before* it is pasted or typed into DocuSign in the first place. Every action is audit-logged for HIPAA. See[MCP integrations](https://www.strac.io/mcp-integrations) and[DocuSign MCP server](https://www.strac.io/blog/docusign-mcp-server) .


Strac detects and redacts sensitive data in real time — the same engine that protects PHI around DocuSign.


## How to Use DocuSign with PHI Safely


1. Execute the BAA, restrict envelope and template access by role, enable full audit trails, and keep PHI out of email subjects and notification previews.
2. Put Strac browser/endpoint DLP in front so PHI is caught before it is entered.
3. If you connect to AI/MCP, run Strac MCP DLP so PHI is redacted before it reaches the model.
4. Keep an audit trail of what PHI was detected, redacted, and by whom.


## 🌶️ Spicy FAQs on DocuSign HIPAA Compliance


**Is DocuSign HIPAA compliant in 2026?** Yes — DocuSign signs a BAA, so it can be used for PHI when configured correctly.


**Can I use DocuSign with Claude or ChatGPT and stay HIPAA compliant?** Only if PHI never reaches a model that isn’t under a BAA. Claude Cowork does not sign one, so connecting DocuSign over MCP can expose PHI. Strac MCP DLP redacts PHI on the MCP path before it reaches the assistant.


**Does DocuSign store PHI?** It can — signed consent forms, HIPAA authorizations, intake paperwork, and the envelope fields that carry patient identifiers. Whether or not that’s intended, it needs to be protected.
