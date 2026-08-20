---
schema_version: "1.0.0"
document_id: "1e08dd44912d7a16d4c63c825efdd192785f4a8070d00354dbdae308119524c7"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/security-reviews-in-marketplace-deals/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:66d6017d4bb6b6db85695f861d61435fea071aa00aa7d25748365c776a290a5e"
---

# How Security Reviews Shape Marketplace Deals

*A marketplace deal passes through two security reviews that have nothing to do with each other. The provider checks the artifact, continuously and without asking. The buyer checks the vendor, and no marketplace listing exempts you from it. Teams that plan for one and are surprised by the other lose weeks in the middle of a closing quarter.*


---


There is a comfortable assumption that being on AWS Marketplace or Microsoft Marketplace means a security review has already happened, so the buyer’s will be lighter.


Half of that is true. A review has happened, and it was rigorous. But it examined a different thing, for a different party, against different criteria — and the buyer’s security team, who did not commission it, will not treat it as evidence of much.


Knowing which review is asking what is the difference between answering a questionnaire in two days and answering it in five weeks.


---


## **The two reviews, side by side**


**The provider reviews your artifact. The buyer reviews your company.**


Provider certification Buyer security review


Who runs it AWS, Microsoft, Google Cloud Your customer’s security team


What it examines The image, package or SaaS integration Your company’s controls and practices


When At submission, then continuously During the deal, sometimes after


Evidence it wants A conforming artifact SOC 2, pen test results, questionnaires


Consequence of failing Listing hidden or unavailable to new subscribers The deal stops


Neither substitutes for the other. Both can stop revenue.


---


## **What the provider actually checks — and keeps checking**


The part sellers underestimate is that certification is not an event. AWS states that it “continuously scans products to verify that existing listings continue to meet any changes to these requirements,” and that when a product falls out of compliance, AWS contacts the seller, and “in some cases, products might be temporarily made unavailable to new subscribers until issues are resolved.”


Microsoft is blunter still for container offers: “Microsoft performs regular security validations on container offers. If vulnerabilities are identified in a published offer, Microsoft reserves the right to hide/deprecate the offer (with or without advanced notification to the publisher) to ensure the safety of our customers.”


So a listing that passed in March can be pulled in September because a dependency aged out, and the first you hear of it may be a customer asking why your product is not there.


The AWS bar for machine images is specific and worth checking against your build:


- “AMIs must pass all security checks and must not contain critical or high-severity unpatched vulnerabilities or malware identified during scanning.”
- “AMIs must use currently supported operating systems and software. Operating systems and software that reached their end of life are not allowed.”
- “AMIs must not be older than two years from their creation date.”
- No hardcoded secrets — “system user and service passwords (including hashed passwords), private keys, or credentials.”
- No password-based authentication for instance services, and for SSH specifically,` PasswordAuthentication` set to` no` with no authorized public keys baked in.
- “A seller must not have access to instances run by a customer” unless the customer explicitly enables it.


The two-year age limit and the end-of-life rule are the ones that catch stable products. A component that has not changed does not need a new version — until the base image ages past the threshold, and then it does. Put a calendar reminder against it, because nothing in your own pipeline will.


Microsoft’s general policies add an obligation most sellers never read: under policy 100.11 you “must report suspected security events, including security incidents and vulnerabilities of your Marketplace software and service offerings, at the earliest opportunity.” That is a disclosure duty to Microsoft, separate from anything in your customer contracts.


---


## **What the buyer asks, and why the marketplace does not help**


The buyer’s review is about your company. Their questionnaire wants your SOC 2 report, your penetration test summary, your subprocessor list, your data residency, your incident response commitments, your breach notification window.


None of this is what the marketplace certified, so “we are on AWS Marketplace” is not an answer to any of it. What the marketplace *does* change is the shape of the surrounding process:


**Vendor onboarding is already done.** The buyer has a commercial relationship with the cloud provider, so the finance, tax and supplier-registration parts are skipped. The security review is not part of that.


