---
schema_version: "1.0.0"
document_id: "de707463cd1eef9d1445621f26d62a67898b31f2d8a2f4277faffa394e8a4b6e"
company_key: "yc-feroot-security"
company: "Feroot Security"
source_id: "yc-feroot-security-news-import-5cc226b40d32"
canonical_url: "https://feroot.com/blog/ccpa-compliance-evidence-logs-run-time/"
published_at: "2026-03-10T15:40:37+00:00"
first_seen_at: "2026-07-22T07:46:19.024825+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:218f9bcdf0e692ff222aa4843b2f3ab03efdbcf9b1c9b93bee2413fced2ebc98"
---

# Proving CCPA Compliance: Logs, Reports, and Run-Time Evidence

CCPA used to audit your policies and paperwork. Then came the Sephora settlement, and things moved to logs, run-time data, and network reports.


The company’s privacy policy said it didn’t sell consumer data. California’s AG ran the site, watched the cookies and pixels fire, and found that in reality, they did. Healthline followed in 2025.[Then Disney in 2026](https://www.feroot.com/blog/ccpa-consent-audit-disney-fine/) . Different companies, common findings. Data gets collected and shared with third parties via tags. GPC gets ignored.[Opt-out toggles don’t always stop the data.](https://www.feroot.com/blog/ccpa-opt-out-vs-consent-compliance/)


*California kept finding the same pattern repeatedly. So it tightened the grip.*


CPPA no longer grades your policies. It tests whether your opt-outs actually worked, whether GPC signals were honored, and whether your notice matched your actual data flows.


If you don’t have immutable, tamper-evident logs showing that opt-outs successfully suppressed downstream data flows last Monday at 2 PM, CPPA would see it as a gap in your defense.


In this article, we’ll break down the evidence you need to prove CCPA compliance and what it takes to generate it continuously.


## What you’ll learn


- Why policy documentation alone no longer satisfies California regulators, and what technical evidence CPPA actually expects to see.
- What consent enforcement, data flow documentation, and run-time monitoring evidence look like in practice, and how each layer builds your compliance defense.
- How to generate evidence to prove CCPA compliance continuously, so you’re prepared before an inquiry arrives, not scrambling after one does.


## Why policy documentation is not enough


A synthesis of multiple cookie banner audits found that over 40% of websites still set tracking cookies without valid consent, and around 60% failed to delete cookies after revocation. And three-quarters of sites failed to notify at least one third party when consent was withdrawn, meaning some vendors kept tracking regardless of what the UI conveyed.


Similarly, a journal titled “Tracking in the Post-Cookie Era” found that more than 75% of tracking activity occurred before users could meaningfully register consent and that tracking often intensified after users withdrew consent.


### This is not a handful of bad actors but a structural pattern


CCPA’s enforcement example page is full of such instances. Opt-out toggles that don’t lead anywhere. Monitoring and detection gaps that leave misconfiguration unpatched for weeks. Tags firing on pages carrying protected health information. And cookie preference links that were broken and stayed broken.


In each case, the documentation and the policy said the right things. But the connection between the policy and the system failed.


### CPPA noticed that the gap is in the stack, not the policy


At the heart of the issue are tags and third-party scripts that run on your website. A privacy notice gets written, reviewed, and published. Meanwhile, your tag manager keeps loading third-party scripts on page render, before the consent state is even evaluated.


Those scripts attach listeners, read identifiers, and fire outbound calls in milliseconds.


And these third-party scripts and tags change constantly. Marketing rotates them. Vendors push updates at their cadence. But your documentation and reviews stay static, and the gap between your policies and reality widens.


Documentation can’t close that gap because it doesn’t move fast enough, and it doesn’t observe anything. Policies and documentation are table stakes now. no longer the proof.


## Three categories of compliance evidence


When regulators investigate, they’re not looking at one type of evidence. At the base is policy documentation, spanning privacy notices, data retention disclosures, and DPIAs. Then comes operational evidence, tying those policies to implementation. Vendor contracts, training logs, and version history go here.


On top of them sits run-time monitoring evidence. It proves how well those controls worked, when they drifted, and how quickly issues were caught and fixed. Regulators focus more on this layer because it’s the only one that can’t be written after the fact.


Let’s look at these layers a bit deeper.


### Category 1: Policy documentation is what conveys your intent


Documents like privacy notices, DPIAs, training materials, and vendor agreements establish what your program is designed to do. Regulators expect this layer to exist, but it’s table stakes now. It rarely wins an investigation on its own.


### Category 2: Operational logs prove those policies were implemented


Consent records, DSAR audit trails, data flow inventories, and rights request logs show that your program is actually running. This layer proves effort and process, but it still doesn’t prove outcomes.


### Category 3: Run-time evidence proves the controls worked


Continuous monitoring of your run-time environment, consent enforcement verification, and tamper-evident logs of actual data flows show that your controls functioned the way your policy said they would. This is the layer California has been demanding in every major settlement since Sephora.


Here’s how those layers break down into specific evidence types.


**Evidence Type** **What It Proves** **Examples**


**Policy Documentation** Stated compliance intentions Privacy notices, procedures, vendor agreements


**Consent Logs** Opt-out and consent preferences were captured with context Timestamped consent records with preference details


**Enforcement Logs** Network-level records showing which tags and pixels stopped firing after opt-out Records of data collection ceasing after opt-out, per vendor


**Data Flow Reports** Mapping of which vendors received what data categories and under which consent state Documentation of third-party data transmission by category


**Run-Time Monitoring** Continuous verification that consent and opt-out signals are honored across all surfaces Real-time verification that consent blocks data collection


**Historical Records** Defensible evidence that compliance posture held during a specific time window Audit trails showing consistent compliance posture


## Consent enforcement evidence


Having a consent banner on your website conveys your intent. But it doesn’t prove it works. It’s just the UI. Evidence is what happens at the network layer and in the backend after the user makes a choice.


When a user opts out of sale or sharing, either via link, a consent banner, or an opt-out preference signal like GPC, any third-party tracking technologies used for cross-context behavioral advertising must not fire in a way that communicates personal information for those purposes.


So to prove that it really worked, that no data flowed to third-party services, you need logs that map opt-out inputs to timestamped sessions when consumers exercised them, and show the system respected those choices.


### What those logs look like and where they come from


Your CMP is the starting point. It should be generating consent state records showing which users opted out and when. From there, your tag manager picks up the thread and should present the logs you need to prove which tags were suppressed as a direct result of that choice. That’s two layers, but neither of them tells you what actually leaves your website.


That’s where your network monitoring layer matters. It captures outbound calls and confirms whether ad-tech vendors stopped receiving identifiers for opted-out sessions. And your GPP string logs close the loop, showing that the correct opt-out signal was encoded and transmitted downstream to third-party partners.


Together, those four sources create a chain of evidence that runs from the user’s choice all the way to the vendor’s receipt of it.


### The gap that regulators keep finding


[The Healthline and Disney cases both showed the same failure.](https://www.feroot.com/blog/ccpa-consent-audit-disney-fine/) The UI registered the choice. But the internal systems didn’t follow. Opt-out toggles changed state on screen while pixels kept firing in the background. GPC signals were acknowledged but never honored, and data kept flowing to third-party ad partners downstream.


## Data flow documentation


Your privacy notice discloses categories of data you collect and the third parties you share it with. Data flow documentation proves that disclosure is accurate.


Under CCPA/CPRA, a defensible data flow map has to be more than a high-level diagram. At minimum, businesses must be able to disclose the types of personal information collected, the sources, the purposes, and who it gets shared with or sold to.


Regulations also expect you to define categories of third parties with enough granularity that consumers can meaningfully understand who receives their data.


For example, name the advertising networks, analytics providers, or data brokers where the data might go. CPRA amendments further require businesses to disclose retention periods or the criteria used to set them, reinforcing that data flows must be tied to the lifecycle, not just recipients.


Meeting those requirements on paper is straightforward. Keeping them accurate is where things get complicated.


### The challenge is matching the pace of your tracking environment


Privacy notices are written once and updated periodically. In between those updates, your tracking environment changes constantly. Marketing adds pixels. Vendors push updates. Product ships new features. Every one of those changes is a potential divergence between your notice and your actual data flows.


Regulators have been explicit about this. And from their point of view, your data map either reflects reality in real-time, or it’s stale.


### Run-time monitoring as compliance proof


Regulators demand to see that you were compliant throughout. Point-in-time assessments only prove you were compliant at the time of testing. In between reviews, you’re flying blind.


Your evidence needs to clearly outline that consent mechanisms are verified continuously and control data collection in real time. And when the environment drifts, regulators expect that your systems detect that instantly and trigger alerts for remediation.


That means detection of consent enforcement failures as they occur, rather than weeks later during a quarterly review. And ongoing validation that your privacy notice accuracy holds as your stack evolves.


### Why continuous matters


Your controls don’t fail on the day you implement them. They drift as new campaigns go live, vendors ship new features, and your third-party scripts load new dependencies.


By[your next periodic audit](https://www.feroot.com/blog/ccpa-audit-preparation-regulator-evidence/) , the gap widens with each passing day and jeopardizes the data of thousands of users across your websites.


The Healthline case makes the perfect example for this. They had the opt-out mechanisms alongside an active consent banner. Yet, somewhere between the user’s choice and the third-party ad partners downstream, the signal broke down, and data on health-related pages started flowing to third-party networks via tags and tracking scripts.


Continuous monitoring of outbound calls, mapped against consent state, would have surfaced that pattern early. Instead, it went unnoticed and only surfaced in an AG investigation.


That’s the difference between a compliance program built on periodic reviews and one built on continuous evidence.


## Building an evidence generation program


Strip away the complexity, and an evidence-ready privacy program comes down to three things. What you log. How do you protect it? How long do you keep it? CCPA gives you some of the minimums. The rest is discipline.


### Step 1: Automate at the point where compliance happens


CCPA requires 24 months of consumer request records at a minimum. But the evidence gap regulators keep finding goes well beyond DSARs. Consent decisions, opt-out enforcement actions, and tag-firing behavior all need to be captured at the moment they occur, not reconstructed later from memory and devtools screenshots.


To do that, it takes instrumenting the run-time boundary where compliance actually happens. Every consent state change, every opt-out signal, every outbound tag call needs to be captured as a structured event, timestamped, and packaged into evidence automatically.


Not exported manually from your CMP once a quarter, or pulled from your tag manager on an ad-hoc basis. It takes evidence that’s generated continuously, at the source, as a byproduct of your stack doing its job.


### Step 2: Capture decisions and enforcement actions in tamper-evident logs


When a regulator reviews your logs, the first question they ask is whether they can be trusted. A log that lives in a shared spreadsheet, a ticketing tool with editable fields, or an internal wiki page doesn’t carry investigative credibility because nothing stops it from being changed after the fact.


That’s what tamper-evidence solves. Append-only, time-stamped, digitally signed logs create a record that can be verified as unaltered. NIST, ISO 27001, and PCI DSS all converge on this principle because it applies equally to security and privacy.


### Step 3: Generate regular reports that document your compliance posture


Regular reporting against your logs creates a documented compliance posture over time, the kind that shows a regulator consistent behavior rather than a single clean snapshot taken a few weeks before an inquiry.


That means reports get generated on a defined cadence, monthly at minimum, that pull from your consent logs, enforcement records, and data flow observations into a structured summary.


And these reports clearly answer the questions auditors would most likely ask, like which properties were monitored? Which consent signals were tested and passed? Which third-party tags were active and under what consent states?


That documentation discipline transforms raw logs into a compliance narrative that a regulator can actually follow.


### Step 4: Capture historical records as your audit timeline


Settlements like Healthline and Disney impose multi-year monitoring obligations. This sheds light on regulators’ methodology, that they scrutinize records from previous years. Thus, your retention policy needs to match the window regulators can look through, for CCPA, which means up to 24 months as the minimum.


That requires a clear plan that documents and captures evidence with investigation in mind. And to do that, logs need to be segmented by time and jurisdiction, and automation that can collect all that without engineering bandwidth.


## How DXComply generates compliance evidence


Everything that we have discussed so far in the article, like automating at the point of compliance, tamper-evident logging, continuous consent verification, and regulator-ready reporting, requires a system built specifically to[prove CCPA compliance with evidence](https://www.feroot.com/alphaprivacy/) .


Because without continuous monitoring and automated evidence collection, you can show a regulator your “Do not sell my personal information” link, but you can’t produce network-level proof that specific third-party pixels stopped firing for opted-out users during a given period.


You can have an inventory that’s manually maintained, but it might not reconcile with what a live crawl of your sites and apps would actually show. And your CMP’s dashboard can paint an overarching picture, but if asked to prove if GPC or opt-out inputs were respected over time, you’d still need to stitch together screenshots and log captures from different places.


Without dedicated tooling, you end up with what regulators increasingly call out. Good-faith intent. Weak technical evidence.


### DXComply closes the gap


It continuously monitors the digital experience layer to automatically log all data collection activity across your websites and apps, and maps technologies to third-party recipients. And it does it all while continuously verifying that consent and opt-out signals are honored at the network layer.


When it finds an anomaly, like a script drifting, a misconfigured tag, or tags firing on pages that collect sensitive data, it triggers triage.


Every event is stored in tamper-evident, append-only logs with retention aligned to CCPA’s requirements. And when an inquiry arrives, it assembles exportable, regulator-ready evidence packages that map run-time observations directly to legal concepts.


## The bottom line


Proving CCPA compliance has moved past documentation. Regulators are reading your network traffic, testing your opt-out flows, and asking for evidence that covers a period of minimum 24 months, not just the audit day. To hold up under that scrutiny, organizations need to generate tamper-evident, continuous proof as a byproduct of how their privacy program runs.


[Schedule a demo to see how DXCompl](https://www.feroot.com/demo-alphaprivacy-ai/) y automatically generates the logs, reports, and run-time evidence that prove CCPA compliance, before a regulator asks for it.
