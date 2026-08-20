---
schema_version: "1.0.0"
document_id: "c257884ff01926a9010826c31f140c7ee17fd214d167dd08e65bac61a13372cd"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/inside-leo-labs-influxdb/"
published_at: "2026-07-21T08:00:00+00:00"
first_seen_at: "2026-07-21T07:31:51.334724+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:dcea1c95db8ac67a02bdee7df9185ae9498bc3cde9e702963ac8e1c4cd5adac1"
---

# Inside LeoLabs: How Radar Engineers Track Over 27,000 Objects in Orbit with InfluxDB

Summary


InfluxDB plays a critical role in LeoLabs’ infrastructure, enabling a lean team to operate with confidence that potential issues will be detected and surfaced in real-time. By offloading the complexity of managing time series data at scale, engineers are free to focus on higher-impact work (such as optimizing their radar network) rather than maintaining and troubleshooting database systems.


Table of Contents


Every month, satellite operators perform thousands of orbital maneuvers as recommended by LeoLabs’ data. They don’t do it to reposition satellites, they do it to avoid hitting things.


With tens of thousands of active satellites and other debris circling the Earth, adding new devices to orbit comes with risk. Satellites are expensive, high-tech devices that require a massive investment to engineer and launch. This means that to get a return on that investment, companies need their satellites to last a long time and to avoid the hazards posed by other satellites and debris in similar orbits. Space is big, but it’s becoming increasingly crowded as investment in the commercial space sector skyrockets.


A satellite in low Earth orbit is unlikely to bump into something else on any given day, but when the expectation is that its lifespan will be on the scale of 5 to 10+ years—thousands of days—that small chance of collision becomes a greater risk. Collisions can take a satellite out, and without the necessary intelligence to avoid other objects around it, there’s no guarantee a satellite can last its full lifespan.


