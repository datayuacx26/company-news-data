---
schema_version: "1.0.0"
document_id: "4e077cf66ec22d1ef49babc1eb5e762d35d5022093efc5b9e7ae5958a33ab2ec"
company_key: "sangoma-technologies-corporation-common-shares"
company: "Sangoma Technologies Corporation Common Shares"
source_id: "sangoma-technologies-corporation-common-shares-news-import-599c15f50aea"
canonical_url: "https://sangoma.com/blog/stir-shaken-explained-why-business-calls-marked-spam/"
published_at: "2026-05-21T20:13:52+00:00"
first_seen_at: "2026-07-22T12:42:18.891395+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:2784ec663bb2ac067c1afd1f10041eb6dba97edbb686ccb1a13b61865ff245b6"
---

# What Is STIR/SHAKEN and Why It Matters for Your Business

STIR/SHAKEN is a call authentication framework, required by the FCC, that digitally signs outbound calls so receiving networks can verify the caller ID hasn’t been faked. STIR stands for Secure Telephony Identity Revisited; SHAKEN stands for Signature-Based Handling of Asserted information using toKENs. The two work together as a technical standard and governance framework for how providers sign and verify calls across IP networks.The impetus was straightforward: US consumers received close to 60 billion robocalls in 2019, eroding trust in voice communications to the point where people stopped answering calls from numbers they didn’t recognize. Congress passed the TRACED Act in response, and the FCC mandated that all voice providers implement STIR/SHAKEN.For businesses, the stakes are direct. When your outbound calls aren’t properly signed, terminating carriers such as Verizon, AT&T, and T-Mobile apply their own analytics on top of the signature check and may flag or block your calls before they ring. That hits answer rates, customer trust, and revenue, particularly for businesses that depend on outbound calls for appointment reminders, order confirmations, and patient follow-ups. STIR/SHAKEN shapes whether your calls arrive.


## **How STIR/SHAKEN Works**


When your business places an outbound call, your voice provider digitally signs it before it leaves their network. Downstream networks verify that signature before delivering the call to the recipient. Three roles make this work.Your voice carrier, which is the originating provider, checks the source of the call and signs it with a digital certificate issued by a certificate authority. That signed call travels through the network until it reaches the terminating provider, which is the recipient’s carrier. The terminating provider verifies the signature and decides whether to deliver, label, or block the call.One thing worth internalizing here: all of this happens at the carrier level. Your phone system is not involved in signing or verification. Whether this is done well or poorly is almost entirely on your provider.


## ****Understanding Attestation Levels****


Attestation is the single most important concept for understanding why calls get flagged. Every signed call carries one of three levels, and the level your provider assigns determines how much trust the terminating carrier extends to your call.


A (Full Attestation) means the provider authenticated the caller and verified they own the number being displayed. This is the highest trust level and the least likely to be flagged by downstream networks.B (Partial Attestation) means the provider authenticated the caller but cannot confirm they own the displayed number. Terminating carrier analytics still treat B-attested calls with reduced trust, and flagging remains common.C (Gateway Attestation) means the provider received the call from another network and cannot vouch for the caller or the number. Calls at this level are frequently blocked outright.Two providers can both claim STIR/SHAKEN compliance while signing at different levels. If one signs at A and the other at B, their customers will see measurably different answer rates. Compliance is a threshold; attestation level is the real performance driver.


## ******The Two Ways STIR/SHAKEN Affects Your Business******


STIR/SHAKEN shapes both sides of your phone traffic: whether your outbound calls reach customers, and whether your team’s inbound lines are protected from fraudulent calls. How your provider implements the framework determines both outcomes.


## **Outbound: Why Your Calls Get Flagged as Spam**


The most common reasons legitimate business calls get flagged, even when signed:


