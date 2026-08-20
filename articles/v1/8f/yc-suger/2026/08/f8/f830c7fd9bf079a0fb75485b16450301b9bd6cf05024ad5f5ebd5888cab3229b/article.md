---
schema_version: "1.0.0"
document_id: "f830c7fd9bf079a0fb75485b16450301b9bd6cf05024ad5f5ebd5888cab3229b"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-listing-readiness-checklist/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:87e9f76fd38c2811e6747272e35adbe21137abe86d1fb4d0cd10c2b9406b25d5"
---

# What It Takes to Get Listed: A Readiness Checklist

*Getting a product onto a cloud marketplace is a review process, not a publish button. The reviews are mostly mechanical, the rejections are mostly avoidable, and the single biggest cause of a slipped launch date is starting the process without knowing what is on the checklist.*


---


Nobody’s first listing is rejected for being a bad product. They are rejected for a company logo that is 200 pixels instead of 220, a EULA link that opens a marketing page instead of a downloadable file, a default password baked into an image, or a description that promises something the product details don’t substantiate.


Every one of those is knowable in advance. Here is the readiness checklist across the three categories that matter — technical, security, commercial — plus the timeline you should actually plan against.


---


## **What are the requirements to get listed on a cloud marketplace?**


**A marketplace listing needs a reviewable product, a declared legal agreement, a working commercial configuration, and — for transactable products — a live integration with the marketplace’s own APIs.** The specifics differ per cloud, but every provider is checking the same four things.


AWS states what its review covers: submissions are “reviewed for policy and security compliance, software vulnerabilities, and product usability.” Microsoft requires transactable offers to meet its general certification policies plus the policies for the specific offer type.


The practical shape is that the *product* work and the *paperwork* are on separate critical paths, and teams routinely serialise them when they could run in parallel.


---


## **Technical readiness**


**For a SaaS or transactable offer** , the integration is the long pole. Microsoft’s requirements for selling through Microsoft are specific and none of them are optional:


- Microsoft Entra ID single sign-on for buyers, and Microsoft Accounts enabled.
- A **landing page** that handles a purchase token and completes onboarding. Microsoft notes it “should be running 24/7” because it is the only way you learn about new purchases.
- A **connection webhook** for asynchronous events, with the same 24/7 requirement.
- Integration with the **SaaS Fulfillment APIs** , and — worth planning capacity for — “critical API changes must be supported within 24 hours.”
- A registered Microsoft Entra application, whose tenant and application IDs can be used in only one Partner Center account.


**For an AMI or container product on AWS** , the checks are about the image itself. From AWS’s own final checklist:


- Remove all user credentials — default passwords, authorization keys, key pairs, security keys.
- Root login locked and disabled; only sudo access accounts.
- HVM virtualization and 64-bit architecture.
- No known vulnerabilities, malware or viruses.
- Buyers get operating-system-level administration access.
- The default user uses a randomized password, or initial user creation verifies the buyer is authorized using something unique to the instance, such as the instance ID.


AWS offers **self-service AMI scanning** in the Management Portal ahead of submission — for new products a scan runs automatically on submission, and for new versions you can trigger one with the Test ‘Add Version’ feature. AWS says the scan “typically completes in less than an hour.” Running it before you submit turns a review cycle into a same-day fix.


---


## **Security and compliance readiness**


This is where enterprise buyers, rather than the marketplace, set the bar. The marketplace review checks that your product is not dangerous; the buyer’s security team checks whether they can approve it.


The work that pays off:


- **A completed security questionnaire** , in whatever form your segment expects, ready before the first deal rather than during it.
- **A documented data flow** — what leaves the buyer’s environment, what you store, where.
- **Whichever attestation your buyers ask for.** Which one depends on your market; the point is having the answer before procurement asks.
- **A decision on the contract path.** Whether you list under the marketplace’s standard contract or your own EULA affects the security review directly, because pre-drafted security addendums exist on some marketplaces.[EULAs and custom terms in marketplace deals](https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/) covers the choice.


