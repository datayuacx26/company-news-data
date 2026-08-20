---
schema_version: "1.0.0"
document_id: "af26f0267a3c7c08b6533223ba960d76b0d6083cb75b456a0a09b853d5d1ffd1"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/ubicloud-burstable-vms"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:478d0ed533bdb1d67e2e87ede14e867171210c8e17d67fa050d8d3cc6d30b479"
---

# Ubicloud Burstable VMs starting at $0.01 per hour

## Ubicloud Burstable VMs starting at $0.01 per hour


March 11, 2025 · 2 min read


Maciek Sarnowicz


Contributor


Since the beginning of Ubicloud, we have offered a Standard family of virtual machines featuring dedicated CPUs, memory and IO capacity suitable for all general-purpose workloads. Those VMs also served as the basis for our managed Postgres services. While those have been appreciated by Ubicloud customers, we would love to make compute accessible to everyone at a much lower price point.


### New Burstable VM family


We are happy to announce that we are bringing in a new family of VMs - Burstable, that cost as low as $0.01 per hour / $6.65 per month. Those small instances provide a fraction of shared vCPU and offer an ability to utilize more cpu for occasionally expanding workload. They are best suited for smaller workloads and request-driven workloads of an uneven nature with occasional spikes in the workload. Examples include smaller websites, smaller SaaS services, development and test workloads, and AI agents. Burstable instances cost a fraction of the Standard instances:


Name # of vCPUs Memory Disk (NVMe) Price/month Sample price per month*


burstable-1 1


2GB 10GB ¼ of standard-2 $6.65


burstable-2 2


4GB 20GB ½ of standard-2 $13.40


* Added NVMe capacity and IPv4 available at an additional $3.50 per month. Prices listed are for the Germany region.
All rates billed in per-minute increments.


- Each VM size offers a specific number of vCPUs. A VM can utilize 50% of the host cpu capacity for its assigned vCPU. If needed, it can also burst to 100% of that vCPU.
- The burstable portion of the CPU capacity is shared among multiple VMs and is not guaranteed


- The memory is allocated at a ratio of 2GB per vCPU


The same burstable VMs are available on our Managed PostgreSQL service, which means you can get a managed PostgreSQL instance with 16GB NVMe storage for just $12.41/month.


PostgreSQL instance # of vCPUs Memory Disk (NVMe) PostgreSQL major version Sample price per month*


burstable-1 1


2GB 16GB PostgreSQL 17 $12.41


burstable-2 2


4GB 128GB (higher storage) PostgreSQL 17 $32.49


### Performance


Below is a simple stress test run simulating a CPU-bound and somewhat bursty workload to illustrate some points of how the new instances work.
‍
We run a 1-cpu workload using the stress-ng utility on a burstable-1 instance. The workloads are sized such that they utilize approximately the full capacity of a burstable instance, but because they are not constant loads, they allow us to see some effects of burstability. We run two scenarios:


- a single burstable-1 instance, representing a scenario where no other workloads are running on the shared CPUs ("low-density" scenario)
- multiple burstable-1 instances, representing a scenario where all instances sharing a set of CPUs are equally loaded ("high-density" scenario)


We also baseline the same workload on a standard-2 instance to observe how it behaves on a relatively “unconstrained” VM. Note that the workload is undersized for this instance and does not fully utilize its capacity.


We can observe the following:


- A burstable-1 instance can process more than 150,000 synthetic stress-ng ops per minute.
- When burstable instances run without much neighboring workload, they can utilize the burstablity as they have room to burst into.


- Conversely, when all instances in a shared CPU set are fully loaded, there is no room for bursting, and the instances cannot achieve the same results.


We encourage you to run your specific workload on our servers and compare between VM sizes and families. Each workload will have its specific characteristics, and it is best for you to decide if Burstable instances suit your workload.


### Conclusion


We are introducing a new family of VMs, targeting test, development, and small production workloads. These Burstable VMs are shared-CPU plans that start at $6.65 per month, and can temporarily burst to 2x CPU capacity under increased load. The bursting is automatic and comes at no additional cost, delivering great performance for the price.


Combining this with the rest of the Ubicloud offerings, you can now get a complete stack, including two burstable-VMs, load balancer for managing the traffic across your two VMs, and one managed PostgreSQL, all together for $25/month.


The cloud is overpriced, and Ubicloud is here to change that. We hope this new family of virtual machines will allow more people to try Ubicloud and see what it offers. Give it a go at[https://console.ubicloud.com/](https://console.ubicloud.com/) and let us know what you think.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)
