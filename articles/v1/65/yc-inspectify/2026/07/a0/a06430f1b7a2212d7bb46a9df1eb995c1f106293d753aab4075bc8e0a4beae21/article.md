---
schema_version: "1.0.0"
document_id: "a06430f1b7a2212d7bb46a9df1eb995c1f106293d753aab4075bc8e0a4beae21"
company_key: "yc-inspectify"
company: "Inspectify"
source_id: "yc-inspectify-news-import-247bec52b53d"
canonical_url: "https://www.inspectify.com/blog/the-path-to-financial-automation"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-22T00:03:40.491241+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:912451c697da663838aa36dc6bb3e6865d50e3cbf5f3ca302b866363401653d1"
---

# The Path to Financial Automation

For Everyone


# The Path to Financial Automation


July 15, 2026


When I joined Inspectify as the first hire in the Finance department four years ago, I was tasked with developing a more cohesive structure for our accounting operations. The outsourced accountants were working in Quickbooks with limited context about the business. “The books” were a bit of a mess. I wasn’t surprised. This isn’t my first start up. My goal was to clean things up and build the foundation to scale the business so that the Finance department could easily enable and digest rapid growth, without being a bottleneck.


‍


At the time I joined in early 2022, the one thing that was fairly well structured was how we paid our contractors, mostly inspectors. Our CTO had configured an API connection with Brex such that any time a job status changed to “completed” in the Inspectify app, it would queue payment via ACH the next day for the inspectors. This automated the payout of ~85% of our expenses, the remainder being software subscriptions, lawyers, etc that we paid via credit card (also Brex) or via Bill.com.


‍


The other side of the coin, revenue operations, was quite the opposite. I’ll spare you the intricate details, but there was an invoice template Google sheet in which we would copy and paste data from the database, save it as a pdf, attach it to an email and send it to the customer. Each invoice was essentially “hand made” with a corresponding list of customer requests and requirements. Customer A needs individual invoices for each job, Customer B wants a bulk invoice weekly, Customer C wants bulk monthly, Customer D wants invoices to be grouped by region, etc. Purchase and work orders were tracked manually as well.


‍


After we formed a subsidiary in Canada I knew Quickbooks wouldn’t suffice as our ERP. The first major upgrade was to select and implement a mid-market ERP system. Having implemented NetSuite, Sage Intacct, and other systems in the past I knew Sage Intacct would be the best fit. Why? NetSuite, in my humble opinion, is a great fit for companies who want to embrace the “suite” approach with a one-stop-shop for everything from your general ledger and financial reporting to your inventory management system. NetSuite can even be your CRM. Sage Intacct has a narrower focus as just a general ledger, financial reporting, and - importantly for us - is great at consolidating multi-currency environments with our new Canadian sub. I call it “best of breed” meaning it’s more focused on doing a core function really well while ready to integrate with other off-the-shelf solutions that do other stuff (collections, CRM, etc).


‍


With the upgrade from Quickbooks to Sage Intacct done in the summer of 2022, we quickly moved on to the revenue operations, aka Accounts Receivable, side of the coin. The unique (read “difficult”) part of Inspectify’s invoicing and collections was, and still is, the diversity of billing configurations. It is both the aforementioned per job, bulk, and grouping of invoices **plus** the added layer of complexity that sometimes the person placing the order isn’t responsible for payment. One billing type, for example, we call “bill to escrow” which is exactly what it sounds like. The customer provides the escrow agent’s information and we send them the invoice to (hopefully) get paid at closing.


‍


Having been a consultant many years ago, I pulled an old gizmo out of the tool box. I still remember the “consultant-y” terminology. An “evaluations matrix.” Matrix makes it sound complicated, but it’s just a rubric in which you list the features and functionality desired, rank them in a “must have,” “nice to have,” etc fashion and then use said criteria to evaluate all options available. We used the evaluations matrix to look at every off-the-shelf revenue tool we could find. Versapay, High Radius, Stripe Connect, Upflow, Unit, Coupa, etc.


‍


