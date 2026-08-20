---
schema_version: "1.0.0"
document_id: "cf9c4661ce20853155a8d7b74820e222cf9c6ce84ebfe420d245820edbffe6c7"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/banking-and-capital-markets/impending-payments-compliance-avalanche"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:eb806e472f060b617ee199b41281e7caaf496a56d6a398531cefcd8d276224e6"
---

# The impending payments compliance avalanche

Recently, I attended the[EBA Winter School](https://www.abe-eba.eu/training-education/eba-summer-winter-schools-and-other-educational-seminars/) in Belgium (I strongly recommend it), and as participants, we were assigned a group task to “develop a new and innovative business idea to increase the profitability of cross-border payments for banks.” Not a surprising task in the current era of[G20 initiatives](https://www.fsb.org/work-of-the-fsb/financial-innovation-and-structural-change/cross-border-payments/g20-targets-for-enhancing-cross-border-payments-2/) aimed at driving down end-user pricing, but the outcome was certainly not what I expected.


Of the six groups tasked, three focused on the cost of compliance activities (screening and anti-money laundering), demonstrating that this is a significant area for banks. What became clear as the task progressed is that there are several challenges in this area that are causing friction, with a potential risk of significant fines from regulators if this activity goes wrong or fails to meet scrutiny.


Here are some of those challenges:


### Challenge 1: Compliance checking software is typically last generation


While some solutions are now emerging that can cope with the demands of inline real-time screening in a high-throughput environment, most banks today are still reliant on risk-based screening and post-processing checks. With a couple of market-leading vendors historically dominating this space, the speed of upgrades has been slow and an area of pressure during the recent ISO 20022 migration.


### Challenge 2: Compliance checks are a solo activity


Mostly out of necessity, as every bank is responsible for checking its own traffic, cross-border payments may have been checked 10 or more times between origination and settlement, with the possibility of being held or stopped at every check. While this is an obvious consequence of an environment where ‘you can only trust things that you check yourself,’ it’s also not conducive to efficient processing. All these checks are a significant point of friction in these types of payments, and potentially unsustainable as we strive to fulfill the G20 objectives.


### Challenge 3: Other people’s decisions aren’t visible


If you’re a correspondent bank in the middle of a payment chain, you generally don’t have visibility on decisions made by previous viewers in the transaction chain. Typically, screening data and outcomes don’t accompany the payment, and many payment delays can be attributed to retroactive requests for the source of funds, extended participant data, or beneficial parties. Further, there are no records to indicate whether a payment has already been checked against a particular sanctions list, and thus, this activity can be repeated multiple times.


### Challenge 4: Compliance is the biggest cause of payment friction and manual work


While there are clear use cases for GenAI to resolve other straight-through exceptions, compliance holds are necessary drivers of manual intervention. However, many of the holds result from a lack of data, which, had it been provided, would have prevented the intervention. Straight-through processing is spoiled by a lack of information.


### Turning compliance into an opportunity


What does this mean for potential savings (and hence improved profits or lower prices)? Clearly, in this space, there’s room for improvement. Solutions like market-leading[CGI Hotscan360](https://www.cgi.com/en/solutions/cgi-hotscan360) are making it possible to screen payment traffic in a real-time environment, but the speed of checking isn’t necessarily the root of the challenge.


Within the payments life cycle, we’ve implemented reconciliation tools such as UETR, structured addresses, and status visibility, including[Swift GPI](https://www.swift.com/products/swift-gpi) . However, there is no information shared on screening efforts. There are local requirements that can be proprietary to every correspondent, but perhaps there are some collaborative gains to be achieved in compliance checking data.


Building on the ideas generated from our Winter School challenge, a common component was exploring ways to share compliance data and providing additional information in the new ISO fields that outline the checks undertaken by each participant in the transaction. However, if you’re doing post-processing checks, clearly this isn’t possible. And, of course, if the only person you can trust is yourself, then sharing compliance data doesn’t help other than to validate your own checks. Put simply, we have a baked-in cost and friction that require a fundamental rethink.


### Where do we go from here with payments compliance?


Of course, we need to continue checking transactions to ensure that money isn’t going to sanctioned individuals or being used to fund criminal or terrorist activities, but is there a better way we can collaborate to make that process more efficient?


I suspect that there might be, and so I put the challenge out there. With an avalanche of real-time compliance checks needed to accompany real-time cross-border transactions, how can we collectively rethink this fundamental area of payments? Join us for a discussion on this topic at[EBAday 2026](https://www.ebaday.com/) in Copenhagen from June 16-17.


In the meantime, to discuss these challenges further or[CGI’s work in addressing them](https://www.cgi.com/en/banking-capital-markets/payments) , feel free to reach out to me.
