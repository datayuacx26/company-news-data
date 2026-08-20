---
schema_version: "1.0.0"
document_id: "7206dd6faf803b2465b7a45fbb9d7f110cd25f9cca33472279d2d0a6a7390776"
company_key: "yc-mercoa"
company: "Mercoa"
source_id: "yc-mercoa-news-import-107a1845dd35"
canonical_url: "https://mercoa.com/blog/planning-for-b2b-billpay"
published_at: "2023-10-20T00:00:00+00:00"
first_seen_at: "2026-07-22T04:05:05.455716+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:2b312d664a77195c6d74d49142f3b7e6a991578f1c95c410a9b4c9e2a0aee3f0"
---

# The Product Manager's Guide to Planning for B2B BillPay

# The Product Manager's Guide to Planning for B2B BillPay


Everything you need to know to launch Accounts Payable automation to your users


Sai Arora - October 20, 2023


Building a B2B bill payment (BillPay) product requires careful planning and execution. Here is a step-by-step guide to help you develop a plan to properly validate if B2B BillPay is right for your users. The time frames will vary based on your organization's product management strategy. Ideally, this should not take you more than a quarter


1.


Identify your Target Market


2.


Identify your Target Market’s Pain Points


3.


Identify Risk, Legal & Compliance Needs


4.


Define your MVP


5.


Ship your MVP


6.


Come out of Beta


## Identify Your Target Market


The first thing you need to do is determine which of your customers or target audience WANT an AP automation product (hint: it’s probably not your entire customer base).


The first thing you need to do is figure out which customer industry segment is most likely to adopt your AP product. You can get an understanding of this by looking at both industry and revenue data for your customers.


Certain industries (like construction, food & beverage, real estate) tend to have more invoice & document-heavy workflows compared to others (eg. freelancing, white-collar consulting). You’ll probably notice a variety of competitors offering stand-alone AP solutions catering to this audience as well (this is a good sign and something we’ll touch upon further in this guide).


Along with industries, you will also want to account for your customers annual revenue. The general rule of thumb is that businesses with healthy revenue ($10M+) will have a larger volume of expenses and spend that they need to manage beyond card programs & receipt tracking. For these types of businesses, their vendors will primarily be sending them invoices that need to be paid via ACH vs payment links for credit cards.


Finally, as you uncover your target audience, you will want to pay attention to how many of your current customers reflect your target audience AND how many of your current prospects reflect your target audience. If 20% or more of your current customer or prospect base reflects your target audience for your AP automation solution, then you have a strong signal to move forward with your AP product. Otherwise, you willl want to consider redefining your target audience requirements.


## Identify Your Target Market’s Pain Points


After you’ve gone through the exercise of determining who wants AP automation, you may find that the audience that wants AP automation from you also represents your larger customers. You will now want to dive deep with this audience to understand their payment pain points. Keep this audience list close and continue to engage with the audience at large as you learn about new pain points.


Here are some common pain points & signals that indicate potential for an AP automation solution:


-


Your users are spending too much time on manual data entry of invoices & financial documents


-


Your users have strained relationships with their vendors due to delayed payments


-


Your users have limited visibility into their overall spend & liabilities


-


Your users’ internal team spend a large amount of time dealing with vendor requests for updates on payment status and submitted invoices


You want to constantly be validating your initial thesis around the pain points of your customers. For some customers, managing the AP process may be a cash flow maintenance exercise. For others, managing the AP process may be an exercise in keeping the business organized. The insights you uncover here will be key to understanding how you ultimately build & offer a differentiated AP experience fine-tuned to your customers needs.


Once you’ve uncovered your customers main pain points around AP, you will want to run a competitive analysis on existing solutions in the market to identify their respective strengths, weaknesses, and where your customers have found short-comings in the solutions.


You will also want to dive deeper into how your customers are currently handling their AP process; sometimes your biggest competitor may just be the status quo of a spreadsheet, email, and a bank portal. Ultimately, the data you gather from these customers will end up informing your MVP, your GTM strategy, and your business model for this product.


## Identify Risk, Legal & Compliance Needs


In launching an Accounts Payable automation product, you are signing up to deliver a financial service to your customers. This means that you are responsible for mitigating any legal & compliance risks involved with launching this product. If you ignore these risks, you are setting yourself up for a whirlwind of fines, fees, and even potential jail time.


Make sure to consult with legal experts about the regulations that govern B2B payment processing in your target market(s). The industry & geography you serve will ultimately define the compliance requirements you will need to uphold. You will also want to work with these experts to ensure your product complies with local and international regulations. Along with this, you will want to flag the need for robust security measures such as encryption, tokenization, and multi-factor authentication to your engineering team. Thankfully, there are quite a few legal firms that specialize in B2B payments advisory services including:


-


Orrick


-


Paul Hastings


-


DLA Piper


## Defining your MVP


By the time you are done running your customer discovery process and your competitor analysis, you will likely have a clear understanding of what your customers are expecting from an AP automation solution. With this, you can now start to define what your MVP (minimum viable product) should look like.


