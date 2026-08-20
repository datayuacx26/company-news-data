---
schema_version: "1.0.0"
document_id: "482a85347a232ab4d10f348bb9ec165ba226ee52e8eb020db0cc3ae9bb05d778"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/chaos-leads-to-resilience"
published_at: "2022-11-10T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:592478b8f264269d4de78632d6c12fde5810e7af9b46214c326008f4f0ddc235"
---

# Chaos Leads to Resilience

Target has adopted a distributed, microservices architecture. With this comes with a heightened level of complexity. Changes or simple outages in interdependent services can trigger major outages in production since global testing and debugging is difficult. Consequently, an outage impacting a critical application’s reliability and availability can lead to cascading application downtime, revenue loss, and potentially a negative impact to our brand.


Chaos experiments are a very effective tool to identify and overcome deficiencies and to build confidence in the resilience of our microservice architecture. In this post, I’ll explain what chaos experiments are, how they can help teams, and how we use them at Target.


What Is a Chaos Experiment?


A chaos experiment is designed to intentionally break or disrupt an application based on an assumption of how it is expected to behave. Running these experiments helps teams improve their understanding of the system and expose possible weaknesses, such as chicken-egg problems or consumers’ implicit assumptions of behavior.


Chaos testing was introduced back in 2010 by a popular streaming service to provide a more reliable, uninterrupted streaming experience to their customers after moving to a cloud infrastructure. The methodology caught on industry-wide in companies that valued always-available systems. There are many other companies across retail and social networks that currently practice chaos testing.


Disaster recovery is a process that helps organizations get back on track during any major outage. Disaster plans need to be tested regularly to validate if applications can be recovered within a specified timeframe. Critical systems need to be tested regularly to make sure they are working as expected when needed. Even though there are several testing mechanisms like Tabletop, Simulation, Checklist Testing, and Full Interruption testing, none help understand how a system’s alerting mechanism or the operations team behaves during an outage. That’s why chaos experiments are important.


Chaos Testing at Target


When we run our own chaos tests, we start with a minimal blast radius (the portion of the application or its components that can be impacted during the chaos experiment) in lower environments such as stage or integration region. For example, bringing down one server, injecting latency between services for a small percentage of traffic or by increasing/decreasing the load on resources such as CPU, memory, IO, and disk gradually. We start small to begin observing our system behavior while running these types of experiments. After gaining confidence in running smaller experiments, we increase blast radius by subjecting more components to potential failure, then rerun experiments.


In 2019, Target conducted about sixteen chaos testing modules across fifteen different applications in a non-production environment. Just before our “peak” season over the holidays in 2020, we performed our first chaos test in a production environment. Key findings helped us identify that our dashboards were not detecting behavior correctly, alerts were not set properly, the applications were degrading with increased latency, and many more. These learnings were important and informed areas of opportunity to improve moving forward. We were also able to validate successful recovery processes and procedures and our team’s preparedness for any outages.


To run chaos experiments in production, we worked with the critical application team and reviewed our architecture, identified dependent components, and deployed fault-injection tools to help us conduct experiments. This class of tools help us to disconnect servers, increase latency between two services, or dramatically increase load on a server. Finally, we validated if all observabilities were in place for running an attack on relevant resources (CPU, storage, memory, and network latency and bandwidth). We want to run and make sure that we have observability to understand the current status of each application while running those experiments.


What We Learned


Some of the findings following a chaos experiment cannot be identified by following traditional ways of disaster recovery testing. With those experiments, we learned that some application’s dashboards were not detecting behavior properly, alerting threshold setups were wrong, and some gracefully degraded with increased latency instead of hard failure. All these would not have been identified by following traditional ways of disaster recovery testing. This makes chaos testing one of the best, most efficient, and most effective methods to test an organization’s disaster recovery plan. It provides a mechanism to verify an application’s resiliency capability and validate operational readiness by purposefully injecting faults so we can know what must happen in order to recover from faults – all in ways that will help us serve our guests seamlessly and without interruption.
