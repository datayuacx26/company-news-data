---
schema_version: "1.0.0"
document_id: "1c3ef558f5b2aa17af5be8aa8c77763294c4b5b70c5cc24fd5b81a63fa04dd6d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authzed-brings-additional-observability-to-authorization-via-the-datadog-integration"
published_at: "2025-06-24T08:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:b213ee28f56d2e64105babc09d73f423a610d93a75d32f07c605f98a03dd26ef"
---

# AuthZed Brings Additional Observability to Authorization via the Datadog Integration

Today, AuthZed is providing additional observability capabilities to AuthZed's cloud products with the introduction of our official Datadog Integration. All critical infrastructure should be observable and authorization is no exception. Our integration with Datadog gives engineering teams instant insight into authorization performance, latency, and anomalies—without adding custom tooling or overhead.


With this new integration, customers can now centralize that observability data with the rest of their data in Datadog—giving them the ability to correlate events across their entire platform. AuthZed's cloud products continue to include a web console with out-of-the-box dashboards containing metrics across the various infrastructure components that power a permissions system. At the same time, users of the Datadog integration will also have a mirror of these dashboards available in Datadog if they do not wish to create their own.


"Being able to visualize how AuthZed performs alongside our other systems gives us real peace of mind," said Eric Zaporzan, Director of Infrastructure, at Neo Financial. "Since we already use Datadog, it was simple to send AuthZed metrics there and gain a unified view of our entire stack."


AuthZed metrics allow developers and SREs to monitor their deployments, including request latency, cache metrics (such as size and hit/miss rates), and datastore connection and query performance. These metrics help diagnose performance issues and fine-tune the performance of their SpiceDB clusters.


### Get Started Using Datadog with AuthZed in 7 Steps


The Datadog integration is available in the AuthZed Dashboard under the “Settings” tab on a Permission System.


1. Go to the dashboard homepage.
2. Select a Permission System for which to submit metrics.
3. Click on the Settings tab.
4. Scroll down to the Datadog Metrics block of the settings UI.
5. Enter your Datadog account API key.
6. Enter your Datadog site if different from the default.
7. Click Save.


To ensure that the dashboard graph for latency correctly shows the p50, p95, and p99 latencies, you’ll also need to set the Percentiles setting for the authzed.grpc.server_handling metric in the Metrics Summary view to ON.


TADA 🎉 You should see metrics start to flow to Datadog shortly thereafter.


I want to thank all of the AuthZed engineers involved in shipping this feature, but especially Tanner Stirrat who shepherded this project from inception and I can't wait to see all the custom dashboards our customers make in the future!


Interested in learning more? Join our Office Hours on July 3rd here on YouTube.


On this page


- Get Started Using Datadog with AuthZed in 7 Steps


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
