---
schema_version: "1.0.0"
document_id: "ef69c402025f696669026661bb5a43bceddd0b7a85904bf1af732d09e292234f"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/when-usage-records-fail/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:fda80182d8013e08bf23b56198aa572b1f49c35242b84fd1ae1bb3d3fb79390b"
---

# When Usage Records Fail: Batching and Retries

*A usage record that AWS rejects is revenue you earned and will not bill. The metering API distinguishes two kinds of failure — records that are wrong, and calls that failed — and they need opposite responses. Retrying a wrong record never works; fixing a failed call is wasted effort.*


---


Metering pipelines are written once, tested against a happy path, and then run unattended for years. They are also the part of a marketplace integration where a silent failure costs money directly rather than through some chain of consequences.


The failures are not exotic. A batch is one record too long. A timestamp is a day old because a job retried after an outage. A dimension was renamed in the product and not in the meter. Each is ordinary, and each produces unbilled usage that no dashboard shows as missing, because a record that was never accepted leaves no trace to be missing from.


Here are the limits, and what the API tells you when you exceed them.


---


## **The limits, precisely**


**AWS Marketplace accepts at most 25 usage records per call, in a request under 1 MB, for a single product, submitted within 24 hours of the event.** All four constraints apply at once.


Constraint Value


Records per` BatchMeterUsage` call 25


Request size Under 1 MB


Products per call One


Age of an event Under 24 hours


End-of-cycle grace period Until 06:00 UTC on the first of the next month


Tags per usage allocation 5


AWS’s wording on timing is worth reading exactly, because it is two separate rules that get conflated:


> “Usage records should be submitted in quick succession following a recorded event. Usage records aren’t accepted 24 hours or more after an event. At the end of each billing cycle, a 6-hour grace period applies. We accept usage records for the previous billing month until 06:00 UTC on the first day of the next month.”


So the ordinary deadline is **24 hours from the event** . The six hours is a separate, narrower allowance at the month boundary — you must submit March’s usage before 06:00 UTC on April 1. Past that, AWS returns` TimestampOutOfBoundsException` .


A pipeline that batches daily is already close to the first limit. A pipeline that reconciles monthly has, in practice, until dawn on the first.


---


## **The one behaviour that decides your batching strategy**


Timestamp failures are all-or-nothing:


> “For` BatchMeterUsage` , if any of the records are outside of the allowed range, the entire batch is not processed. You must remove invalid records and try again.”


One stale record poisons twenty-four good ones. This single sentence is the argument against the obvious design — accumulate everything, submit in batches of 25 — because a single replayed or delayed event silently discards a full batch.


Two practical responses:


**Filter by age before batching, not after.** Drop or quarantine anything approaching 24 hours old *before* it joins a batch. The check is cheap; the alternative is a rejected batch you then have to bisect.


**Group by age, not just by arrival.** Records from a backfill and records from live traffic should not share a call, because they have very different chances of being in range.


---


## **Two failure types, two responses**


The response separates them, and this is the most useful thing in the API:


**` Results`** contains records that were “either honored by AWS Marketplace Metering Service or were invalid,” and AWS is explicit that “invalid records should be fixed before being resubmitted.”


**` UnprocessedRecords`** contains records that hit “errors on the service side that should be retried” — you “can retry the failed request by making another` BatchMeterUsage` call with this list as input.”


So:


Where the record landed What to do


` Results` , honored Mark metered, record the` MeteringRecordId`


` Results` , invalid Fix the data. Retrying identical input fails identically


` UnprocessedRecords` Retry as-is, with backoff


A retry loop that treats every non-success the same will hammer AWS with records that can never succeed, and will do it until the 24-hour window closes. That is the shape of the outage that turns a small data bug into a month of unbilled usage.


Retrying is safe where it is appropriate: “identical requests are idempotent and can be retried with the same records or a subset of records.” You will not double-bill by retrying a genuine service failure.


---


## **The error codes, and what each is really telling you**


Error What it usually means


` TimestampOutOfBoundsException` A delayed job, a replay, or a clock. The whole batch is discarded


` InvalidUsageDimensionException` The product’s dimensions changed and the meter did not


` InvalidCustomerIdentifierException` Metering a customer who does not exist — often a cancelled subscription


` InvalidLicenseException` The` LicenseArn` is wrong, mismatched, or usage sits outside the activation period


