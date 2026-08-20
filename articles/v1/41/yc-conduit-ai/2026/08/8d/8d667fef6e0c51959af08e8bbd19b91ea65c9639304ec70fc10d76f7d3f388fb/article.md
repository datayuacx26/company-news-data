---
schema_version: "1.0.0"
document_id: "8d667fef6e0c51959af08e8bbd19b91ea65c9639304ec70fc10d76f7d3f388fb"
company_key: "yc-conduit-ai"
company: "Conduit"
source_id: "yc-conduit-ai-news-import-d342c7e506de"
canonical_url: "https://www.conduit.ai/blog/what-10-million-guest-messages-are-about"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-04T03:06:46.928467+00:00"
fetched_at: "2026-08-04T05:15:07.724576+00:00"
content_hash: "sha256:01b87bd0f036bae4ab6cee8a1f095d74a5b6a5959985bf00207d0bcab6d8d862"
---

# What guests ask, across 10 million messages

We run guest messaging for tens of thousands of properties — short-term rentals and boutique hotels. That means a lot of messages pass through us, and we had never sat down and read them in aggregate. So we did.


The numbers here come from the short-term-rental side of that book, which is where the volume sits: a year of inbound guest messages, roughly 10 million of them, across 68,625 active properties in 78 countries, between August 2025 and July 2026. Everything below is aggregate. No individual message, guest or operator is identified anywhere in it.


We did not decide the categories in advance. We embedded a random sample of the messages, clustered them into 140 tight groups, read the groups, and rolled them up into topics from what was there. Then we labelled every message in the sample against that taxonomy. The point was to find out what guests ask about, not to confirm what we assumed.


Six things came out of it.


## 1. What guests ask about barely changed


We expected drift. A year is long enough for booking behaviour to shift and for phrasing to move.


It didn't. We tested every word appearing in at least 120 guest messages for a change in prevalence between the first quarter of the window and the last. Out of 454 words, 10 moved by more than 3.5 standard deviations. Of the topics, only a handful moved by more than sampling noise, and the mix at the top was flat all year.


The largest single shift turned out not to be guests at all.


## 2. The biggest change was a Booking.com template


In February 2026, one phrase went from never appearing to showing up in roughly 0.6% of every inbound message, overnight:


> "I'd like to request check-in at 15:00 - 16:00. Is this ok?"


That is Booking.com. They started injecting a canned check-in-time request into guest threads. It arrives formatted as a guest message and reads as one.


It matters because it was the only topic that looked like it was growing. Early check-in requests rose 0.8 percentage points over the year with the canned message counted, and 0.3 points — inside the noise — without it. Guests are not asking for early check-in more often. Booking.com is asking on their behalf, about 35,000 times a year at our volume.


We only caught it because of a check we almost skipped. Alongside the model-labelled sample we ran plain keyword counts over the entire message base, as an independent second opinion. The keyword probe refused to confirm the early check-in trend. Chasing that disagreement surfaced the template. If we had trusted the model labels alone we would have published a guest behaviour trend that was a Booking.com release note.


## 3. Seasons are reversed in the southern hemisphere


Obvious in hindsight, and still satisfying to see in the data.


Mentions of heating peak in December for properties in the northern hemisphere and in June for properties in the southern hemisphere. Both swing about 5.8× between their high and low month. Air conditioning peaks in July up north and January down south. Snow and ski swing 17× and 13× respectively, in opposite halves of the year.


Subject Northern peak Southern peak Swing


Heating December June 5.8× / 5.7×


Air conditioning July January 5.0× / 2.9×


"too cold" January July 7.2× / 9.2×


Snow and ski January July 17.1× / 13.0×


WiFi *(control)* — — 1.3× / 1.2×


The last row is the important one. We ran four subjects with no reason to be seasonal — WiFi, parking, door codes, luggage — as controls. They stay flat, between 1.2× and 1.5× across the whole year, which rules out the seasonal curves being an artefact of our customer mix changing month to month. Without flat controls the rest would be a guess.


Practically: an operator with properties in both hemispheres has a support load that never gets a quiet season, it just changes subject. And a seasonal knowledge base that is correct in July is wrong in January.


## 4. Message volume tracks property count


