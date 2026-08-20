---
schema_version: "1.0.0"
document_id: "cb5a95844ff1afb78b6c3550acf09427caf96a91135a7cdb8f2afa5e9a118188"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/microsoft-makes-passkeys-default-what-identity-and-security-leaders-need-to-do/"
published_at: "2026-07-16T15:41:10+00:00"
first_seen_at: "2026-07-20T23:22:38.756744+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:261a9af8db1f5f632e4b4e6fce3d42ac01beb07ea0f569ff3225cbca54257b54"
---

# Microsoft Makes Passkeys Default: What Identity And Security Leaders Need To Do

Microsoft’s July 2026[announcement](https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/) that passkeys will become the default authentication method for all users in Microsoft Entra ID is a clear inflection point for phishing-resistant MFA adoption. Beginning September 1, 2026, Microsoft will automatically enable passkeys for users currently relying on SMS or voice authentication, and by February 1, 2027, Microsoft will retire its native SMS and voice authentication services entirely.


For years, organizations have viewed phishing-resistant MFA as an aspirational goal reserved for privileged users and high-risk user populations. That era is ending. The combination of AI-enabled phishing campaigns, sophisticated social engineering attacks, and growing regulatory scrutiny is forcing organizations to migrate away from passwords, SMS codes, and other phishable authentication factors. Microsoft’s move effectively makes phishing-resistant authentication the default destination for any enterprise using Entra.


Microsoft is not alone. Salesforce recently announced phishing-resistant MFA requirements for privileged users and introduced a passkey-first registration experience, further reinforcing a broader industry consensus that traditional MFA methods are no longer sufficient for protecting enterprise identities. Apple and Google, along with Microsoft, have been championing passwordless support based on FIDO passkey standards since 2022.


### **Why This Matters**


Security leaders should leverage Microsoft’s announcement as a strategic catalyst, not merely a technical migration project.


Attackers have adapted to traditional MFA. SMS interception, MFA fatigue attacks, adversary-in-the-middle phishing kits, and AI-assisted social engineering continue to erode the protection offered by legacy authentication methods such as OTP passcodes. As a result, organizations must shift their focus from deploying MFA to phishing-resistant MFA adoption. The measure of success should be the percentage of users protected by phishing-resistant authentication methods.


The encouraging news is that the technology ecosystem is aligned around phishing-resistant MFA thanks to continued advancements in passkeys, FIDO2 security keys, platform authenticators, and Windows Hello for Business. User experiences have improved, deployment models have simplified, and vendor support has expanded across workforce applications.


### **Microsoft Timeline**


Users who already sign in with passkeys, Windows Hello for Business, or another phishing-resistant method can continue using those methods, without any disruption, but users who remain enabled for SMS or voice may still receive prompts to register passkeys on eligible devices.[Key transition timeline dates](https://www.microsoft.com/en-us/security/blog/2026/07/13/microsoft-entra-id-security-updates-passkeys-are-the-default-authentication-method-in-entra-id/) :


- **September 1, 2026** — Passkeys become the default authentication experience for Entra ID users. For tenants with users enabled for SMS or voice, those users are auto-enabled for passkeys and nudged for passkey registration upon MFA sign-in.
- **February 1, 2027** — Microsoft-provided SMS and voice are fully retired in Microsoft Entra ID. Every user must be on a phishing-resistant method (passkeys, Windows Hello, or FIDO2) before this date.
- **After February 1, 2027** — Users who use SMS or voice for multifactor authentication will be required to register a passkey before they can sign in. Automatic prompts to register a passkey will be enforced for all users in all tenants. There will be no opt-out option.


Organizations that must retain SMS or voice authentication for operational reasons will be able to select, configure, and manage a third-party telecom provider through the Microsoft Security Store, a partner marketplace. Information on supported providers, deployment guidance, and technical documentation will be forthcoming, via the store, in September.


### **What Organizations Should Do Now**


Forrester recommends that identity and security leaders use Microsoft’s timeline as a catalyst for broader authentication modernization efforts. Focus on:


- **Establishing a baseline.** Identify users who still rely on SMS, voice, OTP applications, or other phishable factors. Understand where legacy authentication remains embedded in business processes. Gauge the cost of sending SMS and email OTP messages.
- **Prioritizing high-risk populations.** Privileged administrators, executives, developers, finance users, and help desk personnel should be among the first cohorts migrated to passkeys and other phishing-resistant authentication methods. In CIAM, focus on high-risk, high-value customer segments.
- **Developing a phased passkey migration strategy.** Avoid treating passkey adoption as a simple technology rollout. Success depends on user readiness, device compatibility, onboarding workflows, recovery processes, and change management.
- **Emphasizing user communications and education ahead of changeover.** Recognize that times of change also present new and unique opportunities for attackers to[exploit users](https://www.bleepingcomputer.com/news/security/entra-passkey-enrollment-vishing-targets-microsoft-365-users/) . Put strong emphasis on user education and the[human element](https://www.forrester.com/report/the-cisos-primer-for-defining-human-element-breaches/RES185396) in advance and throughout the authentication transition process.
- **Retiring weak authentication methods wherever possible.** Organizations should define measurable milestones for reducing and ultimately eliminating dependence on SMS and voice-based OTP authentication.
- **Aligning passkey adoption with broader identity modernization initiatives.** Passkeys should be viewed as a foundational component of a modern identity security architecture that includes continuous risk assessment, strong device trust, identity threat detection and response, and Zero Trust principles. When there is a requirement of using device-bound passkeys, consider using enterprise passkey management and sync solutions, such as 1Password, Keeper, or LastPass.


The question is no longer whether enterprises will adopt phishing-resistant MFA but how quickly they can complete the transition. Organizations that begin now will be better positioned to reduce identity risk, improve user experience, and stay ahead of both attackers and industry mandates.


Forrester clients seeking advice on passkey adoption strategies, phishing-resistant MFA roadmaps, or broader identity modernization initiatives can[schedule a guidance session or inquiry](https://www.forrester.com/inquiry) with us.


Share


-
-
