---
schema_version: "1.0.0"
document_id: "b79a9efaa102a57e07757fd70552bab8dcbfc103115bacfe0f677abce9d555bb"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/are-archils-sandboxes-as-free-as-cloudflare-egress"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:00dc912bd8ed3807a848a21032601a1a104a856d809746b513cb3295adeed0ef"
---

# Are Archil's sandboxes as free as Cloudflare egress?

One thing that customers are most surprised by when they move to Archil is that their sandbox costs usually go to $0 by moving to Serverless execution. This is great for them, but also a little confusing, so I wanted to spend some time explaining how we're able to accomplish this feat of giving away sandboxes for free.


To start, it's important to note that operating a cloud service is distinctly different than just building great software (though, that's a component, of course). You also have to have a team with a mastery of operations (how to respond and manage incidents), and a deep understanding of how to financially align incentives between you and your customers.


To explain this, I want to start out by looking at the cost model of Cloudflare's R2 service.


## Free as in R2 Egress


Most people know that Cloudflare's R2 service is a lower-cost object storage alternative to Amazon S3 that doesn't charge for egress fees. Some people know that the "free egress" is only true until you hit a certain amount of scale, and then Cloudflare is either going to start charging you for egress or throttle your egress.


Compare this to Amazon S3 where egress is considered to be highway robbery, but they will happily support your ability to drive hundreds of gigabytes of throughput out to the internet at a given notice.


Why are these two models different? Consider how egress traffic works from a cloud-provider point of view (and, forgive me, because the specifics of this process aren't my domain of expertise, but the rough shape should be correct).


When you purchase space in a data center (not a hyperscaler), you get the opportunity to rent specific transfer ports to the internet. These transfer ports are rated to some amount of bandwidth (think 100 or 400 gigabit), and not charged by byte transferred. Then, you would pay the data center (or network provider, again idk) some monthly maintenance fee to keep the port online for you and shuttle the traffic back and forth.


Even though the port is charged on a per-byte transferred point of view, there is a maximum amount of traffic that any given port can transfer in a month. For a port that is rated for 100 Gbps (12.5 GB/s), it can only transfer 32 PB at 100% utilization over the course of the month.


If you got charged $32,000 (made up, but assume this is an all-in cost of maintenance, electricity, peering contracts, etc) monthly for access to this port, then you would in fact be paying an effective per-byte transferred cost of $1 per terabyte out to the internet.


If you were AWS and you were selling this to your customers, you would add in some other adjustments to the cost (such as the average utilization of the month, say like 70%, and an infrastructure margin) to come out with the final price that customers pay. In our model world, for simplicity, let's say that Amazon charges people $2/TB for egress.


Now, let's consider the two different models side-by-side. Let's assume that both providers have maxed out their bandwidth already -- they don't have any ability, without purchasing a new port, to serve any additional traffic to the internet from their storage services (contrived, I know). Now, a new customer comes to both Cloudflare R2 and Amazon S3 and says:


I have 1 GB of data that I want to store in your object storage system, and I need to transfer it out so many times over the month that I will rack up 32 PB of egress over the course of the month. Obviously, this customer prefers to use Cloudflare R2 because they won't be charged egress on this data (only $0.02 for storage!). However, because Cloudflare is already at-capacity for egress traffic, they would need to provision an entire additional port for this customer, an additional $32K/mo cost for the business to serve this customer. What happens?


Well, Cloudflare just tells this customer to go somewhere else -- or they throttle the customer when they eventually drive too much egress to their bucket. The Cloudflare business isn't incentivized to take on a new customer at a -16,000% gross margin. On the other hand, AWS is totally happy to serve this customer because they profitably purchase the additional egress port to serve customer traffic at a healthy +50% gross margin.


This is why AWS is so intent on charging for every dimension of the product that they sell. It isn't because they are trying to penny-pinch, it's because they want to properly align incentives between the business and customers to support all of the different shapes of workloads that customers might bring to them.


## Free as in Archil Sandboxes


Now, surely, we've learned that you cannot give parts of your product away for free without causing a big problem for incentives. Therefore, Archil sandboxes being free must be some kind of a problem for the long-term health of the business.


It turns out that this is not the case, because Archil's Serverless execution product isn't free like Cloudflare egress, it's bundled with your storage.


Again, let's consider what's happening from the Archil point of view here. We rent, from the hyperscalers, what they call "dense SSD storage instances". These instances have the regular things that you would expect -- CPU, RAM -- but also a ton of SSD storage locally attached to them. These are the same instances that run products like PlanetScale Metal.


However, a database like what PlanetScale Metal runs has a very different shape than a file system. Databases notably need to use a lot of CPU in addition to the storage that they hold, to run SQL, execute query plans, aggregate results. These dense SSD instances are actually purpose-built by the hyperscalers to serve database workloads (note how many more database vendors there are than file system vendors).


File systems, on the other hand, are not CPU intensive. They just read and write bytes from disk, and shuttle them back to users (who do the aggregation themselves). As a result, file systems that run on these servers often have lots of extra CPU that they aren't using as part of the storage service.


In the cloud world, we call this an "orphaned" or "stranded" resource. We have all of this CPU available and nothing to do with it!


For Archil, serverless execution gives us an answer for how to deal with it. You see, when we came up with the cost model for Archil, we didn't yet know about how we would build and market Serverless execution, so when you buy 1 GB of storage from us, you're actually buying 1 GB of the storage server amortized over the month. This means that the "slice" of the box that you're paying for actually included some amount of this free CPU resource that we would otherwise be throwing away.


As our storage clusters continue to scale, the absolute amount of available CPU "for free" just continues to grow.


There are two ways that we could handle this as a company.


First, we could just accept this as a margin boost -- we have free CPU, awesome! And continue to sell our storage and compute products without taking that fact into considerion.


Or, we could do the thing that's best for our customers and actually *give them* the resources that they are paying for, even if it wasn't what we originally expected.


You can imagine which path we ended up taking. As a result, we give people 30m of sandbox usage each month for *every* $1 in storage spend (5 GB) that is on the platform.


Now, unlike the Cloudflare egress problem, this isn't free forever, it's just free as your resource usage continues to scale. If we're out of capacity for the free CPUs and you need to launch additional sandboxes, we'll happily allow it -- because we have a path to charging you on that additional compute used. We don't throttle you, or tell you to go away. This is aligning incentives between our customers and how we procure hardware to give people the best of both worlds.


But, for the vast majority of customers, who don't have 30m of "active" CPU time each month for every 5 GB of data their agents use, they end up with sandboxes that are completely free of charge. Pretty cool.
