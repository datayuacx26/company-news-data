---
schema_version: "1.0.0"
document_id: "e9bd3e39ca1ec7d7bf8a885ffe77dc8b5d6641b7373d62fdfb7899dd66c345da"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/reservation-hijack-scams-target-travelers"
published_at: "2026-05-28T18:52:41+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:9bfa23bbc5afa64512aea7e5441ca76b4f7f4f012852b3c14df7e031371fb4b4"
---

# When Hotel Scams Know Your Booking: 350 Compromised Accommodations Across 50 Countries

By[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


,[Martin Chlumecký](https://www.gendigital.com/blog/authors/martin-chlumecky)


When we published the first part of this research, we said the investigation was still ongoing. That was not a closing line. We were already looking at the infrastructure, the signals behind the attacks, and the scale of the reservation data abuse we were seeing in our telemetry.


Then something else happened.


Friends, colleagues, and other contacts started forwarding us messages they had received from Booking.com. The messages warned that unauthorized third parties may have accessed information linked to their reservations. Shortly after, media outlets began reporting the same thing: Booking.com had notified affected customers, reset reservation PINs, and said the accessed information could include names, email addresses, phone numbers, booking details, and information shared with accommodations. Booking.com said financial information was not accessed from its systems.


The timing mattered, but not because it started our investigation. It mattered because it publicly confirmed the kind of reservation-data exposure our first article had described from the attack side.


[Reservation Hijack scams](https://us.norton.com/blog/online-scams/reservation-hijacking-scam) work because criminals do not have to guess. A traveler receives a message about a real booking. The message contains details that make sense: the right accommodation, the right trip context, sometimes the right contact information. The link then opens a fraudulent page built around the same accommodation.


A generic[phishing message](https://us.norton.com/blog/online-scams/what-is-phishing) has to create trust from nothing. This scam steps into a relationship that already exists between the traveler, the booking platform, and the accommodation.


While those public warnings were appearing, Gen Threat Labs continued canvassing the data we had available: blocked URLs, fraudulent landing pages, SMS messages, emails, WhatsApp messages, in-app message evidence, and other signals connected to attacks we had already stopped. We used AI-assisted analysis to connect pieces that would otherwise remain scattered: messages, landing pages, accommodation names, URLs, templates, and infrastructure patterns. Human analysts reviewed the results and separated what we could prove from what we could only suspect.


The result is a much clearer picture.


We identified more than **350 compromised accommodations** linked to the Reservation Hijack scam flows, spread across **50 countries** . These were not abstract indicators or generic travel-themed pages. In the cases we analyzed, victims had already received reservation-specific information before being pushed to a fraudulent page customized around the accommodation tied to that booking.


That is the difference between ordinary travel phishing and this model. The attackers are not guessing who is traveling. They already have enough reservation context to make the fraud feel normal.


And they are doing it at scale.


Key figures from Gen Threat Labs’ analysis of compromised accommodations linked to intercepted Reservation Hijack attacks.


### How we connected the evidence


The second phase of our investigation was already underway when Booking.com’s customer notifications started circulating publicly.


Gen’s view of this activity comes from attacks we blocked before they could complete. No single signal tells the whole story. An SMS or WhatsApp message may show the[social engineering](https://us.norton.com/blog/emerging-threats/what-is-social-engineering) . A URL may show the redirect path. A landing page may show the accommodation being impersonated. An in-app message may show where the attacker entered the trusted communication flow. A repeated page structure may link incidents that look separate at first.


Put together, those pieces show how reservation data moves from compromise to fraud.


AI helped us work through that evidence at scale. We used it to normalize accommodation names, cluster similar messages, compare landing-page structures, connect URLs with page content, and flag repeated patterns across channels. This was not “AI says these attacks are connected.” It was AI-assisted triage followed by human review.


From that process, we extracted and cleaned accommodation names from captured fraudulent landing pages and identified **350 compromised accommodations** .


We use “compromised accommodations” carefully here. It means reservation context tied to those accommodations was compromised and used in an attack flow we intercepted. The source of compromise may vary: a hotel account, a booking platform partner account, an email inbox, a property management system, a guest messaging workflow, or a third-party service provider.


The end result for the traveler is the same. Criminals had access to information that should have stayed inside the booking relationship.


This is also a conservative number. It only includes attacks where we had enough evidence to connect the flow to a specific accommodation. It does not include people who received a message and never clicked, attacks outside our telemetry, landing pages we did not capture, or cases where the accommodation name could not be reliably extracted.


So 350 is not the size of the problem. It is the part we could see clearly.


### From scam pattern to repeatable operation


A single compromised hotel account can explain a few victims. A copied phishing kit can explain a short campaign. Hundreds of accommodation-specific scam flows across 50 countries point to something more organized.


The model we see is consistent.


Reservation context enters the fraud chain, sometimes through compromised accommodation-side accounts, inboxes, partner tools, or data obtained from other criminal actors. That context is then used to contact travelers with details that make the message feel legitimate.


The wording, the property, the country change. The logic does not.


That is why this follow-up is not simply “we found more cases.” The first article explained[how Reservation Hijack scams work](https://www.gendigital.com/blog/insights/research/reservation-hijack-scam) . This one shows how far the model has spread.


### A worldwide footprint, led by Europe


The highest number of compromised accommodations in our dataset were linked to **Germany** , followed by **France** , the **United Kingdom** , **Italy** , **Spain** , and the **United States** .


Germany accounted for 49 compromised accommodations in the dataset. France followed with 35, the United Kingdom with 31, Italy with 24, Spain with 20, and the United States with 19.


Together, the top five European countries, Germany, France, the United Kingdom, Italy, and Spain, accounted for **159 compromised accommodations** , about **45%** of the full dataset.


Germany, France and the United Kingdom led the dataset, while the top five European countries accounted for about 45% of all compromised accommodations identified.


The European concentration makes sense. Europe has a dense travel market, a high volume of cross-border bookings, many independent and mid-sized accommodation providers, and a strong dependency on online booking platforms and pre-arrival guest messaging.


Those conditions give criminals useful cover. Travelers expect messages before arrival. They expect payment questions, check-in instructions, policy reminders, and booking updates. A well-timed fake message does not feel unusual.


But the country distribution also shows that this is not a single-region problem. The activity spreads across **50 countries** , with a long tail of destinations beyond the largest clusters.


The largest six country categories account for just over half of the dataset, while the rest of the countries together account for nearly the other half.


That long tail matters. It suggests the model is not limited to a few obvious tourist destinations or a handful of famous hotel brands. Once criminals have access to reservation data and a working scam flow, the same approach can be adapted to different countries, languages, property types, and booking habits.


The property does not need to be famous. It only needs to be real, tied to a real reservation, and recognizable to the victim.


### Not just hotels


Hotels make up the largest share of the dataset, but they are not the whole story.


We identified compromised accommodations across several property types: hotels, aparthotels, apartments, inns, motels, lodges, resorts, guesthouses, B&Bs, hostels, villas, bungalows, and capsule hotels.


Hotels represented the majority of compromised accommodations, but more than a quarter of the dataset involved other accommodation types, including apartments, resorts, guesthouses, hostels and villas.


This is a useful reminder of how fragmented the travel ecosystem really is. A traveler may book a large hotel chain, a family-run guesthouse, a serviced apartment, a hostel, or a villa through similar platforms and messaging flows.


From the traveler’s point of view, the trust model is almost the same. They made a booking. They expect communication. They may not know whether the message came from the property, the platform, a channel manager, a property management system, or a compromised account sitting somewhere in between.


That is exactly the gap criminals are abusing.


### How big could this problem be?


To understand the potential scale of Reservation Hijack scams, we looked at the accommodation properties appearing in our research dataset.


In total, we identified **350 accommodation entries** linked to reservation hijack scam activity. These were not all the same kind of business. The dataset included hotels, aparthotels, resorts, hostels, guesthouses, villas, apartments and other accommodation types. For each one, we estimated the number of rooms, units, beds or capsules, depending on the property type, and used that to calculate the likely maximum guest capacity.


Across the deduplicated dataset, these properties represented an estimated maximum capacity of around **82,000 guests at any given time** . Using a conservative occupancy rate of **50%** and an average length of stay of **2.5 nights** , this translates into approximately **6 million guest stays per year** .


That does not mean 6 million people were scammed. It does not mean 6 million people had their data exposed. What it shows is the size of the opportunity criminals may be looking at when they compromise accommodation or booking communication channels.


A single compromised workflow can give scammers something much more valuable than a random phone number or email address. It can give them context: the name of the accommodation, the dates of the stay, the booking amount, the channel the guest expects to be contacted through, and the right moment to create urgency.


That is why Reservation Hijack scams are so convincing. The scam does not feel like spam. It feels like part of the trip.


### The activity does not look random


We are still investigating the infrastructure behind these attacks, so we are not attributing the activity to a named group.


But the evidence does not look like hundreds of unrelated actors randomly copying one another.


Across the dataset we analyzed, the same fingerprints kept appearing. Many pages used the same internal paths for scripts, images and styles. Several included the same unusual overlay component, repeated internal asset paths, and the same card-validation language, including claims that funds would be reserved and refunded within 10 minutes.


The pages were also built to look specific to the victim. Hotel names appeared in page titles and visible content. Prices varied from one case to another. Check-in and check-out dates were inserted into the page. In some cases, page metadata and images were synchronized with the accommodation being impersonated. The evidence points to a repeatable process for turning reservation context into property-specific phishing pages. Taken together, those patterns are consistent with a phishing kit designed to generate accommodation-specific pages at scale, rather than pages built manually for each victim.


The phishing pages also included a fake live support chat. To the victim, it looked like part of the booking or payment process, but it was controlled from the phishing infrastructure. The payment step could therefore become a live interaction: someone behind the page could react to the victim’s input, ask for another card, explain away a failed “validation”, push the victim to continue, or try to capture a one-time code while the victim still believed they were talking to booking support.


Some of the infrastructure also uses legitimate internet services, including services such as Cloudflare, to place a layer between the visible scam page and the systems behind it. That does not make those services malicious. It means criminals are abusing the same infrastructure that normal websites use, including protective layers that can obscure hosting details, traffic paths, and backend systems.


For investigators, this kind of setup makes the infrastructure harder to trace. For travelers, it removes many of the usual warning signs. They see a page that loads normally, shows the right accommodation, includes a support chat, and refers to the trip they actually booked. At that point, many people will treat the request as part of the reservation process, even when it is not.


### Correct information is not proof of legitimacy


For years, people have been told to watch for bad spelling, odd sender names, generic greetings, and suspicious links. That advice still helps, but it does not solve this problem.


A message can include the right accommodation, the right destination, and details from a real stay, and still be malicious. A page can show the correct hotel name, dates, and a plausible payment amount, and still be fake. A request can arrive at the moment when a traveler expects a check-in or payment message, and still be fraud.


Page behavior can create false confidence too. In some of the phishing pages we analyzed, the fake support chat looked like part of the booking or payment process. If the victim had a question, or if the card “validation” failed, the page could keep the interaction going instead of simply collecting data and stopping there.


That is what makes Reservation Hijack scams difficult for travelers to judge. The victim is not being tricked only by a fake brand or a fake website. They are being pushed through a process built around real context, at the moment when that context feels normal.


Travelers expect hotels and booking platforms to contact them. They expect last-minute requests, check-in instructions, payment checks, ID forms, and confirmation messages. Criminals are not fighting that expectation. They are using it.


### What travelers should do


The safest rule is simple: **do not treat correct booking information as proof that a message is legitimate.**


If a message asks you to verify payment details, confirm a reservation, re-enter card information, pay a balance, reset a PIN, or avoid cancellation, do not use the[link in the message](https://us.norton.com/blog/online-scams/i-clicked-on-a-phishing-link) .


Open the booking platform or hotel app directly. Go to the official website yourself. Use contact details from the original booking confirmation. If needed, call the property using a verified number. Check whether the same request appears inside your official booking account.


This applies even if the message mentions the right hotel, the right destination, or details from your stay.


Booking.com has told customers that it will not ask guests to share credit card details by email, phone, WhatsApp, or text, or ask for a bank transfer that differs from the payment policy in the booking confirmation.


If you already entered payment details into a suspicious page, contact your bank immediately, freeze or cancel the card, and monitor for follow-on fraud. Criminals may reuse the card data later, or sell it to others.


### What accommodations should take from this


For hotels and other accommodation providers, this is not only a guest-awareness issue. Guest communication has become part of the attack surface.


The systems and workflows between the property and the guest are now valuable targets. That can include hotel staff accounts, booking platform partner accounts, email inboxes, property management systems, guest messaging tools, channel managers, and third-party service providers.


The lures we have seen on the accommodation side are not exotic. They look like normal hotel work. Some impersonate Booking.com and claim there is a guest complaint waiting for a response. Others say that a paid reservation needs urgent verification, or that the property’s visibility or ranking may be affected if staff do not act quickly. We have also seen payment and invoice-themed messages designed to make staff open attachments or documents that can lead to credential theft or[malware infection](https://us.norton.com/blog/malware/signs-of-malware) .


That is why the first visible sign of a Reservation Hijack scam may not be the message sent to the guest. It may start earlier, with an email to a shared hotel inbox, a fake partner-support notice, a malicious attachment, or a document that looks like part of normal accounting or reservation handling. Once reservation context is obtained, whether directly or through another actor in the criminal ecosystem, it can be used later to make guest-facing scams much more convincing.


For accommodation providers, staff training needs to cover the workflows criminals are actually abusing. Urgent complaints, reservation-verification requests, payment advice emails, invoice notifications, security-update prompts, and any message asking staff to open an attachment, enable content, sign in again, or act under time pressure should all be treated as possible entry points into the same fraud chain.


The defensive work starts with account protection, but it should not stop there. Accommodation providers need strong[multi-factor authentication](https://us.norton.com/blog/account-safety/multi-factor-authentication) , tight control over who can access guest data, monitoring for unusual logins or message patterns, and a clear process for responding when an account may have been abused. The devices used to manage reservations, guest messages, invoices and payment-related emails should also be protected with reliable security protection. Some of the lures we observed were designed to make staff open attachments or documents that could install malware or steal credentials. In those cases,[antivirus](https://us.norton.com/antivirus) or endpoint protection may be the last barrier before a compromised inbox becomes a compromised booking workflow.


Payment-related communication deserves special attention. If a guest receives a payment or verification request, there should be a reliable way to confirm whether it is real. If an account is suspected of being compromised, guests need to be warned quickly and clearly.


The reputational damage can be painful. Even when the property is also a victim, the traveler may remember the incident as “the hotel message that stole my card details.” That makes workflow security part of customer trust.


### What the wider travel ecosystem should learn


Reservation Hijack scams sit between several parts of the travel industry. The booking platform may see unusual partner activity. The hotel may see confused guests. The traveler sees a convincing message. The bank sees card fraud. A security company sees the landing page.


Each view is partial, and that delay helps the scam continue.


The useful signals are often scattered: unusual partner logins, repeated payment-verification language, guest-message spikes, reused templates, fake landing pages, redirects, and infrastructure overlaps. Sharing those signals faster would make it easier to see when separate complaints are really part of the same operation.


One compromised accommodation can look like a local incident. Hundreds of compromised accommodations across 50 countries point to a repeatable abuse model.


### The next phase of travel fraud


Reservation Hijack scams exploit a simple mistake in how we judge trust online.


Travelers think: “This message knows my booking, so it must be real.”


Criminals know: “If we have the booking context, the victim will do much of the trust work for us.”


The message does not have to be perfect. The website does not have to be flawless. The attacker does not need to convince the victim from zero. They only need to appear inside a relationship that already exists.


Our latest data shows that this model has spread well beyond isolated cases. We identified **350 compromised accommodations across 50 countries** , covering hotels, apartments, hostels, resorts, guesthouses, villas, and other accommodation types. Together, those properties represent a large travel footprint and millions of estimated annual guest interactions.


The investigation is still ongoing. We are continuing to analyze infrastructure, campaign overlaps, and the clues suggesting connected activity behind parts of the operation.


For travelers, the lesson is simple.


Trust your booking. Do not automatically trust the message.


More on this topic


[The Reservation Hijack Scam: How attackers hijack hotel accounts to target guests – From Research](https://www.gendigital.com/blog/insights/research/reservation-hijack-scam)


[The Scam Ad Machine – From Research](https://www.gendigital.com/blog/insights/research/scam-ad-machine-meta)


[Why Join the Navy if You Can Be a Pirate? – From Research](https://www.gendigital.com/blog/insights/research/macos-cracked-software-malware-risks)


[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


Security Evangelist at Gen


At Gen, Luis tracks evolving threats and trends, turning research into actionable safety advice. He has worked in cybersecurity since 1999. He chairs the AMTSO Board and serves on the Board of MUTE.


[Martin Chlumecký](https://www.gendigital.com/blog/authors/martin-chlumecky)


Malware Researcher


Follow us for more
