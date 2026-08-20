---
schema_version: "1.0.0"
document_id: "fdaf9b3aeb34524d34c85a299afa5f215357f535700c3674c121007f56a01908"
company_key: "yc-ekho"
company: "Ekho"
source_id: "yc-ekho-news-import-71d1330defc8"
canonical_url: "https://www.ekho.com/blog/an-open-letter-to-the-dms-category"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-22T17:41:56.838521+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:d45f2ca28ec1a7f637fd0b9e831e7683f245c3e16d7f6dc792d7380da43ce705"
---

# The DMS Is Not Dead. It’s Just Doing the Wrong Job.

AI summary


The dealer management system is one of the most useful pieces of software in the store, and one of the most overloaded. For twenty years it has been asked to be five products at once and blamed for the four jobs it was never built for. This is an open letter arguing the DMS gets its credibility back the moment it goes home to the work it does best.


*The dealer management system is one of the most useful pieces of software in the store. It is also one of the most overloaded. We have spent twenty years asking it to be five products at once, accounting, inventory, customer relationship, sales communication, dealer-of-record storefront, and then blaming it when the four jobs it was never built for run badly. The DMS is not dead. It is doing the wrong job. The category gets its credibility back the moment it goes home to the work it does better than anyone else and opens the doors for the rest of the store to live elsewhere.*


##


We are not here to bury your DMS. We are here to ask it to stop trying to be your whole store.


A confession before the argument.


We have run dealerships on more than one DMS, in more than one configuration, and we keep coming home to the same conclusion. The good ones are very good at the work they were built for. The accounting subledgers, the parts-and-service ledger, the inventory cost roll-up, the month-end close, the fixed-operations reporting, the way a well-configured DMS lets a controller close the books on the fifth working day without a single late-night spreadsheet. That work is real, and the category that does it well is not replaceable by anything any of us are building.


So this is not an obituary. The DMS is not over.


What we are going to say is something the category has been quietly avoiding for a decade. The DMS has been doing five jobs for twenty years and was only ever built for one. The other four got strapped on because nobody else was offering, and because owning the dealer’s data felt like enough leverage to own everything that touches it. Those four jobs have eaten the category’s reputation. The dealer principal who tells you over Monday-morning coffee that her DMS is “the worst piece of software in the store” is wrong about which product is failing her. The product is fine. The product is doing four jobs nobody designed it to do.


We are writing this because the path back to a healthy DMS category, and a healthy dealership operating layer alongside it, requires saying out loud what the category will not say about itself.


*The DMS is not dead. It is doing the wrong job. The fix is going home.*


##


The one job the DMS was built for, and still does better than anyone.


Walk into the back office of a well-run rooftop on the second working day of the month and look over the controller’s shoulder. The DMS is doing exactly what it was built to do.


The new-vehicle inventory subledger reconciles against the floor-plan financing statement. The used-vehicle subledger reconciles against the appraisal log. The parts subledger reconciles against the manufacturer parts statement. The service repair-order ledger flows through to the customer-pay, internal, and warranty subledgers. The technician productivity report lands on the service manager’s desk. The fixed-operations gross-profit roll-up lands on the dealer principal’s desk. The month-end financial statement gets uploaded to the manufacturer in the format the manufacturer demands. The controller closes the books on schedule.


None of that is glamorous. It is also the hardest software problem in the dealership. The DMS holds the canonical ledger for every dollar that moves through the store, in a regulatory environment that requires it to be auditable, against four external statements it has to reconcile, against an inventory that physically moves and has VINs the manufacturer revises.


That is what the DMS was built for. That is what every well-run DMS does well. *That is the job.*


We do not want a future where a controller closes the books inside a CRM, or a salesperson updates inventory cost inside a chat tool, or a parts manager reconciles the manufacturer statement inside a website builder. The accounting subledger is infrastructure, and infrastructure does not need to be re-imagined by every venture-backed startup that finds the dealership category interesting this quarter.


The DMS, doing the DMS job, is one of the most useful pieces of software in the store.


##


