---
schema_version: "1.0.0"
document_id: "dded881a9c261e97f6b967f99f3b534ae889b79656ddc856dc9505b0062ac8a8"
company_key: "yc-aravolta"
company: "Aravolta"
source_id: "yc-aravolta-news-import-85aed3e09b97"
canonical_url: "https://www.aravolta.com/blog/why-dcim-needs-to-change"
published_at: null
first_seen_at: "2026-07-21T07:27:31.872668+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:0725a8ffc5bafbbc384a68ff67177daaf9383198fa46d8bf9fd9c7088854cabe"
---

# Why DCIM Needs to Change

If you run a colocation facility, you already know the pain. Your DCIM tool tracks assets. Your BMS watches cooling. Your EPMS handles power. Your billing lives in a spreadsheet. None of these systems talk to each other, and you are the integration layer. This is the status quo for most colo operators, and it has been for over a decade.


## DCIM Was Never Built for Colos


The major DCIM platforms on the market today were designed for a specific customer: large enterprises managing their own data centers. Think banks, insurance companies, and government agencies with thousands of servers they own and operate themselves. The core job was asset management. Where is server X? What rack is it in? When does the warranty expire?


Nlyte started as an asset and capacity planning tool for enterprise IT departments. Sunbird (dcTrack) was built around the same idea: model your racks, track your ports, manage your cabling. Schneider's EcoStruxure IT grew out of their existing building management and UPS monitoring stack, bolting on DCIM features over time. None of them were built for how data centers actually operate today.


But colocation is a fundamentally different business. You don't just manage assets. You sell power and space to tenants who bring their own equipment. You need to meter every circuit, bill accurately every month, keep cooling within SLA, and do it all with a lean operations team running 24/7. The enterprise DCIM vendors never prioritized these workflows because their paying customers didn't need them.


## The Real Problems Colo Operators Face


Talk to anyone running operations at a 5-50MW colocation facility, and you will hear the same set of frustrations over and over:


### Billing That Still Runs on Spreadsheets


Power billing is the core revenue model for most colos, yet the actual billing process is often manual. Someone pulls meter reads from the EPMS, copies them into a spreadsheet, applies rate structures, and generates invoices. This happens monthly. It is error-prone. Disputes eat up staff time. And when a customer asks for a breakdown of their power usage by circuit, the answer is usually "give us a few days."


### Three Vendors for One Building


A typical colo might have a BMS from Siemens or Honeywell for cooling and environmental monitoring, power metering from Schneider or Eaton, and a DCIM tool from Nlyte or Sunbird for asset tracking. Each system has its own interface, its own alerting, its own data model. When something goes wrong at 2 AM, the on-call engineer has to check three different dashboards to piece together what is happening. That is not a workflow. That is a scavenger hunt.


### Deployments That Take Months


Traditional DCIM deployments are projects, not products. You sign a contract, then spend weeks in discovery. Then weeks modeling your facility. Then weeks training your staff. Professional services bills stack up. Six months later, you have a system that mostly works if someone remembers to keep it updated. This is why so many DCIM deployments end up as expensive shelfware.


### Manual Device Onboarding


Every time a new customer deploys equipment, someone has to manually enter assets into the DCIM. Rack location, power connections, network ports, customer assignment. In a busy colo doing multiple deployments a week, this data entry backlog means the DCIM is always out of date. And an out-of-date DCIM is worse than no DCIM at all, because people stop trusting it.


## What the Enterprise Vendors Got Wrong


Nlyte, Sunbird, and Schneider built software for enterprise IT departments, not for data center operators. The mismatch shows up in specific ways:


- **No native power billing.** Enterprise data centers don't bill tenants for power, so DCIM vendors never built billing workflows. Colo operators have to export data and handle billing externally.
- **Cooling and power in separate tools.** In an enterprise data center, the facilities team handles HVAC and the IT team handles servers. Different people, different tools. In a colo, the same ops team manages both, and they need one view.
- **Tenant isolation is an afterthought.** Enterprise DCIM assumes one organization owns everything. Multi-tenancy, customer portals, and per-tenant reporting were never part of the original design.
- **Pricing based on rack count or sensor count.** This penalizes dense colo environments and makes costs unpredictable as you grow.
- **On-premise first.** Many legacy platforms still require local servers and database infrastructure, adding operational overhead for your already-stretched team.


## What Colocation Operators Actually Need


After spending years talking to colo operators, the wish list is remarkably consistent:


- **Integrated power metering and billing** : Pull meter data automatically, apply rate structures, generate invoices, and let customers see their own usage. All in one place.
- **Cooling and environmental monitoring** : Temperature, humidity, and airflow data alongside power data. Not in a separate BMS that the ops team has to alt-tab into.
- **Alerting that works at 2 AM** : A single alerting system that covers power, cooling, and capacity. Escalation paths that match how your NOC actually works. Not three separate alert streams from three separate vendors.
- **Fast deployment** : Days or weeks, not months. Connect your meters, map your infrastructure, start getting value. Professional services should be optional, not mandatory.
- **Device discovery that reduces manual work** : Scan the network, identify what is connected, and pre-populate asset records. The goal is not zero manual entry, but a lot less of it.
- **Tenant-aware from the start** : Every feature should understand that you have multiple customers sharing infrastructure. Per-tenant dashboards, per-tenant billing, per-tenant SLA tracking.


### Why This Hasn't Changed Until Now


The honest answer is that the colo market was not large enough to attract dedicated software development. Enterprise IT departments were the big spenders, so vendors built for them. Colo operators made do with a patchwork of tools because there was nothing better available.


That is changing. The colocation market has grown significantly, and operators are actively looking for software that fits their actual workflows. The incumbents are trying to retrofit multi-tenant features onto enterprise architectures, but that approach has limits. Building for colo from the ground up produces a fundamentally different product.


## The Path Forward


If you are evaluating DCIM platforms for a colocation facility, ask vendors direct questions. Can I generate a tenant power bill from this system without exporting to a spreadsheet? Can I see power and cooling on the same dashboard? How long does deployment take, and what does it cost? Can my customers log in and see their own data?


If the answer involves "professional services engagement" or "we can customize that," you are looking at the wrong tool. These should be standard features, not custom projects.


Aravolta was built from day one for colocation operators. Power metering, billing, cooling monitoring, and operations in a single platform. We did not start with an enterprise asset tracker and try to bolt on colo features. We started with the problems colo teams deal with every day and built the software around those.
