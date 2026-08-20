---
schema_version: "1.0.0"
document_id: "701d8071b9e746e61d15a8eaf967ee8a0ba824a4a63929c8b83e02383d4c7706"
company_key: "dropbox-inc-class-a-common-stock"
company: "Dropbox Inc."
source_id: "dropbox-inc-class-a-common-stock-rss-58b41d3bcd8e"
canonical_url: "https://dropbox.tech/infrastructure/improving-infrastructure-efficiency-for-growing-demand-in-the-age-of-ai"
published_at: "2026-08-18T17:00:00+00:00"
first_seen_at: "2026-08-18T21:08:26.405333+00:00"
fetched_at: "2026-08-18T21:08:27.594504+00:00"
content_hash: "sha256:ee59b4229665f88c63b87332c3bb486d2e4e2c8d4e0ef72c403b0b8f8f1dafa1"
---

# Improving infrastructure efficiency for growing demand in the age of AI

As demand for AI continues to grow, so does the infrastructure needed to support it. Much of the industry's attention has focused on building more: more data centers, more servers, and more power. But building new infrastructure is only part of the challenge. Engineering teams are also working within constraints on energy, cooling, hardware availability, and physical space, making it increasingly important to get more from the infrastructure that's already in place.


For over a decade, Dropbox’s Infrastructure and Datacenter Engineering teams have continually improved how we plan, operate, and optimize infrastructure. That work spans far more than storage systems. Engineers across Dropbox work together on capacity planning, fleet optimization, hardware lifecycle management, power delivery, cooling, rack design, and facility planning. Rather than treating these as independent problems, we optimize them as parts of a single system, where decisions in one layer influence what's possible in another.


The result is an engineering discipline that extends beyond any single system or optimization. A change in one part of the infrastructure can create opportunities or constraints elsewhere. Increasing storage density, for example, can reduce the amount of hardware needed, while more powerful servers can introduce new energy and cooling requirements. Understanding those tradeoffs allows us to make infrastructure decisions with the entire system in mind.


As infrastructure demand grows, that system-level approach helps us prioritize efficiency and create room for growth before expanding our data center footprint.


## Planning for efficient infrastructure


