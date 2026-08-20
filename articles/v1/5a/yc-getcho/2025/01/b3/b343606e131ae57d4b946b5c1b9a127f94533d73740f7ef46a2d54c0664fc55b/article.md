---
schema_version: "1.0.0"
document_id: "b343606e131ae57d4b946b5c1b9a127f94533d73740f7ef46a2d54c0664fc55b"
company_key: "yc-getcho"
company: "Getcho"
source_id: "yc-getcho-rss-e8ccd9728c81"
canonical_url: "https://getcho.app/blog/same-day-delivery-and-the-doorman/"
published_at: "2025-01-16T19:38:03+00:00"
first_seen_at: "2026-07-20T23:20:29.886918+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:7f757ed7f1ed8c2b6dba406575b331b57bc1823a62c647269de2cfb01538c26f"
---

# Delivery and the Doorman

# Same-Day Delivery and The Doorman


In 1972, Fritz Ritter described his job to Studs Terkel; the interview was featured in Terkel’s iconic collection, *Working* .


“I watch who comes in, goes out. If I see a stranger, I stop him and find out where he’s going. We call upstairs, we have to announce him. In the nighttime now, twelve o’clock, you have the door locked.”


Ritter was a doorman. For 40 years he worked at a 180-unit apartment complex in New York City’s Upper West Side. Half a century later, the job seems not to have changed much; his counterparts today still watch the door, announce visitors, and greet residents.


They have a big new responsibility though: receiving a near-constant stream of packages. Thanks to modern luxuries like e-commerce, DoorDash, and work-from-home, doormen now handle 15 times more deliveries than they did in Ritters’ day..


Getcho is a delivery success platform. We dispatch, monitor, and verify local deliveries for retailers and wine merchants. Most often, these deliveries are carried by Uber Direct, DoorDash and Roadie.


Ensuring success in local delivery means keeping parties on the same page. Usually there are three stakeholders:


1. The customer
2. The retailer
3. The driver


But in New York City and parts of Chicago, there is often a fourth critical stakeholder: the doorman.


## Which doormen receive packages


When a courier can’t deliver a package, the fleet directs them to bring it back.


We hate returns – these get lost ten times as often, waste a trip, and usually upset both the driver and the customer. We build tools that prevent returns. For example:


- Getcho’s AI agent texts with the customer and schedules dispatch around their availability.
- Our Shopify integration chooses the closest store with inventory. That allows for a more precise delivery window.
- Notifications guide drivers to the correct entrance for faster drop-offs.


But doorman delivery policies vary wildly and the lack of standardization creates return risk. Here are just a few ways buildings receive packages in Manhattan alone:


- At[20 Exchange Place](https://streeteasy.com/building/twenty-exchange) , the front desk checks the driver by asking for the name and unit of the recipient. The driver may then proceed to the unit to make the delivery while the building’s system emails the recipient.
- At[160 Water Street](https://streeteasy.com/building/pearl-house) , a concierge accepts the package from the driver, who doesn’t typically go up to the unit.
- In mixed residential & commercial buildings like[225 Cherry St](https://streeteasy.com/building/one-manhattan-square) , doormen often direct drivers to separate freight entrances – even if the package is only the size of a book.
- Some doormen can’t accept deliveries on behalf of residents, period.
- Other buildings use “virtual doormen” that can sign in visitors and couriers from afar.


That’s before considering a bigger challenge: alcohol deliveries. Drivers must verify a recipient’s age at delivery, and many carriers require an ID scan. What should the doorman’s role be?


The doormen at[30 Waterside Plaza](https://streeteasy.com/building/waterside-plaza) won’t scan their own IDs as a rule. I can sympathize with them – their job description doesn’t include holding their personal driver’s license in front of a camera every time someone orders wine.


> ”It’s a wine delivery, can you guys scan your ID? What do you mean, it’s for the building? Who else is back there? What is wrong with you guys?”
> — A doorman talking to his package room attendant


At[3 East 95th Street](https://streeteasy.com/building/mrs-amory-s-carhart-house) , at least a few of the doormen treat the ID scan as part of the job. At some buildings, folks even have their IDs out on the desk.


## Aggregating doorman delivery data


Ahead of the recent holiday season, we began collecting every New York City building’s delivery policy.


Corporate gifting is a mess: Retailers get spreadsheets from third-party groups with a name, a purchase request, and an address that’s often incorrect. Millions of gifts are delivered with little confirmation. A week after the New Year, some gifters wonder why they haven’t gotten a thank you and start asking the gifting group for tracking data.


We are the ones who know the fate of every package and that means knowing every receiving policy.[StreetEasy](https://streeteasy.com/) is a pretty good source of whether a building has a doorman. To find out how a doorman receives packages, we try to pull a front-desk phone or email; sometimes the Google Business listing has one, and other times you can find one on the property management group’s website.


[There are about 3,200 doormen buildings in New York City](https://www.nytimes.com/2010/04/22/nyregion/22doormen.html#:~:text=Like%20sentries%2C%20doormen%20have%20patrolled,apartment%20buildings%20in%20the%20city.) and 20% of the buildings seem to receive 80% of the deliveries. When we can’t find information on a building, we flag it in the system. We monitor deliveries to these buildings very closely and update our data accordingly.


Getcho also works with retailers to add a doorman field to their website’s checkout page. Building residents making a purchase help fill critical gaps in the data.


## Enriching deliveries with doorman policies


With good data, delivering to doormen is straightforward. After a customer makes a same-day purchase on a partner’s Shopify checkout page, the system sees a building address and goes to work:


1. If there is no doorman, it schedules delivery around the customer’s availability.
2. If there is a doorman and they accept deliveries and scan IDs, it schedules for the soonest time within doorman working hours.


1. It also adds delivery notes with the front desk line if possible.
2. It even places an automated call ahead of delivery.


3. If there is a doorman and they don’t accept deliveries, it schedules around customer availability.
4. If there is a doorman and they may or may not accept deliveries, we may dispatch a smaller fleet with more tolerance for uncertainty.


There are other factors like inventory, processing time and even the weather.


### A note on age verification & doormen


I mentioned that alcohol deliveries are substantially more challenging due to the age verification requirement. Surprisingly, messaging has a huge impact on success. Doormen and drivers frequently mistake the intent of the ID check – It’s only an age check, and does not have implications on the custody of the package. Sending automated messages usually clears this up.
