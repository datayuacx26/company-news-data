---
schema_version: "1.0.0"
document_id: "76bbd2b5421690d6676db68b47c219fd6ecb1bef0dd456d75a68f289d45bb241"
company_key: "yc-sixtyfour"
company: "Sixtyfour"
source_id: "yc-sixtyfour-news-import-61b6ef0fd08d"
canonical_url: "https://www.sixtyfour.ai/blogs/how-we-use-agents-for-identity-resolution-on-banned-scammers"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-22T13:40:53.215025+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:c9bee6f03cd8c867b64852a17cbd3e085c4e79dc26aa42b06ff700e55ad1bb5b"
---

# How We Use Agents for Identity Resolution on Banned Scammers

Device fingerprinting works on the bottom 90% of operators, but the top 10% rotate hardware before breakfast. Here is how to stop them.


Field Brief


-


Sift recorded a 354% year-over-year increase in account takeover attacks, driven largely by operators bypassing traditional device fingerprints.


-


Most trust and safety teams respond by banning the account, but banned scammers return within hours using clean IPs and fresh emails, creating the empty footprint anomaly.


-


In theory, this can be solved by tracing a single banned username back through old forums, social accounts, and breach data to find the real person.


-


Our agents do exactly that, cross referencing clear web public records, underground marketplaces, and proprietary databases to map the human behind the keyboard.


The Account Takeover and Ban Evasion Loop


Sift recorded a 354% year-over-year increase in account takeover attacks in Q2 2023. Trust and safety teams ban these compromised accounts daily. The operators simply return the next morning with a clean IP and fresh email address.


This creates the empty footprint anomaly. We define this as a signup method with zero historical presence across breaches, social platforms, or public records. A real person has years of digital depth tied to their primary email. They ordered food, bought shoes, and registered for forums. A returning bad actor has an account created 12 hours ago.


> What's driving a majority of the fraud risk in the last 12 to 18 months has been the sharing of schemes like credit washing and stolen social security numbers on social media. — Frank McKenna, Chief Fraud Strategist at Point Predictive


If a platform relies purely on session telemetry, the attacker already has the playbook to bypass it.


How to Map Cross Platform Fraud Rings


The FBI's Internet Crime Complaint Center reported $12.5 billion in losses across 880,418 complaints in 2023. A massive portion of that volume flows through repeat offenders using burner identities. Real investigations prove that isolated bans do not disrupt these networks. Identity resolution proves exactly how these networks unravel.


Tracing the Reused Username


A marketplace customer banned a top scammer. Twelve hours later, a new applicant appeared. The only mistake the operator made was reusing the same username across niche forums even when he rotated primary emails.


The banned username appeared on seven different platforms. One of those matches traced back to a 2016 gaming forum. The operator had posted there using his real first name. That single post connected to a Reddit account listing his home city. The Reddit account led directly to an email exposed in a 2021 breach data dump. That breach record contained his full legal name. The operator thought he was anonymous. He was actually leaving a traceable map.


How Identity Resolution Works


We do not rely on session telemetry or IP geofencing to map these networks. We built an investigative layer that cross references fragmented signals across the internet.


### The Input Layer


Investigators input a single identifier into the platform. This can be an email, a phone number, a Telegram handle, a cryptocurrency wallet, or a marketplace seller ID.


Our agents query multiple surfaces in parallel. The system searches Sixtyfour proprietary identity databases, dark web underground mentions, and historical breach records detailing password reuse. It maps social profiles across platforms like LinkedIn, GitHub, Reddit, Steam, and Discord. It pulls clear web public records, including LLC registries, court filings, and marketplace seller pages.


### Resolving the Graph


LLM reasoning weighs the quality of these signals. A phone number surfacing in a 2023 breach record tied to an email represents a high confidence link. That same email appearing in two account rental Telegram groups forms another edge.


The output is a clear graph. It shows the connected accounts, the confidence scores for each edge, and the exact platforms the operator uses.


What This Means for Trust and Safety Stacks


Trust and safety teams must adapt their entry gates. Device fingerprinting catches the operator who reuses one phone. It does not catch the operator who buys a $30 burner SIM for each new account.


Teams should monitor for the empty footprint anomaly first. It catches the highest confidence synthetic accounts immediately at onboarding. An email with zero history on GitHub, Reddit, or historical breaches is rarely a high value customer.


In 2023, FinCEN issued advisories explicitly noting that state sponsored actors, including the Lazarus Group, exploit weak onboarding checks to launder funds. Stopping these actors requires looking past the device.


Second, teams must implement identity resolution on banned operators to understand the blast radius. If you do not map the entire network, banning one account just forces the operator to switch tabs. The goal is removing the human behind the keyboard, not just burning their current alias.
