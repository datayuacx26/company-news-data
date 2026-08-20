---
schema_version: "1.0.0"
document_id: "945993a7f9ff006bca9bc1b98191f78f85cae046de7e543a8d2d9bba6946864f"
company_key: "yc-fortuna-health"
company: "Fortuna Health"
source_id: "yc-fortuna-health-news-import-89ab9c75f976"
canonical_url: "https://www.fortunahealth.com/resources/what-consumers-taught-us-in-our-first-two-years-of-building"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:50.497405+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:ee3d4f57a7f439c3265748e3b6211c42e0596db72eec4280e89213dcf6334515"
---

# What Consumers Taught Us in Our First Two Years of Building

When we started Fortuna, we had a clear hypothesis: Medicaid navigation is broken, and a better consumer product would fix it. We were right about the first part. The second turned out to be more complicated.


Two years in, supporting 150,000 enrollments and renewals each month, we've accumulated a set of hard lessons - not from reports, but from testing, shipping, getting it wrong, and improving.


### 1. Nobody thinks about their health coverage — until they have to.


Consumer health loves to imagine an engaged user: someone who logs in, checks their status, stays on top of actions, and is proactive.


But health coverage is not top of mind for anyone. People primarily engage when they have a health need, are facing a deadline (e.g., renewal), or are entering a new life situation (e.g., having first child).


In a pilot with one of the largest provider networks in New York, serving over 1 million patients a year, we proactively reached out to educate members about Medicaid renewals. Click-through was abysmal. The lesson wasn't that members don't care. **It was that we were reaching them in the wrong window.**


TurboTax figured this out decades ago. A third of the entire tax prep industry's annual activity compresses into the two weeks before the April deadline — and **virtually nothing happens outside of tax season.** TurboTax doesn't try to make you think about taxes in August. It makes sure that when April comes, you know exactly where to go.


That reframe changed how we think about engagement. Proactive outreach and detailed dashboards don't drive outcomes on their own. Timing does. We learned to reach people when they need us in the right action window.


### 2. Medicaid data is a snapshot, but used as the source of truth.


The[834](https://x12.org/examples/005010x220) is the EDI standard states use to exchange Medicaid enrollment data with managed care plans and CMS. 834 files include core fields like name, Medicaid type, address, and relevant dates - as well as hundreds of other fields. In many states, it's become the de facto source of truth for member outreach.


The problem is that it wasn't built to work that way.


- **Every state implements the 834 differently.** Some fields are required, some situational, some ignored entirely. Coverage dates are sometimes absent. Some states have built supplemental files to patch the gaps - arriving on different schedules, mid-month.
- **The data is often stale.** New York sends daily files, but other states send monthly files. Many states have implicit 30–45 day data lags, due in part to processing at county and local social services offices. A member's renewal date may have changed. Their household may have already moved to a new plan.


The practical result: we had to build member engagement on top of a snapshot.


As Fortuna expanded across 14+ states, we learned to configure our product around understanding what each state's 834 actually represents - not what we wish it represented.


### 3. Bundling the ask changes completion rates.


When we first reached out to members about renewal, we kept it narrow: complete your renewal. That's the task.


But Medicaid members have other needs that surface in the same moment. Across Fortuna and our health plan partners, 35% of inbound member requests are about finding a doctor or getting an ID card resent. Those aren't distractions from renewal - they're often the reason someone engages at all.


In North Carolina, where we work with health plan partners on member retention, we found that connecting renewal outreach to practical, immediately useful asks - like helping a member confirm their PCP - drove meaningfully higher same-session completion. In Pennsylvania, where members can complete their renewal online through COMPASS but many still receive paper packets, pairing the renewal nudge with SNAP support gave members additional value.


The product lesson for Fortuna: pair renewal outreach with an adjacent ask that's already on the member's mind.


### 4. Clarity beats volume every time.


More outreach does not produce more engagement. We tested this repeatedly: members who received multiple messages about the same action were no more likely to complete it. They were more likely to be confused.


Clarity starts with knowing whether a member needs to act at all. A meaningful share renew automatically through ex parte - the state pulls wage and tax records, confirms eligibility, and the member never has to touch it. In Massachusetts, that accounts for a large share of annual renewals. In West Virginia, less than 20% are renewed ex parte, meaning the vast majority take manual renewal steps. A "your renewal is due" message sent to someone the state auto-renewed isn't just wasted - it trains them to ignore the next outreach message.


Clarity also means using the right, colloquial name. Members often don't recognize "Medicaid" or "Medical Assistance" as their coverage, but they know their plan. Aetna. CareSource. In Connecticut, people know "Husky."


And clarity is almost impossible when the member is already drowning. The state is sending notices. The plan is sending notices. The provider is reaching out about a care gap. To the member, it all arrives as an undifferentiated pile of mail and texts from entities they only partially recognize.


The instinct to send do more calls, emails, texts, and mailers is understandable. But blanketing members with messages - especially with the wrong name and wrong ask - often fails. It also erodes the trust you need for when the message actually matters.


When Fortuna works with partners, we focus on getting the segmentation right and stripping back the volume, driving improved completion and conversion rates.


### What two years actually builds.


These lessons didn't come from the product Fortuna launched on day one. These lessons came in iterations, watching where members dropped off and where they got confused.


The consumers we build for aren't thinking about 834 files or ex parte rates. They're thinking about whether their kid's doctor is still covered. Steve Jobs said simple is harder than complex and we agree. Simplicity isn't merely a design principle, it's a form of respect to your consumer. We’re still learning, but working hard to get there.


‍