**The contract may not be yours.** If the deal uses standardised terms rather than your own paper, the security commitments in it are the marketplace’s standard ones. If your customer needs specific breach notification timelines or audit rights, those belong in custom terms —[EULAs and custom terms in marketplace deals](https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/) covers which document governs.


**Nobody schedules it.** In a direct deal, procurement triggers security review at a known step. In a marketplace deal, the commercial terms are often agreed first, and the security review arrives late — occasionally after the offer has been sent, occasionally after it has expired.


That last point is the practical one.[When you need a signature on legal documents in a marketplace deal](https://www.suger.io/resources/blog/when-do-you-need-a-signature-on-legal-documents-in-cloud-marketplace-deals/) covers the parallel question for legal, and the answer is the same in both cases: the step did not disappear, it just lost its place in the sequence.


---


## **Shortening both**


**Publish the answers before you are asked.** A security page carrying your certifications, subprocessors and architecture summary answers most of a questionnaire without a call. Suger publishes[its own](https://www.suger.io/resources/security/) for exactly this reason.


**Keep an evidence pack current.** SOC 2 report, most recent penetration test summary, architecture diagram, data flow description, subprocessor list. Assembling it during a deal is what turns two days into five weeks.


**Treat a rescan failure as a production incident.** A published listing pulled for a vulnerability blocks new subscribers immediately. Whoever owns your build pipeline should learn about it from monitoring, not from a customer.


**Rebuild on a schedule, not on demand.** The AWS two-year AMI limit and the end-of-life rule mean a rebuild cadence is a compliance requirement, not hygiene.


**Ask about security review during qualification.** One question — “who does your security review and how long does it usually take?” — sets the offer expiry realistically, which is the single most common cause of a reissued offer.


**Get custom terms drafted early if you need them.** If your buyer’s security team always requires specific notification windows, standardise your own custom terms rather than negotiating them per deal. The[listing readiness checklist](https://www.suger.io/resources/blog/marketplace-listing-readiness-checklist/) covers what to have ready before the first deal rather than during it.


---


## **Frequently asked questions**


**Does a marketplace listing replace a buyer’s security review?** No. Provider certification examines your artifact against marketplace policy. The buyer’s review examines your company’s controls, and it still happens.


**Can a listing be removed after it is published?** Yes. AWS continuously rescans listings and may make a product unavailable to new subscribers, and Microsoft reserves the right to hide or deprecate a container offer with or without notice when vulnerabilities are found.


**What is the AWS vulnerability bar for machine images?** AMIs must pass all security checks and must not contain critical or high-severity unpatched vulnerabilities or malware. End-of-life operating systems and software are not allowed.


**Do AMIs expire?** Effectively. AWS states AMIs must not be older than two years from their creation date, so a stable product still needs a rebuild cadence.


**Must security incidents be reported to the marketplace?** On Microsoft, yes. Policy 100.11 requires publishers to report suspected security events, including incidents and vulnerabilities, at the earliest opportunity.


**When does security review happen in a marketplace deal?** Later and less predictably than in a direct deal, because commercial terms are often agreed first. Ask during qualification and set the offer expiry accordingly.


---


## **Takeaways**


- Two separate reviews run on every marketplace deal: the provider checks your artifact, the buyer checks your company.
- Provider certification is continuous. A listing that passed can be pulled later when a dependency ages out.
- AWS bars critical and high-severity unpatched vulnerabilities, end-of-life software, and machine images older than two years.
- Microsoft requires publishers to report suspected security events at the earliest opportunity — a duty separate from your customer contracts.
- The buyer’s questionnaire is about your controls, and no listing answers it. Publish the evidence before you are asked.
- Ask who runs security review during qualification, then set the offer expiry to survive it.


---


Security review is a scheduling problem more often than a technical one. See how Suger’s[agreement and terms management](https://www.suger.io/platform/agreements/) keeps custom terms, standardised terms and their approval state attached to the deal, so the security conversation starts with the right document.
