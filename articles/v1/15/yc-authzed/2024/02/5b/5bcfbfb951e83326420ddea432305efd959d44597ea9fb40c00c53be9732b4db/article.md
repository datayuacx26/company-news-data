---
schema_version: "1.0.0"
document_id: "5bcfbfb951e83326420ddea432305efd959d44597ea9fb40c00c53be9732b4db"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/materialize-early-access"
published_at: "2024-02-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:12708f239080451f58e4cd3e88ed1629524224f5dae66d97c5e83220f66010c3"
---

# Announcing AuthZed Materialize

Today, I'm proud to announce that our latest product,[AuthZed Materialize](https://authzed.com/products/authzed-materialize) , has entered Early Access for AuthZed Dedicated customers.[AuthZed Dedicated](https://authzed.com/products/spicedb-dedicated) is our premier private-cloud authorization service. Dedicated customers are provided access to a dashboard where they can provision SpiceDB deployments to workload-isolated, privately-networked hardware in their cloud provider of choice.


## What is AuthZed Materialize?


AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. These features are critical for folks scaling both the size and the complexity of their permission systems. We're especially excited to see our customers leveraging native filtering capabilities in systems such as PostgreSQL and ElasticSearch after configuring streaming. In the past few months, we've been running this product for a Fortune 500 company and are looking to expand into more customer use-cases to ensure a robust product at launch.


## AuthZed's mission to secure the Internet


Since its inception, AuthZed's mission has been to secure the internet by providing developers the foundation needed to build scalable authorization systems. We began this journey by building SpiceDB: an open source implementation of the most successful known example of authorization tooling in the industry,[Google's Zanzibar system](https://authzed.com/blog/what-is-google-zanzibar) . However, the industry at large is not Google so we've continued to refine Zanzibar's design while maintaining true to its original goals of consistency at a global scale. This led us to innovations such as our schema language, playground environment, dynamic consistency model, reverse-index APIs, and caveats,[our collaboration with Netflix](https://netflixtechblog.com/abac-on-spicedb-enabling-netflixs-complex-identity-types-c118f374fa89) , to integrate the best attributes of ABAC with SpiceDB.


## AuthZed sets the standard for systems inspired by Google Zanzibar


With the release of Materialize, we've set a new standard for what it means to be inspired by Google's Zanzibar. In the image below, you'll see an[architecture diagram](https://authzed.com/zanzibar/#figure-2) from the Zanzibar paper. Circled in that image is a component referred to as Leopard, which was important enough to warrant an entire[dedicated section of its own](https://authzed.com/zanzibar/#3.2.4-leopard-indexing-system) in the paper. Besides Google themselves, we are the only team to have created a functional system inspired by Leopard.


We're eager to share our latest innovation with the world once it reaches general availability.[Stay tuned](https://twitter.com/authzed) if you're eager to hear more. You can also dive into the[SpiceDB Discord](https://authzed.com/discord) if you're looking to learn more or get involved.


On this page


- What is AuthZed Materialize?
- AuthZed's mission to secure the Internet
- AuthZed sets the standard for systems inspired by Google Zanzibar


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
