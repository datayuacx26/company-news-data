---
schema_version: "1.0.0"
document_id: "5c216ad094599eb9d3c790dc8e28a5e899e51dda5d490e49523693c033493fe4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-83888f119c8c"
canonical_url: "https://www.apollographql.com/blog/secure-your-graphql-microservices"
published_at: "2023-07-31T13:51:52+00:00"
first_seen_at: "2026-07-24T16:23:53.794779+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:0906d0e1107002a31fbebebc709f93821d3e54bc6de097d139f167cdf709866c"
---

# Secure your GraphQL Microservices

Federation unlocks superpowers for our queries, enabling us to split up business logic and improve performance with features like` @defer` . However, these same powers can be abused if placed in the wrong hands, so it’s essential to limit who has access to them.


## The threats


Many coordination features of a federated graph rely on an important assumption: **clients always query your router—never individual subgraphs** . If this assumption is violated and clients query your subgraphs directly, you might expose data and capabilities that you *shouldn’t* :


- The` @inaccessible` directive enables subgraphs to define schema fields that *other* subgraphs might need, but that the router *shouldn’t* expose to clients. If clients can query a subgraph directly, those queries *can* include` @inaccessible` fields. And even if you don’t have any sensitive` @inaccessible` fields in your graph today, you might in the future!
- The` @requires` directive enables a subgraph to define schema fields that depend on entity fields from *other* subgraphs. Subgraphs rely on the router to resolve this dependency, ensuring that trusted data is provided. But if clients have direct access to subgraph entity resolvers, that data can *no longer* be trusted.
- The` @override` directive enables a subgraph to take responsibility for a schema field away from *another* subgraph. With direct subgraph access, clients can still query the *original* subgraph field, which can result in inconsistent and unexpected behavior.


This is just a small selection of the available features today, and more will be added over time. The key takeaway is that federation is a microservice architecture that expects a *single* entry point through a controlled router. If subgraphs can be accessed directly, it’s only a matter of time before something unexpected happens, and that unexpected event could be a security breach.


## Protecting your subgraphs


Exposing subgraphs publicly opens the door to several attacks. To mitigate them, we prevent anything but our routers from accessing our subgraphs. First, we can protect our subgraphs at the application level by adding an extra layer of authorization on top of whatever user auth may be in place. We can configure our routers to send a header containing a shared secret to each of our subgraphs:


```text
headers:
subgraphs:
orders:
request:
- insert:
name: "Router-Authorization"
value: "${env.ORDERS_SUBGRAPH_SECRET}"
users:
request:
- insert:
name: "Router-Authorization"
value: "${env.USERS_SUBGRAPH_SECRET}"
```


Notice that we use a unique secret per subgraph, following a security best practice called “the principle of least privilege”. If there’s no reason for the` orders` subgraph to have direct access to the` users` subgraph, then we should not give it the ability to do so (by sending it valid tokens for another subgraph).


These values come from environment variables, so how you set them will depend on where you’re running your routers. Most secret managers have a way to inject values via an environment variable. For example, in GraphOS, you can[set secrets via the UI](https://www.apollographql.com/docs/graphos/routing/cloud-configuration#managing-secrets) .


How this looks on the subgraph side depends on your implementation. Here’s an example using FastAPI in Python:


```text
@app.middleware("http")
async def check_router_security(
request: Request, call_next: Callable[[Request], Awaitable[Response]]
) -> Response:
router_secret = environ.get("ROUTER_SECRET")
if router_secret is None:
return await call_next(request)
if request.headers.get("Router-Authorization") != router_secret:
return Response(status_code=HTTPStatus.UNAUTHORIZED)
return await call_next(request)
```


Here, we’re setting up the expected token as an environment variable called` ROUTER_SECRET` in our subgraph. This value would be injected via the secret manager for your hosting platform. If the secret is not set (such as in local development), we turn off this enforcement; you may want to add a more sophisticated method of ensuring this is never turned off in production. If the secret *is* set, we ensure every request has a matching` Router-Authorization` header. Requests without a valid header will get a bare response with a` 401 Unauthorized` code—we don’t send any additional information that clients don’t need, and unauthenticated requesters don’t even need to know that this is a GraphQL server.


We have examples for many languages and frameworks in our[subgraph templates](https://www.apollographql.com/docs/graphos/graphs/creating-a-subgraph#starting-from-a-template) , so check them out for more inspiration!


#### Extra credit: network protections


As an additional step, depending on your hosting environment, you may be able to use network-level protections to prevent any incoming connections to your subgraphs except your routers. This is typically done via firewall rules (such as access control lists). Not all platforms or architectures will have this option, but if you do, we recommend adding it as a “defense in depth”.


## Ruling out alternatives


There are a few other solutions to this problem that might *seem* viable but can cause issues. Let’s go over them!


First, you might be tempted to apply router authorization checks only for federation-specific subgraph fields (` _service` and` _entities` ), but there are problems with this approach:


- Newer versions of federation might add *more* subgraph fields in the future, which would introduce new vulnerabilities that special-case checks don’t handle.
- This approach doesn’t solve the problem of reaching` @inaccessible` or` @requries` fields via a top-level` Query` field (a standard, non-federated query).


Second, you might be tempted to enforce router authorization per field, maybe even automatically for any field that implements a federated directive. However, federation enables any single subgraph to influence the behavior of the entire supergraph. For example, if` @inaccessible` is applied to a field in *any* subgraph, it’s expected to be enforced on *every* subgraph. This means that per-field, per-subgraph enforcement is fundamentally inconsistent and can lead to unexpected data leakage.


Finally, you might consider disabling introspection as a solution—if attackers can’t discover the hidden fields, they can’t use them, right? This is called “security by obscurity” and is widely considered not security at all. A sufficiently determined attacker *will* eventually guess the names of fields that you don’t want them to find.


The only consistent approach to preventing undesired access to subgraph capabilities is to disable access to subgraphs *entirely* to any client besides the router.


## Get secure


Now that you know the dangers of exposing subgraphs publicly and the best approach to mitigating the threat, there’s nothing left to do but start implementing! Start securing your subgraphs immediately.


Have any questions, comments, or concerns about this post? I’d love to hear about it in our[Discord server](https://discord.com/invite/graphos) ! That’s also where you’ll be notified of upcoming security-related live streams.


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