The United States sends more guest messages than anywhere else. It also has 35,586 of the 68,625 active properties in this dataset — 52% of the footprint. Those two facts are the same fact.


Divide volume by properties and the ranking inverts:


Messages per property per month


Argentina 52.5


Malaysia 45.5


Brazil 43.1


United Kingdom 18.5


**Global average** **14.1**


Australia 10.4


United States 9.6


US and Australian properties are among the quietest in the dataset, the US at 0.68× the global average and Australia at 0.74×. Argentinian, Brazilian and continental European properties generate three to four times as much guest conversation each.


A note on the denominator, because it matters: this divides by actively billed listings, not by everything a PMS has ever synced to us. Only about two thirds of synced listings are live, and that share ranges from roughly a third to over 90% depending on the country — so using the synced count would have distorted the comparison, not just the level.


Occupancy, average stay length, channel mix and local norms about contacting a host all feed into that gap. The number settles something narrower and more useful: a ranking built on raw volume is a ranking of your own footprint, and the per-property rate is the one that should drive staffing.


## 5. Five topics cover half of what guests ask


Set aside one-line acknowledgements, which are replies rather than requests, and what remains concentrates hard. Five topics cover 54% of every question guests ask:


1. Arrival and departure logistics — 17.3%
2. Booking and availability — 10.3%
3. Property and area information — 9.0%
4. Access and entry — 8.6%
5. Verification and registration — 8.5%


Access and entry is the one to watch. 42% of those messages report a problem: a code that doesn't work, or a guest standing outside a door at 11pm. It carries the highest stakes of the five, and it clusters hard on arrival day.


## 6. Guests do not read the check-in message


Every operator sets up a templated pre-arrival message: the code, the wifi, the parking, the arrival window. 908,000 threads got one over the year. So we counted how often the guest asked for something that was already in it.


Counting only threads where the information went out unprompted, before the guest had asked anything, 27.4% of the guests who asked for a door code had already been given it. Check-in time runs at 20.8% and the wifi password at 18.4%. Guests who asked, got an answer and then asked again are excluded; this is the first time they asked.


It is a floor rather than a ceiling. We can only see it when the information was typed into a message body. Guidebook links, PDFs and templates the PMS renders on its own are invisible to this count, and those went unread too.


Timing is most of the explanation. The message goes out days before the information is needed, and the question arrives on check-in day, which carries 24% of everything a guest writes across the whole booking. On that day 1 in 10 guest messages is about getting through a door, and 45% of those report that it is not working.


That is an argument about static documentation in general, not about one template. A guest who will not scroll back through their own phone to find a code they were sent three days ago is not going to open the house manual. The information was correct and complete. It arrived three days early, and at the moment it mattered it had to be found rather than delivered.


So the answer has to meet the question when it arrives, rather than sitting in a document nobody was going to open. Templates still earn their place for the predictable ground, and we have a[setup guide for automated guest messages](https://www.conduit.ai/blog/airbnb-automated-messages-how-to-automate-your-guest-communication) if you need one. They just are not the whole answer.


## How we measured this


Seasonal figures are computed on every message in the window rather than a sample, so they carry no sampling error. Topic figures come from a random 1-in-250 sample of about 70,000 messages, labelled against the taxonomy. We validated that sample against the full message base on four independent keyword probes and it matches to within 0.15 percentage points.


Trends are read on three-month blocks and tested against standard error before we call anything a change, and seasonality is measured on the full base rather than the sample. Those two rules caught the Booking.com template instead of publishing it as a guest trend.


The check-in message figures come from a separate pass over outbound messages across the full population, matching each guest question against whether the operator had already sent that information earlier in the same thread.


Counts are normalised to a round 10-million-message base; shares, rates and significance tests are unaffected. About 11% of what lands in these inboxes is not written by a guest at all — OTA notifications, payout notices, review reminders, vendor alerts — and is excluded from the guest-only percentages above.


---


If there is a theme, it is that guest questions are far more stable and far more boring than anyone building for this market wants them to be. The same handful of things, asked in the same proportions, all year, everywhere. What changes is the weather, the vendor templates, and how many properties you have.


That is good news if you are building systems to answer them. If you are choosing one, we keep a running comparison of[guest communication tools](https://www.conduit.ai/blog/guest-communication-tools) .