As we covered in[The Insider's Guide to Accounts Payable](https://mercoa.com/blog/insiders-guide-to-accounts-payable) , there are **eight** **components** to every AP automation solution. While all eight of these components are important to delivering a holistic AP solution, all eight components do NOT need to be shipped in your MVP. Your customer discovery process will help you uncover which features are **absolutely necessary** for initial go-live.


The goal of your MVP is to **understand** what is absolutely crucial to have your audience manage their AP process on your platform. The goal of your MVP is NOT to be a feature parity replacement for a market leading AP Automation Platform from day one. For most companies, an AP MVP is *usually* comprised of (but not limited to) the following features:


1.


A document upload/aggregation for all open invoices: a single place for users to upload open invoices that they need to pay


2.


A payment scheduling engine to schedule payments for a specific day: let’s users set when they want to make a payment to line up with cashflow availability


3.


Basic approvals workflow functionality: lets your larger customers bake their approval process into your system


4.


Basic set of email notifications (approvals, payment scheduled, payment processing, payment complete/not complete + reasoning): Keeps users informed on the status of their payments


5.


Metadata functionality: Lets users tie invoices back to specific jobs, projects, or groups they are already tracking/managing within your product


6.


ACH payments out


7.


Security measures - Encryption, Tokenization & MFA


8.


Accounting System Sync: Lets users sync their bill payments to specific chart of accounts within their accounting/bookkeeping system


These features help you lay the foundation for a robust AP automation experience that can evolve alongside your users' adoption. They are also relatively lightweight builds and require minimal engineering effort to ship a usable product to your customers. Remember, your goal with your MVP is to understand what is absolutely crucial to your audience. Offering a lightweight MVP like this gives you the space to pivot, iterate, & improve upon your solution in a rapid manner. If you find your MVP taking more than two sprints to build, regardless of your engineering team size, you may want to reconsider your initial feature set.


## Ship your MVP & start gathering insights


If you’ve been following this guide step-by-step, by this point, you should have:


1.


A customer audience you’ve been actively engaging with


2.


A clearly scoped MVP that is ready to be launched


This is where the real magic happens.


You will now want to ship this MVP to your customer audience and start gathering feedback immediately.


In an ideal scenario, you will have invited your customers to some sort of private community (ex. slack, discord, whatsapp, etc) for them to have a direct line to send feedback to you and your team. Set a regular cadence for 30 minute check-in calls with these customers and show up ready to listen. At the same time, you’ll also want to prepare a metrics dashboard that tracks the following metrics month over month:


-


Total number of users onboarded


-


Total number of invoices or bills paid


-


Total number of vendors paid


-


Total Payments Volume


In shipping the MVP, you should set a tactical goal to get your customers to initiate a payment through your platform ASAP (ideally within the first two weeks of shipping your MVP).


You may find customers first start off by running small test transactions through the system. This is totally normal. They are trying to gain an understanding around payment timings in your system compared to how they are currently handling payments.


Some of your customers may already be using an AP solution or running a manual AP process today. In this scenario, you will need to do two things to convince them to try out your MVP. First, you need to showcase the value prop of unifying AP with the core functionality of your system. If you followed the step of correctly identifying your target market’s pain points, this should be a rather trivial thing to do.


The second thing you’ll want to do is offer white-glove support to migrate them over to your solution. This experience will include tasks like migrating all of their vendor information (names, contracts, payment disbursement IDs, etc) from their AP solution/spreadsheet into your system, setting up approval policies that mirror their current process (if applicable), and ensuring payment methods are properly configured.


If your users do not take actions like this within the first two weeks, re-engage with them immediately to determine why they are not taking the actions you want. Based on their feedback, iterate on your MVP until you start seeing the needle move in the direction you want. It is okay to need to build more on top of your existing MVP to make this happen. It is also completely okay to remove functionality from your existing MVP to make this happen. Embrace the ambiguity for the time being.


## Coming out of beta


Ideally, you will want to keep this MVP in a closed beta capacity until you start seeing repeat & predictable results from each of your users. During this time, as you are iterating on your MVP, dive deep into your customer’s perceived value of the product. Experiment with different pricing models & tiers to determine which approach jives best with the majority of your audience. You will want to feed this information to your finance team so they can start modeling out the unit economics of your product. You will also want to relay this information along with your insights to your marketing team so they can make the necessary preparations to take this MVP out of closed beta and into GA.


As you approach GA, you will want to start shifting gears towards more internal go-to-market enablement within your organization. This means:


-


Enabling your marketing team to develop marketing materials & messaging that communicates your unique value props to your target audience


-


Enabling your sales team to train reps to sell this new product effectively,


-


Enabling your customers support team with the tools & training they need to effectively handle any customer inquiries.


These three pillars are ***crucial*** to effectively launching AP to your broader audience.


Building a successful B2B AP Automation product requires a combination of understanding the needs of your target market, creating a user-friendly product, ensuring regulatory compliance, and providing exceptional customer support. By following this guide, you can develop a product that meets the needs of your customers and becomes a valuable tool for their business.


## Mercoa


Want to see how your plan compares against what we've seen work best in the market for B2B BillPay? Let's[talk](https://calendly.com/sai-mercoa/insider-s-guide-to-accounts-payable-followup) .
