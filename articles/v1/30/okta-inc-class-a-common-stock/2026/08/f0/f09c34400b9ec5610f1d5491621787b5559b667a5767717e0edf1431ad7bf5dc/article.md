---
schema_version: "1.0.0"
document_id: "f09c34400b9ec5610f1d5491621787b5559b667a5767717e0edf1431ad7bf5dc"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/product-innovation/okta-chrome-identity-security/"
published_at: "2026-08-12T07:00:00+00:00"
first_seen_at: "2026-08-13T00:40:23.090321+00:00"
fetched_at: "2026-08-13T00:40:27.275055+00:00"
content_hash: "sha256:31547347cd526649bfe9e0b85e2417c1a60ea02a0290bef26bfd170a47dca725"
---

# Defend against session token theft with Okta and Google Chrome Enterprise

Key takeaways


Session token theft over firewall attacks: Adversaries are bypassing network firewalls by stealing active session cookies directly from enterprise browsers via adversary-in-the-middle (AiTM) attacks.


Immediate endpoint remediation: Manual backend access revocation introduces costly delays; active session hijacking requires immediate, programmatic remediation at the source.


Automated session revocation: By configuring Okta’s Identity Threat Protection for Google Chrome Enterprise, security teams can automatically purge compromised browser cookies and terminate hijacked sessions in near real time.


Take a look at your screen right now. If you work at an enterprise, your email, your CRM, and your internal wiki are probably all just tabs. Almost everything you need to do your job lives inside a browser.


Malicious actors know this, too. According to a recent Omdia report, almost half (49%) of organizations[suffered a successful browser-based attack](https://research.esg-global.com/aim/en/reports/515202191/marketing) in the last 12 months.


These adversaries aren't always trying to brute-force a firewall anymore. Instead, they’re increasingly relying on tactics such as adversary-in-the-middle (AiTM) attacks to steal an active session token directly from a user's browser cache. Once they have that cookie, they can walk through the front door.


The traditional way of handling this is surprisingly manual. Security teams get an alert, file a ticket, and try to revoke access on the backend. But when an attacker actively hijacks a session, you can't wait for a human to read a dashboard. You need to sever the connection immediately, right at the source.


## What is Okta's Identity Threat Protection feature for Google Chrome Enterprise?


Through a collaboration with Google, Okta can now instruct managed Chrome browsers to purge session cookies and clear cached browser data when[Identity Threat Protection (ITP)](https://www.okta.com/products/identity-threat-protection/) flags risks for the users in the cloud.


The moment ITP detects a threat, you can programmatically clear the local browser cookies and drop the session entirely.


## Configuring real-time risk detections in Identity Threat Protection


To automate this, you first need to know exactly when a user is under attack. Okta’s Identity Threat Protection evaluates user behavior long after the initial login. It continuously assesses user risk and automatically responds to identity threats across your ecosystem. For example, you can configure entity risk policies to act as tripwires, clearing browser sessions and cookies based on:


- **Suspicious app access:** Catch active app session cookie theft and hijacking attempts in near real time.
- **User-reported fraud:** Clear browser session and cookies the moment an employee clicks "This wasn't me" on a suspicious activity alert.


Check out the[full list of ITP-supported risk detections](https://help.okta.com/oie/en-us/content/topics/itp/detections.htm) in our product documentation center.


## How does automated session revocation work at runtime?


Okta links user sessions to managed Chrome profiles in the background using the[Chrome Device Trust Connector](https://help.okta.com/oie/en-us/content/topics/identity-engine/devices/chrome/edr-integration-dt-chrome.htm) .


Let's look at how this plays out from setup to a real security event at runtime:


1. **Policy configuration:** Define the specific risk levels or events (such as suspicious app access) in an ITP entity risk policy.
2. **Real-time risk signal:** ITP evaluates active sessions in real time. If it identifies a configured detection, your entity risk policy immediately triggers an automated Okta Workflow.
3. **API invocation:** The Okta Workflow invokes the Chrome Clear Browsing Data API.
4. **Endpoint remediation:** The browser’s cookies and session data are immediately cleared without manual admin intervention.


## Key benefits: No-code setup and full audit visibility


Building this remediation chain is straightforward. You don't need to write custom API calls; it's a drag-and-drop action card in Okta Workflows.


Okta’s System Log records every automated wipe, providing a clear audit trail of the triggering policy and targeted profile.


We can no longer afford to treat the browser as just another application. It is another critical attack surface. If you want to protect it, you need the ability to shut down compromised sessions instantly.


**Turn your Google Chrome enterprise browser into an active security enforcement point rather than a vulnerability** . Shut down active identity threats and keep compromises contained. Follow the steps in our[ITP configuration guide](https://help.okta.com/oie/en-us/content/topics/itp/configure-clear-chrome.htm) to start protecting your managed Chrome environment.


About the Author


[Tiger Li Software Engineer Tiger is a Software Engineer at Okta, working on Identity Threat Protection and continuous risk detection. He’s been building secure identity experiences at Okta since 2024.](https://www.okta.com/blog/author/tiger-li/)


[Apoorva Deshpande Engineering Manager Apoorva Deshpande is a hands-on engineering leader and a technology enthusiast with a strong background in Workforce Identity, Zero Trust, and cloud services. Having earned a reputation for driving results, Apoorva has successfully led teams in developing and delivering secure, cutting-edge Identity management solutions that delight customers. He also actively contributes to the OpenID Foundation's shared signals working group and specifications.](https://www.okta.com/blog/author/apoorva-deshpande/)
