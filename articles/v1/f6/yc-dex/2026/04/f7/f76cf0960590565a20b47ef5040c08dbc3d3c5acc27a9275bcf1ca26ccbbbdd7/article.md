---
schema_version: "1.0.0"
document_id: "f76cf0960590565a20b47ef5040c08dbc3d3c5acc27a9275bcf1ca26ccbbbdd7"
company_key: "yc-dex"
company: "Dex"
source_id: "yc-dex-rss-adf3eee30171"
canonical_url: "https://getdex.com/blog/monica-review/"
published_at: "2026-04-06T22:10:00+00:00"
first_seen_at: "2026-07-20T23:23:49.748948+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:537013613e1082096373522581ea0c221c929589b7829ba67baece1aac6a64c4"
---

# Monica Review 2026: Pricing, Features, and Who It's For

[Monica](https://monicahq.com/) is the personal CRM that privacy-focused communities on Hacker News and r/selfhosted keep recommending. It is open-source, self-hostable, and stores your contact data on your own server. It also has no email sync, no calendar sync, no LinkedIn integration, no mobile app, and requires you to manually enter every interaction.


Whether that tradeoff works depends entirely on what you care about. This review covers what Monica actually does, what it costs (self-hosted vs. cloud), where it falls short, and who should use it instead of a more automated tool.


## What Monica does well


Monica is a personal relationship manager, not a networking tool. It was built to help you remember details about the people in your life, not to automate outreach or manage a sales pipeline.


**Contact profiles with real depth.** Each contact gets fields for addresses, phone numbers, important dates, family relationships, work history, food preferences, pet names, and free-form notes. You can document how people are related to each other. My partner's mother's birthday? Monica reminds you. Most professional CRMs do not go this deep into personal details.


**Journaling tied to contacts.** Monica includes a journaling system where entries link directly to specific contacts. If you write about a dinner with friends, you tag each person. Over time, this builds a timeline that is part diary, part relationship log. No other personal CRM I tested offers this.


**Reminders that actually work.** You set reminder intervals per contact (weekly, monthly, quarterly, yearly). Monica sends notifications at 30 days, 7 days, and day-of by default. You can customize these. Reminders sync to CalDAV-compatible calendars.


**Activity logging.** You can log calls, meals, meetings, and custom activity types against contacts. Each entry gets a date, description, and optional emotion tag. It is manual, but structured.


**Debts and gifts tracking.** Monica tracks money you owe or are owed, and gift ideas per contact. I used the gift tracker more than I expected. Small feature. Surprisingly useful around holidays.


**Full data ownership.** Self-hosted Monica stores everything in a MySQL/MariaDB database on your infrastructure. No third-party data processing. No analytics. No tracking. The project explicitly states it will never be ad-supported and will never sell user data.


###
What Monica does not do


**No email integration.** Monica cannot connect to Gmail, Outlook, or any email provider. It does not read your inbox, log email interactions, or auto-create contacts from correspondence. Every email interaction must be manually entered as an activity note. If automatic email logging is what you need, see our full breakdown of[personal CRM tools with email and calendar sync](https://getdex.com/blog/personal-crm-email-calendar-integration/) .


**No calendar sync.** Monica supports CalDAV for pushing its own reminders to your calendar. It does not pull calendar events in. If you had a meeting with someone yesterday, Monica does not know unless you tell it.


**No LinkedIn sync.** No connection import, no job change tracking, no profile enrichment. If your professional network lives on LinkedIn, Monica is effectively invisible to it. Dex is the strongest alternative here — see how we use it as a[LinkedIn CRM](https://getdex.com/product/linkedin-crm/) .


**No AI features.** No automated contact enrichment, no meeting briefs, no suggested follow-ups, no message drafting. Everything is manual and deliberate.


**No native mobile app.** Monica is a web application. On mobile, it loads in a browser. The layout is only partially responsive, and manipulating data on a phone is difficult. Multiple reviewers cite this as a blocking issue.


**Server-rendered pages.** Every click triggers a full page reload. With a few hundred contacts, performance is acceptable. It is not snappy. If you are used to single-page apps like Dex or Mesh, the UX feels dated.


**Limited API integrations.** Monica has a REST API and connects to n8n for automation workflows. But there are no prebuilt integrations with common tools. If you want Monica to talk to anything else, you build it yourself.


**The UI is functional, not modern.** Monica's interface is clean and sidebar-based, but built around forms and manual navigation. Every action requires clicking into a contact, finding the right tab, and submitting a form, which works fine but never feels fast compared to tools like Dex or Mesh that surface information proactively (and rather beautifully).


##
Pricing: self-hosted vs. cloud


Option Monthly cost What you get What you handle


Self-hosted (Docker) $0 software + $5-10/mo VPS All features, unlimited contacts, full data control Server setup, updates, backups, SSL, database maintenance


Cloud hosted (monicahq.com) $9/mo or $90/year All features, managed hosting, automatic updates Nothing (Monica team handles infrastructure)


Free cloud tier $0 Limited to 10 contacts Nothing


**The real cost of self-hosting.** The software is free. The server is not. A basic VPS from DigitalOcean or Hetzner runs $5-10/month. You also need to factor in your time: initial Docker setup (30-60 minutes if you know Docker, several hours if you do not), ongoing updates, database backups, and SSL certificate management. If you value your time at $50/hour and spend 2 hours a year on maintenance, the effective annual cost is $160-220, more than Dex ($144/year) or the Monica cloud plan ($90/year).


Self-hosting only makes financial sense if you already run a home server or VPS for other things and the marginal cost of adding Monica is close to zero. Or if data sovereignty is worth the premium to you, which for some people it genuinely is.


## **The self-hosting experience**


Monica ships as a Docker image. The official docs walk through the setup, and the community on GitHub (22,000+ stars) is active and responsive.


What you need:


- A server running Docker (VPS, home server, or Heroku)
- MySQL 8.0+ or MariaDB 10.3+
- PHP 8.1+ (handled by the Docker image)
- A domain with SSL (Let's Encrypt works)


What to expect:


- Initial setup takes 30-60 minutes with Docker Compose
- The .env configuration file has about 40 variables, most of which have sane defaults
- Email notifications require an SMTP server (Mailgun, SendGrid, or your own)
- Updates mean pulling the latest Docker image and running migrations
- Backups are your responsibility (cron job + mysqldump is the common approach)


If you have self-hosted apps, Monica is straightforward. If "Docker Compose" means nothing to you, use the cloud plan or pick a different tool.


##
Feature comparison: Monica vs. paid alternatives


Feature Monica (self-hosted) Dex ($12/mo) Mesh (free tier)


Gmail auto-sync No Yes Yes


Calendar auto-sync No (CalDAV push only) Yes Yes


LinkedIn sync No Yes (up to 9,000 connections) Yes (1,000 contact cap)


Auto-log interactions No Yes Yes


Mobile app No (web only, poor mobile UX) Yes (iOS + Android) Yes (iOS)


AI features No Pre-meeting briefs, message drafts Contact enrichment


Journaling Yes (linked to contacts) No No


Gift/debt tracking Yes No No


Family relationship mapping Yes (detailed) Basic Basic


Data ownership Full (your server) Third-party hosted Third-party hosted


Open source Yes (AGPL-3.0) No No


Price $0 + server costs $12/mo Free (1,000 cap)


Monica wins on privacy, data ownership, and personal-life features (journaling, family mapping, gifts). It loses on everything that requires automation or integration with the tools you already use for communication.


For a broader look at how these tools stack up, see our[top 10 personal CRMs for networking (2026)](https://getdex.com/blog/personal-crm-for-networking/) .


*Prefer video? We ranked and reviewed the top 10 personal CRMs hands-on:*


## **Who should use Monica**


If you already run Nextcloud, Bitwarden, and Firefly III on a VPS, adding Monica to your stack is trivial. The marginal cost is near zero and you already have the maintenance habits.


Monica was also built to remember your friend's kid's name, your aunt's birthday, and what you talked about at dinner last month. If your use case is "be a more thoughtful person in my personal life," Monica does this better than any professional CRM. For a broader guide on[finding the right personal CRM](https://getdex.com/guides/finding-the-right-personal-crm/) for your situation, that page walks through every major use case.


Journalists, researchers, and anyone with sensitive contacts should also consider it seriously. If your contact list includes sources who could be harmed by a data breach at a third-party SaaS company, self-hosted Monica removes that risk entirely.


And some people find that manually logging an interaction forces them to actually reflect on the relationship. Monica's manual-everything design is a feature for this group, not a bug.


## **Who should skip Monica**


If you are a founder, salesperson, recruiter, or consultant managing hundreds of professional contacts across email, calendar, and LinkedIn, Monica's manual entry requirement will break you. You need auto-sync. Dex ($12/mo) connects to Gmail, Google Calendar, and LinkedIn and logs interactions without manual effort. See our guide to[personal CRM for founders](https://getdex.com/product/founders/) for a closer look at that use case.


No mobile app is a real problem if you need to look up a contact before a meeting while walking to the conference room. Dex and Covve both have dedicated iOS and Android apps.


If you have never run a Docker container, use the $9/month cloud plan. And at $9/month, you should also consider whether Dex at $12/month or Mesh's free tier — with actual auto-sync — is a better value.


Monica is also single-user. No shared access, no pipelines, no handoffs. If two people need to see the same contacts, this is not the tool.


## **The verdict on Monica as a Personal CRM**


Monica is the best personal CRM for people who want full data ownership and are willing to do all the work themselves. It is genuinely good at what it does: structured documentation of personal relationships with reminders, journaling, and family mapping.


But it is not a productivity tool. It does not save you time. It does not automate anything. It adds work to your day in exchange for privacy and depth. For some people, that exchange is worth it. For most professionals managing a network of 200+ contacts, it is not.


If you care about privacy but also need auto-sync with email, calendar, and LinkedIn, Monica cannot help you. That gap is where tools like[Dex](https://getdex.com/) sit: automatic sync with Gmail, Google Calendar, and LinkedIn (up to 9,000 connections), AI pre-meeting briefs, and keep-in-touch reminders, starting at $12/month. You trade data ownership for automation. Whether that trade works for you depends on which problem is bigger: the privacy risk or the 30 hours a year you lose to manual entry.


---


## **Frequently asked questions**


### **Is Monica CRM really free?**


The software is free and open-source (AGPL-3.0 license). Self-hosting requires a server, which costs $5-10/month for a basic VPS. The managed cloud version at monicahq.com costs $9/month or $90/year. A free cloud tier exists but is limited to 10 contacts.


### **Does Monica sync with Gmail or Outlook?**


No. Monica has no email integration. It does not connect to any email provider, does not read your inbox, and does not auto-log email interactions. Every interaction must be manually entered. If email sync is important to you, see our full comparison of[personal CRM tools with email and calendar sync](https://getdex.com/blog/personal-crm-email-calendar-integration/) - it covers Dex, Folk, Mesh, and Streak side by side.


### **Can I use Monica on my phone?**


There is no native mobile app. Monica runs as a web application that you access through a mobile browser. The layout is only partially responsive, and multiple reviewers report that data entry on mobile is difficult. If mobile access matters, Dex and Covve both have dedicated iOS and Android apps.


### **How does Monica compare to Dex?**


Monica and Dex solve different problems. Monica is a privacy-first, open-source tool for documenting personal relationships with no automation. Dex is a professional networking CRM that auto-syncs Gmail, Google Calendar, and LinkedIn connections. Monica is free (self-hosted) but requires manual data entry. Dex costs $12/month but saves roughly 30 hours a year through automation. For a deeper side-by-side, see our[Notion vs. Dex comparison](https://getdex.com/blog/notion-vs-dex/) — many of the same tradeoffs apply. Choose Monica if data ownership is your priority. Choose Dex if time savings and integration are more important.


### **Is Monica still actively maintained?**


Yes. The GitHub repository (github.com/monicahq/monica) shows active development with 22,000+ stars. The project has a core team and accepts community contributions. Updates are released periodically, and the community on GitHub is responsive to issues.
