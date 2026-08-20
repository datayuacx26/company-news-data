---
schema_version: "1.0.0"
document_id: "eff4a0433fb587e46bc3da9f52bba258eedbcb90846436dceea7b03ffacf20c1"
company_key: "yc-aspire"
company: "Aspire"
source_id: "yc-aspire-news-import-2536b3b9499f"
canonical_url: "https://aspireapp.com/blog/email-and-credential-compromise-your-inbox"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-23T02:30:41.255281+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:5c6a760573e8adf8bafd6f728cead1cae8a42c6800285730dd1c5e40e741c34b"
---

# How email and credential compromise turns your inbox into a threat

## Not a phishing email you can just delete


Standard BEC fraud relies on getting someone to act on a fraudulent instruction, such as a spoofed invoice or a fake wire request. The attack chain we're describing here is structurally different. **The fraudster's goal isn't to trick your team into approving a bad payment. It's to gain authenticated access to your account and make the payment themselves, as you.**


That changes everything about how the attack looks. There's no suspicious email to catch. No unusual sender to flag. By the time you'd normally notice something is wrong, the attacker has already been inside your account for hours.


## How credentials get stolen


The entry point is almost always the email address linked to your financial platform or service provider’s account.


Phishing is the most common method: a convincing fake login page captures your email password when you enter it. But credential stuffing has become equally prevalent, and far more insidious. If you've ever reused a password across two platforms, a breach at an unrelated service can hand attackers the keys to your inbox. Automated tools test billions of leaked credential combinations against business email accounts every month.


Then there are infostealers: malicious software that installs silently on a device and captures not just passwords, but active session cookies. According to cybersecurity firm Flashpoint, infostealers accounted for more than 2.1 billion of the 3.2 billion credentials stolen globally in 2024. A single infected device can yield nearly 2,000 session cookies, each one a potential bypass of your login screen entirely.


### OTPs are intercepted before you even see them


Most founders assume that having MFA on their financial platform or service provider’s account provides adequate protection. It does, but only up to a point. If the OTP for that MFA arrives in an email inbox that an attacker already controls, the protection is substantially weakened.


Modern phishing kits, known as adversary-in-the-middle (AiTM) tools, go further. They act as a real-time relay between you and the legitimate login page, capturing your password and your OTP simultaneously and using both to log in before the token expires. Microsoft's Digital Defense Report 2024 recorded a 146% surge in this technique. The attacker doesn't need to wait for your OTP to arrive in your inbox. They intercept it as it passes through.


**A one-time password delivered to a compromised inbox is no longer a second factor. It's a second key handed to the same person who already has the first.**


### Notification suppression keeps you blind


Once inside your inbox, an attacker's first move is almost never to act immediately. It's to make sure you won't see what comes next.


Within seconds of gaining access, attackers create inbox rules that automatically delete or hide messages containing words like ' *payment* ,' ' *security* ,' ' *alert* ,' ' *OTP* ,' or ' *fraud* .' Proofpoint's analysis of compromised Microsoft 365 accounts found malicious inbox rules present in roughly 10% of cases, sometimes created within 5 seconds of the initial breach. In some incidents, auto-forwarding rules redirect every incoming message to an attacker-controlled address, giving them real-time visibility into your operations while hiding all activity from you.


Every security alert, every transaction notification, and every login warning goes directly to the bin. You've been made effectively blind.


### Account changes and unauthorized payments


From that position, the attacker has time and access. They can add new beneficiaries to your account, adjust payment approval settings, or initiate transfers to accounts they control. Because they're operating from a verified session on a recognised device, and because all alerts to you have been suppressed, there's nothing to trigger a flag on your end.


In the cases Aspire's fraud team has reviewed, this is the phase where funds exit. The attacker doesn't linger. Once payments have been made, they exit the account and the trail moves quickly toward mule accounts and, often, cryptocurrency conversion.


## Why this attack is particularly hard to catch


Most fraud controls are built to detect anomalies in behaviour. Unusual payment amounts. New beneficiaries. Logins from unfamiliar locations. These controls matter, and Aspire runs them continuously.


The challenge with inbox-level compromise is that the attacker has time to study your normal behaviour before acting. They see your regular payment amounts, your usual beneficiaries, and your standard operating patterns. They can time their actions to look routine. They can suppress the alerts that would surface the anomaly.