For AWS specifically, the Foundational Technical Review is the structured version of this conversation, and treating it as an exercise rather than a checkbox tends to shorten every subsequent enterprise security review —[scaling IT operations with AWS FTR](https://www.suger.io/resources/blog/scaling-it-operations-with-aws-ftr-and-suger/) covers what that involves.


---


## **Commercial readiness**


The part most often left to the last week, and the part with the least reversible decisions.


- **Pricing model.** Choose deliberately: it freezes at publication on both AWS SaaS and Microsoft.[Cloud marketplace pricing models](https://www.suger.io/resources/blog/cloud-marketplace-pricing-models/) covers the choice.
- **Dimensions and units.** Named, defined, and meterable by your product today.
- **The EULA.** Standard contract or your own. If your own, AWS requires the link to allow customers to download the agreement — an S3 link is the documented example, not a page behind a login.
- **Listing assets to spec.** AWS: product logo 120–640px at 1:1 or 2:1, company logo 220 × 220 with 10px padding. Microsoft: a large logo between 216 × 216 and 350 × 350, one to five screenshots at exactly 1280 × 720 PNG with captions, videos hosted only on YouTube or Vimeo.
- **Descriptions that don’t overclaim.** AWS’s repackaging rules are the clearest statement of the principle: a title “may not use the words *certified* , *original* , or *free* unless these are substantiated in the product details that you provide.”
- **Support contacts.** Microsoft requires a support contact, an engineering contact, and optionally a CSP program contact. Fill in real people.


Two rules that catch specific products: AWS does not permit selling hardware, and a software product requiring a hardware component must state that the hardware is obtained separately and must not include its cost in the price. And repackaged open-source products must add tangible IP beyond the original software, with the short description beginning with a prescribed phrase.


---


## **The timeline to plan against**


AWS publishes its own guidance, and it is more conservative than most launch plans assume:


> “Total request time normally takes 2–4 calendar weeks. More complex requests or products can take longer, due to multiple iterations and adjustments to product metadata and software.”


And, importantly for anyone tying a listing to an event: “We require a completed product request and AMI at least 45 days in advance of any planned events or releases, so we can prioritize the request accordingly.”


The AWS request moves through named states you can track — Draft, Submitted, Action Required, Approval Required, Publishing Pending — and one detail worth knowing: for a new product, after approval you receive a limited listing URL, and **your product is not published until you approve the submission** . The final step is yours, so nothing publishes by surprise. Requests left incomplete for six months expire.


A useful planning habit: work backwards from the date you need to transact, not from the date you want to submit.[How long a marketplace integration takes](https://www.suger.io/resources/blog/how-long-marketplace-integration-takes/) covers the whole sequence including the parts that are not the review queue.


---


## **The checklist, condensed**


**Before you open the form**


- Pricing model chosen and defensible, dimensions defined and meterable
- EULA decided, and downloadable if it is your own
- Logos and screenshots at the exact specifications
- Support and engineering contacts named


**Before you submit**


- AMI scanned with self-service scanning, credentials removed, root login disabled
- Landing page and webhook running, fulfillment API integration tested end to end
- Descriptions checked for unsubstantiated claims
- Security questionnaire and data flow documentation ready


**Before you announce**


- Submitted at least 45 days ahead of any event date
- Limited listing reviewed and approved by you
- A named owner watching the request status


---


## **Frequently asked questions**


**How long does it take to get a product approved on AWS Marketplace?** AWS guidance is that total request time normally takes 2–4 calendar weeks, and longer for more complex products. It asks for a completed request and AMI at least 45 days ahead of any planned event.


**What does AWS check when reviewing a listing?** AWS states that submissions are reviewed for policy and security compliance, software vulnerabilities, and product usability. AMI products are also scanned for credentials and known vulnerabilities.


**What are the technical requirements for a Microsoft transactable SaaS offer?** Microsoft Entra ID single sign-on, a landing page that handles the purchase token, a connection webhook, and integration with the SaaS Fulfillment APIs. Microsoft states critical API changes must be supported within 24 hours.


**Can I host my EULA on my website?** Only if it can be downloaded. AWS requires a custom EULA link to allow customers to download the agreement, and gives an Amazon S3 link as the example. A page behind a login is not sufficient.


**Does my product publish automatically once approved?** No. On AWS you receive a limited listing URL for a new product and the listing is not published until you approve it. The last step belongs to you.


**Can I sell hardware alongside my software listing?** No. AWS does not permit hardware sales. A product that needs a hardware component must state that the hardware is obtained separately and must not include its cost in the listing price.


---


## **Takeaways**


- Reviews check policy and security compliance, software vulnerabilities, and product usability. Almost every rejection is mechanical and avoidable.
- Run AWS self-service AMI scanning before submitting — it typically completes in under an hour and converts a review cycle into a same-day fix.
- Microsoft’s transactable requirements are hard prerequisites: Entra ID SSO, a 24/7 landing page, a connection webhook, and the SaaS Fulfillment APIs.
- Plan against AWS’s own guidance of 2–4 calendar weeks, and 45 days ahead of any event you intend to announce at.
- Commercial decisions freeze. Pricing model, dimensions and EULA are chosen once and changed with difficulty.
- Nothing publishes by surprise: on AWS you approve the limited listing yourself before it goes public.


---


Listing readiness is the same work repeated per marketplace, with different asset specs and a different review queue each time. See how Suger’s[marketplace listing automation](https://www.suger.io/platform/product-listing/) builds and submits a listing to six marketplaces from one product definition.
