---
schema_version: "1.0.0"
document_id: "757b878b381de153a64fea6b0c5646982541ae6fcf09b320d060098003610d66"
company_key: "pegasystems-inc-common-stock"
company: "Pegasystems Inc."
source_id: "pegasystems-inc-common-stock-news-import-897a88525e2a"
canonical_url: "https://www.pega.com/insights/articles/your-data-eu-it-truly-under-your-control"
published_at: "2026-06-03T12:00:00+00:00"
first_seen_at: "2026-07-22T08:29:10.657811+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:697846b962c49c5cb2dd6332d80e7cfa2ca0ea21e3487232258aec0d2a9e09ef"
---

# Your data is in the EU. But is it truly under your control?

If you work in a regulated industry in Europe, you've probably had some version of this conversation…


Your security or legal team is in the middle of a cloud procurement review. Things are moving along. Then someone, an auditor, a regulator, a data protection officer, asks a question that isn't about where the data is stored but who can see it. Who administers the systems it lives in. Whether the people with access to your environment are operating under EU jurisdiction. Whether you can actually prove any of that.


And the honest answer is: maybe. Probably. We think so?


That's not the kind of answer that moves procurement forward. It's the kind of answer that stalls it for months.


Five years ago, it was usually enough to say, “Our data stays in the EU.” But now regulators, auditors, and enterprises are focused on operational sovereignty: not only where data resides, but who can access it, whether personnel reside within the EU, how that access is governed, and whether operations are governed and enforceable under EU legal frameworks.


If your organization is navigating these questions, you're not alone.


## The compliance conversation has moved


GDPR set the foundation. But the regulatory landscape has continued to build. The Schrems II decision, NIS2, DORA, EBA and EIOPA guidance on cloud outsourcing, national data protection mandates that vary by country, the frameworks organizations must navigate are layered and, frankly, getting more specific about what "compliant" actually requires.


The difficulty isn't usually that organizations don't care about getting this right. Most do. The difficulty is that the standard cloud setup, even a reputable provider with EU-region data centers, wasn't designed to answer the new questions being asked. When an auditor asks whether personnel outside the EU can access your production environment, the answer from a standard deployment is often "technically yes, but under strict controls." That answer requires explanation. It requires evidence and it opens conversation threads that slow everything down.


The organizations feeling this most acutely are usually the ones where data sovereignty isn't a compliance preference, it's a hard requirement. Government agencies handling citizen data. Banks subject to strict regulatory scrutiny over how customer data is managed and by whom. Healthcare organizations whose data protection obligations extend to how systems are administered, not just where files are stored.


For these organizations, the gap between "data residency" and "data sovereignty" isn't semantic. It has real consequences, for procurement cycles, for regulatory conversations, for the ability to demonstrate to stakeholders that sensitive data is genuinely protected at every layer.


## What organizations are actually looking for


When we talk to teams dealing with these pressures, a few themes come up consistently.


They want clarity on what data stays within the boundary and what doesn't, so they can complete Data Protection Impact Assessments without having to chase down answers from multiple teams.


They want their most sensitive data to be protected even with privileged operator access, not just through policy and process, but through technical controls they own. Something they can point to and say: even if someone with elevated access tried to look at this data, they couldn't, because we control the keys.


And they want a path to stronger sovereignty while preserving the platform capabilities that are already embedded in their operations. A “more compliant but less capable” tradeoff doesn’t work for organizations that rely on advanced workflow automation and AI-driven decisioning to serve customers.


## How Pega delivers operational sovereignty in the EU


The Pega EU Service Boundary (EUSB) supports organizations that need stronger operational sovereignty controls. It provides an approach that extends beyond data residency, with EU-based infrastructure, EU-resident personnel for operations and support, and documented, auditable access controls.


The four things it delivers:


1. **EU residents manage and support your environment, around the clock.** This is the piece that tends to matter most in regulatory conversations. Day-to-day operations, cloud management, and client support are performed exclusively by EU-resident personnel. The Global Operations Center that orchestrates your[Pega Cloud environment](https://www.pega.com/products/cloud) is isolated within the EU Service Boundary. When someone asks who touched your data, the answer is clear.
2. **Your critical data stays in the boundary.** Cloud Data Storage, Cloud File Storage, Decision Data Storage, and backups are always stored within the EU Service Boundary, on the AWS European Sovereign Cloud in Germany. This is physically and logically isolated infrastructure, not a shared global environment with a European label on it.
3. **Access is controlled, documented, and auditable.** Role-based access controls, least-privilege principles, multi-factor authentication, secure VDI access, bastion hosts, and full logging and monitoring. These controls aren't unique to the EUSB, what's different is that they are applied within a boundary where every operator is an EU resident, closing the gap that other deployments leave open.
4. **You control the keys to your most sensitive data.** The EUSB requires clients to implement Bring Your Own Key (BYOK) encryption for sensitive data fields. Your encryption keys live in a key management system that you control, outside your Pega Cloud environment. It means that even Pega operations personnel can't read that data without your keys, which is exactly the kind of control that satisfies risk committees and data protection authorities.


None of this requires trading away platform capability. The[full Pega product suite](https://www.pega.com/products) operates within the EUSB. Your teams keep working the way they work. The sovereignty layer is underneath.


## Limitations to consider


It’s worth being explicit: the EU Service Boundary can help meet operational sovereignty requirements, but it doesn’t answer every legal or regulatory question that may come up.


In particular, it doesn’t remove every cross-border legal or regulatory consideration that can arise from a provider’s corporate domicile and applicable laws. It also doesn’t fully resolve the range of issues and risk assessments many organizations associate with Schrems II.


That said, the EUSB can still materially strengthen an organization’s sovereignty posture and provide clearer answers during procurement and assurance activities. For many teams, it helps address common auditor questions and reduces uncertainty for internal stakeholders.


## Putting it into practice


There isn't a one-size-fits-all answer to EU data sovereignty. What the EU Service Boundary offers, and whether it's the right fit, depends on your specific requirements, your regulatory environment, and where your current gaps actually are. Reach out to your Pega account team and let's figure out what the right path looks like for your organization.


*Want to learn more about Pega's approach to security and compliance? Visit[pega.com/trust](https://www.pega.com/trust) or read the[EU Service Boundary press release](https://www.pega.com/about/news/press-releases/pega-collaborates-aws-deliver-pega-eu-service-boundary) .*
