---
schema_version: "1.0.0"
document_id: "2cafa47ff359dcec896fadaafbae027eea93df799483806a84733b16ffa66d2e"
company_key: "yc-aglide"
company: "Aglide"
source_id: "yc-aglide-rss-a73e409223ac"
canonical_url: "https://blog.aglide.com/aglide/introducing-aglide-sms/"
published_at: "2025-07-19T00:00:00+00:00"
first_seen_at: "2026-07-24T14:45:00.121943+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:8257614b63105d1bd8a25ade5b6c3f1547d3d920ad8801eab339db594d484814"
---

# Introducing Aglide SMS

Aglide's goal is to connect every non-SAML and non-SCIM application to your identity provider - letting admins automate any lifecycle task, and giving end-users a seamless SSO login experience.


To do that, we need to be able to handle every kind of credential an application might could use. Aglide already managed email addresses for magic links and email OTP, and TOTP secrets for authenticator-based 2FA. Today we're adding the final piece, with the launch of Aglide SMS, Aglide can now automate phone number based authentication systems.


---


## The Problem


Legacy applications, particularly in banking, healthcare, and insurance, haven't caught up with modern authentication standards. Many still only support SMS as a second factor. No TOTP option, no magic link. If you want to log in, you need a phone number.That creates three problems.


### SMS 2FA is a nightmare for shared accounts


When a shared account is gated by SMS, every login depends on tracking down whoever owns the number - typically over Slack, a shared inbox, or a phone sitting on someone's desk. More often than not, it's tied to someone's personal number. When that person leaves, so does the ability to log in.


### SMS 2FA is insecure


Phone numbers can be SIM swapped. A shared account number - often undocumented, unmonitored, and attached to no formal owner - is an easy target. SMS 2FA was never strong; on shared accounts, it's particularly weak.


### SMS 2FA blocks automation


When Aglide signs into an application to complete a SCIM lifecycle task (e.g., provisioning, deprovisioning, etc.), it needs to authenticate through whatever 2FA method the app requires. Without SMS support, any SMS-only app would pause the workflow and prompt a user to input the code manually.


---


## Aglide SMS


Admins can now provision Aglide-managed phone numbers and assign them to both individual user accounts and shared accounts.


- **Seamless SSO login:** When a user signs in to an individual or shared application that uses an Aglide SMS number via Aglide SSO, all they need to do is complete their SSO login, Aglide will handle automatically receive the SMS code and complete authentication automatically.
- **Complete lifecycle automation:** Provisioning, deprovisioning, and access reviews run end-to-end across SMS-gated applications without exception handling.
- **Full 2FA coverage:** Combined with Aglide Managed Emails and TOTP Secrets, Aglide now handles every major automated 2FA method. For anything else, Aglide surfaces the UI to the user directly.


---


## Security


As with everything in Aglide, we took an immense amount of care to ensure this is implemented as securely as possible.


Aglide SMS is powered by Twilio, but Twilio has no visibility into which phone number belongs to which user. When Twilio forwards a message to Aglide, the contents are immediately encrypted with the recipient user's local public key using Aglide's zero-trust model. This means:


- **Aglide can't read SMS your messages.** Messages can only be decrypted by the user on their own device, using their local key.
- **Users cannot read each other's messages.** Each number's messages are encrypted to a single user's key, that only that user has.
- **SIM swap risk is eliminated.** Aglide-managed numbers are not tied to a physical SIM or carrier account in the traditional sense. There is no social engineering vector to hijack them.


---


## What This Means in Practice


For IT teams managing access across a broad application estate, Aglide SMS removes the last manual step in automated provisioning. Applications that previously required a human in the loop for SMS 2FA - whether for day-to-day authentication or IdP lifecycle tasks - can now be fully automated.


For security and compliance teams, it replaces an uncontrolled pattern (shared numbers, personal phones, Slack-distributed codes) with one that is managed, attributable, and auditable by default.


Aglide now covers the full range of 2FA methods across your application estate:


Method Aglide support


**SMS** Aglide-managed numbers, fully automated


**Email OTP / magic link** Aglide-managed addresses, fully automated


**TOTP** Aglide-managed secrets, fully automated


**Other methods** UI surfaced directly to the user


---


Aglide SMS was developed after a key design partner - a $20Bn financial services company - told us they wanted to connect their 70 treasury banking portals, including JP Morgan and Wells Fargo, to Okta to enable SSO and automate lifecycle management. SMS 2FA was the blocker. We'd like to thank them for their feedback and partnership in building this.


Aglide SMS is available now. Contact your account team or reach out athello@aglide.com to get started.