` InvalidProductCodeException` The wrong product code, or records for two products in one call


` InvalidUsageAllocationsException` Allocations do not sum to the total quantity


` InvalidTagException` An invalid tag, or more than five


` ThrottlingException` Back off — this one is a retry, despite the 400


` InternalServiceErrorException` AWS-side. Retry


` DisabledApiException` The API is disabled in that Region


Two are worth dwelling on.


**` InvalidUsageDimensionException` is a product management failure, not an engineering one.** It appears when someone renames or retires a dimension in the listing without changing the emitter. It is the reason dimension names deserve the same change control as a database schema —[designing metering dimensions](https://www.suger.io/resources/blog/designing-metering-dimensions/) covers choosing ones you will not want to rename.


**` ThrottlingException` returns HTTP 400** , which is the status code most retry libraries treat as permanent. Check this specifically in your client, or throttling will look like invalid data.


---


## **The parameter change you cannot ignore**


AWS is migrating SaaS metering to support Concurrent Agreements, which allow multiple purchases of the same product within one AWS account. The consequence for metering is a change of identifier:


> “Starting June 1, 2026, new SaaS products must use` CustomerAWSAccountId` (instead of` CustomerIdentifier` ),` LicenseArn` (instead of` ProductCode` ) to support this feature.` BatchMeterUsage` does not support` CustomerIdentifier` for new integrations. Existing integrations continue to work.”


The reason this matters beyond compliance: a customer with two concurrent agreements has two licences. Metering by customer identifier cannot say which agreement the usage belongs to, so it lands against one of them — and the invoice is wrong in a way that reconciliation will find and nobody will enjoy explaining. If you support concurrent agreements at all, the meter has to carry the licence.


---


## **What to build around all of this**


**Persist the outcome per record, not per batch.** A batch of 25 has 25 fates. Storing “batch 4102 succeeded” loses the invalid ones inside it.


**Alert on invalid, not on failed.** Service failures resolve themselves. Invalid records never do, and they are the ones with a 24-hour clock.


**Reconcile submitted against accepted, daily.** The gap between what your product measured and what AWS acknowledged is the only number that reveals silent loss.[Marketplace revenue reconciliation](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) covers the wider version of this check.


**Watch the month boundary specifically.** More metering revenue is lost at 06:00 UTC on the first of the month than at any other moment, because a backlog that would have been fine on the tenth is fatal on the first.


---


## **Frequently asked questions**


**How many usage records can one AWS Marketplace call carry?** Up to 25, in a request under 1 MB, for a single product. Metering more than one product requires separate calls.


**How late can a usage record be submitted?** Under 24 hours after the event. Separately, records for the previous billing month are accepted until 06:00 UTC on the first day of the next month.


**What happens if one record in a batch has a bad timestamp?** The entire batch is rejected. AWS states that if any record is outside the allowed range, the batch is not processed and invalid records must be removed before retrying.


**Is it safe to retry a metering call?** Yes, for service-side failures. AWS states identical requests are idempotent and can be retried with the same records or a subset. Retrying invalid records will fail identically.


**What is the difference between Results and UnprocessedRecords?**` Results` holds records that were honored or invalid — invalid ones must be fixed.` UnprocessedRecords` holds service-side failures that should simply be retried.


**Why is throttling easy to misread?**` ThrottlingException` returns HTTP 400, which many retry libraries treat as permanent. It should be retried with backoff.


---


## **Takeaways**


- Twenty-five records, under 1 MB, one product, under 24 hours old. All four limits apply to every call.
- The 24-hour submission rule and the 6-hour end-of-cycle grace period are separate rules. Conflating them costs a month’s usage.
- One out-of-range timestamp discards the whole batch, so filter by age before batching rather than after.
- ` Results` invalid means fix;` UnprocessedRecords` means retry. Treating them alike burns the window on records that can never succeed.
- ` ThrottlingException` arrives as a 400 and must still be retried.
- New SaaS integrations must meter by` CustomerAWSAccountId` and` LicenseArn` , or concurrent agreements bill against the wrong licence.


---


Unbilled usage is the quietest revenue leak in a marketplace business. See how Suger’s[billing and metering automation](https://www.suger.io/platform/billing-metering/) validates, batches and reconciles usage records across every marketplace, so a rejected record becomes an alert rather than a silence.
