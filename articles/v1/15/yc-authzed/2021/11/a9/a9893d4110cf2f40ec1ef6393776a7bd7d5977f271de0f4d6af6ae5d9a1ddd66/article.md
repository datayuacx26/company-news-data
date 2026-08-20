---
schema_version: "1.0.0"
document_id: "a9893d4110cf2f40ec1ef6393776a7bd7d5977f271de0f4d6af6ae5d9a1ddd66"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/consistent-hash-load-balancing-grpc"
published_at: "2021-11-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:51f21c21e0449b3bf89aa6e572d1a6a1f9943693078fe86fd9bee6ae0afa2356"
---

# Consistent Hash Load Balancing for gRPC

When you send a query to[SpiceDB](https://github.com/authzed/spicedb) , our distributed authorization database, it will often choose to dispatch to other nodes in the cluster that are more likely to have results for subqueries already cached. Dispatching and caching help keep the latencies for SpiceDB queries low.


## Keeping Cache Usage High


Not all nodes cache all data. Instead, each node is responsible for caching a subset of query results for the cluster. This makes it critical that requests get dispatched to nodes that will have the data cached (if such a node exists), especially on[Lookup](https://buf.build/authzed/api/docs/main/authzed.api.v1#authzed.api.v1.LookupResourcesRequest) requests which have lots of overlapping subproblems.


SpiceDB makes use of a[consistent hash ring](https://github.com/authzed/spicedb/blob/66003383bbc57be19ad474029695ae1793578d71/pkg/consistent/hashring.go) to map requests to nodes. There are[several](https://en.wikipedia.org/wiki/Consistent_hashing)[good](https://web.stanford.edu/class/cs168/l/l1.pdf)[resources](https://itnext.io/introducing-consistent-hashing-9a289769052e) online to learn about consistent hashing, but briefly:


- A unique identifier (based on the grpc subconnection) for each SpiceDB node is hashed and mapped to many locations in the hash ring. The number of copies (virtual nodes) is controlled by the` replicationFactor` .
- Queries are hashed and mapped to locations in the hash ring
- SpiceDB dispatches to the next` N` "clockwise" nodes in the ring. This` N` is often called the` spread` .


When a SpiceDB node receives a dispatch request, it will first check its cache for the answer. If the result isn't already cached, it will find the answer by querying the datastore or by decomposing the query into smaller subproblems and dispatching again. The consistent hashring means SpiceDB can get good cache utilization without an external service coordinating where requests should be sent.


The *consistent* property of the consistent hash ring ensures that if a node is added or removed, the smallest possible number of keys gets redistributed to other nodes.


## Discovering SpiceDB nodes


Each SpiceDB node builds its own consistent hash ring so that no consensus with other nodes is required.


In the past, SpiceDB used a[bespoke discovery service](https://github.com/authzed/servok) to discover what other nodes were available for dispatch. Recently, we switched to[kuberesolver](https://github.com/sercand/kuberesolver) , a[resolver plug-in](https://pkg.go.dev/google.golang.org/grpc@v1.42.0/resolver#Register) for` grpc-go` that watches Kubernetes[endpoints](https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/) to find available SpiceDB nodes. It's a simple matter to enable kuberesolver in a project:


go


1


2


3


4


5


6


```text
import    "github.com/sercand/kuberesolver/v3"


func    main  ()    {
kuberesolver.RegisterInCluster()
// ...
}


```


go


1


2


3


4


5


6


```text
import    "github.com/sercand/kuberesolver/v3"


func    main  ()    {
kuberesolver.RegisterInCluster()
// ...
}


```


Once registered, resolver plugins are referenced by name in the scheme portion of the gRPC address. If you're running SpiceDB on Kubernetes, you can use the Kubernetes resolver via the dispatch flags:


sh


1


```text
--dispatch-upstream-addr=kubernetes:///spicedb.default:50053


```


sh


1


```text
--dispatch-upstream-addr=kubernetes:///spicedb.default:50053


```


which will find all instances behind the` spicedb` service in the` default` namespace.


## Extending gRPC With a Consistent Hash Load Balancer


With kuberesolver, the grpc-go client can find all instances of SpiceDB for dispatch and transparently handle creating and destroying subconnections as needed.


But now, we need to tell the client which nodes to use for any given request. Luckily, grpc-go has a way for us to[register](https://pkg.go.dev/google.golang.org/grpc@v1.42.0/balancer#Register) our[own implementation of a load balancer](https://github.com/authzed/consistent) that is backed by a consistent hash ring.


Like kuberesolver, it's straightforward to start using the consistent hash load balancer:


go


1


2


3


4


5


6


7


8


9


10


11


12


```text
import   (
"github.com/cespare/xxhash"
"google.golang.org/grpc/balancer"


consistentbalancer  "github.com/authzed/spicedb/pkg/balancer"
)


func    main  ()    {
balancer.Register(consistentbalancer.NewConsistentHashringBuilder(xxhash.Sum64, hashringReplicationFactor, backendsPerKey))


// ..
}


```


go


1


2


3


4


5


6


7


8


9


10


11


12


```text
import   (
"github.com/cespare/xxhash"
"google.golang.org/grpc/balancer"


consistentbalancer  "github.com/authzed/spicedb/pkg/balancer"
)


func    main  ()    {


// ..
}


```


The consistent hashring load balancer allows you to configure the hash used, the replication factor for the hashring, and the spread of keys across the nodes. Once the balancer is registered, it can be referenced in an option to the gRPC dialer:


go


1


2


3


```text
conn, err := grpc.Dial( "kubernetes:///spicedb.default:50053"  ,
grpc.WithDefaultServiceConfig( `{"loadBalancingPolicy":"consistent-hashring"}`  )
)


```


go


1


2


3


```text
conn, err := grpc.Dial( "kubernetes:///spicedb.default:50053"  ,
)


```


Any request sent via the gRPC connection can then[set a value in the context](https://github.com/authzed/spicedb/blob/b0370b6cc89d2528653249bdf2220e58da6367df/internal/dispatch/remote/cluster.go#L34) which will be hashed into the ring for node selection.


It's worth noting that there is an existing[internal ringhash](https://github.com/grpc/grpc-go/tree/d542bfcee46d733f7bf8a5e870994379863da2d2/xds/internal/balancer/ringhash) load balancer implementation in grpc-go, but it can't be used without using[xds](https://github.com/grpc/grpc-go/blob/master/examples/features/xds/README.md) .


Hopefully, we've shed a little light on the lightly-documented extension points of grpc-go. If this load balancer sounds like something you'd like to see published as a standalone library, why not let us know on[Discord](https://authzed.com/discord) ?


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Keeping Cache Usage High
- Discovering SpiceDB nodes
- Extending gRPC With a Consistent Hash Load Balancer
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
