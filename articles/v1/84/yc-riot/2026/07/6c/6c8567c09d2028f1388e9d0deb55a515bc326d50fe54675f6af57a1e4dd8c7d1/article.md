---
schema_version: "1.0.0"
document_id: "6c8567c09d2028f1388e9d0deb55a515bc326d50fe54675f6af57a1e4dd8c7d1"
company_key: "yc-riot"
company: "Riot"
source_id: "yc-riot-news-import-992df223e3e3"
canonical_url: "https://tryriot.com/blog/slash-email-security/"
published_at: null
first_seen_at: "2026-07-24T00:01:10.886871+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:b5ef2558e4cb3b22c88432f88210cbf424ea259becaf289e75f88a8e6b28b88b"
---

# Introducing Slash: Email Security for the New Generation of Phishing

Email is still the #1 attack vector into every company – but the attacks hitting our inboxes are a different beast from the ones traditional email security stacks were built to catch. AI has changed what phishing looks like, and established defenses can’t keep up.


**That’s why we're introducing Slash, Riot’s new approach to email security built for the new generation of AI-crafted phishing.**


## The new generation of email security threats


Phishing used to be easy to spot. Bad grammar, spoofed senders, sloppy formatting – the tells were (mostly) obvious. Now, that era is over.


AI has given attackers the ability to write emails that are ultra-sophisticated, contextually convincing, and indistinguishable from legitimate correspondence. They mimic internal tone, reference real projects, and exploit publicly available information to feel just as authentic as the real thing. And worse, these tailored attacks can be launched at scale.


