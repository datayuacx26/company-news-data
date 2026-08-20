---
schema_version: "1.0.0"
document_id: "0cf0ad19572fa148289b832059878fdde38149f35440bd9be9eff1c66d224b27"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/introducing-authzed"
published_at: "2021-02-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:bf19d39dd95d5412317ca29c2b7adf22c864d170d18b68aec3ac55f17bd153f5"
---

# Introducing Authzed

## I Didn’t Know I Couldn’t Do That


“IAM is great!” said nobody ever. With an increasingly complex landscape of SaaS, cloud infrastructure, open source, and enterprise permissions systems, the burden of both building an application, and authorizing access to it has become a tedious chore that is often overwhelming to developers and administrators.


The last big product that my co-founders and I created from scratch was[Quay](https://www.projectquay.io/) , the first private Docker registry. Quay now has GitHub-like organizations and teams, LDAP group sync, and Single Sign On. It didn’t start that way though: when we first built and launched Quay, we had users and repositories. The backend for Quay is built in Python using[Flask](https://flask.palletsprojects.com/en/1.1.x/) . When we needed some support for permissions, it was only natural that we adopt[Flask-Principal](https://pythonhosted.org/Flask-Principal/) , one of the core components of the opinionated[Flask-Security](https://pythonhosted.org/Flask-Security/) project. We stored the relationships between users and projects in the database, and all was well. Until of course it wasn’t.


Our first feature request was for organizations, and team support was not far behind. We found out pretty quickly that Flask-Principal’s model didn’t extend very nicely to arbitrary nesting and relationships, but we were always able to make it work just enough that we didn’t move off. Eventually we started hitting scaling bottlenecks and an inability to flatten our ideal hierarchy into the model. We realized that this permissions model had taken us as far as it could when we actually canceled a nested namespace feature because we couldn’t do recursive queries against our authorization tables in our database.


Quay’s permissions system remains largely unchanged to this day, but so does the feature set it supports and the cloud costs to store and compute permissions only continue to grow.


While at CoreOS, we ran into permissions shortcomings in other areas as well. CoreOS’s[Tectonic](https://coreos.com/tectonic/) was intended to grow into a loosely coupled infrastructure as a service (IaaS) provider, where you brought in your own services through prepackaged open-source services, powered by open source[operators](https://coreos.com/blog/introducing-operators.html) . Unlike AWS, which repackages and rebrands all of the most popular open source services, we wouldn’t have strict control over the list of services available on our cloud platform. This meant that we had to allow some level of customer freedom to define their own services and service permissions for things made available on the service. One of the ways Amazon is able to scale AWS’s IAM is to strictly control the naming, number, and relationships between the services to which they federate access.


From talking to a wide variety of companies, and drawing on our own experience, here is what we have learned about the current state of permissions: most companies evaluate permissions using some mix of source code interpretation of relationships stored in a database. As an example, they may store that person X is an admin of resource Y, and before deciding whether X can view Y there will be a series of checks over all of the relationships that will grant that capability.


Then there are a large number of companies doing something even less sophisticated: we see a lot of pre-shared key basic auth, ownership inference based on object IDs, and single tenancy with no permissions at all.


The smallest group are those that use a dedicated product, such as a policy engine, to enforce authorization uniformly. Even these companies, however, often haven’t thought through scale or consistency considerations with their chosen solutions. For example, systems where policy is evaluated near or in the application frequently have weak policy distribution stories, and can still rely on a database to store relationships. This introduces unpredictable policy decisions when a new policy hasn’t updated everywhere yet, and a single point of failure via the database.


We started to ask ourselves: what would an ideal permissions system even look like?


## Requirements of Despair


When we sat down to think about our requirements for a new permissions system, we decided that there were four main properties that we wanted:


1. Correct decisions
2. Uniform way to model and check permissions from our polyglot services and applications
3. Reliable: this will be a hard dependency for all applications and services
4. A solution that will grow with us
5. Fast enough to use whenever we need to check a permission


Oops, five main properties. For each requirement we can come up with a set of design restrictions and implications. Correctness requires that there be a strong consistency model. Uniform calling pattern from multiple languages implies either a network service, or a core library written in a system language that can be called from all languages (e.g. C, C++, Rust). Reliable means that each individual component must either be replicated or have a reliability rating higher than the overall target reliability of the system. Scalable usually implies some kind of distributed system (beyond a point). And finally, when we sum together all of the implications above, fast usually implies lots of denormalization such as caching.


Whew! No wonder this isn’t something that we can just whip together in a week! If such a service existed it would be incredibly valuable. Valuable enough to build a company around.


## A New Hope


When you can’t find a service to meet your needs, what else is there to do except build it![Authzed](https://authzed.com/) is the world’s first multi-tenant permissions system as a service. With Authzed, companies now have a partner to help with the messy business of storing, computing, updating, and scaling their permissions.


Authzed allows individual companies to model their own permissions system exactly as their app requires. This includes often tricky permissions models such as: recursively defined nested relationships, user defined roles, role-to-role relationships, and checks for subjects that don’t correspond to end users (e.g. services, tokens).


Permissions aren’t modeled in a vacuum: when the business requirements change, the permissions model must change as well. Authzed also allows for updating, testing, and deploying new permissions policy to existing relationships without changes to code.


Our globally distributed hosted service will also scale and grow with you. Without any single points of failure or vertically scaled components, Authzed is highly reliable and scalable. Inspired by the[Zanzibar](https://research.google/pubs/pub48190/) paper from Google, you can be confident that the underlying data and consistency model will scale as far as you need it to.


## Battle Tested


These aren’t just theoretical claims though. We’ve already built two services on top of the model!


First, and less meta, we built (and subsequently decommissioned) ShareWith, a product to allow you to share things that lack their own permissions using a Google-docs inspired sharing model. It supports point-to-point sharing, groups, nested groups, and different permissions levels for each permission granted. To do this we came up with the following diagram that shows a little bit about how the model works:


Second, we also use our service to provide the tenancy framework for itself! Underlying Authzed we have a version of Authzed without any concept of tenancy. We then load in tenant configuration (no different from the kind of tenant configuration that you may write) that describes a simple tenancy model for namespaces, tenants, users, other services (clients), and tokens to represent them. Then, before Authzed processes any requests, it first checks with itself that such a request is allowed based on the bearer token provided and the tenant upon which the request will be performed! The configuration diagram for this model is embedded below:


From implementing permissions systems on top of Authzed twice now, we’ve developed a healthy amount of understanding for how to represent things in our model, and how to run the service in production. We can even help you think through your own permissions model as part of our onboarding process.


## What Comes Next


We’re not done yet though! Currently we have an extensive backlog of improvements that will bring better performance and usability to everyone. We’re currently looking for our first set of design partners and customers to help guide the decisions about the order additional things get built and deployed. If you’ve got a pressing authorization challenge, or are just interested in helping guide the product’s maturity, we would love to talk to you! Please sign up here to speak with one of us directly about how Authzed can fit into your product’s future.


On this page


- I Didn’t Know I Couldn’t Do That
- Requirements of Despair
- A New Hope
- Battle Tested
- What Comes Next


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Product Composable Schemas is GA Composable schemas are now generally available in SpiceDB 1.51.1. Split monolithic schemas into well-defined, reusable components so teams can move faster, make changes, and scale without everything becoming entangled. Apr 15, 2026 · 3 min](https://authzed.com/blog/composable-schemas-ga-and-zed-v1)[Product Composable Schemas is GA Composable schemas are now generally available in SpiceDB 1.51.1. Split monolithic schemas into well-defined, reusable components so teams can move faster, make changes, and scale without everything becoming entangled. Maria Inés Parnisari · Apr 15, 2026 · 3 min](https://authzed.com/blog/composable-schemas-ga-and-zed-v1)
