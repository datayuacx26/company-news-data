---
schema_version: "1.0.0"
document_id: "383fa6e941691dc6ad33e5a64043cb3b6063d90e4c481a2dc4e1010af4484d9e"
company_key: "yc-onesignal"
company: "OneSignal"
source_id: "yc-onesignal-rss-a77922638bdd"
canonical_url: "https://onesignal.com/blog/the-sms-to-rcs-shift/"
published_at: "2026-06-26T19:47:29+00:00"
first_seen_at: "2026-07-20T23:24:05.566771+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:54802c0771a708a9df0fed77f0bce0b09805a452500915938bd2a21974745d0f"
---

# Now playing: The SMS-to-RCS shift (and how a fintech brand took advantage)

Andrea Martin has over 1,300 unread text messages on her phone. She's not unusual, a lot of people's inboxes are more densly populated than you'd think. And as a senior product manager at Ualett, a fintech servicing gig economy workers, she knew her company's messages were probably buried in that same pile.


> "If I don't know who it's coming from, I'm not reading it," she said during our recent Back to the Inbox webinar. "If there's an unrecognized number, I'm definitely not likely to respond."


That's the core problem facing every marketer and product team sending SMS in 2026: deliverability is no longer the hard part. Trust is. And building trust through a channel defined by anonymous short codes and toll-free numbers is an uphill battle. No place is this a steeper climb than in industries like[fintech](https://onesignal.com/blog/building-trust-and-driving-revenue-growth-with-fintech-messaging/) , where an unbranded message promoting a fraud alert or cash advance reminder looks almost identical to an actual scam.


