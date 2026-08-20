---
schema_version: "1.0.0"
document_id: "12aa118168333fe3cd029240f05ee07e0fa9c8848e3fb68619091f3c74cc1ba7"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/test-2fa-using-stably"
published_at: "2025-08-02T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:36aadc46808c7ae1a4c21cf8dc3556e60a8ea9365e2a25ff9ce4bde88c31daf0"
---

# Test 2FA Using Stably

For Engineering Managers, Senior Engineers, and CTOs, 2FA flows are critical—but traditionally painful to test. You need an inbox, you need to extract the OTP or magic link, and you need tests that can run cleanly in CI without flakiness.


Stably makes this trivial.


## 2FA Testing With the Stably No-Code Editor


If you use the Stably no-code editor, everything is built-in out of the box:


1.


**You add a "Setup Email Inbox" step.**


2.


**Stably generates a unique, isolated test inbox.**


3.


**Your app sends the OTP or magic link to that inbox.**


4.


**Stably automatically captures the email and extracts the code/link into a variable.**


5.


**The next step in your test flows uses that variable to complete login.**


No IMAP scripts. No Gmail workarounds. No brittle parsing.


It just works.


## Coming Soon: Inbox Support in the Stably SDK


If your team prefers writing Playwright or TypeScript tests directly, the Stably SDK will soon support the exact same inbox capability:


```text
const inbox = await stably.createInbox();
const otp = await stably.waitForOtp(inbox);


```


This means:


- **Deterministic email capture**
- **Structured OTP/magic-link extraction**
- **Parallel-safe inboxes**
- **Identical behavior to the no-code editor**


All inside your codebase—no custom infrastructure needed.


## Why This Matters for Engineering Teams


### Zero flakiness


Deterministic inbox + code extraction means your 2FA tests are reliable every time.


### Easy parallelization


Every test gets its own inbox—no conflicts, no race conditions.


### Security coverage


Test expired codes, resends, lockouts, and edge cases with confidence.


### Compliance-friendly


Reliable audit trail of login flows for security reviews.


### Best practices by default


Idempotence, no dependencies, parallel-safe tests—automatically applied by Stably's Test Creation Agent.


Your authentication flow stays stable even when your product moves fast.


## Get Started Today


The no-code editor is available now. The SDK support is coming soon.


Ready to make 2FA testing painless? Try Stably and see how we've solved one of testing's most notorious pain points.