The originating provider signs at B or C attestation instead of A, so terminating carrier analytics applies reduced trust by default.Outbound call volume or dialing pattern from a single number resembles robocaller behavior, triggering analytics flags regardless of attestation level.Recipients have previously reported the number as spam, creating a reputation score that persists even after signing is in place.Numbers recently assigned from a carrier pool may carry the prior holder’s bad reputation.The consequence is concrete. A pharmacy’s appointment reminders showing as “Spam Likely” means patients miss appointments. A restaurant group’s reservation confirmations going unanswered means no-shows. A retailer’s delivery alerts getting blocked means customer service volume goes up. In each case, the problem is traceable to provider implementation, not the business’s intent.


## **Inbound: How STIR/SHAKEN Protects Your Team**


On the inbound side, your carrier verifies the STIR/SHAKEN signature on calls arriving at your network. Unsigned calls, or calls signed at low attestation levels, can be labeled as suspected spam before they ever ring your team’s phones.That filtering has practical value beyond convenience. Employees stop chasing every unknown number, which reduces interruptions and lets them focus on actual customer calls. More significantly, it raises the bar for attacks: vishing attempts, CEO impersonation calls, and spoofed-number scams become harder to execute when they can’t pass attestation checks.


## **Why Your Calls Are Still Getting Flagged Even With STIR/SHAKEN**


