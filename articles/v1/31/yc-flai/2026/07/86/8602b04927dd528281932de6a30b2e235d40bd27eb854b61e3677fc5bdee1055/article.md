---
schema_version: "1.0.0"
document_id: "8602b04927dd528281932de6a30b2e235d40bd27eb854b61e3677fc5bdee1055"
company_key: "yc-flai"
company: "Flai"
source_id: "yc-flai-news-import-6bbede6307a5"
canonical_url: "https://www.useflai.com/blog/inbound-vs-outbound-bdc-for-car-dealerships"
published_at: "2026-07-06T14:47:16.978+00:00"
first_seen_at: "2026-07-21T20:24:10.813388+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:0215c06b15eeaeee0fc57f002d10ea14c466f3ae8149c58e59295f0c18e049ca"
---

# Inbound vs Outbound BDC for Car Dealerships (2026)

Your dealership BDC is either making you money or quietly bleeding it. The difference usually comes down to one question: are you capturing demand when it shows up, or are you chasing it after it’s already gone cold?


That’s the real split between inbound and outbound BDC. Not a dictionary definition. Not an org chart exercise. It’s a question about where your biggest leak is, and which one you should plug first.


We built[Flai](https://www.useflai.com/) specifically to help dealerships close both of these gaps, so we’ve seen the data up close. This guide breaks down the staffing math, the KPIs that actually matter, the compliance landmines, and a practical framework for deciding what to fix first at your store.


## What Is a Dealership BDC (and Why Most Get It Wrong)


A[BDC (Business Development Center)](https://www.useflai.com/blog/what-is-a-bdc-in-a-car-dealership) is not “the phone department.” It’s not a room of people answering calls and transferring them. A dealership BDC is a conversion system. Its job is to take customer intent (calls, texts, emails, forms, chats) and turn it into kept appointments for sales and service.


That distinction matters because it changes how you measure success. If you think of BDC as “answering phones,” you measure call volume. If you think of it as a conversion system, you measure appointments booked and kept. The second framing is the one that makes money.


A simple way to think about the pipeline:


1. **Demand arrives** (calls, forms, texts, chats)
2. **Demand connects** to a competent handler (human or system)
3. **Handler identifies intent** (sales vs service, urgency, recall)
4. **Handler makes an offer** (appointment times, next step, transfer)
5. **Customer commits** (appointment booked)
6. **Customer shows** (kept appointment)
7. **Store delivers** (RO completed, vehicle sold, CSI earned)


Every step where a customer drops off is money you’ve already spent to attract but failed to convert. The BDC’s job is to minimize those drops.


### How to Calculate BDC Profit at Your Dealership


For any department (sales or service), your incremental profit from BDC can be approximated with this formula:


**Profit = Demand x Connection Rate x Bookable% x Booking% x Kept% x Value per Kept Appointment**


Where:


- **Demand** = inbound calls/leads + outbound contacts you create
- **Connection Rate** = answered/connected percentage
- **Bookable%** = percent of connected interactions that should become an appointment
- **Booking%** = percent of bookable interactions that actually get booked
- **Kept%** = show rate
- **Value** = gross profit per kept appointment


This equation is your diagnostic tool. It tells you exactly where inbound and outbound BDC each live:


- **Inbound** mainly drives Connection Rate, Bookable%, and Booking%.
- **Outbound** mainly drives Demand (reactivation), Connection (callbacks), Kept% (confirmations and reminders), and Booking%.


**A quick worked example:**


Take 1,000 inbound service calls per month with a 65% connection rate, 30% bookable, 70% booking success, 80% show rate, and $250 profit per kept RO:


Profit = 1,000 x 0.65 x 0.30 x 0.70 x 0.80 x $250 = **$27,300/month**


Now improve just two things: connection from 65% to 85%, and booking success from 70% to 80%:


Profit = 1,000 x 0.85 x 0.30 x 0.80 x 0.80 x $250 = **$40,800/month**


That’s **$13,500 per month from two “boring” operational improvements.** No new marketing spend. No new inventory. Just catching more of the demand you already have.


BDC is real operational leverage. Small improvements compound. The[BDC metrics that actually drive this equation](https://www.useflai.com/blog/bdc-metrics-every-dealership-should-track) are worth understanding at every step.


## How Inbound BDC Works at a Car Dealership


Inbound BDC is any workflow where the customer starts the interaction: phone calls to the main number, chat messages, website forms, texts, email inquiries, and OEM leads.


The inbound job comes down to two things, and you need to do them fast:


1. **Stop the customer from leaving.** Answer quickly, avoid hold, avoid voicemail.
2. **Create the next committed step.** Book an appointment or warm-transfer to the right person.


That’s it. Everything else is a detail of those two objectives.


### Why Inbound BDC Is Usually the Biggest Revenue Leak


Dealership phone performance is fragile because callers are impatient and have alternatives. Car Wars reported that average hold time in 2024 was 3 minutes and 5 seconds. *Three minutes doesn’t sound long until you realize the customer has a competitor’s number one tap away.*


Analysis of Car Wars 2024 phone data paints a sharper picture: roughly **31.8% of unconnected calls** were customers hanging up while on hold, about **32.3% of callers left voicemails** , and the average connection rate across dealerships sat at around **65.2%** .


That means roughly one in three callers who can’t reach you immediately is just gone. They’re calling the next dealership. The[revenue impact of those missed calls](https://www.useflai.com/blog/missed-call-statistics-revenue-lost-at-your-dealership) is more significant than most dealers realize. In some stores, it adds up to hundreds of thousands of dollars per year.


### Why Inbound BDC Fails: The Call Volume Spike Problem


The reason inbound fails isn’t that dealerships don’t care about phones. It’s that call volume doesn’t arrive evenly. Calls come in spikes, and if your arrival rate exceeds handling capacity during a spike, you create hold time. Hold time drives abandonment. Abandonment is lost revenue.


Dealership call data consistently shows predictable peak windows, with[10 AM to 12 PM being the highest inbound volume window](https://www.useflai.com/blog/when-do-dealerships-get-the-most-calls) . If your staffing is “even” across the day, your missed calls will be “uneven,” stacked up exactly when it counts most.


### How to Calculate Inbound BDC Staffing at Your Dealership


You don’t need expensive workforce management software to stop guessing. Start with this:


**Step 1:** Pick your peak window. Example: 10 AM to 12 PM.


**Step 2:** Measure calls offered in that window. Example: 100 inbound calls in 2 hours.


**Step 3:** Measure Average Handle Time (AHT). AHT includes talk time plus holds plus after-call work. Car Wars reported a 4-minute-32-second average service call duration in 2024. Use your own number if you have it. Let’s use 4.5 minutes.


**Step 4:** Compute workload minutes.


Workload = Calls x AHT = 100 x 4.5 = **450 minutes of work**


**Step 5:** Compute agents needed. If the window is 120 minutes and you want 80% occupancy (leaves breathing room for spikes):


Agents = Workload / (Window x Occupancy) = 450 / (120 x 0.8) = 4.69, so **5 agents**


This is why “we have 2 people answering phones” can feel fine at 2 PM and catastrophic at 10:30 AM. When peaks overwhelm your team,[call overflow solutions](https://www.useflai.com/blog/call-overflow-solutions-for-dealerships) become the difference between captured revenue and lost appointments.


### Inbound BDC for Sales vs Service: Key Differences


Sales inbound tends to be high-intent but high-variance. Callers ask about specific VIN availability, pricing and payments, trade-in values, financing pre-qualification, and test drive scheduling. There’s more comparison shopping happening, so speed to connect matters enormously.


Service inbound is more repetitive and schedule-driven, but margin-dense. Customers want to schedule maintenance or repair, ask about recalls, find out “can you take me today?”, check on status, or get pricing for common jobs. These calls have clearer next steps if your agent can access the scheduler and confirm details. Car Wars data showed their customers set over **4.1 million service appointments in 2024** , with the average call lasting about 4 minutes 32 seconds. That 4:32 anchors your staffing math directly.


### Inbound BDC KPIs That Actually Drive Revenue


*If you only track one number, track kept appointments created per inbound demand, split by sales vs service.*


To improve that number, you need a diagnostic layer. Use this funnel:


**Access metrics** (can customers reach you?):


- Calls offered
- Answer rate / connection rate
- Average speed to answer
- Hold time distribution (not just average)
- Abandonment rate
- Voicemail rate


**Conversion metrics** (do answers turn into appointments?):


- Bookable rate (percent of calls that should become an appointment)
- Booking success rate (appointments booked / bookable calls)
- Transfer success rate (warm transfers / attempted transfers)
- Appointment set rate (appointments / calls answered)


**Outcome metrics** (do appointments happen?):


- Kept rate
- RO count or sold count from BDC
- Gross profit per kept appointment


[Flai’s BDC metrics guide](https://www.useflai.com/blog/bdc-metrics-every-dealership-should-track) organizes this same idea as a clean funnel from inbound demand through kept appointments, including benchmarks for what good looks like at each stage.


### How to Improve Inbound BDC Performance at Your Dealership


**Treat speed to answer like a revenue metric.** If customers are waiting 3 minutes, you’ve already lost many of them.[The true cost of hold times](https://www.useflai.com/blog/true-cost-of-dealership-hold-times) is well-documented: hold is not neutral. It’s active damage.


**Kill cold transfers.** Every cold transfer is a chance to create voicemail or abandonment. Warm transfers (introduce the caller, confirm the right person is available) reduce handoff friction and keep callers in the funnel longer.


**Make booking the default action.** Inbound scripts should always lead to “Let’s get you scheduled” for service or “Let’s set a time to drive it” for sales. Not “Can I take a message?” Understanding[why dealership callers hang up](https://www.useflai.com/blog/why-dealership-callers-hang-up-and-how-to-stop-it) is the first step to preventing abandonment.


**Remove the “let me ask my manager” hold pattern.** If agents must put customers on hold to answer basic questions, you have a system design issue: missing knowledge base, missing pricing guardrails, missing inventory context, or missing scheduler access. And when customers can’t reach anyone at all,[voicemail becomes a silent revenue killer](https://www.useflai.com/blog/why-dealership-voicemails-kill-sales-and-what-to-do) that compounds daily.


## How Outbound BDC Works and Why Dealerships Need It


Outbound BDC is any workflow where the dealership initiates contact: calling back missed calls, following up on internet leads, sending appointment confirmations and reminders, recovering no-shows, running service reactivation campaigns, managing recall outreach, equity mining and lease maturity contacts, and post-visit check-ins.


Outbound is *not* “spam people until they buy.” It’s structured follow-up that reduces friction and increases kept appointments. And it exists to do three things:


1. **Recover what you already paid for** (missed calls, paid leads, unsold traffic)
2. **Pull forward demand** (reactivation, reminders)
3. **Reduce no-shows** (confirmations, reschedules)


### Why Outbound BDC Should Focus on Contact Economics, Not Activity


Most dealerships design outbound like this: “Make 100 calls today. Send these templates. Log your activity.” That’s an activity trap. It measures effort, not results.


Outbound should be designed around contact economics. Walk through the math:


- If your contact rate is 10%, then 100 dials = 10 conversations
- If your booking rate per conversation is 20%, that’s 2 appointments
- If your show rate is 60%, that’s 1.2 kept appointments


So the real questions aren’t “how many calls did you make?” They’re: How do we increase contact rate without annoying customers? How do we increase booking per contact? How do we increase show rate?


### Outbound BDC Campaigns That Actually Work for Dealerships


**Must-do campaigns** (unsexy, high ROI):


- **Missed-call callback.** If you miss an inbound call, your best shot is a fast callback with context. The customer was already in action mode.
- **Internet lead follow-up.** Speed matters, but so does persistence and relevance.[AI-powered follow-up](https://www.useflai.com/blog/how-ai-follow-up-is-helping-dealerships-close-more-sales-leads) has changed what “fast” and “persistent” mean. First dealership to respond wins a disproportionate share of deals.
- **Appointment confirmations and reminders.** Outbound becomes a show-rate machine when done right.[Increasing service appointment show rates](https://www.useflai.com/blog/how-to-increase-service-appointments-at-dealerships) starts with a structured confirmation cadence.
- **No-show recovery.** Most “no-shows” aren’t really no-shows. They’re “something came up.” Give rescheduling options immediately.


**Service revenue campaigns** (often bigger ROI than sales):


- **Service reactivation.** Customers who haven’t been back in X months.
- **Declined services follow-up.** High intent, already diagnosed.
- **Maintenance reminders.** Only works if timing and personalization are real.
- **Recall campaigns.** Recalls are operationally simple but behaviorally hard. You’re not fighting logistics. You’re fighting friction.[Flai’s recall campaign guide](https://www.useflai.com/blog/dealership-recall-campaigns-best-practices-ai) puts it plainly: completion rates stay low because getting humans to take action requires removing friction, not just sending notices.


### Outbound BDC Cadences You Can Start Using Today


These aren’t magic numbers. They’re structured defaults you can tune for your store.


**Cadence A: Missed Inbound Call (service or sales)**


Goal: reconnect while intent is still hot.


- Attempt 1: Call back within 5-15 minutes
- Attempt 2: 1-2 hours later
- Attempt 3: Next morning
- SMS after attempt 1 (if consented): “Sorry we missed you. Want to grab an appointment time?”
- Stop after 3-4 attempts unless the customer re-engages


**Cadence B: New Internet Lead (sales)**


Goal: book a visit, not “check in.”


-> Day 0: Immediate call + SMS + email (if permitted)


-> Day 1-3: 1 touch per day, alternating channels


-> Day 4-10: 2-3 touches total


-> Day 11-30: Weekly “value” touch (new price, similar inventory, trade estimate)


The key insight: “value” means information that reduces friction for the customer, not a generic “just checking in.”


**Cadence C: Service Reactivation**


Goal: make it easy to say yes without sounding like a telemarketer.


1. SMS (personalized with vehicle, last visit, simple offer)
2. Call
3. SMS with 2 specific appointment options
4. Polite “breakup” message (opt-out friendly)


### Outbound BDC KPIs That Actually Drive Results


**Contact and deliverability:** Contact rate, right-party contact rate, voicemail left rate, SMS deliverability and reply rate, spam label rate.


**Conversion:** Appointment set rate per contact, appointment kept rate, gross profit per kept appointment, cost per kept appointment.


**Compliance health:** Opt-out rate (STOP), complaint rate, Do Not Call hits, consent capture rate (percent of records with valid consent).


## Should You Fix Inbound or Outbound BDC First?


Most dealerships should fix inbound before scaling outbound. The logic is straightforward: if you’re missing calls today, outbound is just damage control for the mess inbound is creating. Miss calls today, and you create “callback debt” tomorrow.


### How to Diagnose Your Inbound vs Outbound BDC Problem


**Step 1:** Pull the last 2 weeks of call data. Ask yourself:


- What’s our connection rate?
- What’s our average hold time?
- When do we peak?
- How many calls hit voicemail?


If your hold time is anywhere near the **3-minute-5-second average** Car Wars reported for 2024, inbound is your priority. You can also benchmark against[detailed dealership customer experience statistics](https://www.useflai.com/blog/dealership-customer-experience-statistics) to understand how your store compares.


**Step 2:** Pull the last 2 weeks of lead response times. Ask:


- How long until first contact attempt?
- How many leads get zero attempts?


**Step 3:** Compare the two leaks.


- **Inbound leak** = missed calls + abandoned calls + voicemail calls with no booked appointment
- **Outbound leak** = leads not contacted + no-shows + reactivation opportunities ignored


Whichever leak has the bigger dollar value gets fixed first.


### When to Prioritize Inbound vs Outbound BDC: The Rule of Thumb


If customers can’t reliably reach you, prioritize inbound. If customers reach you but nothing happens after, prioritize outbound. Inbound is a queueing problem. Outbound is a consistency problem. Fix the queue first, then automate consistency.


## BDC Org Design: Separate Teams, Hybrid, or Time-Blocking?


This is one of the highest-stakes decisions you’ll make. There’s no universal right answer, but there are clear tradeoffs.


**Option 1: Separate inbound and outbound roles.**


Less context switching and inbound SLAs stay protected. Outbound can be dialer-driven and campaign-focused. The downside: you need enough headcount to specialize, and handoffs between teams must be clean.


**Option 2: Hybrid reps (same people do both).**


Flexible coverage and works well for smaller rooftops. The downside: inbound suffers during outbound pushes, “callback debt” builds, and it’s hard to maintain consistent outbound execution when phones keep ringing.


**Option 3: Time-blocking (a hybrid that can work).**


This is a practical compromise. The logic:


Window Dedicated Role


Peak hours (e.g., 10 AM–12 PM, Mon–Tue) Inbound only


Off-peak hours Outbound blocks


End of day Confirmations and reminders


This matches the reality that[inbound demand is spiky during predictable windows](https://www.useflai.com/blog/when-do-dealerships-get-the-most-calls) , not constant, so you staff to the spike and use the valleys productively.


For most mid-size dealerships, time-blocking is the best starting point. You protect your inbound SLAs during the hours that matter most and still get structured outbound work done.


## In-House vs Outsourced vs AI BDC: Which Is Right for You?


Most content compares these options by cost per month. That’s the wrong lens. The real comparison comes down to three questions:


1. **Can you answer demand at the moment it arrives** , including peaks and after-hours?
2. **Can you reliably book into the real scheduler/CRM workflow** , not just take a message?
3. **Can you maintain quality and compliance at scale?**


**In-house BDC** works best when you have strong leadership and coaching, stable staffing, real QA processes, and coverage across key windows. It struggles when turnover is high, weekends and nights are unstaffed, and peaks overwhelm the team. In-house gives you the most control, but that control only matters if you can actually staff the hours.


**Outsourced BDC** is great when you need overflow coverage fast or lack internal management bandwidth. The risk is real, though: quality variation, weak dealership context (they don’t know your inventory or your service advisors), and the tendency to take messages instead of book appointments. There’s a reason[dealerships increasingly replace outsourced call centers with AI](https://www.useflai.com/blog/why-dealerships-replace-outsourced-call-centers-with-ai) . The message-taking problem is structural, not fixable through vendor selection alone.


**AI BDC** is the 2026 category that changes the equation. Understanding[what an AI BDC actually is and how it works](https://www.useflai.com/blog/what-is-an-ai-bdc-and-how-it-automates-dealership-calls) is essential before evaluating vendors. AI is increasingly positioned as primary coverage after-hours, overflow during peaks, and consistent outbound follow-up. An AI BDC like[Flai](https://www.useflai.com/) handles calls, texts, and emails, books appointments directly into your scheduler, and syncs to your CRM and DMS.


The AI strengths that matter most for dealerships:


- **Instant pickup** during peaks and after-hours (directly attacking hold time and abandonment)
- **Infinite concurrency** (ten calls at once without a hiring scramble)
- **Consistency on outbound follow-up** (systems don’t get busy and forget)
- **Omnichannel continuity** where a conversation can move from call to text to email without losing context


But AI has failure modes you need to plan for: misclassification and wrong routing (sales vs service vs parts), scheduler and CRM integration errors (double booking, missing notes), difficulty with edge cases (angry customers, warranty disputes, complex financing), and brand risk (luxury customers can be less forgiving of imperfect interactions). The[AI voice agents buyer’s guide](https://www.useflai.com/blog/ai-voice-agents-for-dealerships-buyers-guide) covers exactly what to look for, and what red flags to avoid, when evaluating vendors.


The correct framing: use AI for high-volume, structured, repetitive conversations, and escalate everything else cleanly. The best AI BDC setups are not “replace humans” plays. They’re “let humans do what humans do best” plays.


## How[Flai](https://www.useflai.com/) Handles Inbound and Outbound BDC


We built[Flai](https://www.useflai.com/) because we spent months physically inside dealerships (our founders visited over 400 stores) and kept seeing the same pattern: great salespeople and service advisors, terrible phone infrastructure. Calls go to voicemail after hours. Customers sit on hold during the 10 AM rush. Internet leads get a call-back attempt two days later. Recall campaigns sit in a spreadsheet somewhere.


[Flai](https://www.useflai.com/) is an AI communications platform that handles both sides of the BDC equation.


**On the inbound side** ,[Flai](https://www.useflai.com/) answers every call instantly, 24/7. No hold time, no voicemail, no “please leave a message and we’ll call you back.” The AI voice agent talks like a real person, identifies whether the caller needs sales or service, and books appointments directly into the dealership’s scheduler and DMS. It handles overflow during peak windows and takes over completely after hours and on weekends. See how[Flai is transforming dealership service departments](https://www.useflai.com/blog/how-ai-is-transforming-dealership-service-departments) for a deeper look at the operational impact.


**On the outbound side** ,[Flai](https://www.useflai.com/) runs structured follow-up that humans consistently struggle to execute: missed-call callbacks, appointment confirmations and reminders, no-show recovery, service reactivation campaigns, and recall outreach. The system doesn’t get busy. It doesn’t forget to follow up on Friday afternoon. It executes the cadence every time.


**Across channels** ,[Flai](https://www.useflai.com/) operates on voice, SMS, and email with the same underlying intelligence. A customer who calls and doesn’t book can get a follow-up text. A text conversation can escalate to a call. The AI retains context across all of it, so customers never have to repeat themselves.


### Real Dealership BDC Results: What the Numbers Show


The[Freeman Lexus case study](https://www.useflai.com/case-studies/freeman-lexus) tells the story clearly. In a single reporting period,[Flai](https://www.useflai.com/) handled roughly 1,100 calls with zero missed calls and zero hold time. Of those, 426 were bookable calls, and 376 became booked appointments, an **88% success rate** on bookable interactions. The estimated profit impact: **$100,000** .


[Freeman Toyota](https://www.useflai.com/case-studies/freeman-toyota) saw similar results: 1,053 calls handled, 358 appointments generated, and **$93,870 in profit impact** over one month, while augmenting their existing 6-person BDC team rather than replacing it.


[San Leandro CDJR](https://www.useflai.com/case-studies/san-leandro-cdjr) took it further: monthly appointments doubled from 205 to 448, representing a **119% increase** , with[Flai](https://www.useflai.com/) handling 1,563 calls and generating an estimated **$83,000 in profit impact** in the first 30 days.


Even if you discount vendor-reported numbers, the operational takeaway holds: after-hours and overflow aren’t “extra.” They’re a large fraction of demand that most stores are simply dropping.


Across our[case studies](https://www.useflai.com/case-studies) , profit per booked appointment clusters around $260-$270. The math is simple: you don’t need perfect performance. You just need to stop letting calls die.


The Flai platform dashboard surfaces these numbers live for every dealership: total calls handled, scheduled appointments, booking rate, and estimated revenue impact — all from the same screen. Notice the 90% booking rate and $158,760 revenue impact visible in the product UI below, along with dealership groups like Freeman Motors and Findlay already using the platform.


### Why BDC Integration with Your Scheduler and CRM Matters


[Flai](https://www.useflai.com/) integrates directly with dealership schedulers, CRM, and DMS. This is the difference between an AI that takes messages and one that actually books appointments. When a customer calls at 8:30 PM to schedule their 10K service,[Flai](https://www.useflai.com/) checks the real scheduler for open slots, books the appointment, updates the CRM, and sends a confirmation. That appointment exists in the system before the customer hangs up.


No hardware installation. No staff training. The system plugs into your existing phone numbers and tools.


## Outbound BDC Compliance: What You Need to Know Before Scaling


This section is not legal advice. But if you run outbound, you need a compliance spine. The fastest way to kill an outbound program is a spike in complaints, carrier filtering, or TCPA risk.


### TCPA Compliance Basics for Dealership Outbound Calling


The Telephone Consumer Protection Act and related rules regulate certain calls and texts, especially those using automated systems or prerecorded/artificial voices. Penalties are commonly described as **$500 per violation** , with potential for up to **$1,500** if the violation is willful.


Inbound calls that a customer initiates are generally lower risk. Outbound is where the rules matter most, because you’re the one making the contact.


### FTC Telemarketing Sales Rule Updates Affecting Dealership Outbound


The FTC has been updating TSR enforcement and recordkeeping expectations. In March 2024,[the FTC implemented new protections](https://www.ftc.gov/news-events/news/press-releases/2024/03/ftc-implements-new-protections-businesses-against-telemarketing-fraud-affirms-protections-against-ai) that included new recordkeeping requirements and reinforced protections against AI-enabled telemarketing fraud. If you use AI for outbound, keep clean records.


### What Happened to the One-to-One Consent Rule (2025 Update)


If you’ve been tracking TCPA compliance news, you might remember the controversial “one-to-one consent” rule aimed at lead generator consent. As of 2025, the FCC removed this rule after a court decision vacated it.[The Federal Register notice](https://www.govinfo.gov/content/pkg/FR-2025-08-29/pdf/2025-17285.pdf) documents the FCC reinstating the prior version of the consent definition.


Don’t build your compliance program around outdated summaries. The rule landscape moved.


### How to Handle Consumer Opt-Out Requests in Outbound BDC


The FCC adopted rules around honoring consumer revocation requests across communications. If a customer says “stop,” you must treat it seriously and consistently across systems. The effective date has been subject to waivers and extensions.[An FCC order](https://docs.fcc.gov/public/attachments/FCC-25-2A1.pdf) from January 2026 granted an extension related to revocation rules, extending the waiver to January 31, 2027.


Design now for strict opt-out handling anyway. It reduces complaints, rules can become effective later, and messy opt-out logic is a risk regardless of dates.


### SMS Compliance and 10DLC Registration for Dealership Outbound


If you text customers in the U.S. from local numbers at scale, you’re dealing with A2P 10DLC registration and carrier policies.[Twilio’s help documentation](https://help.twilio.com/articles/1260800720410-What-is-A2P-10DLC-) explains that 10DLC involves Brand and Campaign registration and ongoing fees.


Outbound SMS is not “just send texts.” It’s deliverability infrastructure plus consent management.


### Call Recording Laws That Apply to Your Dealership BDC


If you record calls for QA or training, state laws require either one-party or all-party consent depending on where the parties are located. The[Reporters Committee’s state-by-state recording guide](https://www.rcfp.org/reporters-recording-guide/) is a reliable starting point covering all 50 states and Washington D.C.


If you operate across states, the conservative approach: announce recording at the start and provide an option to continue or request another channel.


## 30-60-90 Day BDC Implementation Roadmap for Dealerships


### Days 1-30: Measure BDC Performance and Stop Losing Calls


- Pull call data by hour, day, and department
- Identify your top missed-call windows (often Monday-Tuesday mornings and the[10 AM to 12 PM peak](https://www.useflai.com/blog/when-do-dealerships-get-the-most-calls) )
- Fix routing and transfer rules
- Add callback options and eliminate cold transfers
- **Deliverable:** A weekly dashboard with access metrics and kept appointment outcomes


### Days 31-60: Lock Inbound Coverage and Launch Outbound Cadences


-> Separate inbound coverage during peak windows


-> Build 3 outbound cadences: missed calls, internet leads, and confirmations


-> Train the team on a single definition of success: kept appointments


-> Set up[service appointment scheduling automation](https://www.useflai.com/blog/how-to-automate-service-appointment-scheduling-at-your-dealership) for after-hours demand you’re currently dropping


-> **Deliverable:** SOPs for inbound scripts and outbound cadences


### Days 61-90: Scale Campaigns and Evaluate AI BDC Options


1. Add service reactivation, declined services, and recall campaigns
2. Add language coverage strategy where relevant.[Multilingual BDC service](https://www.useflai.com/blog/guide-to-multilingual-customer-service-for-dealerships) can be a meaningful differentiator in the right markets
3. Evaluate AI or automation for overflow and after-hours (this is where a tool like[Flai](https://www.useflai.com/) typically enters the picture). A full[guide to AI for car dealerships](https://www.useflai.com/blog/ai-for-car-dealerships-guide) is a good starting point for understanding what to evaluate
4. **Deliverable:** A quarterly campaign calendar plus a “what happens when we get slammed” contingency plan


## Inbound vs Outbound BDC: Frequently Asked Questions


**What’s the difference between inbound and outbound BDC in a dealership?**


Inbound BDC handles customer-initiated contacts: phone calls, web forms, texts, chats, and email inquiries. The job is to answer fast and book an appointment. Outbound BDC is dealership-initiated: callbacks on missed calls, lead follow-up, appointment confirmations, no-show recovery, and service reactivation campaigns. Inbound captures demand when it’s hottest. Outbound recovers what inbound missed and prevents no-shows. For a deeper look at[what a BDC does in a dealership](https://www.useflai.com/blog/what-is-a-bdc-in-a-car-dealership) , the full guide covers structure, staffing, and measurement.


**Should I build inbound or outbound BDC first?**


Start with inbound in most cases. If your connection rate is below **80%** or your average hold time exceeds **2 minutes** , you’re losing callers before outbound can even help. Run the diagnostic: pull 2 weeks of call data, calculate your inbound leak (missed + abandoned + voicemail without appointments) vs your outbound leak (leads uncontacted + no-shows + dormant customers), and fix whichever one has the bigger dollar value.


**How many BDC agents do I need for inbound coverage?**


It depends on your call volume during peak windows. The staffing formula: take your calls offered during the peak window, multiply by average handle time, then divide by (window minutes x target occupancy rate). For example, 100 calls in a 2-hour peak window with 4.5-minute AHT and 80% occupancy = **5 agents** . Most stores understaff peaks and overstaff valleys.


**What’s a good connection rate for a dealership BDC?**


Industry data from Car Wars shows the average hovers around **65%** . That’s not good. It means roughly one in three callers doesn’t reach anyone. High-performing stores target **85% or above** . Every percentage point improvement in connection rate drops directly to the bottom line through the BDC profit equation.


**Can AI handle both inbound and outbound BDC?**


Yes, and that’s actually where[AI BDC](https://www.useflai.com/blog/what-is-an-ai-bdc-and-how-it-automates-dealership-calls) shines brightest. On inbound, AI provides instant pickup with zero hold time during peaks and after-hours. On outbound, AI executes follow-up cadences consistently without the execution gaps that happen when human reps get busy.[Flai](https://www.useflai.com/) , for example, handles both inbound calls and outbound campaigns across voice, SMS, and email while booking directly into the dealership’s scheduler. You can[book a demo](https://www.useflai.com/book-demo) to see how it handles your specific call patterns.


**What outbound campaigns have the highest ROI?**


Missed-call callbacks are typically the highest ROI because the customer was already in action mode. Appointment confirmations and reminders come next because they directly improve show rates. After that, service reactivation (customers who haven’t visited in 6+ months) and declined services follow-up (already diagnosed, high intent). Recall campaigns are operationally important but behaviorally challenging because completion depends on removing friction for the customer.


**How do I stay TCPA-compliant with outbound BDC?**


Focus on four things: maintain proper consent records before making automated outbound contacts, honor opt-out requests immediately and consistently across all systems, register for 10DLC if you’re sending SMS at scale, and keep detailed call/text records as the FTC’s updated Telemarketing Sales Rule requires. The compliance landscape has shifted recently (the one-to-one consent rule was removed in 2025), so work with counsel who understands the current state of the rules, not summaries from two years ago.


**What’s the difference between an outsourced BDC and an AI BDC?**


An outsourced BDC uses human agents at a third-party call center. They’re good for fast overflow coverage but often lack dealership context and tend to take messages rather than book appointments. An[AI BDC like Flai](https://www.useflai.com/) uses artificial intelligence to handle calls, texts, and emails. The key advantage of AI is infinite concurrency (handle ten calls at once), 24/7 availability with no staffing gaps, consistent execution on outbound cadences, and direct integration with your scheduler and DMS so appointments actually get booked, not just noted.
