---
schema_version: "1.0.0"
document_id: "20059a6dad61afb89bf676ffe7bed4185b5e34a30b2bfc87aab41520e860b70d"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/so-you-want-to-rent-an-nvidia-h100-cluster-2024-consumer-guide"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:d73eabb67105db062133ffac78ec47cc2047649d8779c218e592de82bfc95971"
---

# So you want to rent an NVIDIA H100 cluster? 2024 Consumer Guide

With the NVIDIA H100 becoming the hottest commodity, you've probably considered it as a birthday gift for your partner or a great graduation present for your kid.


Consumer reports are notoriously lacking on the topic, so we thought we might enlighten you on decision criteria to pick the perfect cluster for your needs.


After an extensive search for a 256 H100 cluster to provide a reliable playing field for the ML team at Photoroom, we learned what to look for when renting one. Below are our learnings.


*This blog post does not contain any private pricing information and does not directly compare any provider against another.*


## Our approach


This was our first time renting a large-ish GPU cluster 24/7. Training interruptions and inability to launch trainings can really hinder the ability of the team to build powerful models. Therefore, we prioritized reliability over all other criteria, even price.


### Price


But since “how much does it cost” is the question on everyone’s mind, we’ll start with costs. Most people measure it in terms of $ per GPU hour. There are 3 criteria that will impact the price:


-


commit size (how many GPUs)


-


commit duration


-


upfront payment


Cluster sizes can go up to few tens of thousand of GPUs, dwarfing the 256 GPU cluster we’re renting. Currently, most clusters can be rented from 1 month to 3 years, after which it becomes more economically viable to buy.


Upfront payments vary from provider to provider, with most of the independent ones requiring at least 25% upfront to secure the deal. Avoid 100% upfront deals to maintain leverage and ensure the provider's financial health before transferring large sums to someone you haven't met in person.


On top of that price, you need to factor in a few elements: cost of support (a few % at most hyperscalers), cost of storage, egress.


### Interconnect and reliability


The interconnect is the neural spine of your cluster. That’s where all of the information flows. An unreliable interconnect can crash your training periodically. Crashes lead to wasted training resources (need to restart from last checkpoint) and ML practitioner headaches (need to babysit the training).


There are two main types of interconnect: Infiniband or Ethernet. Infiniband is from NVIDIA (they acquired Mellanox in 2019). It’s considered the Rolls Royce of the interconnects: expensive but reliable. Many providers want to avoid relying on NVIDIA too much, therefore they rely on an independent Ethernet solution (EFA at AWS, RoCE at Oracle, TCPX at Google Cloud).


From our tests, we found that Infiniband was systematically outperforming Ethernet interconnects in terms of speed. When using 16 nodes / 128 GPUs, the difference varied from 3% to 10% in terms of distributed training throughput\[1\]. The gap was widening as we were adding more nodes: Infiniband was scaling almost linearly, while other interconnects scaled less efficiently.


*Graph comparing throughput between Infiniband vs a purposefully unnamed Ethernet interconnect. Each node contains 8 GPUs*


We also found that Ethernet was less stable overall. We did manage to run 48h uninterrupted training on some Ethernet interconnects, but faced some obscure errors on some others.


*A few examples of obscure interconnect issues that can be painful to debug*


Our recommendation is to test with your workload. If the cluster is 5% slower but 10% cheaper while remaining stable, then it’s worth it to go with Ethernet.


### Spare nodes, node location and SLA


Brand-new GPUs tend to break right after they come out of the factory. When launching a training on a newly-built cluster, it’s not unexpected to see a few % of the GPUs fail. Even after a burn-in test from the GPU provider. Because it will happen to you, it’s a good idea to align with your provider on what happens in that case.


Spare nodes are machines kept on standby in case one of the machines in your cluster encounters an issue. Some providers can swap it instantly (a few minutes) with an API call. Others require you to open a ticket, escalate it and take much longer to swap the node.


A node with a broken GPU is virtually useless; therefore, it’s a good idea to provision in the SLA that a node that doesn’t have 8 healthy GPUs counts as a broken node.


Node colocation is important. Most providers ensure all nodes are in close proximity to each other. If your packets have to traverse multiple switches and move to another building, the chances of training instability are higher. Ensure node location can be enforced.