Being “STIR/SHAKEN compliant” doesn’t mean your calls will stop getting flagged. Compliance is binary, meaning you sign or you don’t. What actually determines your answer rates is the quality of implementation, and there are several reasons flagging continues even after compliance is in place.Signing at B attestation instead of A leaves calls exposed to carrier analytics that treat partial attestation as a reduced-trust signal. Number reputation damage that predates signing persists, because historical call data doesn’t reset when a signature is added. High outbound volume from a single number can trigger analytics flags regardless of attestation level. Terminating carriers apply their own proprietary scoring on top of STIR/SHAKEN, which means a clean signature doesn’t override a poor reputation score.There’s also a gap in the framework that STIR/SHAKEN alone was never designed to close. Fraudsters can spoof numbers that should never place outbound calls in the first place: your inbound-only support line, your appointment desk, numbers used for internal routing. When scammers use those numbers to blast fraudulent calls, recipients report them as spam and that reputation damage lands on your number. Your business didn’t make those calls, but you carry the consequence.The FCC caught up to this gap and mandated[Do Not Originate (DNO) enforcement](https://sangoma.com/blog/do-not-originate-dno-why-itsps-must-act-now-to-stay-compliant-protect-their-networks-and-restore-trust-in-voice-calling/) as of December 15, 2025, requiring all voice providers to block call attempts originating from numbers that should never place outbound calls. If your provider isn’t enforcing DNO, your numbers are still available for spoofing, and every spoofed call erodes a reputation you’ll have to rebuild yourself.STIR/SHAKEN is necessary infrastructure. What your provider does around it — attestation level, DNO enforcement, reputation monitoring — is what actually moves your answer rates. That shift, where compliance determines whether calls connect rather than just whether regulations are met, is[reshaping how the entire telecom stack operates](https://sangoma.com/blog/telecom-compliance-is-not-about-avoiding-fines-anymore/) .


## **What Your Provider Should Be Doing (And What’s On You)**


Understanding where responsibility sits helps you evaluate your current provider honestly.Your[business communication provider](https://sangoma.com/blog/top-ucaas-providers-buyers-guide/) ’s job is to sign all your outbound calls at full A attestation, manage SPC tokens, certificates, and renewals without requiring your involvement, verify inbound calls against STIR/SHAKEN signatures, monitor your number reputation and handle remediation when flagging occurs, and enforce DNO rules on your inbound-only numbers so scammers can’t spoof them. Providers also need to stay current on FCC rulemaking — the framework is still evolving, with active rulemakings on non-IP authentication and Rich Call Data.Your business’s job is narrower: register branded caller ID with your carrier if offered, follow calling hygiene by contacting only opted-in recipients and respecting do-not-call lists, report flagged numbers to your provider promptly, and consolidate outbound calling under a single carrier that signs at A attestation rather than splitting traffic across providers with inconsistent practices.


## **How to Tell If Your Current Telephony Provider Is Actually Doing the Job**


These are the questions worth asking any provider, whether current or prospective.


Do you sign all my outbound calls at full A attestation? A provider signing at B will often answer “we’re STIR/SHAKEN compliant” without specifying level. Push for the direct answer.Do you own the infrastructure that signs my calls, or do you use a third-party signer? Under the FCC’s Eighth Report and Order, providers must now sign calls with their own certificates. Third-party signing arrangements are no longer sufficient for compliance.Do you offer caller ID reputation monitoring and remediation? Signing at A reduces the risk of flagging; it doesn’t eliminate it. A provider without reputation monitoring has no early warning system and no path to remediation.Do you manage SPC token and certificate renewals, or does any of that require action from my team?Do you enforce DNO on my inbound-only numbers?What happens to voice during an internet outage. Do outbound calls still sign and route through failover?Weak answers on attestation level, certificate ownership, or reputation remediation are direct indicators of reduced answer rates. A provider that can’t explain their DNO enforcement is leaving your inbound-only numbers exposed.


## **How to Remediate Numbers That Are Already Flagged**


If numbers are already flagged, remediation takes one of two paths depending on your volume and available resources. **For businesses handling it directly:** start by checking which numbers are flagged using free reputation lookup tools from major analytics providers. Submit disputes with the carriers and call-blocking apps flagging your numbers, and register with free caller registry services where available. Review your outbound dialing patterns. High volume from a single number or calling unverified contacts accelerates flagging. Avoid rotating numbers reactively; short-lived numbers signal suspicious behavior to carrier analytics and tend to get flagged faster than numbers with stable history. Sangoma’s wholesale knowledge base includes a[detailed guide on mitigating Spam Likely labels for SIP trunking customers](https://sangomakb.atlassian.net/wiki/spaces/WCS/pages/663519294/Wholesale+SIP+Trunking+-+Mitigating+Spam+Likely) . **For businesses with high outbound volume** or many numbers, manual remediation doesn’t scale. A managed service monitors reputation across carriers and call-blocking apps continuously, catches flagging before it shows up in your answer rates, and handles dispute filing directly. The difference between the two approaches is the difference between reacting to complaints and catching the problem before customers notice. Sangoma’s[Caller ID Reputation (CIDR) service](https://carrierservices.sangoma.com/caller-id-reputation-monitoring-remediation-sangoma-carrier-voice/) provides continuous monitoring and remediation for businesses that need it.


## **How Sangoma Keeps Your Business Calls From Getting Flagged**


[Sangoma Carrier Voice](https://carrierservices.sangoma.com/voice/) (the carrier network underlying[Sangoma’s unified communications platforms](https://sangoma.com/products/communications-platform/) ) signs outbound calls at full A attestation. Since Sangoma owns both the communications platform and the underlying carrier infrastructure, there is no handoff to a third-party signer and no gap in the signing chain. Businesses running[Sangoma cloud](https://sangoma.com/products/communications-platform/cloud/) ,[hybrid](https://sangoma.com/products/communications-platform/hybrid/) , or[on-premises UC](https://sangoma.com/products/communications-platform/on-premises/) with a Sangoma voice service get A-attestation signing without managing it separately.On the inbound side, calls terminating on the Sangoma network are verified against STIR/SHAKEN signatures, so suspected robocalls are labeled before reaching your team’s phones. Sangoma also enforces DNO compliance across its network, blocking origination attempts from numbers that should never place outbound calls which protects your inbound-only numbers from being weaponized by fraudsters and prevents the reputation damage that follows. You can review Sangoma’s[STIR/SHAKEN compliance page](https://carrierservices.sangoma.com/stir-shaken-compliance/) for specifics on certificate management and attestation infrastructure.For number reputation, Sangoma’s[Caller ID Reputation (CIDR) service](https://carrierservices.sangoma.com/caller-id-reputation-monitoring-remediation-sangoma-carrier-voice/) monitors how your outbound numbers are labeled across carriers and call-blocking apps, surfaces flagging before answer rates drop, and handles remediation directly. Customers using CIDR see answer rate improvements of up to 30%.Sangoma owns the full voice stack from signing, SIP trunking, and wholesale carrier services, which eliminates the third-party dependencies that create attestation gaps at other providers. Support for attestation and signing issues is available 24/7 through US-based teams as part of the standard support relationship. Local survivability with 4G/5G failover keeps voice running during internet outages, so call delivery reliability extends beyond the signing infrastructure.


## **STIR/SHAKEN FAQsWhat is STIR/SHAKEN?**


STIR/SHAKEN is the FCC-mandated call authentication framework that digitally signs outbound calls so receiving carriers can verify the caller ID is accurate. STIR (Secure Telephony Identity Revisited) defines the technical standards; SHAKEN (Signature-Based Handling of Asserted information using toKENs) defines the governance framework providers follow to implement it. Together, they form the infrastructure that determines whether a call is delivered, labeled as spam, or blocked.


## **Do I need to do anything as a business to be STIR/SHAKEN compliant?**


For most businesses, no direct action is required, STIR/SHAKEN compliance is your provider’s responsibility. What you can do is verify that your provider is signing your calls at full A attestation, register branded caller ID if your provider offers it, and maintain clean calling hygiene. The decisions that most affect your answer rates are made at the carrier level, not on your phone system.


## **Why are my business calls still being flagged as spam?**


Several factors can cause flagging even with STIR/SHAKEN in place: your provider may be signing at B attestation instead of A, your numbers may carry historical reputation damage that predates signing, high outbound volume from a single number can trigger analytics flags, and terminating carriers apply their own proprietary scoring on top of the STIR/SHAKEN signature. DNO enforcement gaps can also expose your inbound-only numbers to spoofing-driven reputation damage you didn’t cause.


## **What are attestation levels, and which one does my provider use?**


Attestation levels indicate how much trust a provider can vouch for on a given call. A (Full Attestation) means the provider authenticated the caller and confirmed they own the displayed number. B (Partial Attestation) means the caller was authenticated but number ownership wasn’t confirmed. C (Gateway Attestation) means the call arrived from another network and neither caller nor number could be verified. A attestation gives your calls the best chance of delivery; B and C leave them exposed to carrier analytics that may flag or block them. Ask your provider directly which level they sign at.


## **Does STIR/SHAKEN block all robocalls?**


No. STIR/SHAKEN authenticates caller ID; it doesn’t determine whether a call’s intent is fraudulent. Calls can pass signature verification and still be flagged by carrier analytics based on dialing patterns, reported complaints, or number reputation. Conversely, fraudsters can sign calls through complicit providers and still deliver robocalls. STIR/SHAKEN reduces spoofing; it’s one layer of a broader robocall mitigation approach that also includes DNO enforcement, reputation monitoring, and call analytics.


## **How do I know if my provider signs at full A attestation?**


Ask them directly: “Do you sign all my outbound calls at full A attestation?” Many providers will confirm STIR/SHAKEN compliance without specifying the level. A provider signing at A should be able to answer that question without qualification. If the answer involves phrases like “where possible” or “depending on the call path,” that typically means B attestation is in play for some traffic.


## **Can a number that’s already been flagged be fixed?**


Yes, but it takes active remediation. Flagging isn’t permanent, but it also doesn’t resolve on its own. The process involves identifying which carriers and call-blocking apps have flagged the number, submitting disputes, and in some cases registering with caller registry services. For businesses with high volume or many numbers, a managed reputation service handles this continuously rather than reactively. See the remediation section above for both the DIY and managed paths.
