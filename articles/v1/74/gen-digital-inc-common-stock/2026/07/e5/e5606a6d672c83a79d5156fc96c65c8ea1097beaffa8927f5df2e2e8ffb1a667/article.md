---
schema_version: "1.0.0"
document_id: "e5606a6d672c83a79d5156fc96c65c8ea1097beaffa8927f5df2e2e8ffb1a667"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/insights/reports/threat-report-h1-2026"
published_at: "2026-07-15T12:30:00+00:00"
first_seen_at: "2026-07-22T22:46:07.456131+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:d5771651ea83ff00c90509a9480558909746c94d4e2b92a9b1cff264a2930138"
---

# Threat Report H1 2026

Security Threats & Scams


## Malicious activity moved into trusted context


In H1 2026, many of the most effective attacks did not look like attacks at first. They arrived through hotel conversations, support pages, booking processes, business inboxes, cloud-hosted pages, developer tools and social media ads. The mechanism varied, but the logic was consistent: attackers placed malicious activity inside contexts that users, platforms and security systems already trust.


### Tech support scams moved through familiar infrastructure


Tech support scam detections reached 20.3 million blocked attacks in H1 2026, up 61.6% half-over-half. Part of this increase still reflects broader detection coverage, after Gen introduced a dedicated URL filtering capability for this type of content, but coverage was not the whole story. Gen also observed strong tech support scam campaigns using content hosted on ondigitalocean\[.\]app domains, which helped the scams appear on legitimate-looking cloud infrastructure. The United States remained the largest single target, with 4.67 million blocked attacks. France and Germany together slightly surpassed the US in raw attack count, with 2.54 million and 2.3 million blocked attacks, respectively. Japan also stood out, with nearly 2.2 million blocked attacks. Windows users accounted for 92% of blocked tech support scam attacks, showing that this platform is particularly targeted. Often, Tech Support Scams resemble alerts from Windows Defender.


Tech support scam targeting Apple users


The infrastructure behind these Tech Support Scams often looked legitimate at first glance. In Germany and France, fake Windows error pages impersonated Windows Defender Security Center, used various Windows-style error codes and were hosted on Google Cloud Storage, making the pages look more credible to victims and harder to dismiss as obvious fraud. In France, adult-site malvertising redirected users to fake support pages. In Australia and the US, some campaigns reached victims through Facebook advertising. Other attacks used fake PayPal receipt PDFs, including Microsoft 365-themed lures, to create a billing dispute that victims were told to resolve by calling fraudulent support. The scam script was familiar, but the delivery was better aligned with situations where users already expect errors, invoices, payments or customer support.


### Impersonation scams used official pressure and personal urgency


Government impersonation was the main driver behind the rise in imposter scams. Gen blocked almost 1 million government scam attacks in H1 2026, up 387% from H2 2025. The number of protected users more than doubled, but the activity was heavily concentrated in the United States, which accounted for almost 800K blocked attacks alone, or 81% of the total. One single campaign shown on the image accounted for the majority of the blocks by itself, showing how a single campaign can dominate the half-year numbers when the lure and delivery path scale.


Family impersonation scams also rose sharply, up 454.2% over the second half of 2025. The activity was concentrated in Western Europe, led by the Netherlands, France, Ireland and Germany. These campaigns mostly reached Android users through SMS, and one SMS campaign accounted for 39% of all blocked family scam attacks in the period. Some documented cases used AI voice cloning, but the core technique remains older than AI: urgency, emotional pressure and a message that appears to come from someone the victim already knows. AI can make that cheaper and more convincing. It does not replace the social engineering, it improves the packaging.


Family impersonation SMS Government impersonation lure from the H1 2026 campaign set. The page borrows official-looking IRS framing and moves the user toward downloading and running an app.


### Fake e-shops and scam-yourself attacks scaled through repeatable flows


E-shop scams (fake online stores) became one of the highest-volume scam categories in H1 2026. Gen blocked 114.2 million e-shop scam attacks, up 109% from H2 2025. The US accounted for 38.9 million blocked attacks. Western Europe was also heavily targeted, with the UK, Germany, France, Italy and Spain together accounting for 30.9 million blocked attacks. One large variant accounted for more than 10 million blocked attacks in which threat actors registered .click domains promoted through online ads. In practice, this means a victim could see an ad for a convincing-looking online store, click through to a disposable shopping site and be pushed toward a fake checkout or payment flow. The scale shows how quickly one repeatable store template can spread once the ads, domains and payment lure are in place.


