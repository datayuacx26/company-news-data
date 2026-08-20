---
schema_version: "1.0.0"
document_id: "3b3ecf1b4c8b6d9f543a20bf0a5608435b457648ca63be06f5206affba82b2cb"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/change-marketplace-price-after-launch/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:29500d7db5c3fba411e64657a475cf67f8242cc2036f7bb059a129e9bf484ba2"
---

# Changing Your Marketplace Price After Launch

*Every cloud lets you change the price on a published listing. None of them lets you change it quickly, and most of them will not let you change the pricing model at all. The distinction between a price and a model is the single most expensive thing to learn late.*


---


A deal desk owner is asked to reprice a live listing — a 12% increase, effective next quarter, “same as we did on the website.” On a marketplace that request contains two assumptions that are usually wrong: that the change applies to existing customers, and that next quarter is soon enough.


Here is what each cloud actually permits, how long it takes, and what to do when the answer is no.


---


## **What can you change about marketplace pricing after publishing?**


**The price amount, yes — with notice. The pricing model, generally no.** Those are two different objects and the clouds treat them very differently:


Price amount Pricing model


AWS (SaaS) Changeable, with notice Frozen once published to limited


AWS (AMI/server) Changeable, with notice and cadence limits Change requires AWS review and approval


Microsoft (SaaS) Changeable Frozen after publication


Google Cloud Changeable Set by the plan structure you publish


The rest of this post is really about the consequences of that second column.


---


## **AWS: what happens when you change the number**


AWS’s rule is stated plainly, and the asymmetry in it is the important part:


> “Price changes are automatically reflected on the product page in AWS Marketplace. New subscribers are subject to the price displayed on the product page. For existing subscribers of the public offer, price decreases take effect immediately. However, price increases become effective on the first day of the month following a 90-day notification period.”


AWS gives a worked example: send a price increase notification on March 16, and the new price takes effect on **July 1** — the first day of the month after the 90-day period elapses. Not June 16. The rounding to the start of a month is what catches out anyone planning against a fiscal quarter.


So the practical shape is:


- **Cutting a price is immediate** for everyone, existing subscribers included.
- **Raising a price is fast for new subscribers** — the product page updates — and slow for existing ones.
- **A price rise you want effective on 1 January** needs its notification sent by roughly the start of October, not in December.


For **annual pricing on server products** , AWS is more restrictive again: you can change annual prices every 90 days, must give existing annual customers 90 days’ notice, and the new price “will apply to new subscriptions but will have no impact on existing subscriptions.” Two extra rules come with it — during the 90-day notice period you can’t update the supported instance type, and a price change only reaches an auto-renewal “if the price was changed at least 90 days before the auto-renewal date.” Customers receive an email with the new price before renewal.


AWS also groups changes by type in its submission process, and notes that “some changes can only be made every 90 or 120 days, or when pending changes are in place.” A repricing plan that assumes you can iterate monthly is planning against a cadence the marketplace does not offer.


---


## **The pricing model is a different question entirely**


**On AWS SaaS, the model is frozen the moment you publish to limited:** “Once you create your listing and publish it to limited, you can’t change the pricing model.” That covers the choice between SaaS subscriptions, SaaS contracts, SaaS contracts with pay-as-you-go, and free.


**On server products, a model change is possible but gated.** AWS classifies it as its own request type and states that “not all pricing model changes are supported, and all requests to change models must be reviewed and approved by the AWS Marketplace team.” It flags the free-to-paid case specifically as presenting “significant impact to existing customers,” and offers the alternative most teams end up taking: “propose a new product with additional features and encourage current customers to migrate.”


**Microsoft is stricter and says so twice.** “After you publish your offer, you can’t change the pricing model. In addition, all plans for the same offer must share the same pricing model.” A SaaS offer is either flat rate or per user for its whole life. Two related locks sit alongside it: the minimum and maximum user counts on a per-user plan can’t be edited as part of a plan update, and the decision to sell through Microsoft can’t be changed once the offer is published.


