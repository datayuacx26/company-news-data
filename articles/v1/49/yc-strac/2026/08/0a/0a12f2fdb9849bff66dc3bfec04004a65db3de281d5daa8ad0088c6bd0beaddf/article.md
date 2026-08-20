---
schema_version: "1.0.0"
document_id: "0a12f2fdb9849bff66dc3bfec04004a65db3de281d5daa8ad0088c6bd0beaddf"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/is-shopify-hipaa-compliant"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T20:27:21.764316+00:00"
fetched_at: "2026-08-18T20:27:23.268263+00:00"
content_hash: "sha256:f463e9620e6e92fa33b2de19d151415c47839638197e3557c2a19203f3300e59"
---

# Is Shopify HIPAA Compliant?

- **Is Shopify HIPAA compliant?** No — Shopify does not sign a BAA, so it is not HIPAA compliant for PHI.
- Shopify routinely holds customer health questionnaires, prescription or condition data, and order notes for health products — PHI the moment a covered entity uses it.
- The newest risk is **AI over MCP** : connecting Shopify to an assistant like Claude Cowork (which carries no BAA) can pull that PHI to a model outside your BAA.
- **Strac** keeps PHI safe either way — redacting it on the MCP path and stopping it from being entered where it should not be.


## ✨ Is Shopify HIPAA Compliant?


**No — Shopify does not sign a BAA, so it is not HIPAA compliant for PHI.** HIPAA compliance is never automatic — it depends on a signed Business Associate Agreement (BAA) with the vendor plus how you configure and use the tool. Below is where Shopify stands, where PHI ends up, and the AI/MCP gap most teams miss.


Connect Shopify to an AI assistant over MCP and PHI can leave your BAA. Strac MCP DLP redacts it on the path.


## Does Shopify Sign a BAA?


Shopify does not offer a BAA and is not HIPAA compliant. Health-and-wellness merchants often collect information that edges into PHI — conditions, prescriptions, health questionnaires — and that data is unprotected inside Shopify customer and order records.


## Where PHI Ends Up in Shopify


Even teams that "don’t use Shopify for health data" accumulate PHI in it: customer health questionnaires, prescription or condition data, and order notes for health products. Under HIPAA, a single identifier tied to health information is enough to trigger the rules — so the question isn’t whether PHI *could* land in Shopify, it’s what protects it when it does.


## ✨ The New Risk: Shopify + AI Agents via MCP


Because Shopify already carries no BAA, connecting it to AI makes a bad situation worse. An agent querying Shopify over the[Model Context Protocol (MCP)](https://www.strac.io/blog/mcp-dlp) pulls PHI into a model’s context — and assistants like **Claude Cowork** carry no BAA of their own. The regulated data is unprotected at both ends.


Strac protects PHI across every surface — SaaS, browser, endpoint, MCP/AI, cloud, and DSPM — with one content-aware engine.


## How Strac Protects PHI in Shopify


**Strac MCP DLP** sits on the MCP path, detects PHI in every request and response, and redacts, masks, or blocks it before it reaches the assistant — so an AI that won’t sign a BAA never sees regulated data. And **Strac browser and endpoint DLP** catch PHI *before* it is pasted or typed into Shopify in the first place. Every action is audit-logged for HIPAA. See[MCP integrations](https://www.strac.io/mcp-integrations) and[Shopify MCP server](https://www.strac.io/blog/shopify-mcp-server) .


Strac detects and redacts sensitive data in real time — the same engine that protects PHI around Shopify.


## How to Use Shopify with PHI Safely


1. Keep PHI out of Shopify product, customer, and order fields. Use browser DLP to stop PHI being entered into forms, and move any health intake to a BAA-covered system.
2. Put Strac browser/endpoint DLP in front so PHI is caught before it is entered.
3. If you connect to AI/MCP, run Strac MCP DLP so PHI is redacted before it reaches the model.
4. Keep an audit trail of what PHI was detected, redacted, and by whom.


## 🌶️ Spicy FAQs on Shopify HIPAA Compliance


**Is Shopify HIPAA compliant in 2026?** No — Shopify does not sign a BAA, so it is not HIPAA compliant for PHI.


**Can I use Shopify with Claude or ChatGPT and stay HIPAA compliant?** Only if PHI never reaches a model that isn’t under a BAA. Claude Cowork does not sign one, so connecting Shopify over MCP can expose PHI. Strac MCP DLP redacts PHI on the MCP path before it reaches the assistant.


**Does Shopify store PHI?** It can — customer health questionnaires, prescription or condition data, and order notes for health products. Whether or not that’s intended, it needs to be protected.
