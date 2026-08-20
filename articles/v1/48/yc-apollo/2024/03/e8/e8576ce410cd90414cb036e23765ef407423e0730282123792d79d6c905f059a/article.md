---
schema_version: "1.0.0"
document_id: "e8576ce410cd90414cb036e23765ef407423e0730282123792d79d6c905f059a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/remove-application-logic-from-kubernetes-config"
published_at: "2024-03-05T09:09:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:13.235024+00:00"
content_hash: "sha256:6aee013ea5051f92c129d3597ce3e55f1862fba928b24b82bf78865cb80082c3"
---

# Remove Application Logic from Kubernetes Config

The traditional approach to microservices in Kubernetes requires duplicating application logic into YAML config files. This leads to information fragmentation, inconsistencies, and ultimately bugs. With GraphQL and Apollo Federation, you can reduce the amount of required config, prevent duplication, and have a better experience maintaining your API. In particular, Apollo Federation can:


1. Prevent conflicting API routes, and give better visibility into the entire API.
2. Ensure that routing rules always match the application code.
3. Make microservice dependencies declarative, and avoid custom networking rules between services.


## Catch conflicting routes


The new Kubernetes Gateway API provides a lot of ergonomic improvements over the older Ingress, and puts more power in the hands of application developers to define their routes. While this is likely a good choice for a REST API, it can also lead to unexpected behavior.


Consider a backend team at a retail company who are implementing a new feature: allowing users to subscribe to a product they purchase on a regular basis. They might introduce an` HTTPRoute` rule that looks like this:


```text
rules:
- matches:
- path:
type: RegularExpression
value: /user/[^/]+/subscription
```


Unfortunately, they don’t know that the accounts team has already registered the *same* route to fetch a user’s membership (formerly called “subscription”)! When they roll out these changes, one of a few things could happen:


- The rule isn’t applied because the older membership route takes precedence. This is likely the preferred outcome: it doesn’t break *existing* features (and it’s what[the docs](https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io/v1.HTTPRouteRule) suggest should happen), but it can lead to head-scratching on the developers’ part. Ultimately, they’ll call in the platform team to figure out what’s going on.
- Or, the rule *is* applied because, as is the case with the popular gateway I was testing, the alphabetical order of namespaces is used to resolve conflicts *without* considering the age of the config. This is risky business: the new feature appears to be working fine, and could pass QA despite having introduced a production issue. From here, it’s only a matter of time before the error reveals itself and someone gets paged to diagnose it.


Neither outcome is great. Wouldn’t it be better if the products team were informed of the problem before ever deploying anything? With Apollo Federation, that’s exactly what happens.


First off, the developers can delete their entire` HTTPRoute` ; with Federation, the same code which defines the GraphQL API of a microservice determines how routing can work. Whether locally or in CI for their pull request, the product team encounters an error when they try to build (aka “compose”) their new` subscription` field:


```text
INVALID_FIELD_SHARING: Non-shareable field "User.subscription" is resolved
from multiple subgraphs: it is resolved from subgraphs
"products" and "users" and defined as non-shareable in all of them
```


Not only does CI fail on the conflict and prevent them from deploying something that could potentially break production, but it also tells them which other service they’re conflicting with. These early show-stopping errors carve a much faster path to diagnosis: informing the developer what happened, and where they can go to fix it.


## Preventing incorrect path rules


A lesson we learn time and time again is that there should be a single source of truth for data, and configuration is no different. With a traditional REST microservices setup, there are at least two sources of truth for routing.


First, the developer needs to define the route in their web framework of choice. For example, they might have this FastAPI route in a Python service:


```text
@app.get("/items/{item_id}")
async def read_item(item_id: int) -> Item:
return Item.load(item_id)
```


Second, they need to redefine the route in Kubernetes config. They’ll probably end up with something closely resembling the application code:


```text
rules:
- matches:
- path:
type: RegularExpression
value: /items/\d+/
```


This works fine—that is, until the application updates to use opaque string IDs (like UUIDs). If the developer forgets to check on the Kubernetes config, the route will stop working. You could leave it to your QA team or some careful code review to catch this, or you could remove the possibility of this error entirely.


With GraphQL and Apollo Federation, the routing information all flows from a single source in the code, so it can never be out of sync! When a developer updates their microservice, the standard CI/CD process propagates that change automatically to the router, with no manual checks required. Better yet, if that change would break downstream clients, tools like[Apollo GraphOS](https://bit.ly/3TcpBaP) catch and stop the release in its tracks before it even rolls out to the QA environment. That means less back-and-forth, and more shipping!


## Clean up inter-service networking rules


Most of the time, the only pods that should communicate with a microservice are the API Gateway’s pods. Security best practices tell us to lock down networking to only those interactions that are required. As a result, we’ll usually start each new microservice with a` NetworkPolicy` that looks something like this:


```text
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: microservice-network-policy
spec:
podSelector: {}
policyTypes:
- Ingress
ingress:
- from:
- namespaceSelector:
matchLabels:
name: infra
podSelector:
matchLabels:
app: gateway
```


Here we’re denying all ingress traffic for all pods unless it comes from the API Gateway.


Sometimes, though, one microservice needs data from another. Achieving this typically involves a request to the platform, infrastructure, and/or security teams to add a new rule for communication between those services. Worse, that rule will probably have to be broad enough in scope that *any* traffic is now allowed between those services, *loosening security* more than required. This direct communication also *tightens the coupling* between the microservices, making them harder to change and more likely to break.


With Apollo Federation, services can set up their dependencies declaratively and let the router take care of the rest. In this example, the` totalCost` field requires a *different* service’s` products` field (specifically, the` price` and` shipping` subfields).


```text
type Order @key(fields: "id") {
id: ID!
products: [Product] @external
totalCost: Int @requires(fields: "products { price shipping }")
}
```


Not only does this microservice not need to directly communicate with another microservice, it doesn’t care *which* service provides the data it needs. This decouples the services, freeing up teams to evolve their projects independently. This process also explicitly documents dependencies, so CI can prevent changes that would cause issues for other services.


Only the data that is necessary for particular request will flow between microservices, and *everything* comes through the router. This means there’s a central place to monitor and manage traffic, and networking rules can be simplified and standardized. Because adding a new dependency is a simple code change, developers can submit fewer tickets to the platform and security teams and greatly reduce the risk of shipping broken code.


## Simplify your Kubernetes config today


Using GraphQL with Apollo Federation can save time for your platform, infrastructure, and backend development teams while improving the reliability and maintainability of your microservices. It does this by removing application logic from your Kubernetes config, enabling each team to focus on what matters. If you’re interested in learning more about Apollo Federation,[join our Discord server](https://discord.gg/amVehrwMpJ) and chat with us in the` #apollo-federation` channel. If you have any ideas for improving a microservice platform that we should check out next, we’d love to hear about that too!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