This is why the discovery window matters so much. In high-loss cases, victims typically don't discover the breach until 24 to 72 hours after funds have moved, during routine reconciliation or when a legitimate vendor calls to chase a payment. At that point, recovery options narrow significantly.


### Unusual transaction patterns


The clearest indicator of money mule activity involves rapid fund movement. If your business account regularly receives deposits that are immediately transferred elsewhere, typically within the same day or even within minutes, this represents classic pass-through behavior that regulators specifically monitor for money laundering.


High transaction volumes relative to your actual business operations create another red flag. When your account processes significantly more payments than your business activity would justify, financial institutions will flag this discrepancy. Similarly, if you're receiving or sending payments to numerous unrelated third parties with no clear connection to your business products or services, this signals potential intermediary abuse.


Mismatches between declared business activity and actual transactions represent perhaps the most serious warning sign. If you told your bank you operate a consulting business but transactions show retail purchase patterns, international wire transfers inconsistent with consulting work, or payment flows that don't match service delivery, expect enhanced scrutiny and potential account restrictions.


## Check your inbox for these signs of compromise


Unlike a spoofed email, inbox compromise leaves traces inside your own email account. These are the checks worth running today, especially if you use the same email address for your financial platform or service provider’s account and your broader business operations.


- **Open your email settings and look for filters or rules you didn't create.**


- In Gmail, go to *Settings* and then *Filters* and *Blocked Addresses* .


- In Outlook, check *Rules* under *Settings* . Delete anything unfamiliar immediately.


- **Check your forwarding settings.** Any active forwarding rule to an address you don't recognize should be treated as a confirmed breach. Turn it off and change your password immediately.


- **Review your sent folder and deleted items.** Attackers sometimes use your inbox to send messages as you. Emails you didn't write, or threads you were removed from, are strong indicators.


- **Look for login activity from unfamiliar locations or devices** . Both Gmail and Outlook provide access to recent login history. A login from a country you haven't visited, or a device you don't recognise, warrants immediate action.


- **Check whether notifications on your financial platform or service provider are still active.** If you've stopped receiving transaction alerts or security notifications unexpectedly, that's a direct signal that someone may have modified your settings.


If any of these checks return something unexpected, treat it as a security incident.


Revoke active sessions, change your password across all linked accounts, and contact Aspire's fraud support team immediately at fraud@aspireapp.com or through in-app chat.


## How Aspire’s device bound authentication helps


The fundamental weakness that OTP interception exploits is that authentication is tied to a channel, your inbox, rather than to a physical device you control.


When you enrol a trusted device on Aspire, you register a biometrically verified device through identity confirmation. From that point, in-app authentication is anchored to that device. Even if an attacker has your email credentials, your password, and access to your OTPs, they cannot complete authentication from an unverified device.


This is the same architecture that the Hong Kong Monetary Authority mandated for bank-issued cards in late 2024, under its E-Banking Security ABC framework. Banks that shifted from SMS OTP to device-bound authentication as the default reported a nearly 80% reduction in fraud rates.


Enrolling your trusted device directly closes the OTP interception vector. It's the most effective single action you can take to protect your Aspire account against this specific attack chain. Get started:[How to set up your trusted device](https://help.aspireapp.com/en/articles/12330534-how-to-set-up-your-trusted-device) .


## 5 steps to harden your email account


Trusted Device Enrolment secures your Aspire account at the authentication layer. But the inbox itself still needs to be protected. These 5 steps address the entry point directly.


### 1. Use a dedicated email address for financial services


Your Aspire account should be linked to an email address used exclusively for that purpose, not your main business inbox, not a shared team address. The fewer places that email address appears, the smaller the target it presents.


### 2. Enable MFA on your email account using an authenticator app


If an attacker gets your email password through credential stuffing or phishing, MFA on your inbox is the last line of defence before they're in. Use an authenticator app rather than SMS. SIM swap attacks can redirect text-based codes to a device the attacker controls. An app-based code requires physical access to your device.


### 3. Never reuse passwords across accounts


Credential stuffing works because of password reuse. A breach at a completely unrelated service can hand an attacker a working password for your business email. A password manager makes it practical to maintain unique credentials across every account without having to remember them. Research from Security.org found that password manager users are about half as likely to experience identity or credential theft compared to those who don't use one.