The four jobs the DMS was never supposed to do, and the bill we have all been paying for it.


The trouble started fifteen years ago, when the DMS category looked at the dealer’s screen and noticed what every entrenched category notices. *Everything that touches a deal eventually touches the DMS.* The lead lands somewhere, but the deal funds in the DMS. The salesperson talks to the customer somewhere, but the deal jacket lives in the DMS. The website sells the vehicle somewhere, but the inventory ledger is in the DMS.


The temptation was reasonable. *If we own the canonical ledger, why not own everything upstream of it?* Build a CRM on the same database. Build a desking tool off the same inventory. Build a website that reads the same feed. Build a service-scheduling tool that books against the same calendar. Build a communications platform that logs into the same activity stream. The pitch to the dealer was integration. The pitch to the investor was account expansion.


The pitch made sense on paper. Not in practice. The category that was excellent at accounting subledgers turned out to be mediocre, on a generous reading, at customer-facing software. Here are the four jobs the DMS picked up and never grew into, in the order the dealer feels them.


*Customer relationship and lead routing.* A DMS built for the canonical deal ledger is the wrong shape for the customer-facing funnel. The data model was designed around the deal, not the buyer. The activity log was designed around what the salesperson did, not what the buyer wanted. The DMS-attached CRM module reports on form submissions and call logs because that is what its database knows how to record. The buyer’s research process, the sixteen weeks of activity before she submitted a form, is invisible. The BMW Motorrad dealer in Pittsburgh running her CRM through her DMS-attached module pays per-seat for a surface that records the wrong moment. *The product is doing what it was designed to do. The product was designed to do the wrong thing for this layer.*