### Storage and streaming data


To keep those GPUs busy, you need to keep them fed with data. There are several options. We’ve decided to go with VAST storage: data is stored on dedicated SSD machines in the same data center and streamed. With this technology, we’re able to stream at a very comfortable ~100 Gbps without any hiccups. Note that we work with images, so when working with text, you may have lower data needs.


Most H100 nodes come with 27 TB of physical storage attached to them. There are a few techniques to transform those disks into distributed ones, such as[Ceph](https://docs.ceph.com/) . We decided once more to go with the reliable, less-hacky solution and picked the more expensive VAST. That leaves ~800 TB of storage used only for caching, but we expect the reliability to be worth it.


Not all providers offer all storage solutions, so ask in advance.


Note that depending on where your data is, you might need to move it in advance as streaming it is unrealistic. Assuming our datacenter had enough bandwidth to stream from an AWS zone at 100 Gbp/s and that we would be silly enough to not cache anything, it would cost ~$0.6 / second or $1.6M / month just in egress cost 🤯 \[2\]


### Support, “distance to engineering”, GPU owner


In our experience, nothing beats the convenience of a shared Slack channel with the engineers managing the cluster. If you’ve lived through tickets opened for weeks with no useful answer or resolution in sight, you know how convenient it is to chat and pair program directly with the engineers in charge


Some large providers told us very clearly that a direct line of contact with engineers “will never happen” and that even with the highest support tier we’d need to go through the ticketing system. It’s possible we’re too small to get that privilege. When selecting your provider, ask what kind of support they offer and make sure you have an SLA on response times.


*Example of an interaction that would rarely happen on a ticketing system*


This line of support is key as you can expect issues to arise at all times during training. If you look at Mistral blog posts, they often thank their GPU providers “for their 24/7 help in marshaling our cluster” (Mistral trainings are larger than ours).


### Bare metal vs VMs vs Managed SLURM


You’ll need to determine how you’ll orchestrate and manage the machines in the cluster. There are a few options:


-


Bare metal: You’re provided with access to the machine, and you manage the software running on them from A to Z.


-


Kubernetes (K8s): provider maintains the Kubernetes setup, you select which containers you want to run on each machine


-


SLURM: provider maintains the[SLURM workload](https://slurm.schedmd.com/documentation.html) manager setup


-


Virtual Machines (VMs): provider maintains the hypervisor, you get access through a Virtual Machine.


Our provider didn’t offer to manage SLURM for us, therefore we went with VMs and manage our own SLURM setup. We didn’t go with Bare Metal as it’s more management and VMs offer more options to our provider to properly monitor the machines. Note there’s a very small performance loss from the virtualization (less than 1%).


*Breakdown of the stacks offered on[gpulist.ai](http://gpulist.ai/)*


### Test before you buy


We insisted on getting a 48h uninterrupted run on the cluster before committing. It was also a key requirement to be able to run on the exact same cluster as the one we would be renting. We started with 4 nodes to iron out issues without paying too much, then expanded to 32 nodes. It took a few attempts to get a smooth run, for various reasons (node failing, interconnect issues).


You can also trust your provider that the cluster you’ll be getting will be “almost the same” as the one they can offer to test on, but it’s riskier. Especially if it’s not in the same datacenter. It might be required if the cluster you’re renting is not ready yet.


# Monitoring GPU utilization


Discussing with providers, we learned that the average cluster utilization in the industry is between 30 and 50%. Surprising, isn’t it? I’ve learned that the main reason companies are committing is not to optimize for the price per hour. It is to ensure their ML team has access to GPUs at any time, which is not possible with an on-demand solution.


*GPU utilization of the 256 GPU cluster used by the ML team at Photoroom. Purple is a one week rolling average. We expect the utilization to go up in the next weeks as we’re getting more familiar with the cluster.*


Our dream setup would be to have an on-demand cluster, where you launch trainings and only pay for utilization. We’ve been looking for years and no one was able to offer an SLA on GPU availability (e.g. “95% of the time, you won’t wait more than 10 minutes to launch a training”). Which makes sense: in the current GPU craze, keeping GPUs on standby for your customers would be crazy.


### Electricity sources and CO2 emissions


When running in the US, a 256 H100 cluster consumes ~1000 tons of CO2 in a year \[3\]. That’s 1000 Paris-NYC trips.


Some providers compensate with carbon credits, we believe this has limited impact. The GPU boom causes more gas plants or coal plants to be put online \[4\]. Others build solar and wind plants, injecting green electricity in the grid they consume. A few run GPUs in countries with a grid that runs exclusively on green electricity (hydroelectric, geothermal).


*Electricity intensity for a few of the countries we’ve considered for our GPU cluster.[Source](https://ourworldindata.org/grapher/carbon-intensity-electricity?tab=chart&time=latest..2023&country=FRA~EU-27~OWID_WRL~NLD~USA~IND~DEU~NOR)*


While that criteria might seem secondary to some, as the co-founder of a fast growing startup I believe our decisions can have an impact. By prioritizing environmental considerations, I hope to inspire other companies to do the same.


If you’re an ML practitioner, you can do your part. Help put this important topic on the map by asking your manager “what’s the CO2 footprint of our cluster” and advocate to move towards a greener one.


## Other criteria and tips


-


Availability date: is your cluster available now or will you have to wait? If so, is the date enforced in the contract? We rented another smaller cluster with a different provider and it is currently one month late.


-


Burn-in: will the cluster be extensively tested before you get access to it or will you be the guinea pig figuring out the last issues?


-


Renewal: will you be able to renew with that provider and how much of an advance notice do you need to give?


-


Note that for training, you’re not interested in H100 PCIe and require HXM5. Although they share the name, PCIE H100s are also less powerful (less cores, lower power rating)


-


Some providers don’t own the GPU they rent to you. They manage the support and software stack. If one GPU malfunctions, this adds another layer: you -> provider -> GPU owner -> SuperMicro/Gigabit -> NVIDIA.


-


Ask for the exact name of the datacenter your GPUs will be in, then look at how experienced the company running it is


-


If you get the chance, get some H200s. I’ve been told NVIDIA sells them at the same price as H100. Thanks to the double memory, they end up being faster


## Conclusion


Thank you to David from the Photoroom team who thoroughly ran the benchmarks on the GPUs, slayed CUDA issues and ultimately helped us pick the best provider.


Ultimately, we’ve decided to go with Genesis Cloud, a German provider that offers an Infiniband cluster that runs on CO2-free hydroelectricity. This blog post is not an endorsement or an ad. Nevertheless, we encourage companies who can factor-in CO2 emission in their decision to do it, as the impact on climate is tremendous. As a company, we're only part of the way there, as our inference workloads are running in the US in majority.


If you’re interested in joining a small and efficient ML team who now has the compute to do marvels, have a look at our[open positions](https://www.photoroom.com/company) .


---


*Great articles on the topic of startups running their own training GPU cluster:*


-


**[From bare metal to a 70B model: infrastructure set-up and scripts](https://imbue.com/research/70b-infrastructure/)**


-


**[Training great LLMs entirely from ground up in the wilderness as a startup](https://www.yitay.net/blog/training-great-llms-entirely-from-ground-zero-in-the-wilderness)**


*\[1\] We’re not interconnect experts and probably haven’t spent enough time fine-tuning the configuration.*


*\[2\] Using the list price of 5 cent / GB (AWS’ cheapest egress tier), assuming no caching (which would be absurd).* 12.5 \[GB/s\] * 3600 * 24 * 30 * 0.05 = 1 620 000


*\[3\] kg_co2_per_year = kW_per_node_per_hour * hours_per_year * num_nodes * PUE * co2_emitted_per_kW_hour = 10 * (24*365) * 32 * 1.2 * 0.369 = ~1241 tons, assuming a 100% cluster utilization. A Paris-NYC flight is roughly[1 ton of CO2](https://curb6.com/footprint/flights/paris-cdg/new-york-jfk)*


*\[4\][AI is exhausting the power grid, Washington Post](https://www.washingtonpost.com/business/2024/06/21/artificial-intelligence-nuclear-fusion-climate/) (version without paywall on[msn.com](https://www.msn.com/en-us/money/technology/ai-is-exhausting-the-power-grid-tech-firms-are-seeking-a-miracle-solution/ar-BB1oDl5z) )*
