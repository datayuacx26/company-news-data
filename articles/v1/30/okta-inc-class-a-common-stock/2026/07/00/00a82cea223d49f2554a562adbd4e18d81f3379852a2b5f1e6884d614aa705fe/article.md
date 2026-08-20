---
schema_version: "1.0.0"
document_id: "00a82cea223d49f2554a562adbd4e18d81f3379852a2b5f1e6884d614aa705fe"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/threat-intelligence/vishing-actors-target-microsoft-entra-passkey-enrollment-/"
published_at: "2026-07-04T16:00:00+00:00"
first_seen_at: "2026-07-22T07:05:37.626494+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:cd8a9f36ac24b7dddd9a4c3e9f210fc439c3ddd9af2832de3a5011f81b03b9fa"
---

# Vishing actors target Entra passkey enrollment

## Executive Summary


Since April 2026, a threat actor tracked as O-UNC-066 that operates a DLS site with the name Pink (reported as "CL-CRI-1147" by Palo Alto Networks Unit 42) has deployed a panel-controlled phishing kit targeting the passkey enrollment process for Microsoft 365 customers.


Okta has observed the targeting of enterprise organizations across the food and beverage, technology, healthcare, automotive, construction, and aviation industries by this cluster of activity. The primary motivation of the threat actors is data extortion.


The threat actor registers domains that incorporate the word passkey as part of a voice-enabled phishing (“vishing”) scheme. The threat actor then calls targeted users on the phone in an attempt to persuade them that they need to register a new passkey.


Users are directed to a phishing kit that closely mimics the Microsoft passkey enrollment process. It appears engineered to convince a targeted user they are in the process of enrolling a passkey with Microsoft, while the threat actor simultaneously registers their own passkey in the targeted user’s Microsoft account.