*The dealership’s website and digital storefront.* The DMS-attached website builder exists for the same reason every DMS-attached anything exists: the inventory ledger is in the DMS, so the simplest way to keep the website in sync is to ship a website that reads the DMS feed. That is the only reason these websites still exist. They lose every other contest. They lose to specialized website builders on design quality. They lose to modern content platforms on page-load time and Core Web Vitals. They lose to schema-and-citation-optimized platforms on generative-search visibility. They lose to transaction-engine-backed storefronts on the ability to close a deal online. The Newmar RV dealer in the Twin Cities running her storefront on the DMS-attached module gets a site her CFO loves for inventory accuracy and her buyers, in surveys, tell her feels ten years old. *The only thing the bundled site wins at is convenient bundling with the system the dealer was already paying for.* That is not enough to win a category. The forensic version of what the bundled-website pattern costs in lost generative-search visibility and per-lead cost is in[The $50B Dealer Tax: What Your Stack Really Costs](https://www.ekho.com/blog/the-50-billion-dealer-tax) .


*Customer communications, lead response, and after-hours coverage.* The DMS-attached communications platform is a thin layer over SMS and email, integrated with the CRM module that was already the wrong shape. It logs a conversation. It does not run one. The 11:47 PM Saturday lead from Facebook Lead Ads sits there until Monday morning, because the DMS-attached comms module is a logging tool, not a sales agent. The Bennington marine dealer in Milwaukee paid for the seat anyway. The buyer left for the Sea-Doo store nineteen miles away.


*The customer-facing transaction surface.* The DMS records the funded deal. The DMS was never designed to be the surface the buyer transacts through. The customer-facing checkout, financing, signing, titling, and registration work got farmed out to per-deal vendors precisely because the DMS could not do it, and the DMS-attached “online retail” modules that did get built are reskins of a desking tool never designed for an out-of-state buyer in 2026 trying to close a Sea-Doo deal at 9 PM Tuesday. The DMS still gets to log the funded deal at the end. The buyer’s experience is shaped by the per-deal vendor stack. The DMS gets blamed for the buyer’s experience anyway, because the dealer principal sees the bill for it on the same statement.


Add the four up. The DMS category took on four jobs it was never designed to do, charged premium per-month-per-user pricing for the bundle, and watched its reputation get eaten by software it did not build and could not get to product-market fit on. *The four jobs ate the one job’s reputation.* The controller closing the books on the fifth working day does not get the credit. The CRM module that mis-categorizes 180 form fills a month gets the blame.


That is the bill the category has been paying. The dealer is angry. The DMS deserves better. The dealer deserves better than a single vendor straining to be five products.


##


We have an opinion about what should happen next, and it is not what you might expect.


If we were writing the standard indictment, this is where we would announce that the DMS is over and that Ekho is here to replace it. We are not, because it is not what we believe.


We believe the DMS, doing the one job it was built to do, is one of the most important pieces of software in the dealership. We believe the category should be allowed to go home to that job, do it brilliantly, charge fairly, and be respected for it. We believe the dealer principal in Austin should be able to look at her back-office stack at the end of the month and say *that worked,* the same way she does with her electricity bill.


What should happen is a great unbundling, but a polite one.


The four jobs the DMS picked up and never grew into should go to products designed for them, by teams that wake up every morning thinking about them. The CRM and lead routing should live in software designed around the buyer’s research-and-conversation funnel, not the deal jacket. The website and digital storefront should live in software designed around generative-search citation and transactable checkout, not inventory feed compatibility. The communications and lead response should live in software designed to run conversations, not log them. The customer-facing transaction surface should live in software designed for fifty-state titling, registration, financing, and signing, not a desking module reskinned for the web.


Those four products are what we, taken as a category, call[the DealershipOS](https://www.ekho.com/blog/the-last-generation-of-dealership-software) : the customer-facing operating layer of the dealership, in one product instead of stitched across fourteen. *The DealershipOS is not the DMS killer. The DealershipOS is the DMS friend.* It takes the four jobs the DMS was never supposed to do off the DMS’s plate, leaves the one job the DMS does well exactly where it is, and connects through integrations that respect what the DMS is canonical for and what it is not. Where the DMS is the back-office system of record, the DealershipOS is the front-office system of engagement.


Ekho DealershipOS is our instantiation of that category. It is not the DMS. It does not want to be. It does not store the canonical accounting ledger, does not reconcile against the floor-plan statement, does not run the parts subledger, does not close the books. It runs the customer-facing layer the DMS was never built to run, and writes the funded deal back to the DMS the moment the deal closes. Two pieces of Ekho DealershipOS ship today, the AI Sales Agent and the 50-state Transaction Engine, and the AI-native storefront is the next piece coming online.


That is the only honest relationship between the two categories. The DMS owns the back-office ledger of record. The DealershipOS owns the customer-facing ledger of experience. The integration between them is the seam. The dealer pays one bill for the back office, one bill for the DealershipOS, and gets two products excellent at one job each instead of one product mediocre at five. The partner posture this requires is the one we argued for in[Dealers Don’t Have a Vendor Problem. They Have a Partner Problem.](https://www.ekho.com/blog/dealers-dont-have-a-vendor-problem) , applied at the back-office seam instead of the customer-facing one.


If the DMS category hears this and decides to fight for the four jobs it picked up by accident, it is on the wrong side of its own history. The DMS will lose every one of those four contests over the next five years, not because we are better at building software, but because we are building one product around one job and the DMS category is building five products around one ledger. The dealer will eventually unbundle. The only question is whether the DMS category gets to choose how and when.


If the DMS category hears this and decides to go home to the work it does well, it gets its credibility back in eighteen months. The dealer principal stops cursing the DMS on Monday morning, because the DMS stops being the thing that mis-categorizes her leads and loses her Saturday-night Facebook ad. The DMS gets to be the back-office system of record that closes the books on the fifth working day and gets a thank-you, instead of the bundled provider that takes the blame for the entire stack.


We would prefer the second world.


##


Where the DMS should be focused now, and the integration posture we are asking for.


We owe the category a clear statement of what we are asking, because the quick version of this argument (“everyone unbundle, immediately”) is also the wrong version. The dealer cannot move four jobs to four new vendors in one quarter, and the DMS cannot stop offering customer-facing modules on a Tuesday. The transition has a shape worth being explicit about.


Where we would like the DMS category to invest, in plain language.


*Be the best accounting and ledger product in any vertical software category, anywhere.* Powersports, auto, RV, marine, golf cart, lawn-and-garden, agriculture-equipment. The DMS is the canonical accounting and inventory ledger for the dealership. Be that without compromise. The category that gets this right has a permanent seat at the table.


*Be the best fixed-operations product.* The service department is the dealership’s most profitable square footage most weeks. The DMS owns the repair-order workflow, technician productivity reporting, warranty-claim routing, parts-attach math. There is twenty years of clear runway to make every one excellent. No category outside the DMS is going to compete here, because no category outside the DMS has the data.


*Be the best inventory-cost and floor-plan product.* New-vehicle subledger, used-vehicle subledger, floor-plan reconciliation, aging report, inventory turn math. This is what dealer principals look at Monday morning when they want to know how the business is doing. The DMS owns it. The DMS should be obsessed with it.


*Be the best manufacturer-reporting and compliance product.* Every manufacturer demands a financial-statement upload in a specific format on a specific cadence. Every regulator demands an audit trail. The DMS is the only category that can do this work cleanly, because it is the only category that holds the ledger the reporting is computed from.


*Be the best integration partner in dealership software.* This is the part the category has been worst at. The DMS that wins the next decade publishes a documented, modern, well-maintained interface to the rest of the dealer’s software stack. A real API. Real webhooks. Real event streams. Real partner certifications. The DMS that treats its data as a moat will lose. The DMS that treats its data as infrastructure it gets to host for the dealer will win. The data belongs to the dealer either way. The DMS that admits this first gets a generation of partnership equity the data-as-moat camp will never recover.


What we are explicitly asking the DMS category not to do.


*Stop building modules that compete with specialized customer-facing software.* The CRM module, the website builder, the digital-retailing module, the AI chat add-on, the lead-routing layer, the marketing automation. Every one is a category where a focused product will beat a DMS-attached bundle. *The bundle is the brake. Let the bundle go.*


*Stop pricing the integration as the value proposition.* The integration is not a feature the DMS gets to charge for. It is the dealer’s right of ownership over her data. Pricing it as a value-add tells the dealer the data does not belong to her. The dealer notices.


*Stop using the DMS contract as leverage on the rest of the stack.* The “DMS-certified partner” program, the integration fee charged to other vendors, the data-egress restriction, the per-record API throttling. Short-term revenue tactics eroding long-term defensibility. The vendors paying those fees are quietly building around them. *The leverage is borrowed.*


What we, the rest of the category around the DMS, are asking for in return.


*Give us clean integration points and we will give you a permanent seat in the dealership software stack.* We will write the funded deal back into your ledger every time. We will respect your subledger structure. We will not try to be the accounting product. We will not try to close your books. We will not try to be the canonical inventory ledger. We will be the customer-facing layer that hands you a clean deal at funding, and asks for inventory and pricing data through a real interface you can audit.


That is the détente we believe the category is heading toward, whether by design or by attrition. We would prefer by design. We are writing this because the design version is better for everyone in the picture, including the DMS category itself.


##


What a dealer can do this week, while the rest of this argument plays out.


We owe the dealer a concrete first action, because the rest of the argument plays out on a timescale longer than this quarter.


You cannot replace your DMS this week. The contract is long, the migration is hard, the back-office dependencies reach into accounting and fixed-ops, and the back-office work the DMS is doing for you is fine. *That is not the move.*


The move is to stop letting your DMS own the customer-facing surface of your store, while keeping the back-office relationship with whichever DMS you are on. You can do that this week, in two specific actions, with two products that ship today.


*Action one. Let an AI Sales Agent own the conversation layer, not the DMS-attached CRM module.* The Ekho[AI Sales Agent](https://www.ekho.com/sales-agent) runs across SMS, chat, email, and Facebook Lead Ads. It engages the buyer with full inventory knowledge and live financing prequalification, the moment she raises her hand, in the channel she actually uses. It writes the funded conversation back to whichever CRM surface you want, including your DMS-attached one. The 11:47 PM Saturday Facebook Lead Ads notification gets a real conversation Saturday night, not a Monday-morning autopsy. Your DMS keeps doing the books on the fifth working day. The two products mind their own business.


*Action two. Let a Transaction Engine own the out-of-state and online-retail deals, not the DMS-attached digital-retailing module.* The Ekho[Transaction Engine](https://www.ekho.com/transaction-engine) runs the credit application across a lender waterfall, attaches F&I products inside the purchase flow, generates the deal jacket, captures the signature, files the title in the buyer’s state, registers the vehicle, clears the interstate tax, and routes the funded deal back to your DMS in the format your DMS expects. The work the per-deal vendor stack used to do for $155 a deal is bundled into the DealershipOS. Your DMS keeps the canonical record of the funded deal. Your buyer in Pittsburgh takes her BMW Motorrad unit home Tuesday. The Subaru dealer in Portland closes her first out-of-state deal of the quarter Tuesday afternoon, titled in the buyer’s state by the end of the week, with the funded deal posted back to her DMS by Friday. *One out-of-state deal a month through this path saves enough to offset the platform’s monthly cost.*


Both actions are at the edge of your stack. Neither requires you to rip out your DMS, renegotiate your DMS contract, or even tell your DMS provider what you are doing. They are concrete first moves on the right side of the unbundling, in the parts of the funnel where the DMS was never supposed to be the canonical product anyway. *Start at the edges. The core, the parts of the DMS doing the work the DMS was built to do, can stay exactly where it is.*


##


The détente, in one paragraph.


We do not want the DMS to die. We want it to be excellent. We want the dealer principal in Austin to look at her DMS bill and feel she got her money’s worth, because the DMS is doing the one job it is built to do, and doing it brilliantly. We want the rest of the dealership software stack, the customer-facing layer the DMS was never built for, to live in products designed for that work. We want the two categories to meet at a clean, modern, well-engineered seam, where the customer-facing layer hands the back-office layer the data it is canonical for, and the back-office layer hands the customer-facing layer the data it needs to do its job. We want the dealer to pay two fair bills for two excellent products, instead of one bloated bill for one product trying to be five.


*The DMS is not dead. It is doing the wrong job. The fix is going home, not going away.* We are writing this because we want the category to get there on purpose, in time, with the next ten years of dealership software ahead of it instead of behind it. The dealer is going to unbundle either way. The DMS category, doing its real job exceptionally well and integrating cleanly with everyone else, gets the seat at the table for as long as dealerships exist. We would like to see that future. We would like to be a partner in it.


*Ekho*


## Frequently asked questions


No. We do not store the canonical accounting ledger, do not run the parts-and-service subledger, and do not close your books. We are the customer-facing operating layer of the dealership, the DealershipOS, and we connect to whichever DMS you are running today. The category is defined in Vendor Stacks Are Dead. Long Live the DealershipOS.


We connect to whichever DMS will work with us, on the cleanest integration the DMS provides. Quality scales with the quality of the interface the DMS publishes. We are an active partner with DMS providers willing to publish modern, documented, real-time interfaces. We are a polite stranger to DMS providers that treat dealer data as a moat.


No. The honest version of this argument is the opposite. The DMS is doing real work in accounting, inventory, fixed-operations, and manufacturer reporting. That work is hard, the DMS does it well, and you do not want to migrate it on a quarterly timeline. What we are saying is that the customer-facing modules your DMS sells you, the CRM, the website, the digital-retailing layer, the communications add-on, are not products the DMS does well, and you can replace those at the edges of your stack without touching the back-office relationship at all.


Because the four jobs the DMS picked up over the last fifteen years (CRM, website, communications, digital retailing) are the four the dealer principal interacts with on Monday morning. The accounting and fixed-ops work the DMS does brilliantly is invisible to the customer-facing side of the business. The visible jobs are the ones the DMS was never designed for. The category is taking blame for products it should never have shipped.


Go home to accounting, inventory, fixed-operations, and manufacturer reporting; be excellent at those; publish a real integration layer; let the customer-facing layer live with the products that specialize in it.