We sat down with Jackson Ball, a radar software engineer at[LeoLabs](https://leolabs.space/?utm_source=website&utm_medium=direct&utm_campaign=inside-leo-labs-influxdb&utm_content=blog) , to talk about what his company is doing to monitor satellites and discuss how it’s using InfluxDB to track and leverage radar data. Armed with a massive stream of real-time data, LeoLabs enables satellite operations by handling the around-the-clock task of managing, monitoring, and positioning objects in orbit, ensuring that its customers don’t have to worry about the[10+ fragmentations a year that happen](https://sdup.esoc.esa.int/discosweb/statistics/) in space.


## What is LeoLabs?


LeoLabs is a Silicon Valley-based space technology company that operates a global network of ground-based radars across the United States, Europe, Australia, and other regions. Its systems are designed to detect, track, and characterize satellites and debris in low Earth orbit, contributing to space traffic management, space domain awareness, and collision avoidance.


Rather than relying on existing communications infrastructure, LeoLabs uses purpose-built phased-array radar systems distributed around the world. Each radar actively transmits radio signals into space and measures the reflected returns from objects in orbit, capturing precise data on range, velocity, and position. By combining observations from multiple radar sites over time, LeoLabs builds and maintains a continuously updated catalog of tens of thousands of objects.


This networked approach allows them to revisit objects frequently and refine orbital estimates, enabling customers, which range from commercial satellite operators to defense organizations, to better understand and respond to activity in space.


## The data infrastructure challenge


The comparatively easy part, at least from the perspective of data infrastructure, is tracking satellites and debris. LeoLabs can measure the position, velocity, etc., of an in-orbit object multiple times per day. This amounts to several million measurements per day across assets and several billion measurements to date. Because LeoLabs can more accurately project trajectories based on the quality of this data, the tracking results in hundreds of millions of conjunction data messages (CDMs) per month for their customers. A CDM is a report that’s generated when two objects in orbit are likely to have a close (if not head-on) approach. LeoLabs’ ability to deliver CDMs helps satellite operators perform thousands of maneuvers per month to avoid collisions.


If satellite and debris tracking is the “easy” part, the hard part is making sure that data can be trusted and is as high-fidelity as possible. LeoLabs’ collision avoidance product isn’t defined by the sheer volume of data it provides, but by the precision, reliability, and actionable insight that data drives. While the dataset of objects in orbit may only span gigabytes, it serves as a critical backbone for forecasting trajectories and determining when maneuvers will be required, ultimately enabling timely CDMs. On top of that, ongoing data collection powers an AI imaging system that helps analyze radio interferometry signals to identify and characterize debris clouds, further enhancing space situational awareness.


When forecasting orbits, tumbling, debris clouds, and covariance projections, data quality is paramount. But how do you ensure quality when the quantity of devices deployed around the globe is so high?


Each device that generates transmissions and readings needs to be monitored, connected to the network, and calibrated correctly to ensure all measurements are as accurate as possible. While an object in orbit may generate data multiple times a day, hardware on Earth communicates much more frequently. Information on radar transmission power, server metrics, physical hardware status, amplifiers, and environmental conditions is generated at massive volumes for monitoring and to be alerted on. At LeoLabs, this means 7,000 lines of data written every second, or 600 million lines per day. Needless to say, scale becomes a concern, and general-purpose data infrastructure simply can’t keep up.


## LeoLabs + InfluxDB


LeoLabs operates a global radar network with a lean engineering team. Because its radar systems are distributed across multiple geographies, continuous, automated visibility into hardware health is essential to maintaining reliable operations. That visibility allows engineers to monitor performance, detect issues quickly, and keep the network operating with confidence.


The scale of monitoring required is what drove LeoLabs to InfluxDB. Each radar site generates health telemetry every second to every minute. They pass transmission power, server metrics, amplifier status, and environmental conditions across thousands of devices. After evaluating alternatives and experimenting with the open source version of InfluxDB, the team landed on InfluxDB Cloud Dedicated as the infrastructure that could handle the job.


Only a purpose-built time series database could handle the immense volume of data being generated by the many thousands of devices they own and operate, and by using InfluxData’s data collection agent,[Telegraf](https://www.influxdata.com/time-series-platform/telegraf/?utm_source=website&utm_medium=direct&utm_campaign=inside-leo-labs-influxdb&utm_content=blog) , LeoLabs was able to easily move that data into a central InfluxDB instance. Its current stack is:


- **InfluxDB Cloud Dedicated** : Central storage engine for all high-fidelity telemetry
- **Telegraf** : Universal data agent used to collect and ingest data from radar infrastructure across the globe
- **NetBox + Ansible** : NetBox serves as the source of truth for device configuration, while Ansible automatically generates and deploys updated Telegraf configurations via GitHub Actions


To power their data processing and analytics pipelines, LeoLabs retains several years of full-resolution telemetry history. Engineering teams use that historical data to investigate the long-term health and performance of its radar systems, and analyze how radar behavior changes alongside seasonal atmospheric conditions. Recent efforts have centered on processing this data to strengthen predictive maintenance models and apply machine learning techniques to uncover recurring patterns.


The result is that internally, LeoLabs can function with a proactive, data-driven operational model. Engineers have continuous automated visibility into the health and performance of radar systems across the global network.


With approximately 1,000 alert rules, it’s also pivotal that InfluxDB performs as well as it does. Processing millions of lines of data and checking each one for an alarm requires the efficiency that a purpose-built time series database can bring to the table, and the result is that “fast firing” alerts can trigger within 30 seconds anytime something goes amiss.


The number of tracked objects is growing by the day, and the frequency of close approaches is increasing. This makes the margin for error slim for satellite operators, launch providers, and defense agencies relying on accurate orbital intelligence. LeoLabs is operating at the intersection of growing complexity and real-time data infrastructure, with a lean team and a database layer that gives them confidence the system will surface a problem before it becomes one.


That’s what purpose-built time series infrastructure looks like in the real world: fast ingestion with operational confidence at scale across distributed, global hardware.
