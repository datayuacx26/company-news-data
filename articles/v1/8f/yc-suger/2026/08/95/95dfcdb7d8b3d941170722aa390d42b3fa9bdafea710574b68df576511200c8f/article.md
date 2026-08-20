---
schema_version: "1.0.0"
document_id: "95dfcdb7d8b3d941170722aa390d42b3fa9bdafea710574b68df576511200c8f"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/buy-with-aws-metrics/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:bab161b58068371f6b65a54af96032c6dbeb631926062c392f95478cec738e8c"
---

# Four Metrics to Watch After You Add Buy with AWS

*Buy with AWS is a set of AWS-branded call-to-action buttons a seller places on their own website, which take a buyer into a co-branded AWS Marketplace purchase flow. AWS describes it as “a streamlined way to find, try, and buy products from Partner sites” — and it makes your marketing site a measurable part of the marketplace funnel.*


---


Adding Buy with AWS is usually treated as a web project. Someone adds a script, adds buttons, ships it, and the work is considered done.


Then the question arrives — “is it working?” — and nobody has an answer, because the team that shipped it was measuring page conversions and the team that owns marketplace revenue was measuring agreements, and no one had connected the two.


AWS actually reports on this. There is a dashboard, it is free, and most sellers who have deployed the buttons have never opened it. Here is what it contains, and the four ratios worth building a decision on.


---


## **What does AWS report for Buy with AWS?**


**AWS provides a Buy with AWS dashboard covering site traffic and marketplace outcomes together.** It sits under Insights → Marketing → Buy with AWS in the AWS Marketplace Management Portal, and is also reachable through the Analytics section of AWS Partner Central.


AWS describes it as “an overview of the web traffic, engagements, and agreements created by the customers who choose your **Buy with AWS** call-to-action buttons and visit your **Buy with AWS** procurement page on AWS Marketplace.”


Read that sentence carefully, because it draws a line most summaries of this dashboard blur. **Exactly one metric is measured on your website; the rest are measured on AWS’s procurement page.**


Metric Where it is measured AWS’s definition


Total button clicks **Your site** ”The total number of clicks on the call-to-action buttons, such as Buy with AWS, on the seller website”


Buy with AWS page views AWS’s page ”The total number of visits to the Buy with AWS procurement page. This includes repeat visits”


Total agreements AWS’s page ”The total number of agreements created on the Buy with AWS procurement page”


Unique visitors AWS’s page ”The total number of unique procurement page users”


Bounce rate AWS’s page ”The ratio of users who sign in and leave after viewing one site page to those who visit more than one page”


Average dwell time AWS’s page ”The time between when the user signed in and when that user left the page”


AWS sign-in drop-off rate AWS’s sign-in ”the ratio of users who signed in and created agreements to the number of users who only signed in”


So this is not, as it is often described, a dashboard about your website. It is a dashboard about **AWS’s half of the funnel** , with your button clicks as the entry point.


AWS records clicks on four specific calls to action, plus form submissions:


- **Buy with AWS**
- **Try free with AWS**
- **Request demo**
- **Request private offer**


The tracking comes from a script AWS asks you to insert —` https://bwa.marketplace.awsstatic.com/assets/partner.js` — plus data attributes on the buttons themselves. If your CTA buttons are missing those attributes, the clicks are not counted, and the dashboard will quietly report a funnel with no top.


**Check the instrumentation before you interpret anything.** A zero in this dashboard is far more often a missing data attribute than an absence of demand.


---


## **The buyer flow you are measuring**


Six steps, and each transition is a place to lose someone:


1. The buyer reaches a product page on your website.
2. They click Buy with AWS, or Try free with AWS.
3. They sign in to their AWS account if they are not already.
4. They land on a co-branded procurement page showing “offer details, pricing, and terms.”
5. They subscribe.
6. They return to your website for product setup.