This pretext is well-timed - as of May 2026, Microsoft administrators have been able to create[passkey registration campaigns](https://learn.microsoft.com/en-us/entra/fundamentals/whats-new#general-availability---support-for-passkeys-in-microsoft-entra-id-registration-campaign) that remind or “nudge” users to enrol in passkeys at sign-in, and in[some circumstances](https://mc.merill.net/message/MC1221452) these nudges are on by default.


Threat actors have used this well-intentioned security upgrade as a pretext for abusing the enrolment process to further their objectives.


Our analysis of the phishing kit revealed that it does not attempt to handle federation to third-party Identity Providers such as Okta. Subsequently, we have not observed the compromise of Microsoft accounts directly.


We have nonethless published advice in a[separate blog post](https://www.okta.com/en-au/blog/threat-intelligence/intrusion-actors-self-serve-their-way-into-accounts/) on how to apply phishing resistance to authenticator enrollment and recovery.


## Threat Analysis


In the threat activity we observed, the threat actor creates per-target subdomains that mimic Microsoft Entra ID login pages. The pages are customized using each victim organization's legitimate branding. Generic Microsoft styling is loaded from Microsoft's Content Delivery Network, while the branding elements relevant to each victim organization (logo and background) is pre-staged per subdomain as part of the configuration for any given target and served from the backend of the phishing kit.


The kit is not a transparent Adversary-in-the-Middle (AitM) proxy, one of the most frequently seen types of phishing kits designed to collect credentials, MFA tokens and session tokens. It is an operator-controlled PHP panel in which a threat actor steers victims through various stages of authentication in close to real-time using a 1-second heartbeat polling mechanism. The operator can use the kit to adapt the user experience to each victim's MFA requirements (TOTP, push notification with number matching, SMS OTP) during the session. This operational design is consistent with the vishing tradecraft documented in Okta’s November 2025 public blog post "[Phishing kits adapt to the script of callers](https://www.okta.com/blog/threat-intelligence/phishing-kits-adapt-to-the-script-of-callers/) ." The caller can control and adjust in real time what phishing pages and notifications a targeted user sees.


It is likely that the threat actor uses the kit to takeover the user account and trick the user into approving an attacker-initiated registration of a passkey.


Okta Threat Intelligence used code derived from the phishing kit to recreate the following flow, which closely resembles the passkey registration process for Microsoft Entra.


The first page of the phishing kit ( **/gate** ) reveals a page loading icon while the phishing kit performs anti-analysis checks. The second page ( **/identify** ) requests a username. The phishing kit did not redirect to a federated Identity Provider at the time of our analysis.


Figure 1. The O-UNC-066 Microsoft sign-in page, regenerated using code extracted from the phishing kit.


Fill out the form to access this content.


The next page ( **/password** ) challenges the user for a password. The captured credentials are sent in a POST request with a timestamp and Id to an operator panel at **/backend.php** .


Figure 2. The O-UNC-066 Microsoft password challenge page, regenerated using code extracted from the phishing kit.


Fill out the form to access this content.


Our assumption is that a phishing kit operator (which may be a different individual to the caller on the phone) captures the credentials of the targeted user within a few seconds and enters them at the legitimate Microsoft sign-in page for the targeted tenant.


The targeted user then sees a ( **/processing** ) page that presents another loading screen while the phishing kit awaits the operator’s next instruction. Our assumption is that this small delay is required for a threat actor to authenticate to the user’s legitimate Microsoft account using the stolen credentials, to observe what MFA challenges are presented, and select the next page of the phishing kit to present to the user.


next page of the phishing kit to present to the user.


- If the operator chooses or is forced to complete an SMS OTP challenge, the user is directed to a page called **/submit-otp** . The captured OTP is sent in a POST request to the operator panel at **/backend.php** .
- If the operator chooses or is forced to complete a TOTP challenge, the user is directed to a page called **/submit-authenticator** . The captured OTP is sent in a POST request to the operator panel at **/backend.php** .
- If the operator chooses or is forced to complete a Push MFA challenge, the user is directed to a page called **/approve-authenticator** (see image below) and asked to enter the number supplied by the operator into their authenticator app.


Figure 3. The O-UNC-066 MFA challenge page, regenerated using code extracted from the phishing kit.


Fill out the form to access this content.


At this stage of an attack, the user has been tricked over the phone into approving the attacker’s access to their Microsoft 365 account.


In keeping with the passkey pretext, threat actors can then direct users to the **/passkey/register** page, which asks the user to create a passkey.


Figure 4. The O-UNC-066 passkey registration page, regenerated using code extracted from the phishing kit.


Fill out the form to access this content.


The phishing kit appears to prey on lack of user familiarity with passkey authentication. In a real passkey registration ceremony, the user might expect a system dialog to register a passkey on their device. The passkey pages in this phishing kit appear to mimic this process without registering a passkey.


At the **/passkey** page, the targeted user is presented with a Microsoft-branded page that encourages the user to “save your recovery key” from an attacker-controlled list of BIP-39 phrases. This closely resembles the methods used in some cryptocurrency applications to generate memorable seed phrases.


Figure 5. The optional O-UNC-066 passkey page, using code extracted from the phishing kit


Fill out the form to access this content.


A subsequent **/passkey/check** page asks the user to verify the final word used in the seed phrase.


Fill out the form to access this content.


We are not aware of any direct applicability of BIP-39 seed phrases to Microsoft Entra or its passkey registration process. An attacker that has already gained unauthorized access to a user account can create their own recovery codes using a process that does not require any input from the real account holder.


It is likely that these passkey-themed pages are available to the phishing kit operator as a sleight of hand. It is a distraction to keep a user occupied on a task while the threat actor enrolls their own passkey in the legitimate Microsoft user account.


The **/done** page confirms that a passkey registration was successful. An unsuspecting user that does not fully understand how a passkey is enrolled may genuinely believe that they registered one with Microsoft simply by completing these otherwise meaningless tasks.


Figure 7. The O-UNC-066 campaign confirmation page, using code extracted from the phishing kit.


Fill out the form to access this content.


The operator can choose when to push the **/done** page to the user. At minimum it helps the phishing operation maintain the original pretext. Any time a user enrolls a passkey with Microsoft, the owner of the compromised account receives a legitimate Microsoft email to notify them that a new passkey had been registered in their account. During an attack, the passkey was actually enrolled by the threat actor directly with Microsoft, and the threat actor is in a position to name the passkey with something the targeted user would view as benign (perhaps even borrowing from the seed phrase selected by the targeted user). The passkey enrollment setup the targeted user experienced on the phishing site, by contrast, is likely to only exist to trick the user into thinking the attacker’s enrollment was their own.


## Infrastructure


Threat actors were observed creating subdomains for any given targeted entity under the following domains:


- assignpasskey\[.\]com (2026-06-14, Internet Domain Service BS Corp., DDoS-Guard)
- deploypasskey\[.\]com (2026-04-21, Tucows, DDoS-Guard)
- passkeydeploy\[.\]com (2026-04-23, Internet Domain Service BS Corp, DDoS-Guard)
- passkeyadd\[.\]com (2026-05-08, Tucows, DDoS-Guard)
- setpasskey\[.\]com (2026-05-23, IQWeb FZ-LLC)


So a campaign targeting "exampleentity" might be something like:


**exampleentity\[.\]setpasskey\[.\]com**


The phishing infrastructure observed by Okta Threat Intelligence was hosted on DDoS-Guard (AS57724, Russia) and IQWeb FZ-LLC (AS59692, US).


## Impact


Since April 2026, a threat actor linked to O-UNC-066 has operated a data leak site with the name Pink (reported as "CL-CRI-1147" by Palo Alto Networks Unit 42).


Figure 7. The Pink DLS (data extortion) site


Fill out the form to access this content.


## Recommendations


While this cluster of threat activity has not been observed impersonating Okta, similar campaigns have combined voice-based social engineering and operator-controlled phishing kits:


**Public Blog Post** ( *publicly available* )


- [Phishing kits adapt to the script of callers](https://www.okta.com/blog/threat-intelligence/phishing-kits-adapt-to-the-script-of-callers/)


**Flash Advisory** ( *Okta customers only* )


- [IT Support Impersonated in Requests for Password Resets](https://security.okta.com/product/oktathreatintelligence/it-support-impersonated-in-requests-for-password-resets)


**Threat Advisory** ( *Okta customers only* )


- [Vishing Operators Synchronize Phishing Sites to their Script for Hybrid Social Engineering Attacks](https://security.okta.com/product/oktathreatintelligence/vishing-operators-synchronize-phishing-sites-to-their-script-for-hybrid-social-engineering-attacks)


The recommendations below are specific to the defence of Okta customers.


**ATT&CK**


**Tactic**


**Control Recomendation**


T1566


Phishing


Enroll users in strong authenticators such as Okta FastPass, passkeys or[smart cards and enforce phishing resistance in policy.](https://help.okta.com/oie/en-us/content/topics/identity-engine/authenticators/smart-card-authenticator.htm)


Establish, communicate and evangelise methods of verifying the identity of helpdesk personnel when they contact users.


T1078


Phishing


Deny requests from locations where your organization does not offer services. Okta[network zones](https://help.okta.com/oie/en-us/content/topics/security/network/about-enhanced-dynamic-zones.htm) allow administrators to set policies that deny access to Okta-protected applications by geolocation (country), ASN, IP, or other criteria.


T1078


Valid Accounts
(Initial Access)


Okta authentication policies can be used to restrict access to user accounts based on a range of customer-configurable prerequisites. We recommend administrators restrict access to sensitive applications to devices that are[managed by Endpoint Management tools](https://help.okta.com/oie/en-us/Content/Topics/identity-engine/devices/managed-main.htm) and[protected by endpoint security tools.](https://help.okta.com/oie/en-us/Content/Topics/identity-engine/devices/edr-integration-main.htm)


T1078


Valid Accounts
(Initial Access)


Notify users of every authenticator (factor) lifecycle event using[end user notifications.](https://help.okta.com/en-us/content/topics/security/healthinsight/notifications-factor-enroll.htm)


T1098


Account Manipulation (Device Registration)


Apply[Okta Account Management Policies](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/oamp.htm) that constrain the ability to add or modify authenticators based on network context, device management status and enrolled authenticators.


Specific guidance is provided in the following blog post:
[https://www.okta.com/en-au/blog/threat-intelligence/intrusion-actors-self-serve-their-way-into-accounts/](https://www.okta.com/en-au/blog/threat-intelligence/intrusion-actors-self-serve-their-way-into-accounts/)
