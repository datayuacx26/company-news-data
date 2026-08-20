---
schema_version: "1.0.0"
document_id: "057551b3b75eb63c0fe91b34d05358f52b85d40e7714133fcefbacad497fee3e"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/insights/research/reservation-hijack-scam"
published_at: "2026-03-25T19:56:08+00:00"
first_seen_at: "2026-07-21T21:24:31.621864+00:00"
fetched_at: "2026-07-28T22:17:49.677260+00:00"
content_hash: "sha256:84d0bebb8144ac3d1f61d4f59e40e29a4c2723551922713df1a35686675644c4"
---

# The Reservation Hijack Scam: How attackers hijack hotel accounts to target guests

By[Martin Chlumecký](https://www.gendigital.com/blog/authors/martin-chlumecky)


,[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


It starts with a message that does not feel wrong.


You have a family trip coming up. You made the booking days or weeks ago, saved the confirmation and moved on with your life. Then WhatsApp pings.


The message says it is from the hotel’s Guest Relations team. It references the same property you booked. It mentions details that match your stay. It sounds like the kind of practical note you might expect before a trip, a small inconsistency, a quick verification, a routine check before arrival.


And that is exactly why it works.


When a scam contains real details, it stops feeling like spam and starts feeling like customer service. The attacker does not need perfect writing. They do not need malware. They do not even need a particularly clever pretext. They just need enough truth to make the lie feel safe.


That is the pattern at the heart of what we are calling the[Reservation Hijack Scam](https://us.norton.com/blog/online-scams/reservation-hijacking-scam) .


We call it a **Reservation Hijack Scam** because the attacker is not just spoofing a hotel or sending a generic phishing lure. They are hijacking the context around a real reservation and using it to make fraud feel like routine customer service.


In the stronger versions of the attack, that goes further. Once hotel staff credentials are phished, the attacker may gain access to real hotel or partner accounts and contact guests from within legitimate workflows, for example through a booking platform account or hotel messaging environment. At that point, the scam is no longer only borrowing the hotel’s identity, it is actively impersonating the hotel through systems the guest already trusts.


That is the shift that matters most. This is not just a travel-themed phishing lure dressed up with better wording. In the strongest cases, criminals can move from impersonating the hotel to operating through compromised hotel-side or partner workflows, contacting real travelers in the context of real reservations.


## A scam category built on stolen and relayed trust


Most phishing is generic. It arrives with no context, so your brain treats it with suspicion.


Reservation Hijack scams work differently. They reflect real context back at the victim, the hotel name, the destination, the fact that there is a genuine booking, sometimes the stay window, sometimes even the exact amount due. That reflected truth becomes the bypass.


Add a short fuse, usually 24 or 48 hours, and the attacker gets what they want most, fast compliance before the victim pauses, verifies or calls the property.


This is not just a nicer phishing template. It is a different quality of deception.


Scam message received by a victim using data from a real reservation


## What we found


Our investigation points to two visible fronts in the same scam pattern.


The first is the booking-platform lure. Victims receive WhatsApp messages or similar contact from what appears to be a hotel or guest relations team, then get pushed into a fake guest portal or payment verification page. We have seen this most clearly in Booking.com-linked cases, with additional signs of similar abuse around other travel and hospitality workflows. These messages feel unusually convincing because they contain real booking context.


The highest observed activity is concentrated across parts of Western Europe, notably the United Kingdom, France, and Germany, alongside the United States, Brazil and Australia.


We have also seen this scam pattern surface across multiple channels, including Booking.com messaging, WhatsApp, SMS and email. That matters because it shows this is not tied to a single delivery trick. It is a flexible fraud workflow that can move wherever guests are most likely to trust and respond.


The second is the **hotel software abuse path** , where the attacker does not just imitate a hotel from the outside, but appears to operate from inside the hotel’s workflow. For example, Cloudbeds is a hospitality management platform used by hotels to handle reservations, guest communications, and other day-to-day operations. In the Cloudbeds-related cases we reviewed, the attack chain begins with phishing hotel staff, not guests. Once the attacker steals staff credentials, they can log into real hotel management tooling, access reservation data and use guest communication features to target upcoming travelers. Depending on the systems tied to that hotel workflow, that may also let the attacker operate through legitimate booking or messaging accounts associated with the property, effectively communicating with real customers as if they were the hotel itself.


That shift matters.


At that point, the hotel is no longer just the theme of the scam. Its real accounts, workflows and communication channels can become the delivery mechanism.


That hotel-side compromise path is not theoretical. In partner-targeted examples we uncovered, attackers impersonated Booking.com security communications and pushed accommodation partners to install what was presented as a mandatory security update. In reality, the attached material led the victim to run a malicious command ([Scam-Yourself Attack Tactic](https://www.gendigital.com/blog/insights/reports/threat-report-q3-2025) ) that deployed a remote access trojan, giving the attacker a foothold that could then be used to steal credentials and abuse the partner account.


Phishing e-mail message posing as Booking.com security team


## How the scam works, from start to finish


At its core, this scam follows a simple pattern, even if the entry point can vary.


First, the attacker gains access to trust, typically by phishing hotel staff and stealing credentials to a real hospitality platform or connected hotel account. That gives them visibility into genuine reservations, the contact details of real travelers, and in some cases the ability to communicate through legitimate hotel-linked workflows.


Next, the attacker identifies real travelers. That is what makes the scam feel different from ordinary phishing. The victim is not chosen at random. They are contacted in the context of a real trip, with details that match an actual booking.


The level of personalization depends on how much information the attacker has. When they can access rich reservation data, such as the property name, stay dates, guest name, payment context or booking details, they can build a highly tailored spear phishing message that feels almost indistinguishable from routine customer service. When they only have limited data, such as an email address or phone number, the scam may look more generic, but it can still exploit hotel branding, urgency and the expectation of pre-trip communication.


That difference is visible in the lures themselves. In high-context variants, the message may name the guest, reference a specific stay date and amount, or lead to a payment page already populated with reservation details. In lower-context variants, the flow is far more generic and relies on the victim to fill in the missing details themselves.


In the weaker version of the scam, the attacker uses that information to craft convincing messages from the outside, through WhatsApp, SMS or email lookalikes. In the stronger version, the attacker may message the guest from a real hotel or booking-related account they have taken over. The difference is important. The victim may not just see a plausible hotel message, they may see what looks like a legitimate continuation of their existing booking conversation.


In one case we reviewed, the phishing message was delivered through a legitimate Booking.com partner communication thread after the accommodation partner account had been compromised. The malicious request was injected into an otherwise normal conversation, making the attack especially difficult to detect because it arrived from a trusted source rather than a spoofed sender. The follow-up warning from the hotel highlights the confusion and damage that can occur once a real partner account is abused:


Phishing injected into a legitimate Booking.com message thread


Then comes the message. It usually arrives over a fast, informal channel such as WhatsApp or SMS, and it is written to feel administrative rather than alarming. The goal is not to scare the victim, but to make the request feel routine, a payment issue, a booking confirmation, a verification step or an urgent action before arrival.


In the examples we reviewed, the exact wording and format varied by platform, but the underlying pattern stayed consistent: the message borrowed hotel context, introduced a practical problem, and pushed the guest toward a verification or payment step that benefited the attacker.


From there, the victim is pushed into a controlled flow. That may be a fake guest portal, a spoofed payment page, or even a branded PDF that adds one more layer of credibility before the victim reaches the real theft stage.


The theft happens when the traveler enters payment details, approves a transfer or otherwise complies with the fake verification process. At that point, the attacker has turned stolen trust into financial fraud.


And the damage may not stop there. Payment details can be reused, tested with small charges, sold or leveraged in follow-on scams. What begins as a believable hotel message can end in broader financial compromise.


That is what makes this scam different. It is not just phishing with a travel theme. It is a workflow attack built on stolen context and relayed trust.


## The Cloudbeds path, step by step


Cloudbeds has publicly warned of an industry-wide phishing campaign targeting hospitality providers and described credential-phishing attacks impersonating Cloudbeds login pages as the root cause rather than a Cloudbeds platform breach.


That point matters especially for small and medium-sized hospitality businesses. Large travel brands usually have bigger security teams, more formalized controls and more mature fraud processes. Smaller properties often rely on lean teams, fast guest communication and a handful of operational platforms to keep everything moving. That makes them particularly vulnerable when attackers target employees directly with credential phishing.


The attack chain we mapped is brutally efficient.


First, hotel staff are lured to fake login pages and their credentials are stolen. Then the attacker logs into the real management environment. From there, they can see future reservations, guest names, contact details, stay information and sometimes payment context. In some scenarios, that access may also extend beyond visibility. If the compromised hotel workflow is connected to guest-facing messaging on booking or hospitality platforms, the attacker may be able to contact customers from the property’s legitimate account presence. That makes the fraud especially dangerous, because the message may arrive inside a channel the traveler already associates with their real reservation.


In the cases documented in our working material, some victims received SMS messages containing detailed reservation information and links that led to professionally styled PDFs impersonating hotel groups. Those PDFs pushed “payment verification” within 48 hours, using generic branding, urgency, and a prominent call to action to move the victim toward the real theft stage. The PDF step appears to function as a buffer, one more layer of legitimacy for the victim, and one more layer of indirection for scanners and filters to deal with. In some cases, the PDF itself was hosted on legitimate partner storage that had been hijacked, giving the lure another borrowed layer of trust before redirecting the victim onward.


Victim is redirected to typo-squatted domains designed to harvest card details, bank transfers or other payment information


From there, the victim is redirected again, typically to typo-squatted domains designed to harvest card details, bank transfers or other payment information. Example domains documented in the shared material include frontdesk-reservation\[.\]com, frontdesk-online\[.\]biz, and hotel.form842987\[.\]digital.


## One scam, multiple surfaces


This is the part many people miss.


The WhatsApp message, the SMS, the fake PDF, the lookalike payment page, the stolen card, these are not separate incidents. They are one pipeline. And that pipeline can include direct impersonation of the hotel itself. Sometimes the attacker stands outside the workflow and imitates the property. Sometimes the attacker steps inside a compromised hotel or booking-related account and speaks from a position the guest already trusts. Different surface, same scam family.


First, the attacker gets context. Next, they use that context to deliver a believable message through a channel people act on quickly. Then they move the victim into a verification flow that feels administrative, not dangerous. If payment details are entered, the harm does not stop at one fraudulent charge. Cards can be tested with small transactions, then used for larger purchases, then resold or retried later.


This is what makes the scam so dangerous. The same fraud logic can be adapted to different platforms, different levels of stolen context, and different points in the guest journey.


## Why this matters now


Hospitality has become an unusually attractive environment for this kind of fraud.


Hotels run on urgency, constant guest communication and high-trust operational workflows. Travelers expect messages about check-in, balances, room preferences, arrival timing, and identity verification. That makes it easier for attackers to hide malicious intent inside language that would look normal in almost any other context.


And when the attacker is working with real reservation data, or worse, using compromised hotel or booking-linked accounts to contact guests directly, the old consumer advice starts to bend. Checking the link still matters, but it is no longer enough on its own. The victim is not just being tricked by branding. They are being tricked by context.


## What travelers should do


If a hotel contacts you via Booking.com messaging, Airbnb, WhatsApp, SMS or email and asks you to verify payment details, do not tap the link. The same caution applies if the message appears to come from an existing reservation thread or from what looks like the hotel’s real account on a booking platform. Trust the booking, not the message.


Go to the booking site, hotel site, or official app yourself. If you need to call the property, use a trusted contact route from the original booking or the verified website, not the number or link inside the message.


If you already entered payment details, assume compromise. Contact your bank, cancel the card, enable transaction alerts and watch for follow-on attempts.


## What hotels should learn from this


The lesson for hospitality is not just “train staff better,” although that matters. It is that hotel software, messaging workflows, and guest communication tools now sit directly on the fraud path.


The hotel is no longer only a victim. In the eyes of the guest, it can become the face of the scam. In the worst cases, it can also become the voice of the scam, when attackers use compromised hotel or partner accounts to message guests as if they were legitimate staff.


That means account protection, phishing-resistant authentication, tighter controls around exports and guest messaging, anomaly detection and faster incident response are no longer nice-to-haves. They are part of brand protection.


Because once attackers learn how to turn operational systems into trust amplifiers, they do not need to break in loudly. They can simply log in, relay stolen trust to the guest, and let the workflow do the rest.


## One final thought


For years, the best advice on travel scams was simple: watch for bad grammar, generic messages, and suspicious links.


That advice still matters. But it belongs to an earlier phase of the problem.


The next phase looks like this: the message knows your booking, sounds plausible, and may arrive through a channel or account the traveler already trusts. That is the point where customer service and fraud begin to overlap.


That is the Reservation Hijack Scam.


In the next part of this series, we will look more closely at the infrastructure, indicators and investigative signals behind it.


More on this topic


[GhostPairing Attacks: from phone number to full access in WhatsApp – From Research](https://www.gendigital.com/blog/insights/research/ghostpairing-whatsapp-attack)


[Cyber predictions for 2026 – From Leadership Perspectives](https://www.gendigital.com/blog/insights/leadership-perspectives/predictions-2026)


[OpenClaw: Handing AI the keys to your digital life – From Research](https://www.gendigital.com/blog/insights/research/openclaw-autonomy-risks)


[Martin Chlumecký](https://www.gendigital.com/blog/authors/martin-chlumecky)


Malware Researcher


[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


Security Evangelist at Gen


At Gen, Luis tracks evolving threats and trends, turning research into actionable safety advice. He has worked in cybersecurity since 1999. He chairs the AMTSO Board and serves on the Board of MUTE.


Follow us for more
