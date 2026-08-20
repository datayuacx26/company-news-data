---
schema_version: "1.0.0"
document_id: "b72ef58171edb06419a3b8dedd1bdcd1eaca1749869de0686d55e0eb41008aa8"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/github-secret-scanner"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:7a5cd5e55eb12a5d4e31e2ea4fb0d06771a8111cf7d3339a621a026b2ce9a014"
---

# GitHub Secret Scanner

API keys leak in ways that are easy to miss: an` .env` file is committed by mistake, a key is hardcoded in a quick script, or a snippet is pasted into a public gist or issue.


It happens to our customers every day, and for email, an exposed key can lead to serious deliverability issues that can be hard to recover from.


**Resend is now a[GitHub secret scanning partner](https://github.blog/changelog/2026-07-15-improvements-to-secret-scanning-and-public-monitoring/) .** When a Resend API key is exposed anywhere GitHub scans (public repositories, gists, issues, and more), GitHub notifies us the instant it appears and we act on it automatically.


## What happens when a key leaks


1. **Detected instantly** : GitHub matches Resend key patterns across public content and alerts us the moment one shows up.
2. **Revoked automatically** : Once we're notified, we disable the exposed key immediately.
3. **Your team is notified** : We send an email to all admins with a link to where the key was found, so you can rotate it and clean up.


This security protection is **on by default for every Resend account.**


## Why this matters for deliverability


A leaked key is always a security problem, but for email it can also be a deliverability problem.


The moment a Resend key goes public, anyone can send email from your verified domain. An attacker can use the exposed key to send spam or phishing from your verified domain, causing mailbox providers to flag your domain for spam or block delivery entirely.


**Domain reputation can take weeks of careful sending to rebuild.** Automatic revocation closes that window as quickly as possible to protect your account and reputation.


## Conclusion


Exposed credentials should never become a deliverability crisis. Now, if a Resend key ever ends up somewhere public on GitHub, it's revoked before it can do any harm.
