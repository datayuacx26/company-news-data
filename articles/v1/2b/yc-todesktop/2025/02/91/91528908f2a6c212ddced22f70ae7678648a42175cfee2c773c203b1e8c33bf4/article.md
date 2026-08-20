---
schema_version: "1.0.0"
document_id: "91528908f2a6c212ddced22f70ae7678648a42175cfee2c773c203b1e8c33bf4"
company_key: "yc-todesktop"
company: "ToDesktop"
source_id: "yc-todesktop-news-import-b82ee277d06d"
canonical_url: "https://www.todesktop.com/blog/posts/security-incident-at-todesktop"
published_at: "2025-02-28T19:00:00+00:00"
first_seen_at: "2026-07-24T04:14:47.478048+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:1e7ec2c57472ab7f7e3957579966e1322166cc36798e29e8e55ed6e3b88137e3"
---

# Vulnerability Report at ToDesktop on October 2nd, 2024

Impact No customer data or applications were compromised


Action required No action required from customers


Resolution time 26 hours


## Summary


On October 2nd 2024, a security researcher (T/A xyz3va) reported that she was able to gain access to ToDesktop’s Firebase Service Admin Account.


**We have reviewed logs and inspected app bundles. No malicious usage was detected. There were no malicious builds or releases of applications from the ToDesktop platform.**


We promptly patched the vulnerability and rotated all keys. In addition to patching the vulnerability, we engaged a third-party cybersecurity company,[Doyensec](https://www.doyensec.com/) , to conduct an independent audit of our platform and build pipeline.


## Timeline


All timestamps are in Coordinated Universal Time (UTC).


Date and Time What Happened


October 2nd, 6:36pm–6:58pm The researcher reported the vulnerability, and we acknowledged and confirmed it.


October 2nd, 9:00pm Rotated all affected credentials.


October 2nd, 10:00pm Conducted a preliminary review of logs and investigated recent releases, with no immediate signs of unauthorized access.


October 3rd, 8:13pm Deployed a patch for the vulnerability to production.


October 5th, 12:17pm Completed a review of the logs. Confirming all identified activity was from the researcher (verified by IP Address and user agent).


We resolved the vulnerability within 26 hours of its initial report, and additional security audits were completed by February 2025.


## Actions and remediations


These are some of the actions that we have **already taken** :


### Immediate fixes


1. Rotated all sensitive tokens.
2. Patched the vulnerability by removing the Firebase Service Account token and implemented a restricted-access solution, granting only the minimal permissions necessary for builds.
3. Enhanced alerts for suspicious usage of sensitive access tokens.


### Long-term enhancements


- **Security audits and testing**


- Completed a third-party security audit with independent cybersecurity company, Doyensec. The audit was completed on January 21st, 2025. A subsequent retest was completed on February 25th, 2025.
- We are committing to undertake annual security audits in the future.


- **Access control and authentication**


- All customers must now use a security key ([WebAuthn token](https://webauthn.guide/) ) to release app updates, ensuring only authorized releases. This key-based authentication is managed separately from our main systems to prevent unauthorized access even if other parts of our infrastructure are compromised.
- Implemented access control permissions for build and release processes, allowing account admins to assign specific permissions to team members and restrict sensitive actions.
- To prevent unauthorized additions to teams, inviting new users now requires authentication with a security key, ensuring only authorized admins can expand access.
- Sensitive actions, such as updating secrets or inviting new users, now trigger notification emails to the account owner.


- **Infrastructure and tooling**


- We’ve applied the Principle of Least Privilege (PoLP) to our ephemeral build containers, meaning each process has only the minimum access needed to function. We’ve also open-sourced two tools we built to help achieve this:[KeyVault Gatekeeper](https://www.npmjs.com/package/keyvault-gatekeeper) and[Firestore PoLP](https://www.npmjs.com/package/firestore-polp) .
- Introduced an option for customers to[self-host updates and downloads](https://github.com/ToDesktop/self-hosted) . The source code is freely available and open-source, with setup instructions[accessible on GitHub](https://github.com/ToDesktop/self-hosted) . This allows customers to manage their own self-hosted update and download infrastructure if they choose.


- Added[security policy](https://www.todesktop.com/security-policy) and[security features](https://www.todesktop.com/security) documents to our website.


## How this affects you


**No action is required. No malicious usage was detected. The vulnerability was patched within 26 hours of first being reported.**


This leak occurred because the build container had broader permissions than necessary, allowing a` postinstall` script in an application's` package.json` to retrieve Firebase credentials. We have since changed our architecture so that this can not happen again, see the " **Infrastructure and tooling** " and " **Access control and authentication** " sections above for more information about our fixes.


Had this vulnerability not been identified and responsibly disclosed, a malicious actor could have potentially caused significant harm. In the worst case, this would have allowed an attacker to access the ToDesktop database, user accounts, and deploy unauthorized updates to applications.


I’m grateful that the security researcher came forward and disclosed the vulnerability responsibly.


## Why are we disclosing now?


Since no malicious activity occurred, we followed industry-standard advice indicating immediate disclosure was not required. However, in the spirit of transparency, we chose to disclose after completing internal and third-party audits.


## Conclusion


We sincerely regret that our security measures fell short, potentially putting customer applications at risk. However, I am proud of ToDesktop's rapid response in addressing the vulnerability and our proactive steps to enhance the platform's security.


While the initial report and fix occurred in October 2024, we took additional time to complete a third-party audit with Doyensec, finalized on February 25th, 2025. After undertaking internal and third-party audits, we are confident in the security and resilience of the ToDesktop platform. This incident has reinforced our commitment to security, and we are dedicated to continuously improving our processes to protect our customers and their applications.


We thank the security researcher, T/A xyz3va, for identifying and responsibly disclosing this vulnerability. She has shared more technical details[here](https://kibty.town/blog/todesktop) .


**No malicious usage was detected.** If you have any concerns or questions, please contact us atsecurity@todesktop.com .