Many of the decisions that shape infrastructure efficiency happen months or sometimes years before new capacity goes into production. Whether someone is uploading a file to Dropbox or asking[Dash](https://dash.dropbox.com/) a question, they expect the product to respond without delay. Delivering that experience requires engineers to forecast how customer demand and workloads will change, determine when additional resources will be needed, and understand whether our existing environments can support them.


Dropbox has operated large-scale infrastructure for over a decade. Our hybrid model combines[Magic Pocket](https://dropbox.tech/infrastructure/improving-storage-efficiency-in-magic-pocket-our-immutable-blob-store?utm_source=chatgpt.com) , the core Dropbox storage system, with colocated data centers, where we manage our own servers and networking equipment in facilities operated by specialized providers. This gives our engineers visibility across software, hardware, and the physical data center environment.


As customer demand grows and workloads change, particularly with the growth of[AI-powered products and features](https://dropbox.tech/machine-learning/how-we-turned-ai-evaluations-into-better-responses-in-dash-chat) , engineers have to plan not only for how much additional capacity is needed, but where and how it can be deployed. New hardware has to fit within a facility's infrastructure constraints while accounting for hardware availability and future product needs. A server that provides more compute or storage, for example, may also require more energy or cooling, changing how much hardware a rack or facility can support.


The goal is to understand those tradeoffs early, add capacity deliberately, and preserve enough headroom for growth, maintenance, failures, and changing workloads. Once additional compute and storage capacity is deployed, the focus shifts to making the most of the systems already in production.


## Continuously optimizing the active fleet


Planning helps ensure infrastructure is ready for anticipated demand, but workloads rarely behave exactly as they did when that infrastructure was first deployed. Customer behavior changes, products evolve, and new capabilities introduce different demands on the underlying systems. AI is a prime example because AI-powered features can change both the scale and shape of infrastructure demand, placing new demands on compute, storage, memory, and networking.


That makes efficiency an ongoing engineering problem. Rather than treating deployed infrastructure as fixed, Dropbox continually adapts how the active fleet operates as those demands change. Sometimes that means reducing how much hardware is in use when demand is lower. Sometimes it means shifting work to parts of the fleet with resources available. And in other instances, advances in hardware allow the same physical infrastructure to support substantially more storage.


These approaches work at different layers of the system, but they share the same objective: getting more useful capacity from the infrastructure already in place before adding more of it.


**Letting capacity rest when it isn't needed**
Infrastructure has to be provisioned for periods of higher demand, with additional headroom built in for reliability and maintenance. That means not all available capacity is needed at all times. When hardware is sitting idle or excess capacity is available, keeping every component fully powered consumes energy without providing additional value. Deep Sleep is one way Dropbox reduces that overhead.


Deep Sleep is a Dropbox infrastructure initiative that reduces power consumption when hardware isn't actively needed. Depending on the hardware, that can mean spinning down hard drives into standby mode or powering down idle servers altogether. A server becomes eligible for Deep Sleep through automated fleet management algorithms. Dropbox is able to balance energy efficiency with performance and reliability because servers can return to service within minutes. For workloads that require lower latency, we can also selectively spin down idle hard drives rather than powering down the entire server.


The challenge is determining where those power-saving measures can be applied safely. Engineers have to preserve enough available capacity to meet operational and reliability requirements while identifying hardware that doesn't need to remain fully powered. That allows Dropbox to reduce the energy consumed by underused infrastructure without compromising the capacity our products depend on.


Reducing the power consumed by idle hardware is one way to make the existing fleet more efficient. Another is making better use of the infrastructure that's already online.


**Balancing work across the fleet** Having enough capacity to meet overall demand is only part of operating infrastructure efficiently. That capacity also needs to be available where the work is happening. One part of the fleet may be approaching its limits while another has room to take on more work. Without a way to address that imbalance, Dropbox could end up adding more infrastructure instead of making better use of what’s already available across the fleet.


To avoid that, Dropbox continually monitors how workloads are using resources across our infrastructure. The team identifies imbalances by monitoring spare capacity and workload distribution across the fleet, looking for areas where available headroom is falling or work is concentrating unevenly. When demand is uneven, teams can rebalance workloads across systems or bring additional capacity online where it's needed. Some adjustments happen automatically, while larger changes are reviewed and validated by engineers. This allows us to take advantage of available resources elsewhere rather than treating a localized constraint as a need for more infrastructure overall.


That balancing has to continue as the fleet changes. Customer behavior shifts, products introduce new workload patterns, and the infrastructure itself evolves. Continually adapting where work runs helps Dropbox get more from the capacity that's already online.


Balancing workloads helps make better use of the capacity we already have, but efficiency can also come from increasing how much that infrastructure can support in the first place.


**Increasing storage density** Another way to get more from existing infrastructure is to increase how much each piece of hardware can support. Advances in storage technology have allowed Dropbox to store significantly more customer data on each drive. One example is shingled magnetic recording, which packs data more densely onto a hard drive without increasing its physical size.


Those gains compound across the infrastructure. When each drive holds more data, fewer drives are needed to provide the same amount of storage. That can mean fewer servers and racks, less cabling, and lower power and cooling requirements. In turn, increasing the capacity of a single component can reduce the resources required across an entire deployment.


That system-level impact is also why total power consumption doesn't tell the full story of efficiency. As Dropbox grows and stores more customer data, overall energy use may increase even as the infrastructure becomes more efficient. A more useful measure is watts per petabyte, or the amount of power required to support a petabyte of storage. Since 2020, watts per petabyte across our storage infrastructure have improved by more than 50%. Today, it takes less than half as much power to support the same amount of storage as it did in 2020.


Together, all of the approaches described above help Dropbox get more from existing infrastructure by reducing unnecessary power consumption, making better use of available capacity and increasing how much each piece of hardware can support. How long that hardware can reliably remain in service matters, too.


## Extending infrastructure over time


Replacing equipment too early can leave useful capacity on the table, while keeping it too long can introduce reliability and performance risks. Hardware doesn’t become unreliable simply because it reaches a particular age, nor does keeping equipment longer always make sense. To make those decisions, Dropbox monitors how hardware performs in production. Metrics such as annual failure rate help engineers understand how different components and generations of equipment behave over time.


As demand for infrastructure continues to grow, those lifecycle decisions become increasingly important. Getting more from existing infrastructure isn't only about how efficiently hardware operates while it's in service. It's also about understanding how long that hardware can continue operating reliably before additional investment is needed. That data informs whether hardware can remain in service, should be repaired, or needs to be replaced.


Understanding hardware performance holistically allows us to extend the useful life of equipment when it continues to perform reliably instead of relying solely on a fixed replacement schedule. When performance declines or newer hardware provides meaningful improvements in capacity, reliability, or efficiency, we can plan a transition. Reliability comes first. Extending a hardware lifecycle is valuable only when equipment continues to meet our operational standards.


We repair hardware whenever practical to extend its useful life. When equipment can no longer remain in service at Dropbox, we work with trusted partners to resell or responsibly recycle it. Together, these decisions help maximize the value of equipment throughout its lifecycle rather than treating deployment and replacement as the only meaningful milestones. But eventually, new hardware does need to come online. And as servers become more powerful and storage becomes denser, deploying them can introduce a new set of constraints in the physical infrastructure that supports them.


**Engineering beyond the hardware**
Deploying new hardware isn't as simple as swapping one server for another. Before new hardware can go into production, the physical environment has to be able to support it. Our team plans for the power, airflow, rack layout, cabling, and other physical requirements each deployment needs, working closely with our colocation providers along the way.


Those decisions are closely connected. A server that stores more data or delivers more compute may also draw more power or generate more heat. That can change how racks are designed, how equipment is cooled, and even how much hardware a particular area of a data center can support. As infrastructure becomes more powerful and denser, getting more from the hardware depends on making sure the environment around it can evolve, too.


One recent example illustrates how those tradeoffs play out in practice. As Dropbox deployed its[seventh-generation servers](https://dropbox.tech/infrastructure/seventh-generation-server-hardware) , the increased power requirements exceeded the capacity of the existing rack power design. Rather than rebuilding the underlying facility infrastructure, the Hardware Engineering and Datacenter Engineering teams redesigned the rack power architecture, doubling the number of power distribution units per rack while continuing to use the existing busways. The result supported the new hardware without requiring major changes to the data center itself.


It's a reminder that infrastructure improvements don't stop with the hardware itself. As demand grows and hardware evolves to meet it, each new generation has to fit within the constraints of the environment around it.


## What years of operating infrastructure have taught us


The work reflects more than a decade of engineering investment across Dropbox's infrastructure. Capacity planning, fleet optimization, storage systems, hardware lifecycle management, and data center engineering each address different challenges, but together they help us make better use of the infrastructure that supports our products.


None of this work is one and done. As customer demands change, hardware evolves, and new technologies introduce opportunities and new constraints, we improve our infrastructure, each update building on the ones that came before it.


Those lessons have become even more relevant as demand for digital infrastructure continues to grow. AI is accelerating the need for storage and compute across the industry, but the underlying engineering challenge hasn't changed. Infrastructure still has to scale while remaining reliable, efficient, and resilient. As these technologies evolve, we'll continue building systems that support the products our customers rely on today while giving us the flexibility to support what's next.


~ ~ ~


*If building innovative products, experiences, and infrastructure excites you, come build the future with us! Visit*[dropbox.jobs](https://jobs.dropbox.com/) *to see our open roles.*


*This blog post may contain forward-looking statements within the meaning of the Private Securities Litigation Reform Act of 1995. Words such as "believe," "may," "will," "anticipate," "expect," "plan," and similar expressions are intended to identify forward-looking statements. We have based these forward-looking statements largely on our current expectations and projections about future events and trends, and these forward-looking statements are made only as of the date of this blog post. We assume no obligation and do not currently intend to update any such forward-looking statements after the date of this post.*