Fake tutorial scams, part of the broader “Scam-Yourself Attack” pattern, increased 193% in H1 2026, rising from 1.79 million to 5.26 million blocked attacks. These attacks do not always try to break into the device directly. Instead, they guide the user into doing the attacker’s work: following fake instructions, copying commands, approving prompts or completing deceptive verification flows. Many sites imitated familiar anti-bot and security checks, including Cloudflare-style messages such as “Checking if you are human” and “needs to review the security of your connection before proceeding.” FakeCaptcha variants were a major component of this activity, with two campaigns alone accounting for more than 416K blocked attacks. Windows users were disproportionately targeted, accounting for almost 87% of blocked fake tutorial attacks, but the activity was also visible on Android and iOS, making it a cross-platform scam pattern rather than a Windows-only problem.


Security Threats & Scams: H1 2026 Signals


#### Tech Support Scams


20.3M


Blocked attacks, up 61.6% vs H2 2025.


#### Imposter Scams


1M


Reached almost 1M blocked attacks, up 387% H/H.


#### Family Impersonation


5x increase


Family scams spread in Western Europe.


#### Scam Ad Machine


4.51M


Scam-related ads in the analyzed dataset.


#### E-shop Scams


114.2M


Blocked attacks, up 109% vs H2 2025.


#### Fake Tutorial Scams


+193%


5.26M blocked attacks in the “scam yourself” pattern.


### Reservation hijacking put fraud inside real customer service


