---
schema_version: "1.0.0"
document_id: "dc39690cc2a72450a3944290b9ec9e44dd713e159816b10fc8553199d1386074"
company_key: "yc-farel"
company: "Farel"
source_id: "yc-farel-rss-64b3b2dac96a"
canonical_url: "https://farel.io/company/blog/aer-arann-islands-launch/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.436446+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:1e2f0ec60e5f909faf8bd37b0a6e759b772d772fd1380719401fc878db22bd58"
---

# Aer Arann Islands Goes Digital After 56 Years of Paper

## Key takeaways


- Aer Arann Islands, the Irish airline serving the Aran Islands since 1970, went live on Farel on July 13, 2026.
- For 56 years the airline ran every operation manually: bookings, changes, refunds, check-in, manifests, and weight and balance. All seat availability lived in one physical book.
- Passengers can now book online with real-time availability. Island stations see the same data as the mainland for the first time.
- Farel extended the platform for PSO passenger categories: free government-funded tickets for island residents, resident discounts, and student fares.
- Aer Arann Islands is the first airline to go live on Farel in the European Union.


Three phone calls came in at the same time. Three agents needed to check seat availability. There was exactly one place to check it: a large paper book on a desk at Connemara Airport. So the agents formed a line. The first one found the flight, wrote down the passenger, and passed the book to the next.


The staff have a name for it. Not "the reservation system." Not "the ledger." Just The Book.


I watched this happen in June 2026, standing in the office of an airline that has flown safely for 56 years. It is the kind of scene most people in airline software assume died out decades ago. It didn't. It just moved somewhere quiet.


## Who is Aer Arann Islands?


Aer Arann Islands is an Irish airline that has connected Connemara Airport in County Galway with the three Aran Islands - Inis Mór, Inis Meáin, and Inis Oírr - since 1970. The fleet is three Britten-Norman Islanders. The flights take less than ten minutes, gate to gate, which makes them some of the shortest scheduled routes in the world. The airline carries more than 45,000 passengers a year, and the service operates under a[Public Service Obligation contract with the Irish government](https://gov.ie/en/department-of-rural-and-community-development-and-the-gaeltacht/news/our-rural-future-49-million-air-service-contract-for-the-aran-islands/) , because for islanders this is not a scenic hop. It is the bus to the mainland.


And in all those 56 years, the airline never had a reservation system. Bookings, passenger lists, refunds, changes, notifications, check-in, manifests, weight and balance - all of it was done by hand.


## How do you run an airline on paper?


Carefully, and with a lot of patience. All availability lived in The Book. If a passenger wanted to know whether there were seats on Friday's flight, they called, and an agent checked the page. If three passengers called at once, they waited for each other, because the agents were waiting for each other.


The islands had it harder. Remote stations had no copy of The Book, so island staff couldn't always see who was booked, who was on the waitlist, or what had changed since the morning. Tickets were often written out by hand at Connemara, flown out with the passengers, and the paper came back on the return flight.


Weight and balance ran on a dedicated instrument: a rotating cardboard disc and a laminated load sheet. Passenger weights, baggage, and fuel went in with a grease pencil, the needle showed whether the load was safe, and after the flight everything was wiped clean and started over. Seats weren't assigned in a system either - staff stood at the aircraft and pointed each passenger to a seat based on weight, to keep the Islander balanced.


To be clear: this worked. For decades. The airline's safety record over 56 years speaks for itself. But "it works" and "it scales" are different sentences.


## Why change after 56 years?


Because the plans got bigger than the paper. Aer Arann Islands is expanding: a fourth Islander is expected by the end of summer 2026, new islands and routes are on the table, and flights to Galway - the nearest major city - are a real prospect. A new generation of the team also arrived, and for them a single physical book holding the airline's entire availability was not an acceptable operating model.


Not everyone agreed at first, and I understand why. Some of the staff have been with the airline for 20 or 30 years. Their processes were built over decades and they worked. Then the new generation shows up with a plan to move everything online, and of course there was stress and pushback. That part is normal. What mattered is what happened after people actually sat down with the system: the resistance faded, and the team started asking for more.


A lot of credit here goes to Darragh Fallon, who led the project on the airline's side. He flew from island to island training staff, wrote internal instructions, and fed us a steady stream of detail about how the operation really works - the edge cases no demo ever shows you. That knowledge is why we could adapt Farel to their model instead of forcing their model into software.


## Two weeks next to the runway


For the implementation I lived for two weeks beside Connemara Airport, running training sessions and configuring schedules, fares, and ancillary services with the team.


The mood in those sessions told me everything about how ready this team was. People are so tired of The Book that at an all-hands meeting, CEO Peter McKenna joked that once Farel was live, they would burn it in public. It was a joke. Probably. Nobody has scheduled the ceremony yet, but I have asked to be invited.


Even the bus drivers told me the company had needed this for years. When the drivers are lobbying for a reservation system, the change management is going to be fine.


## What changed on July 13?


Aer Arann Islands now runs on Farel: an airline website with online sales and real-time availability,[reservations](https://farel.io/platform/reservation-system/) , check-in,[departure control](https://farel.io/platform/departure-control-system/) , electronic manifests, and ancillary services. Passengers no longer call to ask if there are seats. They look, they book, they show up.


For island operations this is the biggest shift. Every station now sees the same live data: who bought a ticket, who is on the waitlist, who has checked in, what bags and extras they paid for. The paper tickets flying back and forth are gone. Ancillaries went online too, and not just bags: the bus between Connemara Airport and Galway is now bookable with the flight - free for eligible islanders, paid for tourists, and the drivers see a live list of who to pick up.


Some of what Aer Arann Islands needed didn't exist in Farel before this project, so we built it:


- **PSO passenger categories.** Elderly island residents receive a set number of free government-funded tickets per year, then switch to a resident discount. Students have their own fares, and residents and tourists are separate categories. Farel now manages all of this natively - eligibility, free-ticket counters, discounts.
- **Actual passenger weight at check-in.** Agents record each passenger's real weight in the system during check-in, and it flows straight into the electronic manifest that replaced the printed one. On a nine-seat Islander, that number is not a formality.
- **"Notify me" on sold-out flights.** When a flight in the booking engine or mobile app has no seats left, passengers can register their interest instead of giving up or settling for a worse date. The commercial team sees that pent-up demand by segment and date, and has the contacts to reach out. When your entire fleet is three nine-seat aircraft, sold out is a daily event - and if enough people wanted that Friday flight, the team can simply put another rotation on the schedule and go tell them about it.


## What's next?


The most interesting one: paid seat selection, including the co-pilot seat - the most honest premium seat in aviation. Aer Arann Islands won't sell it until the automated weight and balance module fully replaces the disc, because on a nine-seat Islander, balance is not a detail. Once the system confirms a load is safe, the seat next to the pilot goes on sale.


Beyond that: the fourth aircraft, new island routes, potential Galway flights, and a longer-term ambition to move to electric aircraft. The goal on our side is to keep extending the platform until the manual processes are gone entirely - not most of them, all of them.


Aer Arann Islands is the first airline to go live on Farel in the European Union, and I can't imagine a better first: 56 years of history, a team that knows exactly why it wanted to change, and an operation that keeps three islands connected to the rest of the country. Thanks to Peter, Darragh, Mragakshi, and the whole team for trusting us with it.


The Book has been retired. The bonfire is pending.


If your airline still runs on paper, spreadsheets, or a book with a name,[book a demo](https://farel.io/request-a-demo/) - we have done this before.


Share