Ualett’s answer was[RCS (rich communication services)](https://onesignal.com/rcs) . **And their first campaign produced a 40% open rate** .


Here's what they did, what they learned, and what marketers should take away before[building their own RCS program](https://onesignal.com/sms-vs-rcs-messaging) .


### Why Ualett chose RCS: Branding as a trust signal


Ualett’s business runs on trust (their tagline is literally "Built on Trust.") so when their marketing team flagged that RCS would let them show[a logo, a verified brand name, and a recognizable sender profile in every message](https://onesignal.com/blog/rcs-business-profiles-explained-how-your-brand-appears-to-android-ios-users/) , it wasn't a tough sell internally.


> "As soon as you see a message from Uallet hit your inbox, you know who that message is from," Andrea explained. "You can identify the sender immediately."


For a fintech sending OTP codes, fraud alerts, remittance reminders, and promotional campaigns, this distinction matters enormously.


A fraud alert from a verified, branded sender looks like help. The same message from a 5-digit short code looks like a threat.


This is what we mean when we say **trust is the new deliverability** . Historically, messaging teams optimized for reach and volume. Now, the more abstract bottleneck is whether the person on the other end believes the message is worth opening. Branded RCS senders address that problem directly.


### Where they started: Marketing first, then transactional


If you're evaluating RCS and wondering where to pilot it, Ualett’s path is instructive. They didn't try to migrate everything at once.


They started with marketing campaigns — the most visible, highest-risk use case for trust issues, since promotional messages from unknown numbers are the most likely to get ignored or flagged as spam. The rich media capabilities of RCS (images, multiple CTAs, card layouts) also made the value proposition obvious on the marketing side.


From there, they expanded into transactional communications. The logic was simple: if they were going to brand the marketing messages, they should brand the transactional ones too. A customer who receives a beautiful, branded campaign message shouldn't then get an unbranded text about their account activity from a random number.


**The takeaway for marketers:** Don't wait to have a comprehensive RCS strategy before starting. Pick one use case and build your playbook from there. Transactional expansion follows naturally.


### Building the campaign: Multiple CTAs change everything


One of the biggest functional differences between SMS and RCS is **the ability to include multiple calls-to-action in a single message** .


Wallet identified 20 to 30 existing SMS messages they could upgrade to RCS cards or rich media messages. The exercise forced them to think differently about what a message could accomplish.


Their first campaign targeted a specific segment: clients who needed to get their accounts back into good standing. The message included three CTAs: one to[deep-link into the app](https://onesignal.com/blog/deep-linking-best-practices/) , one to a landing page with more information, and one to call the servicing team directly.


What used to require a live agent interaction (or worse, a customer dropping off entirely) became a self-service decision point. The customer could read the message, understand their options, and take action without friction.


**Practical note on CTAs and platform differences:** Android renders multiple CTA buttons natively within the message. On iOS (which now supports RCS since Apple adopted the standard in 2024), multiple CTAs are consolidated into an "Options" button.


It's a slightly different experience, but still far more actionable than a plain SMS. Test both with OneSignal's built-in iOS/Android previewer before sending.


### The Results: A 40% open rate and more weekend activity than ever


Ualett launched their first major RCS campaign on a Friday, targeting gig workers (drivers and delivery workers who are most active on weekends.) The message offered help getting accounts back in good standing, with three clear CTAs.


The results:


- **~40% open rate** — substantially higher than comparable SMS campaigns
- More weekend app activity than they'd ever seen
- A measurable spike in customers taking the specific actions the message asked for (adding debit cards, completing remittances, etc.)


Was it the branded sender? The image? The multiple CTAs? Probably all three. But Andrea's instinct is that the brand recognition was a significant driver. Because when you trust who the message is from, you're more likely to act on what it says.


### The approval process: What to expect and how to prepare


The[RCS approval process](https://onesignal.com/blog/how-to-get-approved-for-rcs-6-tips-to-streamline-the-rcs-approval-process/) is one of the most common barriers cited by brands considering the channel, and it's worth being honest about: it takes time, and it requires work. But it's getting faster, and the biggest delays are in your control.


Here's what Ualett had to navigate:


**Updating terms of service and privacy policy.** RCS compliance requires that your terms and privacy policy explicitly state that customers may receive RCS communications. This sounds simple, but for Ualett it meant engaging engineering and legal teams to push updates through the right channels.


**Collecting brand assets.** You'll need a logo, a banner image, a brand color, your website URL, a support phone number, and a contact email. Have these ready before you start.


**Carrier approval.** Your RCS sender has to be approved by Google and downstream carriers. OneSignal manages this process and works with you to ensure compliance before submitting, which matters because submitting a non-compliant application means going to the back of the queue.


**Realistic timeline:** Once you're fully compliant and ready to submit, approval is trending toward 2-4 weeks. The longer lead time most brands experience is almost entirely pre-submission, due to factors like getting the right teams aligned, updating policies, and assembling brand assets. If you start that work now, you're already ahead.


**One practical tip:** Start with your compliance review. Don't wait until you've designed your first campaign to discover your privacy policy needs to be updated.


### Cost planning: How Ualett thinks about RCS ROI


RCS messages cost more than SMS, and the pricing varies based on message type (standard rich vs. rich media). Ualett approaches this with a formal forecasting model, and their framework is worth borrowing:


**Map your existing SMS messages first.** Identify which campaigns are candidates for RCS. Estimate volume, then calculate what your budget shift looks like if you move those messages from SMS to RCS.


**Think in terms of LTV impact, not just message cost.** The right question isn't "does RCS cost more than SMS?" It's "does RCS generate enough incremental action to justify the delta?" Ualett’s weekend campaign already gave them early evidence that it does.


**Shift budget, don't add it.** Wallet redistributed budget from SMS and in-app notifications toward RCS rather than treating it as a net-new expense.


**Use the cost guide in the editor.** OneSignal's RCS editor shows you in real time whether your message qualifies as a standard rich message or rich media, and the cost difference between them. Ualett actively adjusts message content to manage which tier they land in.


### The early mover advantage (and why it won't last)


When Andrea looks at her own inbox, she sees maybe five brands sending branded RCS messages (Crate & Barrel being one of them.) Most other brands are still sending from five-digit short codes.


That's the opportunity right now. When every other message in your customer's inbox looks like spam, a branded, visually rich message from a verified sender stands out. And as adoption increases, these branded messages will become the baseline. But for teams moving now, the signal-to-noise ratio works in your favor.


> "I think that eventually it'll take over SMS," Andrea said. "Most brands will move this way. And the ones that are still messaging with an unbranded number could just get marked as spam."


Apple's adoption of RCS was a turning point. In a market where more than half of smartphone users have iPhones, a channel that didn't reach iOS devices was never going to replace SMS. It does now.


### Ready to build your RCS program?


Whether you're[evaluating RCS for the first time](https://onesignal.com/blog/meet-the-rcs-community-figma-kit-everything-you-need-to-start-designing-rcs-messages/) or ready to start the approval process, we're here to help. OneSignal's team works with you through compliance, approval, and your first campaign, so you don't go to the back of the queue with a non-compliant submission.


[Get in touch](https://onesignal.com/rcs#contact) to learn more about RCS at OneSignal, and let's get your text strategy working at its fullest potential.