### 4. Audit your inbox rules regularly


Make it a monthly habit to review your email filters and forwarding settings. This takes under two minutes and is one of the few checks that can surface a breach that's already in progress. An attacker relying on notification suppression depends on those rules staying hidden. A regular audit removes that cover.


### 5. Block legacy authentication protocols


Older protocols like POP3 and IMAP basic auth can bypass modern MFA entirely, because they were built before MFA existed. If your email provider allows it, disable these protocols entirely for your business account. Google Workspace and Microsoft 365 both offer settings to enforce modern authentication only.


## Complying with your regulatory authority


Regulators have converged on the same conclusion: SMS OTPs are no longer an adequate authentication baseline, and device-bound verification is the direction of travel. Here's country-specific guidance you should know:


### Singapore (Monetary Authority of Singapore, MAS)


The Monetary Authority of Singapore's[Technology Risk Management Guidelines](https://www.mas.gov.sg/regulation/guidelines/technology-risk-management-guidelines) require adaptive authentication, device intelligence, and session monitoring across all online transactions. The MAS Cyber Hygiene Notice mandates MFA on administrative and internet-facing accounts. MAS also maintains active[scam prevention guidance](https://www.mas.gov.sg/schemes-and-initiatives/scam-alert) that includes specific advice on email account security for businesses linked to financial services.


### Hong Kong (Hong Kong Monetary Authority, HKMA)


The HKMA's updated Supervisory Policy Manual[TM-E-1 (October 2024)](https://www.hkma.gov.hk/eng/key-functions/banking/anti-money-laundering/) and its April 2025 E-Banking Security ABC framework set out explicit controls against OTP interception, including a requirement that financial institutions prevent scenarios where the same device is used for both login and OTP receipt. The HKMA framework makes device-bound, in-app authentication the mandated default, a direct regulatory response to the exact attack chain this article describes.


### United States of America (Financial Crimes Enforcement Network, FinCEN)


FinCEN's[Business Email Compromise Advisory (FIN-2019-A005)](https://www.fincen.gov/sites/default/files/advisory/2019-07-16/Updated%20BEC%20Advisory%20FINAL%20508.pdf) identifies out-of-band verification of any payment or banking detail change as the core operational control for businesses. This means calling a known, pre-verified phone number to confirm any change to wire details, not replying to the email thread where the change was requested. When a business email has been compromised, that thread may already be under attacker control.


## How Aspire’s controls complements your security


Aspire's platform-level controls are designed to catch and limit damage even when a customer's email has been compromised. These aren't a substitute for the steps above, but they're a meaningful backstop.


- **AI-driven anomaly detection:** Aspire monitors transaction patterns, login behaviour, and device signals continuously, flagging activity that deviates from your normal profile before funds can move.


- **Trusted Device Enrolment:** Biometrically verified device authentication that removes reliance on email-delivered OTPs.


- **Session intelligence:** Logins initiated from unrecognised devices can be restricted. MFA is enforced across all high-risk account actions.


- **Real-time alerts:** Aspire sends immediate notifications for high-risk activity. These only work if your notification preferences are active and your inbox is intact, which is why the inbox hardening steps above are essential.


- **Fraud support:** If you suspect your account has been compromised, contactfraud@aspireapp.com or reach the fraud support team directly via in-app chat for immediate escalation.


## Your role in staying protected


Business email compromise at the payment-instruction level is one kind of threat. What this article covers is different: an attacker who gains authenticated access to your financial account by working through your inbox, suppressing your alerts, and moving funds as you.


The defence is built in layers. Device-bound authentication closes the OTP interception vector at the Aspire level. Email-specific MFA, unique passwords, and regular inbox audits close it at the entry point. Neither layer alone is sufficient. Both together make this attack substantially harder to pull off.


Enrol your trusted device. Review your inbox rules. And if anything looks out of place, contact Aspire's fraud team immediately. Time is the variable that separates a recoverable incident from a permanent loss.


Set up your Aspire Trusted Device now:[help.aspireapp.com/en/articles/12330534-how-to-set-up-your-trusted-device](http://help.aspireapp.com/en/articles/12330534-how-to-set-up-your-trusted-device)
