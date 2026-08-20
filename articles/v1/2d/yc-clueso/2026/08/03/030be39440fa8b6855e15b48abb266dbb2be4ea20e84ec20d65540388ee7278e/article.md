---
schema_version: "1.0.0"
document_id: "030be39440fa8b6855e15b48abb266dbb2be4ea20e84ec20d65540388ee7278e"
company_key: "yc-clueso"
company: "Clueso"
source_id: "yc-clueso-news-import-fdc543697114"
canonical_url: "https://www.clueso.io/blog/how-to-blur-part-of-a-video"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T16:53:27.700400+00:00"
fetched_at: "2026-08-18T16:53:29.108728+00:00"
content_hash: "sha256:f22c2fe45c733fa4f57404a7eb4413f87c3921a63ea80c5dcc90f717ef68a0ca"
---

# How to record product demos without exposing customer data

## Building a clean demo environment before you hit record


A dedicated demo tenant keeps customer information out of the recording from the start. Build it separately instead of cloning a production account, since a clone can preserve names, files, and hidden metadata. Match the production interface and feature settings closely enough for the workflow you plan to show, and pick[screen recording software](https://www.clueso.io/solutions/screen-recording-software) that gives you control over exactly what the capture includes.


1.


**Create accounts for each role in the demo.** Use a company-owned domain and invented names, such asmaya@northstar-demo.example . Give each account only the permissions required for the recording. Check profile photos, account menus, and user lists because those areas can expose whoever created the tenant.


2.


**Seed realistic dummy records.** Add fabricated customers, tickets, projects, or invoices that support the exact story in your script. Keep the records internally consistent. For example, an invoice should reference the same dummy company and contact shown elsewhere. Use clearly fake payment details and never copy production records as templates.


3.


**Disable connections to live systems.** Disconnect shared inboxes, CRM syncs, payment processors, analytics feeds, and webhooks before recording. An integration can import a real contact or trigger a notification even when every visible record started as dummy data. Replace required connections with sandbox accounts where possible.


4.


**Prepare each screen before recording.** Open the correct record, set filters, clear recent searches, and close unrelated browser tabs. Turn off desktop notifications and messaging previews. Reset the demo tenant to a saved starting state before every take so earlier attempts do not change counts or activity histories.


5.


**Run the full script once without recording.** Watch menus, search suggestions, autofill fields, and error messages for unexpected data. Confirm that every click stays inside the demo tenant. If the walkthrough requires a production screen, identify it in advance so you can apply redaction or synthetic replacement during editing rather than discovering the exposure after publication.


Once the environment is ready, an[AI screen recorder](https://www.clueso.io/features/screen-recorder) that captures cleanly and hands off straight into editing saves a re-record if something still slips through.