We ended up implementing[North49](https://www.north49.com/) in 2023. It was a small Canadian company based in BC that had built a software solution on top of EVO Payments’ merchant account. It integrated with Sage Intacct and checked (almost) all the boxes on our evaluations matrix. At about the same time, EVO Payments acquired North49 and[Global Payments](https://contact.globalpayments.com/general-payments/?utm_source=google&utm_medium=cpc&utm_content=22446243491-799364233093&utm_campaign=25Q1_RG-Mktg_GB_GP_20904-WAV_PD_BRANDED-G-SEARCH&utm_term=global%20payments%20merchant%20account&gad_source=1&gad_campaignid=22446243491&gbraid=0AAAAACR8Jwp6X2L4OdeCZpshe0qTtaUOn&gclid=Cj0KCQjwrs7RBhDuARIsAIVfBD0UFUWbBpUGE4EOBwOx-x3SCHT16m5M3fZfTy_ExDVijW-zQNUKsHUaAniqEALw_wcB) (NYSE: GPN) acquired EVO. But I digress.


‍


Unlike our expenses, which have now switched from Brex to Tipalti (separate story), we don’t have full end-to-end control over our revenue operations. Specifically, I am talking about how our customers pay us. We can send an invoice and ask that customers pay one way or the other, but ultimately our invoice becomes a bill in their accounts payable process, whatever that may be. Inspectify is “just another vendor” to whomever is sitting in the Accounts Payable chair at each customer.


‍


In 2025, we realized that we had streamlined and automated just about every aspect of our revenue operations that we possibly could, at least with off-the-shelf solutions. Invoices were imported, not “hand made,” sent via email from our ERP system, and customers could click a link on our invoices to pay via credit card or ACH. However, unlike[Field of Dreams](https://en.wikipedia.org/wiki/Field_of_Dreams) , we built it and our customers (mostly) did not come. Specifically only about 10% of our revenues were paid via North49, which we started calling “our billing portal.” For the 90% who paid in other ways (Coupa,[Bill.com](http://bill.com/) , Ramp, check, wire, etc), we still have to look at the bank transactions, cross reference the remittance advice or check memo, and - too often - clarify with the customer before manually reconciling payments and marking invoice(s) as paid.


‍


Here we are in 2026 and ~90% of our customers still don’t use our billing portal to make payment. And having started my career working for a public company doing accounts payable, I get it. Inspectify is “just another vendor.” And unlike Amazon or Google, we don’t (yet) have the clout to demand our customers pay via a specific payment method.


‍


So along comes AI. So hot right now, I know. But hear me out. The manual process of cross-referencing remittance advice emails, looking at the memos on check pdf scans, and simply narrowing down whether you can reconcile a payment from a customer based on whether the dollar amount could be anything other than the invoice you think it should be is something AI could presumably do. Right? Simple, but tedious given we do 20,000+ jobs and receive almost a thousand payments (big and small) each month.


‍


Insert Claude Cowork (yeah, the AI tool from Anthropic) which we are now delegating work to as if it were an employee. Claude has “its” (correct me on the pronoun please) own email address, its own user profile (with very limited access) in our ERP system, and receives all remittance advice, check pdf scans, etc. It is currently being tasked with the aforementioned tedious yet simple task of cross-referencing, reconciling, and posting customer payments. Hopefully soon, us humans will simply have to step in with the few transactions Claude isn’t able to figure out on its own.


‍


The goal, to be clear, is to avoid manually reconciling thousands of payments as we grow. One way to do it would be to require or incentivise our customers to pay via our billing portal, in which payments are posted in our ERP automatically and thus require no reconciliation whatsoever. We might pull on that lever too at some point, but for now we are giving Claude a chance to close the gap. And in our first test run in May, it increased the 10% we don’t need to manually reconcile to just over 50%. As we feed Claude more context, I’m confident the % will continue to rise. And if not, we hope you’ll understand why we might start charging our customers a “check processing fee.” It is 2026 after all. 🙂


‍


‍


By Kyler Farr, Head of Finance
