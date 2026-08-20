---
schema_version: "1.0.0"
document_id: "cb30060f518e8003abc70dc484452f24ed5a7fed5be1bf9584c36f5386179f80"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/will-cloudflare-r2-win-customers-from-amazon-s3"
published_at: "2021-10-06T13:00:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:5f3bbcf881bfa11e40d328b499ed6adca8567b87d97daff8595190b05f9cffcc"
---

# Will Cloudflare R2 Win Customers from Amazon S3?

Cloudflare just announced[Cloudflare R2](https://blog.cloudflare.com/introducing-r2-object-storage/) , a[cloud object storage](https://www.taloflow.ai/cloud-object-storage) service with some incredible features. Keep in mind, it is currently in early access so we don’t know everything yet.


Cloudflare R2 promises to solve three main problems that make incumbent providers like Amazon S3 more complicated:


1. Free Egress/Bandwidth
2. Automatic tiering and pricing for per-operation charges
3. Simple integration with[Cloudflare Workers](https://workers.cloudflare.com/)


It also offers a supposedly easy migration from S3-compatible storage buckets, and automatic replication of “blobs” across the world. It has quickly become a competitor to other[cloud object storage vendors.](https://www.taloflow.ai/cloud-object-storage/best-vendors)


In this post, I’m going to cover the following:


1. What “R2” means
2. Who could benefit from switching
3. What would stop ideal customers from adopting Cloudflare R2
4. Who is better served by other providers
5. What we still don’t know about Cloudflare R2
6. Negative Hacker News comments (As well as a few positive ones)


‍


## What does “R2” mean?


“R2” is a clever play on the name styling of Cloudflare R2’s main incumbent competitor, Amazon S3, which stands for **S** imple **S** torage **S** ervice. At this point we think it will officially be a stand in for “ **R** apid” and “ **R** eliable”, but according to Cloudflare, R2 is also a stand in for any one of the following:


- **R** eally **R** equestable;
- **R** epositioning **R** ecords;
- **R** idiculously **R** eliable; and
- **R** adically **R** eprogrammable


The real question is: “which vendor will be first to launch *Q1 Cloud Object Storage* ?” 😝


If you think we're on to something, you just might be able to launch the next big thing with the help of[object-storage-name-generator.com](https://object-storage-name-generator.com/)


## Are the savings worth switching to Cloudflare R2?


Right now, Cloudflare R2 has **cheaper** and **simpler** pricing than several incumbent cloud services like Amazon S3 and Azure Blob, but is still well behind others on raw storage cost. When comparing Cloudflare R2 to[Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) ,[Wasabi](https://wasabi.com/) or[Storj DCS](https://www.storj.io/how-it-works) , R2 is about 3 times more expensive for storage.


However, $0.015 per GB is not necessarily a deal-breaker here. Cloudflare R2 might shine better than other[Amazon S3 alternatives](https://www.taloflow.ai/blog/8-amazon-s3-alternatives) when it comes to egress and read/write operations.


At this point, we will take Cloudflare’s word for it: Egress will be zero cost. But, Backblaze B2 and Wasabi also have a bit of an answer to R2’s “zero egress”, which we cover in the next section.


Cloudflare has also given itself wiggle room when it comes to the cost of read/write operations, although it’s implied that whatever it does *should be* very developer-friendly.


*“R2 will zero-rate infrequent storage operations under a threshold — currently planned to be in the single digit requests per second range. Above this range, R2 will charge significantly less per-operation than the major providers.” - Cloudflare*


Note that Cloudflare in their R2 launch post said **“infrequent”** storage operations, without specifying what those are and the rules that apply. ( **Author's Note:** We changed this paragraph based on an observant comment made on HN that pointed to a miscalculation)


At a high level, the use cases that would suit Cloudflare R2 best are those where bandwidth costs are big enough to outsize the benefit of working with a provider with 3x cheaper storage, like Backblaze and Wasabi, who incidentally are part of the[Bandwidth Alliance](https://www.cloudflare.com/bandwidth-alliance/) championed by Cloudflare that offers zero egress fees relevant to some (not all!) use cases*.


*** ***Using Backblaze and/or Wasabi via Cloudflare CDN is limited:*** *You can’t serve large amounts of media unless you’re paying more for your usage. The CEO at Cloudflare specified that these restrictions would not apply to Cloudflare R2 (*[Terms of service link here](https://www.cloudflare.com/terms/#28-limitation-on-serving-non-html-content) *,*[Hacker News comment here](https://news.ycombinator.com/item?id=28682885) *).*


In order, here are the use cases that are most likely going to see huge improvements from moving to Cloudflare R2. Note that they are reading-heavy use cases:


1. **Real-Time Workflow:** Product process-oriented use case that is receiving a lot of real-time data and then either displaying or analyzing it.


2. **Customer-Facing and Content-Driven:** Usually doesn’t have heavy upload volumes but serves a mixture of graphics, text, searches. These kinds of use cases are already heavily dependent on CDN to meet the application’s requirements.


Before I go any further,[here’s a link to the waitlist](https://www.cloudflare.com/products/r2/) for Cloudflare R2.


‍


## Future goals for Cloudflare Workers


According to a recent Discord Q/A with the Cloudflare workers team, Cloudflare R2 is going to be deeply integrated with[Cloudflare Workers](https://workers.cloudflare.com/) .


This integrated compute layer can act like a proxy to enable “[slurp mode](https://news.ycombinator.com/item?id=28703464) ”. Workers would handle the request for an object. If the object isn’t already in Cloudflare R2, they would request the object from Amazon S3 and store it in Cloudflare R2.


While other capabilities are still unknown, Cloudflare workers would likely be able to manage:


- CORs and other headers
- Access control


The Cloudflare workers integration with R2 will not be as strong as the AWS Lambda integration with S3. For more information on how they compare, check out the feature comparison of[Cloudflare R2 vs Amazon S3](https://www.taloflow.ai/blog/cloudflare-r2-vs-amazon-s3) .


‍


## Why Ideal Customers Should Hesitate To Switch


**We don’t know everything yet.** However, seemingly ideal customers for Cloudflare R2 should hesitate to switch because of two reasons: ****


1. **There could be a catch:** Whether it really offers unlimited zero-cost egress is a bit hard to fathom. One astute Twitter observer commenting on the launch[pointed out](https://twitter.com/CarlHasselskog/status/1442877506403504130) how unrealistic it would be for Cloudflare to allow its customers to pull out several Petabytes of data out of network at zero cost. ****


1. **The savings are greater elsewhere:** If you’re switching off of a Big 3 provider, you may as well maximize your savings. Backblaze B2 and Wasabi are part of the[Bandwidth Alliance](https://www.cloudflare.com/bandwidth-alliance/) . Wasabi **already** offers zero fee egress, albeit its offering comes with a fair use policy / limit that doesn’t allow you to download more than you have stored in any given month. Aside from that, Storj DCS, Backblaze B2, and Wasabi* are 3 times cheaper than Cloudflare R2 for storage. ****


** Wasabi will charge you for a minimum of 3 months of storage for any uploaded data. For example, if you upload a file and then delete it within 1 month, you’ll get charged for the remaining 2 months.*


*‍* ****


## Who Shouldn’t Be On Cloudflare R2


**We don’t know everything yet.** However, some potential customers should probably stay or pick with another provider if the following applies:


1. **Your use case is mostly about writing to your storage service → Go with Storj DCS, Wasabi* or Backblaze B2:**
Example 1: Your use case is primarily Archival and Compliance
Example 2: Your use case is primarily Backup and Recovery
2. **Your use case has high connectedness → Stay/go with your primary cloud provider, like AWS:** Your storage service is communicating a lot with services offered by your existing provider. For example, you’re using a lot of Amazon Athena.


‍


## What we still don’t know about Cloudflare R2


- **What kind of consistency R2 will provide?** Still unclear.
- **When will R2 support Azure for migrations?** It doesn’t yet.
- **What S3-compatible implementation is being used?** It is tough to cover all the features of Amazon S3.
- **Is R2 actually going to be faster?** They’ve employed storage tiering strategies to keep the costs low. When you do that you sacrifice access speed for things you sometimes need.
- **Can AWS respond and become more competitive on price?** Well, yes; AWS can absorb the margin impact elsewhere to buy it time to innovate in storage. AWS simply has a massive shell-game. If we step back a bit...AWS Lambda was probably unprofitable for AWS at first (it wasn’t optimized), but AWS had various ways to change the margin in other areas to focus on driving serverless adoption.


## Hacker News Comment Responses


‍


### Negotiating Price


*"As for bandwidth, CloudFlare becomes uninteresting the moment your account starts to see any real consumption, even AWS are easier to negotiate with." (*[View Comment](https://news.ycombinator.com/item?id=28684066) *)*


**Our take:** Fair enough. We’ve helped several companies get discounts from Amazon S3 or Amazon CloudFront CDN anywhere from 5 to 15%. AWS is willing to play ball and is aggressively fighting for enterprise customers.


‍


### Bucket Notifications


*"There is one major reason S3 remains the king of storage for mobile media uploads: bucket notifications. Does R2 implement this feature? If so, I’m going to have to run some experiments with this…" (*[View Comment](https://news.ycombinator.com/item?id=28683691) *)*


**Our take:** As some other HN users commented, Amazon S3 is loaded with features, particularly in the area of “eventing”. For example, if you wanted to trigger a Lambda off of an event on Amazon S3, you can!


If Cloudflare R2 lacks eventing it's going to be too much of a drawback for any use case that requires real-time workflows triggered off of bucket events.


‍


### Future Price Competition


*"How will other providers respond to this now? AWS, GC, and others do not really pay for egress charges themselves. Those super high egress charges are pretty ridiculous." (*[View Comment](https://news.ycombinator.com/item?id=28683628) *)*


**Our take:** There's a lot of margin here for the behemoth cloud providers to play with and they can respond quickly. We should stay tuned for changes if Cloudflare R2 manifests itself as a real threat post-launch.


‍


## Want to Continue The Conversation?


We love chatting with developers about the future of cloud, and cloud object storage in particular. If you want to riff on a decision you're looking to make, drop us a line here:[\[email protected\]](https://www.taloflow.ai/cdn-cgi/l/email-protection#afdbcacec2efdbcec3c0c9c3c0d881cec6)
