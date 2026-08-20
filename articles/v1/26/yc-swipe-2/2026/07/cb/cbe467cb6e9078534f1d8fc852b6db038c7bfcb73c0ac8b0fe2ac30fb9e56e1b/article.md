---
schema_version: "1.0.0"
document_id: "cbe467cb6e9078534f1d8fc852b6db038c7bfcb73c0ac8b0fe2ac30fb9e56e1b"
company_key: "yc-swipe-2"
company: "Swipe"
source_id: "yc-swipe-2-rss-f682396601ff"
canonical_url: "https://vectorx.com/the-shinyhunters-salesforce-breach-wave/"
published_at: "2026-07-08T19:14:19+00:00"
first_seen_at: "2026-07-24T02:59:28.395514+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:6d01f76ddee8a893713a2ea5615b2424be55076c297cc9d721588f6caee3fd01"
---

# The ShinyHunters Salesforce Breach Wave: Why Your Staff Is the Real Firewall

If you’ve been in business technology for the last 18 months, you’ve seen the headlines. ShinyHunters. Salesforce. Millions of records. Another big name compromised.


But here’s what most articles miss: these breaches have almost nothing to do with Salesforce being insecure. They have everything to do with a threat group that figured out how to exploit the one thing every platform tries to protect against…


Human behavior.


Let’s talk about what actually happened, why it matters for your team, and what you can do about it.


## What Actually Happened?


Starting in mid-2025, a coordinated campaign targeted Salesforce customers. Not through a vulnerability in Salesforce itself, Salesforce Security explicitly stated they found “no vulnerability inherent to the Salesforce platform.” The campaign exploited two primary attack vectors:


### Vector 1: Social Engineering via Voice Phishing (Vishing)


Attackers (tracked as ShinyHunters, also known as UNC6040) called help desk employees and sales staff, convincing them to approve malicious OAuth-connected apps inside their Salesforce org. They often impersonated “Data Loader” support or trusted vendors. The result? The attackers gained legitimate access tokens and could pull data directly through the Salesforce API with MFA completely bypassed because the token was authorized by an actual employee.


**Victim Scale:** Over 700 organizations were compromised through this method. Names that made headlines include Google (2.5M customer records), Workday (70M individual records),


### Vector 2: Misconfigured Experience Cloud Guest Access


A separate but related campaign exploited overly permissive guest user configurations on public-facing Experience Cloud sites. Salesforce issued an official security advisory on March 7, 2026, warning customers to audit their guest user settings.


**Victim Scale:** Hundreds of organizations, including Loblaw (75.1M records), Hallmark (7.9M records), Cisco, LexisNexis, ADT, Grubhub, and many more, were impacted either directly through misconfigured portals or through supply chain compromises.


The common thread? These were preventable attacks that required zero technical sophistication. No malware. No zero-day exploits. Just a phone call. A misconfiguration. A form field.


## Why Salesforce Isn't to Blame


This is important to get right: Salesforce’s platform security is enterprise-grade. The breaches happened because the attack surface isn’t just your firewall — it’s your people, your configurations, and your third-party integrations.


Salesforce themselves acknowledged this directly in their security advisory:


“Salesforce Security has been tracking an increase in threat actor activity which targets misconfigurations of publicly accessible sites.”


The platform provided the canvas. The attackers exploited how it was being used, not how it was built.


Want an Independent Security Audit?


LET'S CHAT


## The Real Story: Staff Training Is Your First Line of Defense


Here’s what I want every operations leader, IT manager, and department head to take away from this: the hackers didn’t breach Salesforce. They breached your organization’s awareness.


Think about the two attack vectors again:


1. The vishing attacks: These worked because someone on the phone sounded confident, used the right terminology, and got past whatever approval process existed. A staff member who’d been trained to recognize the red flags, like “Why am I being asked to approve a Connected App at 2 PM on a Tuesday?” could have stopped the attack at step one.


2. The Experience Cloud misconfigurations: These aren’t platform failures. They’re configuration oversights. Every org that lost data had a guest user profile with more permissions than necessary. Someone set that up. Someone needed to audit it. Simple as that.


The attackers aren’t geniuses. They’re opportunists. They found the path of least resistance and took it.


## What Should Your Team Do About This?


You don’t need a million-dollar security platform. You need basic operational hygiene:


### 1. Train staff on the "OAuth Red Flags"


Your team should know:


- No Salesforce admin should approve a Connected App without understanding what it does. If someone calls asking you to “just approve this Data Loader update,” that’s your first red flag.


- Check the app’s permissions. Does a chatbot really need access to your entire customer database? Does a survey tool need API write access?


- Audit your authorized apps quarterly. Salesforce has a built-in report for “Connected Apps Used.” Run it. Know what’s connected.


### 2. Audit Experience Cloud Guest User Settings


This is literally what Salesforce told customers to do, and it takes 20 minutes:


- Go to Setup → Experience Cloud → Guest User Settings


- Review the guest user profile


- Ensure it only has access to the objects and fields your public site actually needs


- If the guest user can see “Contact,” “Account,” or “Case” objects with edit access — that’s your exposure


### 3. Implement "Verify Before You Approve"


Create a simple protocol: any request to modify Salesforce permissions, install an app, or change integration settings must be verified through a second channel (email to a known address, in-person confirmation, or a secondary approver). This single policy would have blocked the vast majority of the vishing attacks.


### 4. Monitor for Unusual Data Exports


The attackers didn’t sneak in through a back door. They walked through a front door, pulled data systematically, and the only organizations that noticed were the ones that were:


- Watching their API call patterns


- Setting up alerts for bulk data exports


- Reviewing query logs (the attackers actually deleted theirs because they knew someone might check)


### 5. Treat Third-Party Integrations as Part of Your Attack Surface


When your Salesforce org connects to Drift, Salesloft, Gainsight, or any other integration — those connections are part of your security perimeter. Rotate tokens regularly. Revoke access for integrations you no longer use. Salesforce and Salesloft revoked the Drift integration after the August 2025 breach — but by then, 700 organizations had already been compromised.


## The Bottom Line


The ShinyHunters campaign is a cautionary tale, not a condemnation of any platform. The attackers won because they exploited the gap between what Salesforce provides and how organizations actually use it.


Here’s what the data shows: organizations that have basic security awareness training, quarterly permission audits, and a “verify before you approve” policy for Salesforce changes have been largely unaffected. The breaches happened to organizations that were using enterprise-grade security and treating it as an IT problem rather than an organizational one.


The lesson isn’t “be afraid of Salesforce.” The lesson is: your people are your strongest security control, and they need the right training to be effective.


Train them to question. Train them to audit. Train them to verify. The rest is configuration.


*Want to audit your Salesforce org’s exposure? Start with Connected Apps, review your Experience Cloud guest user profiles, and run a Security Health Check. These are free tools built into Salesforce.*
