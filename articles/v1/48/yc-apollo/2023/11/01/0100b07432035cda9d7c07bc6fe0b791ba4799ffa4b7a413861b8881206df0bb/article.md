---
schema_version: "1.0.0"
document_id: "0100b07432035cda9d7c07bc6fe0b791ba4799ffa4b7a413861b8881206df0bb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/deploying-the-apollo-router-at-apollo"
published_at: "2023-11-06T12:32:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:60a16c1095c4882ca6d1172d5c4ab3096904ec0c9473da8f12cd0338d71f720b"
---

# Deploying the Apollo Router at Apollo

Apollo doesn’t just build Federation – we use it internally as well. Like many of our customers, we recently upgraded a fleet of Apollo Gateway instances to Apollo Router. This fleet ran the superschema for our “Apollo Studio” graph in multiple variants: development, staging, and production.


Our team recently upgraded to the Apollo Router with very positive results which we’d like to share alongside some helpful tips.


## **Replacing a Gateway with the Apollo Router**


The Apollo Router is Apollo’s replacement for the Gateway @apollo/gateway, and provides higher performance with lower resource consumption. It was an easy choice to replace the existing gateway fleets with new router fleets to serve the various Apollo Studio graph variants.


Migrating from gateway to router requires a bit of a mindset shift. The gateway is essentially a JavaScript toolkit intended to be highly customizable with all users providing their own customizations for their own particular execution environment. The Apollo Router is a product. It leverages the experience and knowledge of Apollo to provide a performant, scalable GraphQL Federation experience while still offering customization to users.


Apollo had deployed a highly customized gateway and migrating to the router was largely a task of mapping custom gateway functionality to standard router functionality. Most of this was driven by router configuration (header forwarding). We also wrote a Rhai script for simple header and cookie manipulation and a coprocessor that provided custom logging in a different language (TypeScript).


Once we had figured out how to migrate our custom gateway functionality, it was then simple to adopt the standard router container image and deploy this via a custom Helm chart which leveraged the standard router Helm Chart to do the heavy lifting. You can read more details about this in the Kubernetes section below.


Because the router provides a standard, configuration-driven execution environment, it is simpler to utilize standard tools such as Helm to manage deployments. This drive to standardization delivers benefits to Apollo in terms of product feature development speed and for customers, since it is much simpler to deploy a standard router than it is to develop and deploy your own custom gateway.


## **Performance Comparison**


When Apollo migrated to the Apollo Router, we were looking for improvements in resilience, scalability, responsiveness, and resource consumption. Let’s take a look at some of the details of our migration.


### **Resources**


Our router CPU consumption is approximately 60% that of the gateway. Router memory consumption is approximately 13% of the gateway. That’s a substantial operational cost-saving reduction.


#### **Gateway**


Our existing gateway configuration is fairly consistent across our three environments. Since there is no standard Helm Chart for deploying the gateway, we have a custom deployment mechanism and this doesn’t take advantage of features such as affinity, pod disruption budgets, or autoscaling. There is minimal variation in CPU and memory requests with a varying number of fixed deployments of each according to environment.


**Environment** **Instances** **CPU (Request, Limit)** **Memory (Request, Limit)**


Development 1 0.25, 1 512Mi, 512Mi


Staging 30 0.25, 1 2Gi, 2Gi


Production 40 0.5, 1 2Gi, 2Gi


**Totals** **71** **27.75, 71** **140.5Gi, 140.5Gi**


The “Totals” figures are approximate and based on the observation that resource consumption measurements indicate that for most of the time requested resource is sufficient.


There are no affinity, pod disruption budgets or autoscaling configuration.


#### **Router**


The Apollo Router comes with a maintained Helm chart which makes it simple to set features such as pod disruption budgets, affinities, autoscaling, and many other enterprise Kubernetes features. We take advantage of the router Helm chart to vary the resource requirements across environments with more flexibility than we have with our gateway.


Development 2-12 0.7, 1.5 1.5Gi, 1.5Gi