The pattern across all three clouds is the same. **The number is data; the model is structure.** Data can be updated. Structure is what buyers accepted, what procurement systems parsed, and what the billing pipeline was built against — so it is either immutable or gated behind a human review.


[Cloud marketplace pricing models](https://www.suger.io/resources/blog/cloud-marketplace-pricing-models/) covers how to choose the model in the first place, which is the decision this post exists to tell you that you cannot easily revisit.


---


## **What to do when the answer is no**


Four options, roughly in order of how often they are the right one.


**1. Private offers.** The fastest legitimate path to different pricing for a specific customer, and it does not touch the public listing at all. A private offer carries its own price, term and payment schedule. If the repricing request is really “this one enterprise deal needs different economics,” this is the answer and the public price change was never needed.[Private offers](https://www.suger.io/platform/private-offers/) covers the mechanics.


**2. A new plan or dimension rather than a new price.** Adding a dimension is a smaller change than repricing an existing one, and it lets new customers buy the new shape while existing agreements run out on the old.


**3. A new listing, with a migration.** This is AWS’s own suggested alternative for an unsupported model change, and it is cleaner than it sounds — the old listing stops taking new subscribers, the new one carries the new model, and existing customers move at renewal rather than mid-term. The cost is that you support two listings for a year.


**4. Wait for renewal.** Existing agreements end. If the increase can ride a renewal instead of an amendment, most of the notice-period problem disappears.[Marketplace renewals](https://www.suger.io/resources/blog/aws-marketplace-renewals/) covers running that deliberately rather than letting it happen.


---


## **The sequencing mistake worth avoiding**


The expensive version of this goes: sales agrees a price increase for the new fiscal year in November, the deal desk submits it in December, existing customers see it in April, and finance has already booked the uplift.


Two habits prevent it:


- **Put the notice period in the plan, not the submission.** Work backwards from the effective date you actually need, add the 90 days, add the rounding to the first of the month, and you have the date the request has to be in.
- **Decide the model as if it were permanent** , because on the marketplaces it effectively is. The time to argue about per-seat versus consumption is before the first listing, not after the first hundred customers.


---


## **Frequently asked questions**


**Can I change my price on AWS Marketplace after publishing?** Yes. New subscribers get the new price as soon as the product page updates. For existing subscribers of a public offer, decreases take effect immediately and increases take effect on the first of the month following a 90-day notification period.


**How long does an AWS Marketplace price increase take?** Longer than 90 days. AWS’s own example: notify on March 16 and the new price takes effect July 1 — the first day of the month after the 90-day period elapses.


**Can I change my AWS Marketplace pricing model?** Not for SaaS. AWS states that once a listing is published to limited, the pricing model can’t change. For server products, model changes need AWS review and approval, and not all are supported.


**Can I change pricing on a published Microsoft Marketplace offer?** You can change prices, but not the pricing model. Microsoft states the model can’t change after publication, and all plans in one offer must share it.


**How do I give one customer different pricing without repricing the listing?** Use a private offer. It carries its own price, term and payment schedule for a named buyer and leaves the public listing untouched.


**Do price decreases need 90 days’ notice?** No. On AWS, price decreases for existing subscribers of the public offer take effect immediately. The notice period applies to increases.


---


## **Takeaways**


- Price amounts are editable; pricing models mostly are not. Treat them as two different decisions with two different reversibility profiles.
- On AWS, decreases hit existing public-offer subscribers immediately; increases take effect on the first of the month after a 90-day notification period.
- AWS annual pricing on server products can change every 90 days, needs 90 days’ notice, and only reaches an auto-renewal if changed at least 90 days beforehand.
- AWS SaaS pricing models freeze at limited publication. Microsoft’s freeze at publication, and every plan in an offer must share one model.
- A private offer is usually the right answer to “this customer needs different pricing,” and it never touches the public listing.
- AWS’s own suggested route around an unsupported model change is a new listing plus a migration at renewal.


---


Repricing across several marketplaces means the same decision executed three times, in three consoles, on three notice clocks. See how Suger manages[marketplace listings and pricing](https://www.suger.io/platform/product-listing/) from one place, so a pricing change is one workflow rather than three separate submissions.
