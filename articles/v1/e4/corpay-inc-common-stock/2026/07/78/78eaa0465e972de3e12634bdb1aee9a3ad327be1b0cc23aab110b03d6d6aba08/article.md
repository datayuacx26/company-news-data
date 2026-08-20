---
schema_version: "1.0.0"
document_id: "78eaa0465e972de3e12634bdb1aee9a3ad327be1b0cc23aab110b03d6d6aba08"
company_key: "corpay-inc-common-stock"
company: "Corpay Inc."
source_id: "corpay-inc-common-stock-news-import-8649661f155f"
canonical_url: "https://www.corpay.com/resources/blog/rebate-optimization-ap-spend"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T05:10:41.901974+00:00"
fetched_at: "2026-07-29T05:10:43.603545+00:00"
content_hash: "sha256:fb4cb205fd1f269da80a482269b5f81d7a5f75b06adb45e441339315ca1f3d8d"
---

# Rebate Optimization: Turning AP Spend into Revenue

##


1. How does AP spend become rebate revenue?


1. Which AP spend actually qualifies for rebate?
2. How much rebate can you realistically earn?


2. Why do rebate programs damage vendor relationships?


1. Why do suppliers refuse card payments?
2. How does managed enrollment change the acceptance math?


3. How do you design the payment mix to maximize rebate without forcing cards?


1. When should a supplier stay on ACH or check?
2. How does virtual card rebate also extend your working capital?


4. How do you measure and prove rebate capture?
5. How Corpay turns AP spend into measurable rebate revenue


An **AP rebate** is cash paid back to a buyer for routing supplier payments through a commercial or virtual card. The rebate is funded by interchange, which means the program only earns what your suppliers actually agree to accept.


