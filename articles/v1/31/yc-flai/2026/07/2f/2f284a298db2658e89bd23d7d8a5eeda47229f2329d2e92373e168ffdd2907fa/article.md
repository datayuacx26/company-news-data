---
schema_version: "1.0.0"
document_id: "2f284a298db2658e89bd23d7d8a5eeda47229f2329d2e92373e168ffdd2907fa"
company_key: "yc-flai"
company: "Flai"
source_id: "yc-flai-news-import-6bbede6307a5"
canonical_url: "https://www.useflai.com/blog/set-up-a-bdc-at-dealership"
published_at: "2026-07-06T14:47:16.978+00:00"
first_seen_at: "2026-07-21T20:24:10.813388+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:05b8b9660646ac7fb37cc8627d9643be037eea824983756128c601ca89ddac68"
---

# How to Set Up a Dealership BDC: 2026 Best-Practices Guide

Most dealerships don’t have a lead problem. They have a response problem. A[McKinsey report published in January 2025](https://www.mckinsey.com/industries/automotive-and-assembly/our-insights/boosting-auto-sales-productivity-a-playbook-for-excellence) found that **more than half of new leads (56%) come in after hours** , and only **37% get addressed within the first hour** . A[Car Wars analysis shared by Dealership Guy](https://news.dealershipguy.com/p/how-a-data-company-found-the-secret) put the average dealership hold time at **3 minutes and 5 seconds** , with nearly a third of unconnected calls being customers who just hung up while waiting. And[J.D. Power’s 2024 CSI study](https://www.jdpower.com/business/press-releases/2024-us-customer-service-index-csi-study) showed customers waiting more than five days for a service appointment on average, with **35% of mass-market customers going elsewhere** because they needed service “immediately.”


The[revenue impact of missed calls](https://www.useflai.com/blog/missed-call-statistics-revenue-lost-at-your-dealership) adds up faster than most dealers realize. That’s not a staffing problem you solve by telling your team to “pick up faster.” It’s a systems problem. The system you build to fix it is called a BDC.


This guide walks through the complete setup: what a dealership BDC actually does, how to structure it, how to staff it, which processes to build, what tech you need, which KPIs to track, and how to avoid the mistakes that kill most BDCs before they get traction. Whether you’re building from scratch or fixing a broken one, everything here is practical and built for 2026 realities.


## What Does a BDC Do at a Dealership?


BDC stands for Business Development Center. In a dealership, it’s the team (or system) responsible for turning customer interest into booked appointments that actually show up. That’s it. Everything else is a means to that end. If you want a deeper primer before diving into setup, our guide on[what a BDC is in a car dealership](https://www.useflai.com/blog/what-is-a-bdc-in-a-car-dealership) covers the fundamentals.


A functioning BDC typically owns:


- **Inbound phone answering and routing** for sales and service lines
- **Internet lead response** from website forms, chat, and third-party lead providers
- **Outbound follow-up** on no-shows, unsold leads, and lost opportunities
- **Appointment setting and confirmation** across both departments
- **Service retention campaigns** including recalls, overdue maintenance, and reactivation


The critical thing most people miss is that you’re really setting up two distinct operations. A **sales BDC** turns shoppers into showroom appointments and test drives. A **service BDC** turns callers and outreach into booked repair orders. The scripts are different, the metrics are different, and the workflows are different. If you try to run both with one vague process, you end up with the classic failure mode: a message-taking layer that everyone hates and nobody trusts.


There’s a useful mental model from queueing theory here. Calls and leads arrive in bursts, not in a smooth line. Your people capacity is fixed by schedules. That mismatch between unpredictable demand and fixed capacity is what creates holds, missed calls, late responses, and dropped leads. *The key principle: as utilization approaches 100%, wait time doesn’t just increase, it explodes.* So “we’re staffed” can still mean terrible hold times if you’ve sized for average volume instead of peaks. Effective[call overflow solutions](https://www.useflai.com/blog/call-overflow-solutions-for-dealerships) address this structural problem directly.


And the BDC isn’t just about answering. It’s a conversion funnel: attempted contact, to actual contact, to appointment set, to appointment shown, to vehicle sold or repair order written. If you don’t track the full funnel, you can’t fix it.


## How to Write a BDC Charter for Your Dealership


Skip this step and every department in the store will try to use the BDC as free labor. The charter is a one-page document that defines what the BDC does, what it owns, and (just as importantly) what it doesn’t own.


Your charter should specify:


**What the BDC exists to do:** Book appointments that show, for Sales and/or Service.


**Which channels it owns:**


- Phone inbound (which numbers)
- Internet leads (which sources)
- Texting and email
- Outbound campaigns (which ones)


**Which outcomes it owns:**


- Appointments set
- Confirmation messages sent
- No-show recovery attempts within a defined window
- Lead notes logged in CRM within a set timeframe


**What the BDC does NOT own:**


- Price negotiations beyond basic guardrails
- Deep technical diagnosis
- Finance quoting
- Warranty authorization


**Escalation rules:** When does the BDC transfer to a sales manager or service advisor? Define this clearly, or you’ll get reps either escalating everything (useless) or trying to handle things they shouldn’t (dangerous).


Before you build the BDC itself, check two blind spots. First, do you actually have capacity for the appointments you’re trying to create? If your service schedule is already backed up for five days, pumping more appointments into it will harm customer satisfaction.[J.D. Power’s CSI research](https://www.jdpower.com/business/press-releases/2024-us-customer-service-index-csi-study) is essentially saying that capacity and communication matter most. Fix service lane capacity before you fix lead flow. Understanding how to[improve CSI scores at your dealership](https://www.useflai.com/blog/how-to-improve-csi-scores-at-your-dealership) often starts with exactly this kind of structural fix.


Second, make sure you’re solving the right bottleneck.[Cox Automotive’s 2025 Service Industry Study](https://www.coxautoinc.com/insights-hub/new-cox-automotive-study-finds-dealerships-have-lost-12-of-service-visits-to-competition-since-2018/) found that customers are leaving dealerships for service partly due to communication problems. If your store answers calls but fails to communicate clearly afterward, a BDC needs strong follow-up processes, not just more dialing. The full picture of[dealership customer experience statistics](https://www.useflai.com/blog/dealership-customer-experience-statistics) is worth reviewing before you finalize your charter.


## BDC Operating Models: In-House, Outsourced, or Hybrid?


You have three real options, and the right choice depends on your constraints.


**In-house BDC.** You hire, train, and manage BDC reps yourself. This gives you tight culture control and deep store knowledge. Your reps learn the inventory, know the advisors by name, and understand the nuances of your customer base. The hidden cost is management attention. A BDC is not set-and-forget. It’s a factory line that needs daily coaching, script refinement, and performance tracking. If your BDC manager leaves and nobody fills that gap, performance collapses fast.


**Outsourced BDC.** A third-party call center handles your calls and leads. This works when you need coverage fast, can’t hire in your market, or need after-hours support without building a second shift. The risk is real: if the vendor can’t book directly into your systems, they become a message-taking service. And message-taking is where leads go to die. Brand dilution is also a concern, especially for luxury stores where customers expect a premium phone experience. There’s a reason[more and more dealerships are replacing outsourced call centers with AI](https://www.useflai.com/blog/why-dealerships-replace-outsourced-call-centers-with-ai) : the economics and quality just work out better.


**Hybrid BDC (humans + AI).** This is where AI BDC tools come in. The hybrid model uses automation or AI for the things it handles well (after-hours answering, overflow during peaks, appointment confirmations, reminders, and multilingual support) while keeping humans for edge cases, complex objections, and relationship-driven conversations. It solves the two biggest constraints traditional BDCs face: coverage gaps and response speed. A[virtual receptionist for car dealerships](https://www.useflai.com/blog/virtual-receptionist-for-car-dealerships) is often the first layer of this hybrid approach, covering the hours and peaks that humans can’t.


If you’re evaluating the hybrid approach, skip the marketing demos and focus on integration depth, reliability under real call volume, and how cleanly the system hands off to humans when it needs to. Understanding[what an AI BDC actually is and how it automates dealership calls](https://www.useflai.com/blog/what-is-an-ai-bdc-and-how-it-automates-dealership-calls) helps you ask better questions when evaluating vendors. We’ll get deeper into AI and how[Flai](https://www.useflai.com/) fits into this model later in the guide.


## How to Size and Staff Your Dealership BDC


Staffing is where most BDC plans go wrong. The math isn’t complicated, but it goes against the instinct to staff for averages.


**Start with two numbers you can measure this week:**


1. **Inbound demand:** calls per hour during your peak windows
2. **Average handle time (AHT):** average minutes per call, including wrap-up notes


If you don’t have call tracking, pull your phone logs and sample 50 calls. That’s enough to get directional numbers. Our data on[when dealerships get the most calls](https://www.useflai.com/blog/when-do-dealerships-get-the-most-calls) can give you a baseline if you’re starting from scratch. Monday mornings between 10 AM and noon are the most demanding window for most stores.


**The basic staffing formula:**


Agent-hours needed per hour = (calls per hour x AHT in minutes) / 60


Then add a buffer. Humans aren’t robots, and call arrival is unpredictable. A practical rule: plan for **70% to 80% occupancy** during peak windows, not 95%.


**Worked example for a service line:**


- Peak hour: 24 calls/hour
- AHT: 6 minutes
- Workload: (24 x 6) / 60 = 2.4 agent-hours per hour
- With 75% occupancy: 2.4 / 0.75 = **3.2 agents**


So you need 3 to 4 people covering that peak hour, accounting for breaks and variability.


This is the first-principles reason BDC staffing feels expensive: you’re paying for peak capacity, not average workload. The rep who seems “idle” at 2pm is the same rep preventing a 45-second hold time at 10am. It’s also exactly why the hybrid model with AI is appealing. AI doesn’t need break coverage and doesn’t degrade under volume spikes. The goal is to[never miss a dealership call](https://www.useflai.com/blog/never-miss-a-dealership-call-at-your-dealership) , and humans alone can’t always guarantee that.


## Dealership BDC Roles, Org Chart, and Compensation


A high-functioning BDC usually needs these roles, even if some start as part-time responsibilities:


**BDC Manager (or Team Lead):** Owns process, coaching, reporting, and the staffing schedule. This person is the single throat to choke for BDC performance. If nobody owns this role, the BDC becomes a political orphan and results collapse.


**BDC Reps:** Handle calls and leads, set appointments, and manage follow-up tasks. These are your front-line people.


**QA/Trainer (can be the manager early on):** Scores calls, refines scripts, and runs onboarding for new hires.


**Ops/Admin Support (optional):** Handles list pulls, campaign setup, and data hygiene. Nice to have but not essential at launch.


**The org chart that works:** Reps report to the BDC Manager. The BDC Manager should have a dotted-line relationship to both the GSM (for sales outcomes) and the Fixed Ops Director (for service outcomes). If the BDC manager reports to nobody with authority over either revenue stream, the department gets deprioritized every time there’s a floor deal or a busy Saturday.


### What Do BDC Reps and Managers Get Paid?


Compensation varies by market. Here are current reference points based on[Indeed](https://www.indeed.com/career/business-development-center-representative/salaries) and[ZipRecruiter](https://www.ziprecruiter.com/Salaries/BDC-Representative-Salary) data as of early 2026:


Role Hourly / Annual


BDC Representative ~$17.94/hr ($37,315 annually)


BDC Rep (ZipRecruiter avg) $43,114/year


[BDC Manager](https://www.ziprecruiter.com/Salaries/Bdc-Manager-Salary) $48,904/year


Treat these as rough midpoints since titles vary wildly by responsibility level.


### How to Structure BDC Compensation Around Outcomes


BDC comp plans fail when they pay for activity instead of outcomes. Use a two-part structure:


- **Base pay** for stability and retention
- **Variable pay** tied to verifiable outcomes


The variable piece should emphasize kept appointments (shown), not just set appointments. A smaller bonus per appointment set, a bigger bonus per appointment shown, and an extra kicker for same-day or after-hours bookings keeps incentives aligned with what actually generates revenue. If you pay big money per appointment set without a show component, you’re training your team to spam-book.


## BDC Phone Scripts and Workflows for Sales and Service


A BDC setup is 80% process design. Without clear workflows, your reps will each improvise, and your results will be all over the place.


### How to Handle Inbound BDC Calls at a Dealership


The goal of every inbound call isn’t “answer the question.” It’s to solve the customer’s problem and, when appropriate, convert them to a next commitment (usually an appointment).


**The 5-step inbound call skeleton (works for both sales and service):**


1. **Connect and orient** the caller
2. **Clarify their intent** (service, sales, parts, or something else)
3. **Collect minimum viable details** (vehicle, contact info, issue)
4. **Offer two specific appointment times** (not “when do you want to come in?”)
5. **Confirm and document** (send a confirmation text immediately)


Here’s a service inbound script you can customize:


**Greeting:** “Thanks for calling \[Dealer\]. How can I help today?”


**Classify:** “Is this for service, sales, parts, or something else?”


**Job type:** “What are we working on? Routine maintenance, a warning light, a noise, or something else?”


**Vehicle and contact:** “What vehicle is it?” / “What’s the best number to text your confirmation?”


**Two options:** “I can do Tuesday at 10am or Thursday at 2pm. Which works better?”


**Set expectations:** “Great, you’re set. Please arrive 10 minutes early.”


**Confirm:** “I’m sending a confirmation text now. If anything changes, reply to that message.”


For sales, the structure is the same but the discovery is lighter. Focus on what vehicle they’re interested in, new or used, and move quickly to offering two visit times. If you want to go deeper on automating this workflow end to end, our guide on[how to automate service appointment scheduling at your dealership](https://www.useflai.com/blog/how-to-automate-service-appointment-scheduling-at-your-dealership) covers the full process design.


### How to Respond to Internet Leads from Your BDC


Speed is still the biggest multiplier for internet leads. Industry data from multiple sources consistently shows that leads responded to within 15 minutes convert at far higher rates, but **19% of leads receive a response in over an hour** . The customers who submitted those late-response leads didn’t sit around waiting. They called somebody else.


**The 10-minute rule (practical default):** Your first touch should happen fast enough that the customer still remembers submitting the lead. That means a call plus text within 10 minutes of the lead arriving.


A solid follow-up cadence after the first touch:


- **Same day:** 2 to 3 more attempts at different times
- **Next 3 days:** 1 to 2 touches per day
- **Day 4 through 14:** 3 to 5 touches total (mix of value messages and asks)
- **Day 15 and beyond:** Weekly nurture if the customer opted in


Your texts should feel like they came from a human who read the lead. Something like: “Hey \[Name\], this is \[Rep\] at \[Dealer\]. I saw your request on the \[Vehicle\]. Are you aiming to come in today or is tomorrow better?” Our guide on[how AI follow-up is helping dealerships close more sales leads](https://www.useflai.com/blog/how-ai-follow-up-is-helping-dealerships-close-more-sales-leads) covers how to automate this cadence without losing the human feel.


### How to Confirm Appointments and Recover No-Shows


Your appointment set rate is not your endgame. Show rate is. Plenty of BDCs book aggressively but never follow up, and half the appointments evaporate.


**A simple show management system:**


1. Confirmation message immediately after booking
2. Reminder the day before
3. Reminder the day of (morning)
4. If they no-show: reschedule text within 15 minutes


That no-show recovery text is worth its weight in gold:


“Hey \[Name\], we had you scheduled for \[time\] and wanted to make sure everything’s okay. Want to reschedule for later today or would tomorrow work better?”


This recovers more appointments than most dealers expect.


### How to Run Service Retention and Recall Outreach Campaigns


This is where a BDC stops being a cost center and becomes a revenue engine.[Cox Automotive’s 2025 study](https://www.coxautoinc.com/insights-hub/new-cox-automotive-study-finds-dealerships-have-lost-12-of-service-visits-to-competition-since-2018/) found that dealerships handled **12% fewer service visits than in 2018** , and only **54% of owners** with cars two years old or newer returned to the selling dealer for service in 2025 (down from 72% in 2023). Those are customers you already sold a car to, walking out the back door.


A service retention playbook through the BDC should cover recall outreach scheduling, declined service follow-up, overdue maintenance reminders, and lost customer reactivation. This work is repetitive and high-volume, which makes it a natural fit for AI automation. Our guide on[dealership recall campaigns best practices](https://www.useflai.com/blog/dealership-recall-campaigns-best-practices-ai) covers how to run these effectively, and our resource on[how to increase service appointments at dealerships](https://www.useflai.com/blog/how-to-increase-service-appointments-at-dealerships) covers the broader retention strategy.


## Dealership BDC Tech Stack: What You Actually Need


A BDC with bad tools turns into sticky notes and tribal knowledge. Here’s the minimum viable stack for 2026:


Category Purpose


**CRM** Lead status, tasks, appointment tracking


**Telephony + call tracking** Routing, recording, analytics


**Texting (integrated with CRM)** Compliant customer messaging


**Email with templates** Automated sequences and tracking


**Scheduler** Service and test drive slot booking


**Reporting** Funnel metrics by source, rep, and hour


**Knowledge base** Store policies, hours, directions, pricing guardrails


The single most important principle for your tech stack: every call and lead must become a CRM record, every appointment must be written back to the system of record, and every follow-up must be a task, not a hope. If your systems don’t sync, your BDC will spend half its time on double entry, and productivity dies.


The[Flai platform’s Tekion integration](https://www.useflai.com/blog/tekion-integration) is one example of how deep CRM and DMS sync enables the AI to act on real data, booking appointments directly into the scheduler without manual handoffs.


## What KPIs Should a Dealership BDC Track?


You can’t manage what you don’t measure, but measuring the wrong things is worse than measuring nothing. Here’s what actually matters, tracked by source, by rep, and by hour:


- **Speed to answer** (for inbound calls)
- **Abandonment / missed call rate**
- **Speed to lead** (for internet submissions)
- **Contact rate** (did you actually reach a human?)
- **Appointment set rate**
- **Appointment show rate**
- **Sales close rate or service RO written**
- **Revenue per kept appointment**


Industry data consistently shows phone leads convert to appointments at dramatically higher rates than internet leads, often nearly double. Every missed call is a missed appointment at a much higher rate than a missed email. Our guide on[BDC metrics every dealership should track](https://www.useflai.com/blog/bdc-metrics-every-dealership-should-track) breaks down the complete measurement framework.


**Your dashboard should answer these questions every single day:**


1. How many inbound calls did we miss yesterday?
2. What hour did we miss them?
3. How many leads aged past 30 minutes without a response?
4. How many appointments did we set, and how many showed?
5. Who is underperforming on contact rate?
6. Which lead sources are wasting money?


If your dashboard can’t answer those questions in under two minutes, your reporting needs work.[Dealership customer experience benchmarks](https://www.useflai.com/blog/dealership-customer-experience-statistics) help you set realistic targets and spot where you’re lagging.


## BDC QA, Coaching, and Compliance


### How to Build a BDC Coaching System That Works


Talent gets you started. Coaching is what builds a great BDC. Here’s a rhythm that works:


- **Daily:** 10-minute huddle covering yesterday’s numbers and today’s priorities
- **Weekly:** 30-minute team training focused on one specific objection or one script improvement
- **Weekly:** 3 calls per rep scored and coached (replay the moment the call went sideways, then redo the line together)
- **Monthly:** Process review with sales and service department heads to maintain alignment


**QA Scorecard.** Score each reviewed call on a 1-to-5 scale across these dimensions: greeting and professionalism, correct call classification, discovery quality, appointment ask, offering two specific times, confirmation and next steps, CRM documentation, and compliance.


Understanding[why dealership callers hang up](https://www.useflai.com/blog/why-dealership-callers-hang-up-and-how-to-stop-it) is essential coaching context. Your QA reviews should flag the exact moments where customers disengage, then your team rehearses fixing those moments. Sharing[the true cost of dealership hold times](https://www.useflai.com/blog/true-cost-of-dealership-hold-times) with your reps during coaching helps them understand that a 3-minute hold costs real money.


### BDC Compliance: TCPA, DNC, and 10DLC Requirements


This isn’t legal advice (talk to your counsel), but here’s the operational reality your BDC needs to handle.


**Consent for automated calls and texts.** The[FCC’s consumer guidance](https://www.fcc.gov/consumers/guides/stop-unwanted-robocalls-and-texts) makes clear that autodialed calls and texts to mobile phones require consent. If your BDC uses any automated texting or calling features, build consent capture and opt-out handling into every workflow.


**Do Not Call compliance.** The[FTC’s National DNC Registry](https://www.donotcall.gov/) has[more than 253 million actively registered numbers](https://www.ftc.gov/reports/national-do-not-call-registry-data-book-fiscal-year-2024) as of FY2024. Your BDC needs a clear policy for outbound marketing calls, list scrubbing, and honoring internal DNC requests.


**Text message protections.** The[FCC has extended DNC protections to text messages](https://docs.fcc.gov/public/attachments/DA-24-907A1.pdf) , meaning marketing texts to numbers on the registry require prior permission. And the[FCC has codified](https://docs.fcc.gov/public/attachments/FCC-24-24A1.pdf) that consumers can revoke consent “in any reasonable manner.” If someone texts “STOP” or “please don’t text me,” that means stop. Make this automatic.


**One-to-one consent.** The FCC adopted a[one-to-one consent rule](https://www.fcc.gov/document/faqs-one-one-consent-rule-tcpa-prior-express-written-consent) to close lead-generator loopholes.[Legal challenges followed](https://www.akerman.com/en/perspectives/saved-by-the-bell-eleventh-circuit-vacates-fccs-one-to-one-consent-rule-days-before-effective-date.html) , and the rule’s status has been in flux, with[reports that the FCC formally abandoned the requirement](https://www.consumerfinanceinsights.com/2025/09/15/the-fcc-issues-final-rule-formally-eliminating-the-one-to-one-consent-requirement/) . Practical move regardless: treat third-party lead consent as suspect until documented, capture your own consent on first contact, and keep records of opt-in language, timestamps, and source.


**10DLC registration.** If you text customers using 10-digit long codes in the U.S., carriers require A2P 10DLC registration.[Microsoft’s Azure documentation](https://learn.microsoft.com/en-us/azure/communication-services/concepts/sms/messaging-policy) lays this out plainly. If your text deliverability is inconsistent or messages fail silently, you might have a registration problem, not a rep problem.


## 30/60/90-Day Dealership BDC Launch Plan


Don’t try to launch everything at once. Phase it.


**Days 1 to 15: Baseline and Design**


- Pull 30 days of call and lead data
- Map your current funnel (where are leads falling off?)
- Write the BDC charter
- Pick your operating model
- Draft scripts and escalation rules


**Days 16 to 45: Build, Hire, Wire**


- Hire reps and a manager (or promote an internal lead)
- Set up CRM workflows and dispositions
- Wire telephony to log calls into CRM
- Create text and email templates
- Build your KPI dashboard


**Days 46 to 60: Pilot**


- Start with one department (service or sales) or one lead source
- Run QA daily for the first two weeks
- Fix scripting and routing issues fast
- Track set rates and show rates daily


**Days 61 to 90: Scale**


- Expand to all lead sources and call types
- Add proactive campaigns (no-shows, recalls, reactivation)
- Move the coaching cadence to weekly
- Build a “playbook library” of scripts and approaches that work


## Why Dealership BDCs Fail (and How to Prevent It)


Most BDCs don’t fail because of bad people. They fail because of structural problems that were baked in from the start.


**Measured on activity, not outcomes.** If you pay and coach to “calls made” instead of “appointments that showed,” you’ll get a lot of dialing and very little revenue. *Fix: tie everything to kept appointments.*


**No one owns the process.** If the BDC doesn’t have a dedicated manager with authority to change scripts and workflows, it becomes everyone’s department and nobody’s responsibility. *Fix: one person owns the funnel metrics.*


**Too many handoffs.** Every handoff is a drop-off point. If the BDC takes a call, writes a note, hands it to a coordinator, who passes it to an advisor, who then calls the customer back, you’ve built a game of telephone. *Fix: decide what the BDC resolves directly versus escalates, then train to resolve.*


**No real system access.** If the BDC can’t book appointments directly in your scheduler, they become message-takers. Message-takers lose customers. *Fix: integrate with your scheduler and CRM so direct booking is the default action.*


**No show management.** A set appointment that doesn’t show isn’t success. It’s a vanity metric. *Fix: confirmations, reminders, and no-show recovery become standard work, not optional extras.*[Why dealership voicemails kill sales](https://www.useflai.com/blog/why-dealership-voicemails-kill-sales-and-what-to-do) is the same structural failure in a different form. Every point of friction between the customer and a confirmed appointment is a failure of your process, not just your people.


## How AI Is Changing Dealership BDC Setup in 2026


Building a great BDC with humans works. But it comes with two constraints that are hard to solve with people alone.


**Coverage.** Humans need breaks, get sick, take vacations, and go home at night. But[56% of your leads arrive after hours](https://www.mckinsey.com/industries/automotive-and-assembly/our-insights/boosting-auto-sales-productivity-a-playbook-for-excellence) . Building a second shift is expensive. Building a third shift for weekends is even more expensive. And even with full staffing, lunch rushes and Monday morning spikes create hold times that push customers to competitors. For a complete picture of[how AI is transforming dealership service departments](https://www.useflai.com/blog/how-ai-is-transforming-dealership-service-departments) , including the specific workflows AI handles best, that resource is worth reading before you make staffing decisions.


**Speed.** The difference between a 10-second pickup and a 3-minute hold is often the difference between a booked appointment and a lost customer. Humans can only answer one call at a time. When three lines ring simultaneously at 5:30pm, two of those callers are waiting.


This is exactly why we built[Flai](https://www.useflai.com/) . Our AI BDC platform was designed specifically for dealerships, by a team that spent months physically embedded in service bays and back offices across hundreds of stores.[Flai](https://www.useflai.com/) answers calls instantly, 24/7, in any language. It books service appointments and test drives directly into your scheduler. It handles confirmations, reminders, recall outreach, and follow-ups across voice, text, and email, all through one system that integrates with your existing CRM and DMS.


The hybrid model in practice looks like this:[Flai](https://www.useflai.com/) handles the high-volume, structured work (after-hours calls, overflow during peaks, appointment confirmations, recall campaigns, FAQ responses) while your human team focuses on complex objections, relationship-building, and the conversations that genuinely need a person. Multilingual support is part of the equation too. Our guide on[multilingual customer service for dealerships](https://www.useflai.com/blog/guide-to-multilingual-customer-service-for-dealerships) explains why this matters more than most dealers realize.


The results from dealerships already using the platform speak for themselves:


**Freeman Lexus** ([case study](https://www.useflai.com/case-studies/freeman-lexus) ) Over 1,100 calls handled with zero missed. 376 appointments booked from 426 bookable calls, an **88% conversion rate** . Estimated **$100,000 profit impact** .


**San Leandro CDJR** ([case study](https://www.useflai.com/case-studies/san-leandro-cdjr) ) Monthly service appointments jumped from 205 to 448 in the first month.[Flai](https://www.useflai.com/) booked 304 of those and generated an estimated **$83,000 in profit** .


**Freeman Toyota** ([case study](https://www.useflai.com/case-studies/freeman-toyota) ) 1,053 calls handled and 358 appointments scheduled, with a **$93,870 profit impact** in one month.


You can view all[Flai case studies](https://www.useflai.com/case-studies) for a broader picture of results across dealership types.


If you’re evaluating AI for your BDC, our[complete buyer’s guide for AI voice agents for dealerships](https://www.useflai.com/blog/ai-voice-agents-for-dealerships-buyers-guide) covers the 12 questions that separate good vendors from bad ones. Here are the ones that matter most:


- Can it book into your real scheduler (not a fake demo calendar)?
- What percentage of interactions are fully handled without a human?
- How does it hand off to your team, and does it pass full context?
- What happens if integrations go down?
- Does it support compliant texting with opt-out handling?
- Can you review transcripts and recordings to coach from them?


Ready to see how an AI BDC would work at your store?[Book a demo with Flai](https://www.useflai.com/book-demo) and we’ll walk through your specific call volume, staffing setup, and DMS integration.


## Frequently Asked Questions


**How much does it cost to set up a dealership BDC?** Costs depend heavily on your operating model. An in-house BDC with 3 to 4 reps and a manager will run roughly **$200,000 to $300,000 annually** in fully loaded compensation alone, based on current market rates. Add technology costs (CRM, telephony, texting platform) and you’re looking at another **$20,000 to $50,000 per year** . AI-powered options like[Flai](https://www.useflai.com/) can significantly reduce headcount needs while expanding coverage to 24/7. The[Glendale Infiniti case study](https://www.useflai.com/case-studies/glendale-infiniti) is a useful reference for understanding what an AI-first model can deliver.


**How many people do I need for a dealership BDC?** It depends on your call volume. Use the staffing formula above: (calls per hour x average handle time in minutes) / 60, then divide by 0.75 for a reasonable occupancy buffer. Most mid-size single-point dealerships start with **2 to 4 reps** and a part-time or full-time manager.


**Should my BDC handle both sales and service?** Yes, but with separate workflows. Sales and service have different scripts, different metrics, and different customer expectations. A single team can handle both, but the processes need to be distinct. Some larger dealerships run completely separate sales and service BDC teams. Understanding[what is fixed operations in a dealership](https://www.useflai.com/blog/what-is-fixed-operations-dealership-guide) helps clarify why the service side has unique requirements that a sales-focused team often underestimates.


**What’s a good appointment show rate for a dealership BDC?** Show rates vary by department and lead source, but most well-run BDCs target **60% to 70%** for sales appointments and **75% to 85%** for service appointments. The key to improving show rate is a solid confirmation and reminder system, not just setting more appointments.


**How long does it take to get a BDC up and running?** Plan for **90 days** to reach full operation: two weeks for data gathering and process design, four weeks for hiring and tech setup, two weeks for a controlled pilot, and the final month for scaling and optimization. Rushing means skipping QA and coaching, which leads to poor performance and early burnout.


**Can AI replace a traditional BDC entirely?** Not yet, and that’s not the right framing. AI handles structured, high-volume work well: after-hours calls, overflow, scheduling, confirmations, and reminders. Humans remain essential for complex negotiations, emotional conversations, and anything requiring real judgment. The hybrid model wins. Our[AI for car dealerships guide](https://www.useflai.com/blog/ai-for-car-dealerships-guide) explains the full range of what AI can handle and where humans remain essential.


**What’s the biggest mistake dealerships make when setting up a BDC?** Measuring activity instead of outcomes. A BDC that tracks “calls made” instead of “appointments that showed” will optimize for volume over value. Tie your compensation, coaching, and reporting to the metrics that actually generate revenue: kept appointments, show rates, and revenue per appointment.


**Do I need a dedicated BDC manager?** Yes. A BDC without a dedicated manager becomes a political orphan. Someone needs to own the scripts, coach the reps, manage the schedule, track the KPIs, and fight for the department’s resources. In the early days this can be a team lead who also handles calls, but the management responsibility needs to sit with one person.