Steps 3 and 4 are the ones outside your control and inside AWS’s. Step 6 is back in yours, and it is where an unprovisioned buyer becomes a support ticket — covered in[when a buyer pays but provisioning never happens](https://www.suger.io/resources/blog/marketplace-provisioning-failures/) .


---


## **The four metrics worth a decision**


Raw counts on this dashboard are close to useless — they move with traffic. The ratios are what change behaviour. Each of the four below names the decision it drives, because a metric that drives no decision does not belong on a dashboard.


### 1. CTA click rate — clicks ÷ page views


**The decision: whether the button is in the right place.**


This is a page design question, not a marketplace question. A low click rate on a page with healthy traffic means the button is below the fold, competing with a “Contact sales” CTA, or on a page whose visitors are not in a buying posture.


The useful cut is *per page* , not in aggregate. One product page usually carries the whole number, and the aggregate hides which one.


### 2. Agreement rate — agreements ÷ CTA clicks


**The decision: whether your public offer is buyable.**


This is the sharpest signal on the dashboard, because everything between the click and the agreement belongs to AWS: sign-in, the co-branded page, the offer terms, the subscribe action.


Clicks that do not become agreements usually mean one of four things: the buyer has no AWS account with purchasing rights, the public price is not the price they expect to pay, the terms need legal review, or the offer requires a conversation. The last two are not failures — they are a signal that the deal wanted a private offer instead. Which is what the fourth CTA exists for.


### 3. AWS sign-in drop-off rate


**The decision: whether AWS’s own steps are costing you deals.**


This is the metric nobody looks at and the one you cannot get anywhere else. AWS defines the drop-off rate as “the ratio of users who signed in and created agreements to the number of users who only signed in” — people who reached AWS, authenticated, and then left without buying.


That population is worth understanding, because it is not a marketing problem. They were interested enough to sign in. What stopped them was the offer, the terms, or the permissions on their AWS account. Bounce rate and dwell time on the procurement page tell the same story from the other side, and all three are measured on AWS’s page rather than yours — so do not read them as feedback on your own site.


### 4. Self-serve mix — Buy with AWS agreements ÷ total agreements


**The decision: whether this is a channel or a checkbox.**


Track the share of your AWS Marketplace agreements that originated from a Buy with AWS click, against those from private offers and direct marketplace listing traffic. If the share is not moving over a couple of quarters, you have a button rather than a channel, and the fix is usually placement and offer design rather than more instrumentation.


Watch the “Request private offer” CTA separately here. It converts into a human conversation rather than an agreement, so it will never appear in the agreement count — but it may be the highest-value button on the page. Counting it as a failed self-serve purchase is the most common misreading of this dashboard.


---


## **What the dashboard cannot tell you**


Three gaps, all structural.


**It does not know your accounts.** The dashboard reports visitors and agreements; it does not know which of your named accounts they belong to. “Did our top 50 target accounts click?” is not a question it can answer, and it never will be.


**It does not carry campaign attribution.** Clicks are attributed to CTA buttons, not to the campaign that produced the session. If you need to know which campaign generated marketplace agreements, that join has to happen in your own analytics, keyed on your session data.


**It is AWS-only, and so is the mental model it encourages.** Microsoft and Google Cloud have their own consoles, their own vocabulary, and their own refresh schedules.[AWS Marketplace seller dashboards](https://www.suger.io/resources/blog/aws-marketplace-seller-insights/) covers the other eight and their blind spots;[cloud marketplace metrics](https://www.suger.io/resources/blog/cloud-marketplace-metrics/) covers what a cross-marketplace reporting layer needs.


---


## **The instrumentation checklist**


Before you read a single number:


- **The script is on every page carrying a CTA** , not only the primary product page.
- **Every CTA carries the required data attributes.** Buttons styled to match your site are the ones that most often lose them in a redesign.
- **Branding review is complete.** AWS requires compliance with its trademark guidelines and the Buy with AWS creative and messaging guidelines, with branding review approval before launch.
- **Your seller profile logo is current** , since it appears on the co-branded procurement page.
- **The post-purchase return path works.** Step 6 is yours, and a buyer who subscribes and cannot get in has converted into a support ticket.
- **Someone owns the dashboard.** A metric with no owner is a metric nobody acts on.


Suger was the AWS launch partner for Buy with AWS —[the launch post](https://www.suger.io/resources/blog/buy-with-aws-launch/) covers what shipped and why the buyer-side experience matters more than the button.


---


## **Frequently asked questions**


**What analytics does AWS provide for Buy with AWS?** A dashboard under Insights → Marketing → Buy with AWS in the Management Portal, also available through AWS Partner Central Analytics. It reports unique visitors, page views, average bounce rate, number of agreements, and clicks on Buy with AWS CTA buttons.


**Which buttons does AWS track?** Four: Buy with AWS, Try free with AWS, Request demo, and Request private offer. AWS also records form submissions. Tracking depends on the AWS partner script and the required data attributes on each button.


**Why does my Buy with AWS dashboard show no clicks?** Most often the instrumentation, not the demand. Check that the AWS partner script is present on every page carrying a CTA and that each button still has its required data attributes — redesigns frequently drop them.


**What is a good conversion rate for Buy with AWS?** There is no published benchmark, and any figure quoted without a source should be treated as invented. Measure your own agreement rate per CTA click and compare it against itself over time.


**Does the dashboard tell me which accounts clicked?** No. It reports visitors and agreements, not your named accounts, and it carries no campaign attribution. Both joins have to happen in your own analytics.


**Should Request private offer count as a conversion?** Not as a self-serve one — it produces a conversation, not an agreement, so it will never appear in the agreement count. Track it separately, because on enterprise-priced products it is often the most valuable button on the page.


---


## **Takeaways**


- Only one metric on this dashboard — button clicks — is measured on your site. Page views, unique visitors, bounce rate, dwell time, and agreements are all measured on AWS’s procurement page.
- Four CTAs are tracked: Buy with AWS, Try free with AWS, Request demo, Request private offer. Tracking depends on the AWS script and the data attributes on each button.
- Watch ratios, not counts: click rate per page, agreements per click, the AWS sign-in drop-off rate, and the share of agreements originating from the button.
- A zero is usually broken instrumentation. Verify the script and attributes before interpreting anything.
- Request private offer will never show up as an agreement. Counting it as a failure misreads the dashboard.
- The dashboard knows no accounts, no campaigns, and no other marketplace. Those joins are yours.


---


Site behaviour on one dashboard, agreements on another, and your CRM on a third is how marketplace attribution goes missing. See how[reporting in Suger](https://www.suger.io/platform/reporting/) brings offers, entitlements, and revenue into one dataset you can join to your own analytics.
