---
schema_version: "1.0.0"
document_id: "86a901feeb94190063ac524ba5e6fb33e34499220431d785446983bde7ba2272"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/canary-transfers"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-21T23:41:43.316828+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:d5fb9d43a5a5ba0e2f835b296db75858101bed6baedf10914968be2a7ba4b133"
---

# Canary transfers: testing banking infrastructure in production — Increase

There’s a saying in software:[reality has a surprising amount of detail](https://johnsalvatier.org/blog/2017/reality-has-a-surprising-amount-of-detail) .


The Federal Reserve documents exactly when wire transfers are supposed to settle. What the manual *won’t* tell you is that during a major Amazon Web Services outage, the Fed might stay open an hour late to give banks more time to finish their daily settlements. We noticed because our own wires settled faster than expected. That’s the kind of detail you only discover by exercising the banking system in production, every minute of every day.


At Increase, we connect directly to the Federal Reserve and Visa, which means their surprises are ours to catch. Prevention handles the failures you anticipate. For everything else, the goal is to shrink the time between a failure happening and the moment you notice it. Our solution is almost embarrassingly literal: we send ourselves money, all day, every day, on every network we operate, and get paged when it doesn’t arrive on schedule. We call these payments *canary transfers* .


## The late file


In the early days of Increase, an uncaught head-of-line blocking serialization bug caused us to submit a file of transfers to the Fed late, and we missed a same-day ACH settlement cutoff. The transfers landed hours behind schedule. For a customer running payroll, that’s serious: the money has to arrive on time or people can’t pay their mortgages. Worse, it took hours to notice. Had we caught the delay immediately, we could have re-sent the transfers over wires; by the time we saw the problem, that option was gone.


The postmortem fixed that bug, but the next incident will be something else. Whatever it is, we’d rather find out in five minutes than five hours.


## Sending ourselves money all day


Engineers already have a tool for this shape of problem: synthetic monitoring. In a typical web stack, you run a script that continuously hits your endpoints so you find out something is broken before your users do. A canary transfer is the same idea, except the request is money and the endpoint is the Federal Reserve.


Our first version was under fifty lines of code: send an ACH transfer between two accounts we own, then poll the receiving account until the balance moves. We built it on top of the same public API our customers use, so anything customer-facing in the stack gets exercised on every loop.


ACH is ideal for canaries because its timing is[predictable](https://www.frbservices.org/resources/resource-centers/same-day-ach/fedach-processing-schedule.html) . A transfer we hand to the Fed at 10:00 AM Eastern Time makes the 10:30 AM same-day window and settles at 1:00 PM, so we place a canary in every outbound FedACH file and page if a canary transfer in that file hasn’t settled by 1:05 PM. We time the canaries so one settles during every ACH window. That signal turned out to be useful enough that we now publish it on our[public status page](https://status.increase.com/) .


We run a version of this on every rail. Wires settle almost instantly, so we send ourselves one every hour, and we do the same over Real-Time Payments and FedNow. Cards get the full loop: every minute, we issue a new card on our infrastructure and authorize a live transaction. And to cover the physical world, we mail ourselves at least one paper check every day, which tests printing, the mail, and lockbox deposit end to end. Somewhere in a USPS truck right now is an envelope we addressed to ourselves, running an integration test at the speed of mail.


## What a canary can and can’t tell you


A canary tells you the air is bad, not which gas is leaking or where it’s leaking from. If a file gets stuck, the round trip fails and we get paged, but the canary won’t say whether the cause was a bad deploy, a Fed issue, or something we haven’t imagined yet. To find the root cause, we rely on a separate invariant-monitoring framework called[Checker](https://increase.com/articles/automated-invariant-monitoring) , which continuously asserts specific conditions about the state of our systems. A canary tells us a file is stuck; a failing check tells us which step it’s stuck on.


Sometimes the answer is that nothing is wrong on our end at all. We occasionally get paged because a payment network itself is running slow. That’s fine! Those pages still shrink the time between something changing and someone starting to look.


And those false alarms carry real information. Because we authorize a card transaction every minute, we know within a minute if there are any issues with the Visa network. Because we mail ourselves a check every day, we know when the USPS is having a slow week. So when a customer asks why the check they mailed a week ago still hasn’t been deposited, we can answer from our own data: the mail really is slow, and we know because we’re feeling the same conditions our customers feel.


## Boring is reliable


If you build infrastructure that touches the physical world, reality’s surprising details will eventually break your models. Unlike monitoring that watches from the outside, canaries participate: our money rides the same rails, meets the same cutoffs, and sits in the same mail trucks as our customers’ money.


Sending yourself money all day will never be glamorous. It’s still the most dependable way we’ve found to keep learning the details the manual leaves out.
