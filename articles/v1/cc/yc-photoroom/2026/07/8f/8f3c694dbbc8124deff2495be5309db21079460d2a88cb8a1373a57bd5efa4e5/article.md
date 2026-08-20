---
schema_version: "1.0.0"
document_id: "8f3c694dbbc8124deff2495be5309db21079460d2a88cb8a1373a57bd5efa4e5"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/hunt-cheap-gpu-deep-learning"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:d8a40f4c2eae9e2e599b35583dcaed86a62f0cc723cf6d98538b232e377b0367"
---

# The Hunt for Cheap Deep Learning GPUs

*Recently, I have been struggling to find cheap and reliable GPUs to train deep learning models. In this article, I will summarize the options you have to run deep learning computations on GPUs.*


Not too long ago, you could rent a beefy GPU machine for 100€/month. Hetzner, a German server provider, was offering those specs:


It was fast and reliable. The good times. However, they discontinued this offering. Nowadays, if you want to get a GPU for deep learning, you have several options:


-


Use a cloud provider (GCP, AWS, Azure)


-


Use a cloud provider with preemptible machines


-


Rent a bare metal machine


-


Build your own


## Foreword


Hetzner offered cheap and reliable servers. They had a[good reputation](https://hn.algolia.com/?dateRange=all&page=0&prefix=true&query=hetzner%20gpu&sort=byPopularity&type=comment) . Why did they stop? While there is no official reason, it is likely that changes in the Nvidia's license is the reason. NVIDIA[updated their license](https://www.datacenterknowledge.com/machine-learning/nvidia-wants-block-use-cheaper-consumer-gpus-data-center) to ban the use of consumer GPUs (e.g. 1080, 2080 models) in their data centers. Therefore, most large server provider stopped offering cheap GPU servers.


## Using a Cloud provider


Google Cloud, AWS and Azure all offer GPU machines. This is the most expensive option in our list. In theory, you can scale your cluster's size on demand. They offer GPUs for training (V100) and inference (T4).


**My experience:** some providers run unscheduled maintenance on your machine. It means they will need to kill your instance to migrate it to another (but keep the content of the disk). You get a 1 hour termination notice for GCP, more for the others. It very inconvenient when you start a large training over the weekend, only to realize that your machine has been killed on Friday evening. On top of that, some regions sometimes run out of GPUs. This means that when attempting to create a machine, it will fail. This does not happen often, but when it does it is very annoying.


**Pros:**


-


Scaling on demand (limited by quota and availability)


-


Can pick any number of CPUs (useful for preprocessing-intensive jobs)


**Cons:**


-


Unscheduled maintenance is a pain(1 hour notice for GCP, ~24hours for AWS, can happen once a week)


-


Expensive


## Using preemptible instances


Most cloud providers offer preemptible machines, with a significant discount (at least 50%, often more). In exchange, you accept that your machine can be killed at any moment. It is not very convenient when training models and saving the checkpoints every epoch. Working around that takes a lot of engineering.


**My experience** : my instances are sometimes killed in less than an hour, making it unusable. Try it out and see if it works for you (might depend on the region)


**Pros:**


-


Cheaper


-


Scalable


**Cons:**


-


Machine can be killed at any moment


## Renting a bare metal machine


Some providers are still offering consumer GPUs, officially not for deep learning. A Google search will yield plenty of them. You can also look[here](https://www.serverhunter.com/?search=BF6-4AC-E30) . The price vary from provider to provider.


**My experience** : Reliability is not great. I made the mistake of using one of those servers as a production server. Then, it went down on a Saturday at 1 am. Here is the support's answer:


YMMV, and you must make your own arbitrage between price and reliability.


**Pros:**


-


Cheap, plenty


-


No weekly scheduled maintenance


**Cons:**


-


Sometimes unreliable (YMMV)


-


Does not scale quickly as with a regular cloud provider (need to order the machine, sometimes need a monthly commitment)


## Subrenting a server


I never tried this, but[vast.ai](https://vast.ai/) is a marketplace offering very affordable prices. Anyone can list a GPU there, therefore I am not exactly sure how reliable it is.


## Build your GPU server


If you have the time and the rack space, building your own GPU machine might be the cheapest option. Depending on how cheap you need to go, keep an eye for used GPUs on eBay. Keep in mind that you will have to pay for electricity and that having a noisy machine heating your office in the middle of summer is the best way to turn your colleagues into enemies.


**Pros:**


-


Cheapest option (depending on electricity cost)


-


Custom specs (useful if you need plenty of storage)


**Cons:**


-


Time consuming


-


Not convenient (noise, heat)


## What we ended up doing at Photoroom


For training, we built our own machine (using 2080 TIs). For larger training, we use GCP with V100s and cross fingers that there will not be any maintenance event. For inference, we use GCP's T4 GPUs, in a managed instance group. This means that if they need to kill a machine for maintenance, they will automatically spin up a new one.


## Conclusion


Please keep in mind that I am not endorsing any of those options, pick one at your own risk. In the end, it's a trade-off between price, convenience, reliability and scalability. Also note that running inference on CPUs can be cheaper. A few helpful links:


-


[Server Hunter](https://www.serverhunter.com/?search=BF6-4AC-E30)


-


[Low End Talk Forum](http://lowendtalk.com/)


-


[Ask HN: How can I quickly trim my \[GPU\] AWS bill?](https://news.ycombinator.com/item?id=23798347)


Any idea on how to improve this ? Any comment? Reach out on Twitter