That last clause is where most programs come apart. The pitch is straightforward and genuinely attractive, and[how virtual card rebates turn AP spend into revenue](https://www.corpay.com/resources/blog/virtual-card-rebates) is well-trodden ground at this point. Somebody models total payables against a rebate rate, produces a number with a lot of zeros in it, and the board approves the program.


Then the program meets the supplier base. Suppliers who don't want to absorb card acceptance costs push back, some add a convenience fee to the invoice, and a few decline card payment outright. AP practitioners describe the pattern often enough on public forums that it reads as a genre: a program targeting a large concentrated vendor, a vendor that balks at the interchange, and a finance team that ends up reverting to internal payment runs with nothing to show for the project.


Rebate optimization is a supplier enrollment and payment mix problem, not a "put everything on a card" problem. Get the eligible-spend math honest, understand why a supplier says no, and build a mix that pays each vendor the way it will actually accept. Revenue follows from acceptance, never the other way around.


***Key Takeaways***


-


Rebate is funded by interchange, so the earning rate is capped by what share of your spend suppliers will accept on card, not by your total payables.


-


Realistic capture is a product of three variables multiplied together: card-eligible share of spend, the rebate rate, and the acceptance rate you actually achieve. Two of those get quietly assumed in most business cases.


-


Suppliers who decline commercial cards overwhelmingly point to acceptance fees, which makes the objection an economic one rather than a technology gap.


-


Managed enrollment materially changes the outcome, roughly doubling supplier acceptance compared with a self-service portal invitation.


-


Most AP dollars still move by ACH, so a program that mandates cards will fail. Design the mix, then measure it.


-


Virtual card payment produces two benefits at once, since the rebate lands on spend you were making anyway and the card grace period extends your days payable outstanding.


## How does AP spend become rebate revenue?


Paying a supplier with a commercial or virtual card generates interchange, and the card issuer returns a share of that interchange to the buyer as a rebate. The buyer earns money on spend it was going to make regardless.


The mechanic is worth understanding precisely, because the way it's funded explains every constraint that follows. When you pay by card, the supplier's acquiring bank deducts a merchant discount rate from the payment before the supplier receives it. Part of that goes to the card network, part to the issuing bank, and part comes back to you.[Interchange fees and card rebates](https://www.corpay.com/resources/blog/interchange-fees-card-rebates) covers the full flow, but the short version is that your rebate is the supplier's cost. Nobody is creating money. You're capturing a slice of a fee your supplier is paying, in exchange for the supplier getting paid faster and with less collection effort.


That framing is uncomfortable for some finance leaders, and I'd argue they should sit with the discomfort rather than route around it. A rebate program that can't articulate the supplier's side of the trade is a program that will get blindsided the first time a large vendor runs the numbers.


The pool being drawn from is genuinely large. Datos Insights, cited by Visa, put global commercial card spend at $3.57 trillion in 2024, rising toward $5.07 trillion by 2028. Juniper Research's 2024 B2B payments analysis projects B2B card payments growing from $11 trillion in 2024 to $22 trillion by 2028, with virtual cards holding a 70.3% revenue share of the B2B card market in 2024. The direction of travel is not in question. What varies enormously is how much of it any individual buyer manages to capture.


### Which AP spend actually qualifies for rebate?


Card-eligible spend is the subset of your payables going to suppliers who accept commercial card payment and don't offset the cost back to you. Everything else stays on ACH, check, or wire, and earns nothing.


In practice, the file sorts into a few recognizable groups:


-


Mid-size recurring suppliers are the sweet spot. They value fast, predictable payment, their margins can absorb acceptance costs, and they typically lack the leverage to demand a surcharge.


-


Large strategic vendors are the hardest. They pay attention to acceptance costs, they have negotiating power, and they're usually the concentration that made the business case look good in the first place.


-


Long-tail small suppliers often accept readily but represent little spend, so they improve your acceptance percentage without moving the revenue number much.


-


Regulated and public-sector payees frequently can't take cards at all, or can only take them with a surcharge passed straight back to you.


-


Payroll, taxes, rent, and intercompany transfers are simply out of scope.


The trap is that these groups don't distribute evenly. A payables file where most dollars sit with eight strategic vendors has a very different rebate ceiling than one with the same total spread across 400 mid-size suppliers, even though a spreadsheet that only looks at total AP treats them identically.


Before you model anything, sort last year's payments by vendor, flag the ones already accepting cards somewhere in your organization, and separate the concentration from the tail. That exercise usually takes an analyst a day and reprices the entire business case.


### How much rebate can you realistically earn?


Multiply three numbers, not one. Realistic rebate equals card-eligible share of spend, times your rebate rate, times the acceptance rate you actually achieve, and the third factor is the one that gets assumed away.


Visa's 2024 research on commercial card acceptance found self-service supplier enrollment typically delivering 20% to 35% acceptance, while managed enrollment reaches 40% to 60% and some mid-market programs exceed 70%. Run those bands against a real payables file and the spread stops being academic.


Take a $200 million file with $80 million of spend sitting at suppliers who could plausibly take a card. At a representative rebate rate of 1%, full acceptance would produce $800,000. Acceptance is never full. Under a self-service program the realized number lands closer to $160,000 to $280,000, while managed enrollment puts it somewhere around $320,000 to $480,000.


None of those outcomes are bad. All of them fall dramatically short of the headline number somebody presented in the kickoff meeting, which is why so many rebate programs get labeled failures when they were really just oversold. That gap traces back to a forecasting failure six months before launch rather than to anything the program did after it.


Ask for the acceptance assumption in writing before you approve the business case. If nobody can tell you what acceptance rate the projection assumed, the projection is a total-spend calculation wearing a program's clothes.


## Why do rebate programs damage vendor relationships?


Programs damage relationships when they treat card acceptance as a policy the buyer can impose rather than a commercial term the supplier has to agree to. The supplier's response is predictable, and it escalates.


The sequence is consistent enough to write down:


1.


A buyer announces that payments are moving to card, usually by email, usually with a deadline.


2.


Suppliers who were never consulted discover they'll net less on every invoice from that point forward.


3.


Some absorb it quietly, others add a convenience fee to the next invoice, and a few refuse outright and demand ACH.


4.


In the sharpest cases a supplier holds shipment until payment terms get renegotiated, and by then the conversation with the CFO is no longer about rebate revenue.


Visa's 2024 acceptance research quantifies the root cause cleanly: 68% of suppliers who don't currently accept commercial cards cite high fees as the primary barrier. That barrier is economic rather than attitudinal. A supplier looks at a two-to-three-percent haircut on revenue and decides faster payment doesn't cover it. Any program design that doesn't answer that objection directly is going to keep meeting it.


Preventing the damage comes down to sequencing. Enroll before you switch, negotiate rather than announce, and accept "no" as a valid answer that routes the supplier to a different payment method instead of a fight.[Vendor management best practices](https://www.corpay.com/resources/blog/vendor-management-best-practices) apply here more than in almost any other AP initiative, because you're asking suppliers to change their economics, not just their remittance instructions.


### Protect cash flow with modern AP


Modernize AP to cut costs, speed approvals, and mitigate payment risk — gaining the real-time visibility to protect cash flow and scale with confidence.


[Download the whitepaper](https://www.corpay.com/resources/whitepapers/protect-cashflow-with-ap)


### Why do suppliers refuse card payments?


Because acceptance costs them a percentage of every payment they receive. On a $50,000 invoice, a merchant discount rate in the two-percent range costs the supplier a thousand dollars, and for a business running single-digit margins that's a meaningful share of the profit on the job.


Faster payment does have real value on the supplier side, and it's worth being specific about what that value is rather than waving at it. Card payment typically settles in days rather than the thirty to sixty a check cycle takes, it removes collection effort, and it eliminates the credit risk of an unpaid invoice entirely.[Accepting virtual card payments](https://www.corpay.com/resources/blog/accepting-virtual-card-payments) lays out the supplier's case in full. For a supplier financing receivables on a line of credit, getting paid twenty-five days sooner can be worth more than the acceptance cost.


The suppliers who refuse are usually the ones for whom that trade genuinely doesn't work. High-volume, low-margin distributors. Suppliers with strong balance sheets and no receivables financing cost. Vendors large enough that your business isn't worth a concession. Those refusals are rational, and treating them as an objection to overcome rather than information about the supplier's economics is how programs go wrong.


### How does managed enrollment change the acceptance math?


Managed enrollment roughly doubles acceptance because someone actually calls the supplier. Self-service programs email a portal link and wait. Managed programs work a target list, explain the trade, negotiate terms, and route the ones who decline to a different payment method.


The gap between those two acceptance bands separates a rebate program that pays for itself from one that quietly gets shelved. And the mechanism is unglamorous — outreach capacity, supplier-specific conversations, and the persistence to follow up with a controller who ignored three emails because enrolling in a buyer's payment portal was never going to be their priority.


What managed enrollment also buys is segmentation intelligence. A team working the phones learns which suppliers will accept at what threshold, which ones want net terms adjusted in exchange, and which ones should never be asked again.[Why vendor enrollment determines your virtual card program's success](https://www.corpay.com/resources/blog/vendor-enrollment-virtual-card-success) goes deeper on the operational side, but the strategic point is that enrollment quality, not card technology, is the variable that decides program economics.


This is the part software alone consistently gets wrong. A platform can issue a virtual card in seconds. It cannot talk a skeptical AR manager at a family-owned distributor through why accepting a card is worth two percent, and that conversation is the actual product.


## How do you design the payment mix to maximize rebate without forcing cards?


Segment suppliers by acceptance likelihood and fee tolerance, then assign each segment a payment method it will accept. The goal is maximum rebate subject to the constraint that no supplier relationship gets damaged, which is a different optimization than maximum card penetration.


Start from a fact that reframes the whole exercise. The Federal Reserve's 2025 Payments Study found cards accounted for 79% of noncash payments by number but only 8% by value in 2024, while ACH represented 74% of noncash value. The dollars overwhelmingly move by ACH, and they always have. A rebate program that assumes it can invert that ratio is fighting the structure of B2B payments rather than working inside it.


**Supplier segment**


**Typical acceptance**


**Recommended method**


**Rationale**


Mid-size recurring vendors


High


Virtual card


Values speed, absorbs acceptance cost, low surcharge risk


Long-tail small suppliers


High


Virtual card


Easy wins; improves acceptance rate, modest dollar impact


Large strategic vendors


Low


ACH, or card only if negotiated


Fee-sensitive with leverage; surcharge and relationship risk


Low-margin distributors


Low


ACH


Acceptance cost consumes a large share of job margin


Public sector and regulated payees


Very low


ACH or check


Often prohibited from accepting, or surcharge is passed back


*Segment guidance based on Visa 2024 commercial card acceptance research and Federal Reserve 2025 payment-mix data.*


The programs that hold up over time treat this table as a living document. Suppliers move between segments as their own economics change, and a vendor who declined two years ago during a cash-rich period may accept now.


### When should a supplier stay on ACH or check?


Keep a supplier on ACH when the acceptance cost exceeds what faster payment is worth to them, when they've told you a surcharge is coming, or when the relationship carries supply risk you're not willing to price. Those are three different tests and any one of them is sufficient.


ACH is also just cheaper to run at volume, which matters for the majority of spend that will never be card-eligible anyway.[What an ACH payment is](https://www.corpay.com/resources/blog/what-is-ach-payment) covers settlement timing and the same-day options that close most of the speed gap suppliers care about. Pairing ACH with[supplier payments automation](https://www.corpay.com/resources/blog/supplier-payments-automation-benefits) gets you the operational benefit of a modern payment program even on the spend that earns no rebate at all, which is worth remembering when the program gets evaluated purely on rebate dollars.


Check deserves a harder look. RPMG Research's 2022 Virtual Card Benchmark Survey put the end-to-end cost of a virtual card transaction near $13 against roughly $38 for a check, a $25 per-transaction difference. If you're processing thousands of checks a year, moving that volume to electronic payment saves real money whether or not any of it earns rebate. Plenty of programs justify themselves on that alone.


### How does virtual card rebate also extend your working capital?


The card's grace period delays your cash outflow while the supplier gets paid immediately. You earn the rebate and hold the cash longer, which is unusual because those two normally trade against each other.


That combination is why virtual cards keep gaining share. RPMG's same benchmark survey found 83% of companies reported that virtual cards improved their working capital position. The mechanism is straightforward, since payment leaves your account when the card statement settles rather than when the supplier is paid, effectively extending your[days payable outstanding](https://www.corpay.com/resources/blog/days-payable-outstanding) by the length of the billing cycle without renegotiating a single supplier term.


Scale makes the point. The Hackett Group's 2025 Working Capital Survey identified roughly $1.7 trillion trapped in excess working capital across large U.S. companies. Payment instrument selection isn't going to release all of that, but it's one of the few levers that improves the buyer's position without pushing cost onto suppliers, which is exactly what makes card programs worth defending. Treating[a card program as a cash flow tool](https://www.corpay.com/resources/blog/card-program-optimize-cash-flow) rather than a rebate scheme also tends to survive CFO scrutiny better, because the working capital benefit doesn't depend on hitting an acceptance forecast.


## How do you measure and prove rebate capture?


Track rebate per payment against the invoice it settled, not as a lump credit at quarter end. If your rebate arrives as a single line item with no per-transaction detail, you can't tell which enrollment efforts worked and which segments underperformed.


The measurements worth maintaining are narrower than most dashboards suggest:


1.


Card-eligible spend as a percentage of total AP, refreshed quarterly, since it moves as vendors are added and as enrollment progresses.


2.


Realized acceptance rate, measured by dollars rather than by supplier count. Enrolling 200 small vendors and one strategic account are not the same achievement.


3.


Rebate per thousand dollars of eligible spend, which normalizes for spend volatility and exposes rate changes you might otherwise miss.


4.


Supplier churn and surcharge incidents, tracked deliberately. That's the cost side of the program, and no rebate report will surface it for you.


That fourth metric is the one I'd insist on. A program earning $400,000 in annual rebate while three suppliers quietly added convenience fees totaling $180,000 is netting far less than the report claims, and nobody finds out until someone reconciles invoice amounts against the original quotes.


Per-invoice attribution depends on payment data flowing back cleanly, which makes[payment reconciliation](https://www.corpay.com/resources/blog/payment-reconciliation) the foundation rather than an afterthought. When each card payment carries the invoice reference through settlement, rebate becomes an auditable line in the P&L. When it doesn't, rebate is a number somebody sends you and you take on faith.


## How Corpay turns AP spend into measurable rebate revenue


The rebate only exists if the supplier accepts the payment, which is why we run enrollment as a managed service rather than a portal invitation. Our team works your supplier file, has the acceptance conversation vendor by vendor, and matches each supplier to the method it will actually take, with virtual card where the economics work and ACH or check where they don't. As Mastercard's #1 commercial B2B issuer with a network of more than four million accepting vendors, we often find a meaningful share of your suppliers are already enrolled and accepting card payment somewhere in our network before we make a single call.


Measurement is built into the same flow. Payments originate from your ERP through[Corpay's payments automation](https://www.corpay.com/ap-automation/payments-automation) , settle against the originating invoice, and return rebate detail at the transaction level rather than as an unexplained quarterly credit. If you're standing up or defending a card program,[Corpay Commercial Cards](https://www.corpay.com/commercial-cards) is where the program economics live, and[Corpay Complete](https://www.corpay.com/corpay-complete) brings payables and payments together for teams that want one system handling invoice through settlement.


## Frequently Asked Questions


### What is an AP rebate?


An AP rebate is cash returned to a buyer for paying suppliers with a commercial or virtual card. It's funded by interchange, the fee the supplier's bank deducts when it accepts a card payment, so the rebate a buyer earns is directly tied to how much of its spend suppliers agree to take on card.


### How much rebate can a company realistically earn on AP spend?


Multiply card-eligible spend by the rebate rate, then by your actual supplier acceptance rate. Because acceptance typically lands well under half of eligible spend under a self-service program, realistic capture is a fraction of what a total-spend projection implies. The acceptance assumption drives the outcome more than the rebate rate does.


### Why do vendors refuse to accept card payments?


Acceptance costs them a percentage of every payment, typically in the two-to-three-percent range, which bites hard at low-margin businesses. Visa's 2024 acceptance research identifies high fees as the primary barrier cited by suppliers who don't take commercial cards. Suppliers with strong cash positions gain little from faster payment and have the least reason to accept.


### How do you run a rebate program without damaging supplier relationships?


Enroll before you switch, negotiate rather than announce, and treat a refusal as a routing decision rather than an objection to overcome. Suppliers who decline get paid by ACH or check instead. Programs that mandate cards across the board are the ones that produce convenience fees, disputes, and supply disruption.


### Which AP spend should stay on ACH or check instead of a card?


Keep spend on ACH when the supplier is fee-sensitive with negotiating leverage, when a surcharge would be passed back to you, or when the relationship carries supply risk. Public-sector payees, regulated recipients, and low-margin distributors generally belong here permanently, as do payroll, tax, and intercompany transfers.


### How do virtual card rebates also improve working capital?


Payment leaves your account when the card statement settles rather than when the supplier is paid, so your days payable outstanding extends by the billing cycle while the supplier still receives funds immediately. RPMG's 2022 benchmark survey found a large majority of companies reported improved working capital positions after adopting virtual cards.


#### David Luther


###### Product Marketing Program Manager


David Luther, MBA is a product marketing program manager with years of experience in commercial banking, finance, and technology sectors, with research and writing appearing in financial publications.


Payments Automation


Commercial Cards


##


1. How does AP spend become rebate revenue?


1. Which AP spend actually qualifies for rebate?
2. How much rebate can you realistically earn?


2. Why do rebate programs damage vendor relationships?


1. Why do suppliers refuse card payments?
2. How does managed enrollment change the acceptance math?


3. How do you design the payment mix to maximize rebate without forcing cards?


1. When should a supplier stay on ACH or check?
2. How does virtual card rebate also extend your working capital?


4. How do you measure and prove rebate capture?
5. How Corpay turns AP spend into measurable rebate revenue


#### Switch to Corpay


Discover how making the move to Corpay streamlines payments and strengthens your business.


[Talk to an Expert](https://www.corpay.com/resources/blog/rebate-optimization-ap-spend#contactUsSection)
