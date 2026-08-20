---
schema_version: "1.0.0"
document_id: "238a127e33c46688ec254e3b29c6f3cb42b6fec97dd6fd4a9ca08e481d976d64"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/email-deliverability-life360-915585ea6f61"
published_at: "2025-07-22T19:35:08+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:ab2d4f557a6306447ca5885cb389283de6bde0cb7c710011040bbcb227b3b859"
---

# Email Deliverability@Life360

Email Deliverability


Life360


Engineering


DNS


Spf


# We Fixed Our Email Deliverability @Life360 (So You Can Too)


[Asha Kasinath](https://medium.com/@asha_18524?source=post_page---byline--915585ea6f61---------------------------------------)


7 min read


·


Jul 22, 2025


--


Press enter or click to view image in full size


**Why did the email get sunburnt at the beach party?**


**Spoiler: That sunburn? It was our deliverability problem. Read on to find out more.**


## 📭 Emails Weren’t Landing — and No One Knew Why


It all began with a login problem.


Some users weren’t getting their One-Time Passcodes (OTPs) by email. No OTP meant no access.


OTPs are a key part of the Life360 login flow, designed to give users a fast, frictionless way to sign in without needing to remember a password. Just enter your phone number or email, get a temporary code, and you’re in. It’s simple, secure, and there’s no “Forgot Password?” drama.


But when those emails didn’t arrive, our sleek login flow ground to a halt.


We assumed it was a simple deliverability hiccup. But the deeper we dug, the more we found. Our legacy email system, dating back to 2008, had no clear owner, limited visibility, and critical misconfigurations.


Behind the scenes, we had:


- Domain authentication quietly failing
- Shared IP addresses hurting our reputation
- Test automation sending garbage traffic
- Some of our IPs on blacklists


Fixing OTPs led us to rethink how we handle email, and we ended up building a better, more transparent, and reliable system.


## 🗺️ Lost in Legacy: The Email Infrastructure Expedition


A person opens a door to reveal a dusty, cobweb-filled server room with old hardware racks. A monitor displays a mail icon, symbolizing outdated or neglected email infrastructure. The scene conveys legacy tech that’s been left untouched for years.


Our email system was a relic from Life360’s early days. Many teams used it. Few understood it.


Critical emails like OTPs, password resets, and account activations were all tangled together. There was no separation of concerns, no clear monitoring, and no obvious way to know if things were working.


## 🔍 OTP Debugging Adventures


Put yourself in a developer’s shoes:


- You trigger the OTP flow. The email never arrives.
- You check logs, yes, the email request went out.
- You try to find the dashboard for email delivery. But where is it?
- Days go by as you hunt down how to gain access to this elusive dashboard.
- Finally, you get in. You see charts, but nothing helpful.
- You look for OTP metrics. It’s all too broad, too delayed.
- And still, you can’t tell if the user ever got the email.


## 🎭 Deliverability Drama Uncovered


We audited the system and found several root causes:


**🧨Misconfigurations**


- Missing or malformed **SPF** , **DKIM** , and **DMARC** records.
Domain authentication was failing silently.


**📉 IP Reputation Woes**


- QA and Production environments shared a single IP.
- Microsoft had blacklisted our IPs due to bounce rates and suspicious patterns.


**🤖 Automation Misfires**


- Test emails were being sent to invalid domains like life360-automations.com.
- These invalid targets were contributing to spikes in hard bounce and blocks.


## How We Cleaned House


### ✅ Step 1: Domain Authentication


We properly configured SPF, DKIM, and DMARC using the email provider’s out-of-the-box tools and deployed everything using Terraform. This provided a consistent, trackable infrastructure.


### ✅ Step 2: IP Pool Isolation


We segmented traffic into function-specific IP pools:


Press enter or click to view image in full size


Diagram showing Life360 email sending architecture. A Life360-branded envelope icon branches into two email types: “OTP Emails” and “Password Reset Emails.” Each type is associated with two dedicated IP addresses—IP1 and IP2 for OTP and IP3 and IP4 for Password Reset—illustrating IP pool isolation by function.


- **OTP emails** → 1–2 IPs
- **Forgot password** → 1–2 IPs
- Also, **** separate IPs for **QA** and **Prod**


This allowed us to monitor and preserve IP reputation by flow.


### ✅ Step 3: Aligning rDNS with Sending Domains


We configured reverse DNS (rDNS) to match our sending domains. This step ensures that the IP address used to send email properly resolves back to the authenticated domain — a key deliverability signal for many mailbox providers. Misaligned or missing rDNS records can lead to lower trust and increase the chance of emails getting filtered or blocked.


### ✅ Step 4: Monitoring IP Reputation via Microsoft SNDS


Mailbox providers don’t always tell you directly when things go wrong. Tools like SNDS and Google Postmaster give senders a behind-the-scenes look into how their infrastructure is performing from the inbox provider’s perspective. We enrolled our IPs in Microsoft’s Smart Network Data Services (SNDS), a tool that provides visibility into how Microsoft sees your email traffic. It surfaces insights like:


- Volume of email sent
- Spam complaint rates
- IP reputation and trap activity


### ✅ Step 5: Stopping Automation-Generated Noise


We introduced a logic change to prevent automation-generated test emails from hitting email providers’ APIs, especially those targeting fake or invalid domains used during QA. This significantly reduced the volume of invalid email traffic, protected our sender reputation, and helped ensure production deliverability remained healthy.


This means we can debug issues faster and stop sending to known-bad addresses immediately.


### ✅ Step 6: SPF Cleanup — Because Silent Fails Are the Worst


SPF records have a DNS lookup limit of 10. We were dangerously close to hitting it, thanks to years of accumulating “includes” for third-party tools. We pruned it down with the help of Life360 ITOPS, preserving necessary includes and ditching the legacy bloat, ensuring reliable authentication without the risk of silent failure.


## 🧠 OTP: Our Accidental Deliverability Hero


When we launched Email OTP for frictionless login and settings, we also opened the door to a powerful deliverability advantage, whether we intended to or not.


Every OTP email acts as a mini signal of email validity.


- If it’s delivered and used, the address is likely healthy.
- If it bounces or goes ignored, it may be fake, mistyped, or inactive.


**The potential outcome:**


→ A naturally cleaner database over time


→ Improved inbox placement rates


→ A healthier, more responsive email ecosystem


We’re just beginning to explore how OTP signals can inform our broader deliverability strategy.


## 📉 What Was Failing (And Why It Mattered)


Here’s what 120,000+ bounce events taught us:


Press enter or click to view image in full size


Donut chart showing email bounce reasons. The largest segments are “Mailbox unavailable” at 60.2% (orange-yellow) and “Invalid address” at 29.4% (orange). Smaller segments include “Unclassified” at 9.2%, “Technical” at 0.8%, “Reputation” at 0.4%, “Content” at 0.02%, and “Frequency/volume too high” at 0.01%. Legend with corresponding colors is shown below the chart.


- **60.2%** → Mailbox unavailable (deactivated inboxes, full mailboxes)
- **29.4%** → Invalid addresses (typos, temp domains)
- **9.2%** → Unclassified/transient issues
- **<1.2%** → Technical errors or reputation-based rejections


This data helped us prioritize smart validation tools and observability upgrades.


## 🔮 The Road Ahead


### 🛡 Smart Email Validation at Input


We’re evaluating various **Email Validation tools** to catch malformed, fake, or disposable addresses **before** sending.


### 🛰 Enhanced Real-Time Monitoring


Integrating more webhook-based insights with client behavior to proactively guide users when emails fail, not after the fact.


## 🛠 Building a New Email Path? Here’s the Playbook


If you’re setting up a new email stream, whether it’s for OTPs, password resets, or user notifications, here’s a general playbook we’ve found helpful for building a strong, deliverability-friendly setup:


Press enter or click to view image in full size


Blueprint-style visual titled “New Email Flow” with a checklist of steps: Create subuser account, create API keys, add sending domain with authentication, create link branding to match sender domain, get dedicated IPs (no sharing), apply reverse DNS (rDNS), and a final note saying “And you’re done. Start sending” with a paper airplane icon.


**Use different domains/accounts for Segregation:** Each email flow (like marketing, OTP, or support) should ideally live in its own domain/account. This keeps traffic isolated and makes metrics easier to monitor.


- **Set Up Domain Authentication per account:** Authenticate your sending domain with SPF, DKIM, and DMARC. This helps mailbox providers verify that your emails are legit.
- **Create account-specific API Keys:** Generate API keys scoped only to the subaccount’s needs. This keeps access secure and controlled.
- **Add Link Branding for Your Domain:** Configure link branding to ensure that any click-tracked links match your domain, a key signal for trust and inbox placement.
- **Use Dedicated IPs Where Appropriate:** Dedicated IPs give you more control over your sending reputation. Group IPs by email type to protect critical flows (like OTPs) from less predictable ones.
- **Set Up Reverse DNS (rDNS) Records:** Once IPs are assigned, configure reverse DNS to match your sending domain. This closes the loop and signals to receiving servers that your infrastructure is authenticated and well-managed.


All these steps, from domain auth to rDNS, work together to establish trust with mailbox providers. They reduce the risk of your emails being flagged as spam and improve the chances of reaching the inbox reliably.


## 💌 Final Thoughts


Email deliverability isn’t just an ops problem,


It’s a **product experience** issue.


If you ever spot a similar issue in your own systems — users not receiving key emails, mysterious login failures — here’s what helped us:


- Traced the problem end-to-end, from user reports to email infrastructure
- Audited domain authentication (SPF, DKIM, DMARC).
- Moved from shared to dedicated IPs.
- Cleaned up noisy, unnecessary traffic.
- Prioritized clear ownership and monitoring for the existing flow.


When we got all that right, everyone won: engineering, support, and most importantly, the end user.


## **😂 📧** And before we go, here’s the punchline to that email joke…


## 🥁 *Drum roll please…* 🥁


Press enter or click to view image in full size


*Did you get it? SPF isn’t just sunscreen — your domain’s Sender Policy Framework is just as important. 😉*