[Reservation Hijack scams](https://www.gendigital.com/blog/insights/research/reservation-hijack-scam) are another good example of scammers breaking into trusted daily digital flows. The attack uses real reservation context, and in stronger cases compromised hotel-side or partner workflows, to make fraud feel like normal customer service. Victims may receive WhatsApp, iMessage, SMS, email or booking-platform messages that reference a real trip, a real property or a real payment issue. Once enough of the message is true, the false part becomes much harder to detect.


### Malvertising gave fraud operators reach


Scam advertising gave fraud operators a different advantage: reach. In our[Scam Ad Machine research](https://www.gendigital.com/blog/insights/research/scam-ad-machine-meta) , we analyzed 14.57 million ads representing 10.76 billion impressions across the EU and UK over 23 days. We identified 4.51 million scam-related ads, nearly one in three ads in the dataset, generating 143.8 million impressions in the EU and 304.11 million impressions across the EU and UK in less than a month.


The[follow-up research](https://www.gendigital.com/blog/insights/research/the-scam-ad-machine-part-ll) showed why this model survives individual takedowns. Scam advertisers used multi-facet ads, disposable advertiser accounts, deceptive presentation tricks and short-lived campaign tactics. A scam ad does not need to stay online forever. It only needs to look acceptable long enough to reach the right audience.


### Banking trojans and RATs were wrapped in local context


Malware campaigns followed a similar trust-abuse pattern, but with more regional tailoring. Banking trojan campaigns in Czechia, Slovakia and Poland used email-based JavaScript droppers that impersonated normal business correspondence, such as shipment notices, scanned document notifications or invoice-related emails. One lure simply told the recipient: “Dear Customer, I am attaching a scanned copy of the shipment.” In some cases, the messages came from compromised corporate mailboxes, which helped them look legitimate to recipients and email security systems. The payload modified proxy settings and installed browser add-ons to intercept banking sessions. In Brazil, fake payment PDFs impersonated ecommerce and banking brands to target the Boleto payment ecosystem.


RAT (Remote Access Trojan) campaigns were also localized and staged. In Italy, fake invoice PDFs, including Booking.com-themed lures, showed realistic Italian error dialogs, then delivered Vercel-hosted scripts with per-victim JavaScript obfuscation before moving to Blogspot-hosted PowerShell stages. In Poland, fake invoices delivered a steganographic .NET loader that installed Remcos RAT. In Czechia, a compromised third-party domain served multi-stage PowerShell that installed Babylon RAT. The payload families were not the novelty. The delivery layer was doing the work: local language, familiar documents, credible hosting and several steps before the final malware.


### Infostealers and crypto malware fed the next stage of fraud


Infostealers remained one of the main upstream suppliers for account takeover and identity fraud. Gen Threat Labs identified[Remus, a new 64-bit infostealer attributed to the Lumma Stealer family](https://www.gendigital.com/blog/insights/research/remus-64bit-variant-of-lumma-stealer) . The analysis tied Remus to Lumma through shared obfuscation, string handling, syscall behavior and browser credential-theft techniques, and documented an Application-Bound Encryption bypass used by Remus and Lumma.


Crypto appeared in H1 2026 not only as a scam lure, but also as part of malware infrastructure. One cryptocurrency campaign used a four-stage infection chain that ended in a Rust-compiled multi-coin clipboard hijacker. The malware monitored the clipboard for wallet addresses across 21 blockchain types, including BTC, ETH and LTC. When a victim copied a wallet address, the malware replaced it with an attacker-controlled one, redirecting any pasted transaction to the criminals.


The same campaign also used Binance Smart Chain for command-and-control resolution through EtherHiding. Rather than relying only on a hardcoded C2 domain that can be seized or taken offline, the malware retrieved infrastructure pointers from data stored on-chain. That makes disruption harder: defenders can still block the resolved infrastructure, but they cannot remove the blockchain record like they would a malicious domain.


Telemetry deep dive


### A Kimsuky-looking spike turned out to be coinminer activity


Most of this report looks at trends. But when a line suddenly jumps, the first job is not to explain the trend, it is to explain the jump.


That happened in January. Gen telemetry showed a visible increase in activity initially associated with obfuscation techniques previously observed in Kimsuky-related campaigns. At first glance, that could look like a possible actor signal. The campaign told a different story.


The rest of the evidence did not fit Kimsuky activity. The campaign targeted general consumers, not espionage targets. The lures referenced OnlyFans dumps and BTC wallets in VBS filenames. The payloads, code patterns and domains did not resemble known Kimsuky operations, and the activity led to coinminer detections rather than espionage tooling.


After analysis, the increase was classified as coinminer activity. Reused techniques can make telemetry noisy, especially when a detection is tied to behavior that later appears in unrelated campaigns. A spike has to be checked against the payload, lure, infrastructure, affected users and geography before it becomes a conclusion.


Obfuscation techniques previously associated with Kimsuky-related activity appearing in coinminer campaigns, 7-day moving average


### Software supply chain attacks abused developer trust


Developer tools and software packages also became a delivery route. Instead of targeting each victim directly, attackers compromised pieces of the software supply chain: public package repositories, developer accounts, code repositories and update processes. These are systems developers use every day, which is exactly why they are attractive. If a trusted package or repository is altered, malicious code can reach users through a normal install or update.


H1 2026 saw several examples of this model. Attackers compromised[Node Package Manager (npm) and Python Package Index (PyPI) packages](https://safedep.io/mass-npm-supply-chain-attack-tanstack-mistral/) ,[hijacked maintainer accounts](https://www.stepsecurity.io/blog/axios-compromised-on-npm-malicious-versions-drop-remote-access-trojan) to publish malicious versions, and used[compromised GitHub accounts](https://www.stepsecurity.io/blog/forcememo-hundreds-of-github-python-repos-compromised-via-account-takeover-and-force-push) to push malware into existing repositories while preserving a convincing commit history. AI developer tooling was also affected: in one case, a compromised npm publishing token was used to push an unauthorized[Cline CLI update](https://thehackernews.com/2026/02/cline-cli-230-supply-chain-attack.html) that installed OpenClaw on developer systems during an eight-hour window.


These incidents are different in detail, but the lesson is the same: the software supply chain is a trust system. Attackers are not only trying to fool end users with fake pages and scam messages. They are also trying to compromise the tools and components that legitimate developers trust, so the malicious code arrives through channels that look routine.


Gen Threat Labs saw a related version of the same trust problem on macOS. In March, we documented a[cracked-software distribution chain](https://www.gendigital.com/blog/insights/research/macos-cracked-software-malware-risks) where users looking for free or older versions of Mac apps were pushed through mirror sites, torrent uploads, forums, Telegram channels and repacked archives. In one tracked wave, Gen protections blocked about 108,000 attempts to launch these applications in the first 48 hours. The payloads included cryptominers, infostealers, backdoors and adware, but the more important mechanism was permission abuse: install guides often told users to disable Gatekeeper, turn off System Integrity Protection, run crack tools with administrator privileges or grant Full Disk Access. A Mac user may think they are only bypassing a license check. In practice, they may be giving an unsigned binary the permissions it needs to steal data, persist on the machine or use the device for someone else’s profit.


The main lesson from H1 2026 is that attackers are not abandoning old tactics. They are placing them inside trusted systems with better timing and better context.
