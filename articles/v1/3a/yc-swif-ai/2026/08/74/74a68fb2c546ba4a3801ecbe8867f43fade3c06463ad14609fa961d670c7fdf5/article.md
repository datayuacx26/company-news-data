---
schema_version: "1.0.0"
document_id: "74a68fb2c546ba4a3801ecbe8867f43fade3c06463ad14609fa961d670c7fdf5"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/password-statistics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T10:00:33.682122+00:00"
fetched_at: "2026-08-05T10:00:35.732088+00:00"
content_hash: "sha256:3ace7a0ac0ae54f619c6a044093f37d05d2857358dfe66398123c80944c24103"
---

# Password Statistics for 2026: Reuse, Cracks, Breaches, and the Passkey Shift

Passwords are still the dominant authenticator on the internet, and they are still the thing attackers most reliably exploit. The[Verizon 2025 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) attributes 22% of breaches to compromised credentials as the initial access vector, and finds that 88% of basic web application attacks involved stolen credentials.[Microsoft’s 2025 Digital Defense Report](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) counted more than 7,000 password attacks per second, with 97% of identity attacks taking the form of password spray. Analysis of more than a billion malware-stolen passwords by[Specops Software and Outpost24](https://specopssoft.com/blog/report-one-billion-malware-stolen-credentials/) found that 98.5% of them are weak. The figures below are the password statistics IT, security, and compliance teams should be tracking in 2026.


## **Key password statistics at a glance**


- **22%** of breaches in 2025 used stolen credentials as the initial access vector, per the[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) . Credentials remain the second most common entry point, just ahead of vulnerability exploitation at 20%.
- **1.08 billion** malware-stolen passwords were captured and analyzed over a 12-month period by[Specops and KrakenLabs](https://specopssoft.com/blog/report-one-billion-malware-stolen-credentials/) . Of a 10-million sample, 98.5% met the modern definition of a weak password.
- **97%** of identity attacks in 2025 were password spray attempts, and Microsoft blocked an average of more than 7,000 password attacks per second, per the[Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) .
- **78%** of the world’s most common passwords can be cracked in less than a second, up from 70% a year earlier, per[NordPass and NordStellar](https://nordsecurity.com/press-area/americans-most-common-password-is-secret-) . "Secret" is now the most common password in the United States.
- **3%** of compromised passwords met basic complexity requirements in the infostealer data analyzed in the[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) , and the median user had only 49% unique passwords across their accounts.
- **19%** of authentication attempts to SSO providers reviewed by Verizon were credential stuffing, per the[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) .
- **3 weeks** is now enough to brute-force an eight-character lowercase password using twelve consumer RTX 5090 GPUs against bcrypt, per the[2025 Hive Systems Password Table](https://www.hivesystems.com/blog/are-your-passwords-in-the-green) . Year over year, consumer GPU cracking speed dropped cracking times by roughly 20%.
- **69%** of online users have at least one passkey, 75% recognize the term, and 48% of the top 100 websites now support passkeys, per the[FIDO Alliance](https://fidoalliance.org/fido-alliance-champions-widespread-passkey-adoption-and-a-passwordless-future-on-world-passkey-day-2025/) .


**$4.44 million** was the global average cost of a data breach in 2025, with credential-driven attacks remaining a core part of the story, per the[IBM Cost of a Data Breach Report 2025](https://www.ibm.com/reports/data-breach) .


‍


### **Compromised credentials and the breach economy in 2026**


Compromised credentials are not the only path into an enterprise, but they are still the most reliable one. The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) attributes 22% of breaches to stolen credentials as the initial access vector, just ahead of vulnerability exploitation at 20%. Within the basic web application attack pattern, the dependence is even more lopsided: 88% involved stolen credentials, with brute force and credential stuffing the most common tactics.


The economics line up. The[IBM Cost of a Data Breach Report 2025](https://www.ibm.com/reports/data-breach) put the global average breach cost at $4.44 million, down 9% from $4.88 million the year before, but the US average climbed to $10.22 million. IBM’s analysts have begun summarizing modern intrusion practice with the phrase "attackers are logging in, not hacking in," which is consistent with what Verizon and Microsoft are seeing on the wire.


[Microsoft’s 2025 Digital Defense Report](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) adds the volumetric layer. Microsoft saw more than 7,000 password attacks per second in 2024, and 97% of all identity attacks were password spray. Out of 15.9 billion account creation requests in the first half of 2025, more than 90% came from bots, and 45% of login attempts were "valid username, wrong password," confirming how widely usernames are already exposed.


### **The most common passwords in 2026 are still the worst ones**


Seven editions in, the[NordPass and NordStellar Top 200 research](https://nordsecurity.com/press-area/americans-most-common-password-is-secret-) continues to find that the world’s most common password is "123456." It has held that title in six of the last seven years. "Admin" is second globally, and the United States now has its own grim distinction: the most common password is "secret." The dataset for the 2025 edition was built from public breaches and dark-web repositories collected between September 2024 and September 2025, across 44 countries.


The most striking number from the same NordPass research is that 78% of the world’s most common passwords can be cracked in less than a second, up from 70% the year before. Forty percent of the most common passwords used by individuals and by business representatives are the same, meaning the home-account problem walks straight into corporate logins.


Specops’ view from the malware logs is consistent. The[Specops 2025 Breached Password Report](https://specopssoft.com/blog/report-one-billion-malware-stolen-credentials/) lists the top five stolen passwords as "123456," "admin," "12345678," "password," and "Password." Specops also reports that 230 million stolen passwords actually met traditional complexity requirements (eight or more characters, one capital, one number, one symbol), which is precisely why NIST has moved away from those rules.


### **Password reuse and the credential stuffing pipeline**


Reuse is what turns a single breach into a wave. Infostealer infection data analyzed in the[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) shows that in the median case only 49% of a user’s passwords are distinct from each other, meaning more than half are duplicates across services. Verizon’s same dataset found that only 3% of compromised passwords met basic complexity standards, so the typical victim is reusing weak passwords as well as a few stronger ones.


That reuse fuels credential stuffing, which is now a measurable share of normal authentication traffic. The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) analyzed SSO provider logs and found that the median daily share of credential stuffing was 19% of all authentication attempts. In other words, on a typical day, roughly one in five login attempts to the average SSO tenant is an attacker trying credentials harvested elsewhere.


### **Infostealers are the new password breach**


The credential supply chain has changed. Old-style breaches still happen, but the largest source of fresh stolen passwords today is infostealer malware running on consumer and corporate endpoints.[Specops and Outpost24](https://specopssoft.com/blog/report-one-billion-malware-stolen-credentials/) captured and analyzed 1,089,342,532 stolen passwords over a single 12-month window pulled from infostealer logs, almost entirely through KrakenLabs threat intelligence. The volume continues to climb year over year.


Quality is the other story.[Specops](https://specopssoft.com/blog/breached-passwords-analysis-heatmap/) analyzed a 10-million random sample from that 1-billion-plus list and found that 98.5% of the breached passwords were weak by the report’s definition (under 15 characters, or fewer than two character classes). Strong, long, complex passwords are the rare exception in the infostealer corpus, not the rule.


For defenders the practical takeaway is that any password an employee has typed on an infected personal device should be considered compromised, regardless of how strong it is, how rarely it is reused, or how long the company’s rotation policy is. The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) specifically calls out infostealer-derived credentials as a pathway into corporate SaaS and SSO environments, since those credentials often bypass legacy network controls.


### **How long does it actually take to crack a password in 2026**


Cracking economics keep moving against defenders. The[2025 Hive Systems Password Table](https://www.hivesystems.com/blog/are-your-passwords-in-the-green) tested twelve RTX 5090 GPUs against bcrypt with a work factor of 10. An eight-character lowercase password fell in about three weeks. An eight-digit numeric PIN fell in 15 minutes. An eight-character password using upper case, lower case, numbers, and symbols held up far better, requiring roughly 165 years against the same rig.


Year over year, consumer GPU cracking sped up by about 20%, per[Hive Systems](https://www.hivesystems.com/blog/are-your-passwords-in-the-green) . The bigger jump came from AI accelerators. Hive’s same research found that AI-grade hardware (the kind powering large language models) accelerated cracking by more than 1.8 billion percent compared with consumer GPUs on certain workloads, collapsing what used to be a billions-of-years timeline into a few hours. The implication is that the security margin from a strong-but-short password is rapidly disappearing.


This is the practical reason[NIST Special Publication 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) (the modern federal standard for digital authentication) now treats length as the primary control. The current guidance requires a minimum of eight characters when MFA is in place and recommends or requires longer passphrases (up to 15 characters or more) for single-factor scenarios. NIST also tells systems to support at least 64 characters and to drop forced periodic rotation and composition rules, both of which it found made users pick weaker passwords.


### **Passkey adoption and the passwordless shift**


Passkeys finally crossed the line from "interesting" to "default" in 2025. According to the[FIDO Alliance](https://fidoalliance.org/fido-alliance-champions-widespread-passkey-adoption-and-a-passwordless-future-on-world-passkey-day-2025/) , 75% of consumers now recognize passkeys (up from 39% in 2023), and 69% have at least one passkey set up. Forty-eight percent of the top 100 websites support passkeys, more than double the share in 2022, and the total number of accounts that can use passkeys passed 15 billion, roughly doubling year over year.


The performance gap is striking. The[FIDO Passkey Index](https://fidoalliance.org/wp-content/uploads/2025/10/FIDO-Passkey-Index-October-2025.pdf) reports a 93% passkey login success rate compared with 63% for traditional passwords, and a 30% conversion lift on flows that offer passkey sign-in. On the enterprise side, FIDO finds that roughly 87% of businesses have deployed or are deploying passkeys, a sharp jump from prior years.


Consumer behavior is starting to follow. The[FIDO Alliance](https://fidoalliance.org/fido-alliance-champions-widespread-passkey-adoption-and-a-passwordless-future-on-world-passkey-day-2025/) reports that 54% of consumers familiar with passkeys say they are more convenient than passwords and 53% say they are more secure, while 47% of consumers abandon a purchase when they forget the password for that account. Passwords are still the dominant authenticator, but the economic case for moving away from them is now visible in the funnel data.


### **MFA, password spray, and the limits of "just add a second factor"**


Multi-factor authentication blunts most credential attacks but does not eliminate them.[Microsoft](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) attributes 97% of identity attacks to password spray, which targets accounts with weak or reused passwords where the second factor is missing or weak. Less than 1% of identity attacks fall into other categories such as SIM swapping, MFA fatigue, adversary-in-the-middle phishing, and token theft, per Microsoft, but those categories are growing quickly. Microsoft reported a 146% increase in adversary-in-the-middle phishing attacks year over year.


The conclusion across all three primary datasets is the same: SMS and one-time passcode MFA is increasingly bypassable, while phishing-resistant factors (FIDO2 security keys and passkeys) remain effective. The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) specifically flags credential stuffing as a vector that MFA enforcement reliably defeats, which is part of why MFA gaps continue to show up in the postmortems of high-impact breaches.


### **Emerging password trends in 2026**


**Length is overtaking complexity in policy.** The current[NIST SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) guidance keeps the 8-character minimum but explicitly recommends longer passphrases and drops mandatory composition and periodic rotation rules. Specops’ finding that 230 million breached passwords met legacy complexity rules is the empirical case for the shift.


**Infostealer logs are now the primary stolen-password supply.** A single 12-month window produced more than 1.08 billion stolen passwords per[Specops](https://specopssoft.com/blog/report-one-billion-malware-stolen-credentials/) . Even strong passwords typed on a compromised endpoint should be treated as burned.


**Passkeys cross the consumer-default threshold.** At 69% of online users carrying at least one passkey and 48% of top sites supporting them ([FIDO Alliance](https://fidoalliance.org/fido-alliance-champions-widespread-passkey-adoption-and-a-passwordless-future-on-world-passkey-day-2025/) ), passwordless authentication has moved from pilot to production for the largest consumer platforms.


**Credential stuffing is now a background load on every identity provider.** The[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) found that 19% of authentication attempts in observed SSO logs were credential stuffing, on a typical day. Rate limiting, anomaly detection, and impossible-travel rules are no longer optional.


**Workforce and consumer password habits are the same problem.** NordPass found that 40% of the most common passwords used by individuals and by business representatives are identical, per[NordPass](https://nordsecurity.com/press-area/americans-most-common-password-is-secret-) . Personal credential reuse is therefore a corporate risk.


**AI-grade cracking changes the security margin on every offline hash.** Per[Hive Systems](https://www.hivesystems.com/blog/are-your-passwords-in-the-green) , AI accelerators have accelerated cracking by more than 1.8 billion percent versus consumer GPUs in some workloads, which means any leaked password hash is more dangerous in 2026 than it was in 2023.


For broader context on the trends above, see our[MFA statistics](https://www.swif.ai/blog/mfa-statistics) and[identity theft statistics](https://www.swif.ai/blog/identity-theft-statistics) roundups.


### **How swif.ai helps**


swif.ai gives IT and security teams a single console to enforce device, identity, and compliance controls across the macOS, Windows, and Linux endpoints behind the numbers above. Explore[swif.ai unified endpoint management](https://www.swif.ai/products/unified-endpoint-management-uem) to see how it works.


If you are looking for more detailed[Cybersecurity statistics](https://www.swif.ai/blog/cybersecurity-statistics) , check out our detailed article here.