These trends are showing up in the numbers. According to our data, at a typical company with 200 employees, about 150 attacks slip through existing defenses every month. And with data breaches costing companies[$4.4m USD on average](https://www.ibm.com/reports/data-breach) , every phishing attack that does make it through has the potential to cause huge damage.


## Why traditional email security falls short


To understand why[Slash](https://tryriot.com/slash/) works differently, it helps to understand the three ways existing email security tools are hitting a wall.


### 1. Speed constraints of MX-level filtering


Most email security solutions are plugged directly into your DNS MX record. This is simple, but it comes with a hard constraint: any detection has to occur fast enough to avoid delaying mail delivery. Deep, AI-powered analysis of the kind actually required to catch sophisticated phishing simply takes more time than an MX-level filter can afford.


To manage this limitation, these tools default to shallow, rule-based checks, because that's all their architecture allows.


### 2. The false positive problem


The second issue is noise. Legacy tools rely heavily on static-rule banners slapped onto emails, like "external sender," "exercise caution," and similar generic warnings. When everything gets flagged, nothing feels dangerous anymore. Employees learn to ignore the banners entirely, making them essentially pointless.


### 3. Email security tools built for yesterday's attacks


Email security tools were built to detect obvious malicious payloads: infected attachments, corrupted links, and suspicious senders. That’s because yesterday's threats were technical and identifiable.


Today, AI-generated phishing emails are indistinguishable from legitimate business communication. There’s no suspicious payload, and no red flag – just a perfectly crafted message. That’s why yesterday’s tools can't keep up.


In short, we need something different – so we built it.


## Slash: post-delivery detection built for modern phishing


Slash takes a fundamentally different approach: it operates **post-delivery** . By stepping outside the MX-level speed constraint, Slash can afford to run the kind of thorough, AI-powered email security detection that today’s ultra-sophisticated attacks actually require.


Even better? It does it all without holding up your mail flow. Here’s how.


### What Slash actually checks


Slash combines multiple layers of email security detection into a single system:


- **Semantic analysis** across content, headers, URLs, and attachments – looking for the underlying phishing scenario, not just surface-level red flags.
- **Static checks** , including DMARC, DKIM, SPF, and domain reputation.
- **An LLM trained on real attack examples** , updated daily, which catches 97% of the attacks that make it past the static checks.
- **URL and QR code analysis** , since attackers increasingly hide malicious links behind redirects or QR codes to dodge conventional scanning tools.
- **AI-powered attachment analysis** , which surfaces[hidden malicious content](https://insight.scmagazineuk.com/pdf-and-html-email-attachments-often-malicious) buried inside PDFs, Word documents, and other file types.


### The network effect: Riot's attack database


Every Slash customer benefits from Riot’s shared attack database. When an attack is resolved for one customer, the domain reputation data generated from that resolution strengthens protection for the entire network.


Joining Slash means joining a system that gets smarter with every threat it blocks – not just for you, but for everyone on it.


### More signal, less noise


Because we’ve built such a thorough detection system, Slash can afford to be selective about when it *actually* alerts employees. The result: a false positive rate under 1%, compared to between 8–10% for competing solutions.


This difference is huge. It's the difference between employees who trust every alert they see and employees who've learned to click through warnings without reading them.


## Inbox: from manual triage to automated response


Of course, detection is only half the problem. What happens *after* an employee reports a suspicious email matters just as much as what comes before.


Before we launched[Inbox](https://tryriot.com/inbox/) , that process was manual. An admin had to open the reported email, investigate it themselves, and decide whether it was malicious or legitimate – every single time. It’s a slow, repetitive process, and it’s tough to scale.


With Inbox, we’re automating that entire investigation. Inbox runs the checks, gathers the signals, and shows the full reasoning behind its verdict: every check, every signal, and clear rationale for why an email is malicious or safe. No more manual digging through headers and links to reach a conclusion someone else could reach for you automatically.


### Why speed matters here


With the new wave of phishing attacks, speed isn't a nice-to-have – it's the whole point. A malicious email can reach employees within minutes of being sent. A manual investigation and response, by contrast, can take hours. Every step in that chain – detection, triage, remediation – creates a bigger window of exposure for us.


Thanks to Inbox, we’re automating as much of that chain as possible – including remediation decisions – to close that window as tightly as it can.


## Attack resolution: what happens after detection


Catching a threat is one thing. Resolving it permanently without creating more work for your team is another.


### Block threats


When Slash blocks an attack, it's blocked company-wide, and it stays blocked. From this point onwards, similar attacks are automatically filtered on receipt, with no further admin intervention required. In other words, you're not fighting the same threat twice.


### Automatic identity verification


Here’s a classic attack: an employee receives an urgent email that appears to be from their founder, asking for help closing a deal while "traveling in New York" – a detail that's often publicly available on LinkedIn and trivially easy for an attacker to exploit.


This kind of CEO fraud is one of the most effective attack types precisely because it weaponizes psychology, not just technology. But we’ve found a solution.


When Slash detects a potential impersonation, our AI agent Albert reaches out directly to the impersonated colleague through a **secure secondary channel** – Slack, Teams, or similar – to confirm whether or not they’ve actually sent the email.


If the impersonation is confirmed, the email is moved to spam across the entire organization automatically, with no admin intervention required.


### Employee follow-up notification


When employees report a suspicious email, they don't just get silence in return. They receive the final verdict, a "What gave it away?" breakdown listing the specific signals that were detected, and a thank-you for their vigilance. This is our way of closing the loop, reinforcing secure behavior, and building stronger habits over time.


## Slash + Inbox: shrinking the exposure window


Individually, Slash and Inbox each solve part of the email security problem. Together, they compress it, shrinking the exposure window:


- **Inbox** automates triage and response the moment a suspicious email is reported, cutting admin workload and response time.
- **Slash** goes a step further upstream, alerting employees directly from within the email itself and actively encouraging reporting, rather than waiting for someone to notice something's wrong on their own.


Working together, these two tools ensure that as many high-confidence threats as possible are filtered out before they ever reach an employee’s inbox. This way, the exposure window – the gap between an attack landing and a threat being neutralized – shrinks to nearly zero.


## With Slash, we’re rethinking email security for the AI era


As phishing gets more sophisticated, email security tools have got to get deeper, not just faster. Post-delivery detection isn't a workaround – it's what makes real AI-powered analysis possible in the first place.


That’s why we’ve built Slash and Inbox on that principle, empowering your teams to make better, safer decisions from detection all the way through to resolution.


**Want to see Slash in action?[Book a demo](https://tryriot.com/demo/?utm_source=direct_traffic&utm_medium=blog) to see how it can change your email security for good.**


## FAQ


1. **What is Slash?** Slash is Riot’s email security tool for the new generation of phishing, giving employees instant alerts inside suspicious emails and stopping attacks before anyone clicks.
2. **What permissions does Slash require?** Slash needs read access to analyze emails and write access to add alerts directly in Google and Microsoft mailboxes.
3. **What if I need to restore an email?** Admins can easily restore purged emails directly from Inbox.
4. **How does Slash interact with Simulation?** Slash doesn't analyze simulation emails, preserving their training value. While Slash catches sophisticated threats, phishing tactics evolve constantly – Simulation continues to train employees to stay vigilant, as no detection is ever 100% perfect.
5. **Does Slash slow down email delivery?** Emails arrive normally. Slash analyzes them instantly after reception.
6. **Does it work with other anti-spam tools?** Slash works seamlessly alongside existing email security tools. While anti-spams filter emails upstream, Slash operates directly in the user's mailbox via APIs – complementing your defenses without interference.


[Spear Phishing in 2026: How AI-Powered Attacks Bypass Traditional Defenses Benjamin Netter Founder](https://tryriot.com/blog/what-is-spear-phishing/)[3,000 Years Later, What Can The Odyssey Tell Us About the Birth of Social Engineering? Tom Baragwanath Head of Content](https://tryriot.com/blog/odyssey-social-engineering-interview/)[Phishing Training for Employees: The Complete 2026 Guide (That Actually Changes Behavior) Tom Baragwanath Head of Content](https://tryriot.com/blog/phishing-training-for-employees/)[Shadow IT: Why It Happens, Why It's Risky, and How DLP Can Help Thibault Bernard Product Manager](https://tryriot.com/blog/shadow-it/)[Ransomware Training: How to Reduce Risk Without Wasting Budget Benjamin Netter Founder](https://tryriot.com/blog/ransomware-training/)[Smishing Explained: The Threat Vector CISOs Can No Longer Ignore Tom Baragwanath Head of Content](https://tryriot.com/blog/smishing-attack/)