Staging 2-12 2, 3.5 1.5Gi, 2Gi


Production 3-12 4, 6 4Gi, 5Gi


**Totals** **7-36** **17.4, 28** **18Gi, 22Gi**


I’ve aggregated the requests and limits for the main containers within a router pod: router and the coprocessor. The resources are approximately 50/50 for each container.


Because we are using horizontal pod autoscaling (HPA) with the router, it’s harder to calculate the “Totals” figure. However, we note that autoscaling is never triggered in development or staging. In production it is triggered, but fairly infrequently, so that the net result is approximately the same as running 3 routers constantly.


The router is configured to have affinity with our Istio mesh ingresses, anti-affinity with other router pods (to improve node throughput and reliability) and we have a pod disruption budget set to a maximum unavailability of 1 (to improve reliability). We have configured horizontal pod autoscaling from a minimum of 2 (3 in production) and a maximum of 12, with triggering set for 75% CPU consumption.


### **Responsiveness**


The primary measure of responsiveness is request duration, as measured from arriving at the router from a client to sending the response to the client.


**Federator** **Average Response Time (ms)**


Gateway 510


Router 165


We couldn’t directly compare gateway and router responsiveness over the exact same time interval since we were not mirroring traffic but directing traffic to one or the other. For purposes of this comparison, we collected the data on multiple, separate days across the same time period (8 hours) in each day and averaged the results.


## **Sizing and Tuning**


Our production workload varies over the course of 24 hours, with a peak during U.S. working hours and quieter times during the rest of the day. In a typical week, we are processing about 70M requests.


In this scenario, we’re measuring Client Request Rate per Second (RRS). We have sized our router fleet based on the following data:


Average RRS: 115 (70M / 604800) Peak RRS: 815 Baseline Subgraph Latency: 150ms Average Client Request Size: 1k Average Client Response Size: 1m Number of Instances: 3 – 12


## **Kubernetes**


We use Helm to manage our Kubernetes router fleet. We have an Apollo in-house Helm chart which provides Apollo-specific configuration details such as:


- Rhai Scripts
- Istio configuration


The in-house Helm chart makes use of the Router Helm Chart to provide most of the heavy lifting for the configuration of:


- Router (header propagation, max request sizes, introspection, etc…)
- Standard Kubernetes Functionality (Resources, Affinity, Pod Disruption Budgets, Sidecars, etc…)


We separate our Helm configuration files (values.yaml) into a common file and an environment-specific file. We make use of the fashion in which Helm progressively applies values, by ordering the files for application with common values first and environment-specific values following later in the command.


## **Observability**


The router ships with a lot of observability for tracing, metrics, and logging. Most of this is easily configurable or can be enhanced with Rhai scripts or external coprocessors.


We use[DataDog](https://www.datadoghq.com/) to visualize the captured metrics.


## **Getting started with the Apollo Router**


Apollo has many resources to help you get up and running with the Apollo Router.


- Watch this Apollo tech talk to[learn more about the Apollo Router’s extensibility model](https://www.apollographql.com/events/tech-talk/extending-apollo-router-3/) .
- [This Champions Corner with Wayfair](https://www.apollographql.com/events/champions-corner/how-wayfair-slashed-costs-simplified-infra-and-cut-latency-in-half-with-apollo-router/) discusses how they scaled their router during Black Friday.
- [The updated docs](https://www.apollographql.com/docs/router/) now include more how-tos and reference guides for[deploying the Apollo Router in Kubernetes](https://www.apollographql.com/docs/router/containerization/kubernetes/) .


You can also connect with us as well as other users in our[Apollo Champions Slack community](https://events097286.typeform.com/joincommunity) . You can also[find us on Discord](https://discord.com/invite/VZNZV2ujaY) if you prefer that channel. Let us know if you have any questions!


Written by


Gary Pennington


[Read more by Gary Pennington](https://www.apollographql.com/blog/author/garypennington)
