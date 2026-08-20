---
schema_version: "1.0.0"
document_id: "531a4ff9fc6ebde5892e5d78bd35fcc86bb2e041b9cdc223cf87f3f9662f166e"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/what-are-bounced-emails"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:b7dd64a4c339780e82f2286512e63152a0d0e4c0ef4cb7a1b662824b7b5c6305"
---

# What Are Bounced Emails?

A bounced email is a message the receiving mail server refuses to accept and returns to the sender instead of delivering. The server sends back a bounce notification explaining why. Bounces come in two flavors: hard bounces, which are permanent failures, and soft bounces, which are temporary problems. Watching and acting on both matters, because a rising bounce rate drags down your sender reputation and the deliverability of every future send.


## What is the difference between a hard bounce and a soft bounce?


The split comes down to whether the failure is permanent or temporary, and it changes what you should do next.


A **hard bounce** is a permanent rejection. The address does not exist, the domain is wrong, or the server has flatly refused the mail. Retrying will not help, and you should stop sending to that address immediately.


A **soft bounce** is a temporary failure. The mailbox is full, the server is briefly unavailable, or the message is too large for the moment. The address is probably fine; the conditions just are not. A sender can retry a soft bounce later and often succeed.


Hard bounce Soft bounce


Nature Permanent Temporary


Typical causes Invalid address, wrong domain, blocked sender Full mailbox, server down, message too large


Right response Suppress the address, stop sending Retry later, suppress only if it persists


Reputation impact High if repeated Lower, but watch for patterns


One nuance worth holding onto: a soft bounce that keeps repeating starts to behave like a hard bounce. A mailbox that has been full for weeks is, for practical purposes, dead. Most platforms convert a persistent soft bounce into a suppression after a set number of attempts.


## What causes a bounce?


Bounces trace back to a handful of recurring reasons.


- **Invalid address.** The mailbox does not exist, often from a typo, an abandoned account, or a fabricated signup. This is the most common hard bounce.
- **Full mailbox.** The recipient is over quota. Usually a soft bounce, since the box may free up.
- **Blocked.** The receiving server or a security gateway refused your mail, sometimes because your IP or domain is on a blocklist, sometimes because the content tripped a filter.
- **Policy rejection.** The recipient's server enforces a rule you did not meet: missing or failing authentication, an unaccepted attachment type, or a sending pattern that looks abusive.


If you are seeing a lot of policy and block rejections, the cause is usually upstream. Authentication gaps and content that reads as spam are the usual suspects, and[how to reduce spam score](https://bird.com/en-us/blog/how-to-reduce-spam-score) walks through tightening both.


## Why do bounces hurt sender reputation?


Mailbox providers read your bounce rate as a proxy for how well you manage your list. A clean list rarely bounces. A list full of invalid addresses bounces constantly, and that is exactly the signature of a spammer who scraped or bought addresses rather than earning them. So when your bounce rate climbs, providers lower their trust in your domain and IP, and more of your legitimate mail lands in spam or gets rejected outright.


The damage compounds. Hard bounces to addresses that never existed are the worst offenders, because hitting nonexistent mailboxes (including spam traps built from dead addresses) is a strong negative signal. Keeping bounce rate low is one of the foundations of[email deliverability best practices](https://bird.com/en-us/blog/email-deliverability-best-practices) .


## How should you handle bounced emails?


The goal is to stop sending to addresses that bounce, and to stop bad addresses from getting on your list in the first place.


**Honor suppression lists.** When an address hard bounces, it should go on a suppression list so you never mail it again. Bird maintains suppressions automatically and lets you inspect and manage them; the[suppressions guide](https://bird.com/en-us/docs/guides/email/suppressions) covers how that works, and the[suppressions product page](https://bird.com/en-us/products/email/suppressions) shows the bigger picture.


**Practice list hygiene.** Periodically remove addresses that have bounced or gone cold. A smaller, engaged list almost always outperforms a large, stale one on both deliverability and bounce rate.


**Validate before you send.** Catching invalid addresses at signup or before a campaign prevents hard bounces entirely. An[email validation API](https://bird.com/en-us/blog/what-is-an-email-validation-api) checks whether an address is well-formed and likely deliverable, and[Bird's recipient validation](https://bird.com/en-us/products/email/recipient-validation) plugs that check into your signup and send flows.


**Read your events.** Every bounce is delivered to you as an event with a reason code, so you can tell a hard bounce from a soft one and react accordingly. The[events guide](https://bird.com/en-us/docs/guides/email/events) documents the event types and their fields.


A related signal is the deferral, a temporary 4xx response that is not yet a bounce. If you are seeing messages held rather than rejected,[what are deferred emails](https://bird.com/en-us/blog/what-are-deferred-emails) explains the difference and what to do.


## FAQ


### Is a bounce the same as a deferral?


No. A bounce is a final rejection: the server will not take the message. A deferral is a temporary 4xx response asking you to retry later. A deferral can eventually turn into a bounce if retries keep failing, but on its own it is not one.


### What bounce rate is acceptable?


There is no universal threshold, and any specific number depends on your audience and mailbox providers. The practical rule is to keep it as low as you reasonably can and to investigate any sudden spike, since a jump usually points to a list-quality or authentication problem.


### Should I retry a hard bounce?


No. A hard bounce is permanent, so retrying wastes effort and risks further reputation harm. Suppress the address and move on. Retries are appropriate only for soft bounces.


### How do I stop bad addresses from bouncing in the first place?


Validate addresses at the point of collection and use confirmed opt-in so you only send to people who genuinely subscribed. That keeps invalid and mistyped addresses off your list before they ever bounce.


## Where to go next


Bounces are normal in small numbers and dangerous in large ones. Suppress hard bounces, retry soft ones, validate early, and keep your list clean, and your bounce rate stays where mailbox providers want it. Start with the[suppressions guide](https://bird.com/en-us/docs/guides/email/suppressions) and the[events guide](https://bird.com/en-us/docs/guides/email/events) to see how Bird surfaces and acts on bounce data.
