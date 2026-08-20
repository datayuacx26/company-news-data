---
schema_version: "1.0.0"
document_id: "6cd4098825d6954d7b4cc3a1313768ee3518b74d4d12c6bb818297ff0f125ea7"
company_key: "yc-freshpaint"
company: "Freshpaint"
source_id: "yc-freshpaint-news-import-105ad9035257"
canonical_url: "https://www.freshpaint.io/blog/how-orthocarolina-integrated-epic-data-into-our-marketing-technology-stack"
published_at: "2026-07-21T19:56:56.021+00:00"
first_seen_at: "2026-07-24T09:09:59.893678+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:8fdf7492a62b5cbfd4ae9f2398fbf6a0ee7b3f1513ca84a226645f2c1e4e8477"
---

# How OrthoCarolina Integrated Epic Data into Our Marketing Technology Stack (Without Losing Our Minds)

If you want the full story of how OrthoCarolina dropped cost per lead by 80% and turned compliance into a growth engine, I’m happy to share that that write-up is[available for your reading pleasure](https://www.freshpaint.io/case-studies/orthocarolina) .


This ** is ** not ** that ** story *.* This ** is the narrower, messier sequel: **how my multi-location orthopedic practice succeeded in wiring Epic (MyChart) into our marketing technology stack in 90 days, so we can now reliably measure revenue from marketing activity** . **


As Freshpaint’s recent[State of Healthcare Marketing 2026 report](https://www.freshpaint.io/ebooks-reports-and-more/the-state-of-healthcare-marketing) finds, only 37% of healthcare marketing leaders say their executives view marketing as a “strategic growth driver”; related, only 1% of marketers can connect over half their spend to outcomes.


Connecting marketing tactics to bottom-funnel outcomes is the ultimate lever to change that dynamic. Below, I’m covering:


- What worked well in our Epic integration journey
- What didn’t work as easily
- What I’d do differently if I were starting from scratch, and
- The lessons you can take forward in enacting an Epic integration at your own healthcare organization.


##### ***Check out the***[recording of the webinar](https://info.freshpaint.io/freshpaint-webinar-how-to-safely-optimize-ads-from-appointment-data-in-epic) ***where I joined Freshpaint experts to cover the integration of Epic into OrthoCarolina’s marketing technology stack in depth.***


## Where Our Story Starts Today: With Ad Platforms Optimizing for Booked Appointments, EHR Integration Was the Next Hill to Climb


At OrthoCarolina, our footprint is big enough that we effectively function as the “third system” in our market (based in Charlotte, North Carolina), behind two major hospital systems. That visibility is great for growth and not so great for inconsistent governance.


To solve for that, we’d already:


- Put a[HIPAA‑safe tracking foundation](https://www.freshpaint.io/ad-performance) in place with Freshpaint.
- Re‑connected downstream signals like booked appointments with the ad platforms, driving unified, cross-channel measurement.
- Proved that compliance and performance are not in conflict. In our case, they accelerated each other, unlocking data-driven geographic expansion strategy, operational improvements that improved booking efficiency, and many more growth outcomes.


While booked appointments are helpful, it’s fair to say that arrived appointments pay the bills. **The obvious next frontier for us was to tie marketing investment directly to arrived appointments within our EHR, Epic.**


With a 9-16% no‑show rate, even small improvements in arrivals at OrthoCarolina could translate into meaningful incremental revenue without increasing ad spend.


So the question became: “How do we get *just enough* Epic data out to optimize marketing, without turning this into a three‑year IT science project?”


That’s the problem we set out to solve.


## Step 1: Reframing Epic as a revenue and risk project


If you pitch Epic integration at your organization as “more granular marketing attribution,” it will die somewhere between legal and IT.


We framed it instead around two things:


1. **Risk:** As a highly visible, physician‑owned practice in our market, we’re an easy target for regulatory scrutiny. We’d already moved to a privacy‑first tracking foundation precisely to avoid becoming a cautionary tale.
2. **Revenue:** We were already using Freshpaint to tie booked appointments back to channels and improve CPL by 80%. The next logical lever was to optimize on **arrived** visits and show rates — something only Epic could tell us.


That framing changed the stakeholder conversation:


- **Legal & compliance** saw Epic integration as an extension of the same risk‑reduction work we’d already done, not as a workaround.
- **Physician leadership** understood this as a way to protect and grow revenue per visit, rather than an exercise in chasing more leads.
- **IT and analytics** saw a bounded, enterprise data‑sharing problem, not an open‑ended request for “all the data.”


Only after we had that coalition and aligned understanding in place did we get serious about the “how.”


## Step 2: Getting Epic, OrthoCarolina, and Freshpaint on the Same Page


Epic may seem like a closed ecosystem. For example, their documents live behind customer access and vendors don’t get free rein to poke around. For months, most conversations in the market about “Epic + ad performance” seemed to end with some version of: *“We’ve tried this. It’s not possible.”*


What changed for us wasn’t a new product feature; it was getting **all three parties on the same call:** Our Epic analysts, our internal web and growth team, and Freshpaint engineering.


On that joint call, Epic did something important: they pointed us to a specific hook that already existed in **MyChart** , including the query parameter on MyChart URLs.


That gave us a home for a Freshpaint identifier that could:


1. Ride along from our public site into MyChart.
2. Survive login and scheduling flows.
3. Re‑appear in an Epic report we could safely export.


Once that pattern clicked, the integration stopped resembling magic and started looking more like plumbing.


## Step 3: Wiring MyChart into Freshpaint (Without Touching Epic code)


The next phase was a three‑link chain.


### Link 1: Tagging MyChart links on our site


On orthocarolina.com, any “Schedule in MyChart” button points to a MyChart domain.


Freshpaint shipped us a small script that our web team dropped onto the site. Its job was simple:


- Watch for clicks on tags whose hostname matches our MyChart domains.
- Read the Freshpaint device ID from the browser.
- Append that device ID into the Epic query parameter before redirect.


Conceptually:


1. The user clicks a Google ad.
2. Lands on our site (Freshpaint sees the click, assigns a device ID).
3. Clicks a MyChart scheduling link.
4. Script adds the device ID into Epic.
5. MyChart passes that value through into Epic’s Clarity tables.


No code inside Epic and no invasive SDKs — just URLs and a field Epic already supports.


### Link 2: Getting the right appointments *out* of Epic


Freshpaint and our Epic analysts found the specific data sources in Epic where the query lands. From that, a recurring export was built, to include the identifier we used, appointment ID and timestamps, and basic metadata (location, service line, status: booked/attended/canceled).


Security and data governance were non‑negotiable. The steps we hardwired:


- Files landed on a secure SFTP or portal that Freshpaint could access programmatically (not via a human logging in and dragging CSVs around).
- Nothing flowed *back* into Epic. The data only moved one way, in a format legal and compliance had signed off on.


Now we get to the part where a lot of teams get stuck. “Two weeks of work” for an integration can easily span:


- Web / MyChart configuration
- EHR analysts
- Reporting developers
- InfoSec reviews


What helped us was treating this as a **joint project with Freshpaint** , not just an internal IT ticket. When our Epic team had questions, Freshpaint engineering was there to answer them live, which ensured our team had immediate access to subject matter expertise and undoubtedly accelerated the process by days, if not weeks.


### Link 3: Joining the dots and pushing “attended” back into ad platforms


Once those exports started landing, Freshpaint:


- Joined the Epic files back to:


- Freshpaint’s own event stream (page views, clicks, call events)
- Ad platform click IDs and campaign data


- Created conversion events that demonstrated appointments booked and attended, based on Epic fields.
- Sent those events back into Google and Meta as optimization goals, not just reporting metrics.


From this point on, the Epic integration wasn’t just a reporting feed; it was actively shaping how platforms spent our money.


## Where We Underestimated the Work


I wish I could say it was all smooth once the plumbing was in, but there were three big areas where we had to recalibrate expectations.


### 1. Scheduling UX was a bigger constraint than Epic


As we started to see more of the funnel, it became obvious that **our own scheduling experience** was a major drag on performance:


- Only ~25% of our appointments are booked online. Our target is 55-65%.
- The current flows inside Epic, shaped by years of complex provider preferences, required 11-13 clicks, whereas competitors’ took 2-3.


An Epic integration into our martech stack wouldn’t fix that. So we went back to physician leadership and got approval to **fully rebuild** our scheduling system on top of Epic, with online booking share and show rates as explicit success metrics.


In hindsight, I would have paired “Epic + Freshpaint” and “scheduling UX overhaul” as a single program from day one.


### 2. Replicability across Epic flavors isn’t guaranteed (yet)


We and Freshpaint are very bullish; everything we’ve seen suggests the field patterns are repeatable for **Epic + MyChart open scheduling** installs.


But we’ve also learned to be candid about what we still don’t know:


- Hosted vs. on‑prem Epic environments may behave differently.
- Some organizations rely more on authenticated MyChart flows than on public open scheduling links.
- Collaboration and understanding is key; not every Epic team has the same comfort level with extracting and sharing these fields.


Today, my mental model is: *The pattern is real, but it’s still in “design‑partner” mode, not a shrink‑wrapped implementation guide.*


This is an exciting place to sit if you’re willing to be an early mover and test and learn. It’s frustrating if you’re expecting a PDF you can just hand to IT.


### 3. The narrative around experiments matters as much as the data


There’s one more lesson we learned, and it’s more about **communication** than code.


When you run early Epic experiments, the data will be noisy: Volumes are small, scheduling flows are in flux, and not all sources (walk‑ins, phone, third‑party call tracking) are equally represented.


On Freshpaint’s side, engineering lead Andrew Lai was explicit about this risk: unless we actively manage the narrative, someone else will interpret the experiment for us, and not necessarily in our favor. We were mindful that early tests be treated as trials, so that the conclusion didn’t become, “Epic integration didn’t move the needle. The product failed.”


Internally at OrthoCarolina, that meant:


- Treating the first 3-6 months as a *beta* , not a finished feature.
- Making sure our execs understood what we were testing and what a successful test looked like, even if the metric movement was modest.
- De‑briefing results to project team members jointly with Freshpaint, instead of in separate rooms.


If I could rewind, I’d start that expectation‑setting before the first Epic file ever landed.


## The Practical Epic Playbook I’d Hand Another Marketer


Here’s how I’d distill our experience for another marketing leader who wants the Epic upside.


### 1. Anchor the project on **arrived appointments**


Make your north star unambiguous: Move from “cost per lead” to “cost per **arrived** appointment,” then eventually to **revenue per channel** .


Frame every stakeholder conversation, including Epic’s, around that outcome. It’s easier to collaborate and drive alignment toward a metric everyone already cares about than on abstract “attribution.”


### 2. Build a lightweight, cross‑functional Epic squad


You don’t need a 40‑person steering committee, but you *do* need a small group who knows they’re co‑owning the experiment. Before involving your vendor, map:


- Legal / compliance
- CIO / Epic leadership
- Analytics / reporting
- Growth / marketing


Get them to agree to:


- A narrow first use case (e.g., one service line, open scheduling only).
- A definition of success that includes **learning** , not just lift.


### 3. Use existing Epic hooks first, not custom builds


Ask your Epic team (or your vendor, with Epic on the call) very specific questions:


- “Can we pass an external identifier into MyChart via an existing Epic hook?”
- “Where does that land in Clarity for our install?”
- “Can we get a recurring export of that field plus basic appointment outcomes?”


Lean on fields Epic already supports, and don't be afraid to rely on their expertise, before you even think about deeper customization.


### 4. Keep the first file dumb and safe


For your first export, resist the temptation to boil the ocean. Aim for:


- One identifier
- Appointment ID and date/time
- Basic status (booked / attended / canceled)
- A small handful of non‑sensitive fields (location, high‑level service line)


You can always add more columns later. Getting **any** file flowing and validated is the real milestone.


### 5. Pair Epic work with a ruthless look at scheduling UX and call flows


Don’t let Epic integration be a distraction from more obvious friction points, such as:


- How many steps does it take to book online?
- What percent of volume is booked via phone‑only?
- Is your current call tracking stack going to survive an Epic‑centric world?


We discovered that our biggest gains would come from improving the **plumbing into Epic** (scheduling, call routing) at least as much as from pulling more data **out** of it.


### 6. Treat your first 6-12 months as design‑partner R&D


Finally, be honest with yourself and your org: this is still emerging territory. That doesn’t mean you wait. It means you:


- Choose a vendor who’s willing to be in the trenches with you — on calls with Epic, with your analysts, and with your execs. In our case, an Epic integration would never have happened without Freshpaint, who I look to as more of a partner than a vendor.
- Socialize early that some experiments will underwhelm, but each one buys you pattern recognition you can’t get any other way.


For us, that partnership mentality has mattered as much as any individual feature. The transition from seeing Freshpaint as “just” a compliance vendor to a[true performance partner](https://www.freshpaint.io/customers) , with epic muscle behind it (pardon the pun, it had to be done), has been critically important.


## Where We Go From Here


With Epic data now flowing through Freshpaint and into OrthoCarolina’s marketing tech stack, we are now in the early days of:


- Closing the gap in our no-show rate,
- Improving visibility into booked appointment patterns, and
- Measuring associated incremental revenue increases.


As you can see from my experience, Epic integration is not a one‑click toggle. It’s a multi‑stakeholder negotiation wrapped around a very specific piece of plumbing.


Done well, though, it’s one of the few projects that can legitimately move you **from CPL to revenue per channel** , and from “marketing asks for budget” to “marketing is driving revenue by decreasing our no-show rate.”


That’s a hill worth climbing.


‍


#### **Go deeper:**


##### • Want to integrate your Epic data into your martech stack? Freshpaint is ready to help –[book a call with the Freshpaint team](https://www.freshpaint.io/contact-sales) .


##### • Hear Blair speak to the[hidden cost of last-click thinking](https://www.youtube.com/watch?v=YJzuiqwzm38&list=PLvqxNqG_W-TtewKDPKUw238TCZmxRf8D5&index=12&t=405s) on our *Marketing Rounds* podcast.


##### • Check out how the Freshpaint platform enables you to[lower your CPL and CPC](https://www.freshpaint.io/improve-marketing-performance) without raising spend.
