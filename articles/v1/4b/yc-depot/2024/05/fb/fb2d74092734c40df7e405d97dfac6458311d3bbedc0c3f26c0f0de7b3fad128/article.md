---
schema_version: "1.0.0"
document_id: "fb2d74092734c40df7e405d97dfac6458311d3bbedc0c3f26c0f0de7b3fad128"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-envoy-discovery-service"
published_at: "2024-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:bab217d495e65f1382f60586a35dfc53931e654fc27ead4f29b6ed82cb0e45de"
---

# Building a custom Envoy discovery service for Depot

A common question about Depot is how we securely connect you directly to your BuildKit instance when running` depot build` .


When running inside AWS, we host your BuildKit instance behind a public IPv4 IP address and connect you directly to the instance via an on-demand mTLS certificate we issue per build.


However, when outside of AWS and inside of something like Fly.io, we need to do the same kind of routing but without the ability to have a public IP address.


To do this, we need to route the BuildKit TCP traffic based on the TLS Server Name Indication (SNI) and forward it to the correct host. Each host has its own unique mutual TLS (mTLS) configuration for a given build.


In this post, we will discuss how we built a custom Envoy discovery service for Depot to route traffic based on SNI to the correct BuildKit instances running on Fly.io.


## Background


Each Docker build project in Depot is a BuildKit instance for each architecture you're building for. When you run` depot build` , we create a new BuildKit instance on-demand and connect you to it via a secure mTLS connection. If you run` depot build --platform linux/amd64,linux/arm64` , we create two BuildKit instances, one for each platform.


Our[provisioning system](https://depot.dev/blog/infrastructure-provisioner-v3) constantly launches and destroys machines in response to your build commands.


As mentioned above, inside AWS, we connect you to the public IPv4 address of the builder instance. We issue a unique mTLS certificate for each build and connect you to the instance via that certificate. It's rather straightforward, as we just route the build to the builder's public IP.


But recently, we began testing using Fly.io and their machine API as an additional cloud provider for BuildKit builders. Those familiar with Fly will recognize that connecting to a public IPv4 address in that scenario is not as simple as it seems. There isn't actually a unique public IPv4 address for each Fly machine, so we had to get creative.


We decided to have our` depot build` client use a single known public IPv4 address and have a proxy route traffic to the correct ephemeral builder. To do this, though, we needed to be able to dynamically update the proxy's route configuration without having to restart the proxy or intervene manually, as we are constantly launching and destroying machines.


To do that, we plunged headfirst into running our own Envoy proxy with a custom discovery service that would route traffic based on SNI to the correct builder instance.


## Getting the proxy working with Envoy & SNI


To get started, we needed a way to dynamically discover new builders and update the Envoy proxy configuration to route traffic to the correct builder based on SNI.


To handle the initial discovery, we knew we could use Fly.io's internal DNS service to discover newly provisioned builders and use their special` TXT` records to find their machine details, such as region and internal IPv6 address.


This allows each of our builders to be issued a unique and ephemeral mutual TLS configuration with a unique SNI.


### What is SNI?


TLS Server Name Indication (SNI) is a long-established extension to the Transport Layer Security (TLS) protocol. The plaintext hostname allows clients to indicate the hostname they are attempting to connect to during the TLS handshake.


SNI has been used to allow multiple domains to be served over a single IP address by allowing a proxy to read the SNI and thus route traffic without needing to decrypt the traffic.


The Envoy proxy has built-in SNI filtering during the TLS handshake to differentiate traffic streams for our different builders.


### Why Envoy?


[Envoy](https://github.com/envoyproxy/envoy) is an open-source proxy used most famously in Istio and, thus, Kubernetes. It has dynamic configuration capabilities and extensibility through its APIs.


Typically, Envoy's configuration is given via a file, but you can also create a server that Envoy will use to fetch configuration updates. We created the server to programmatically react to builder instances being created and destroyed.


This configuration server is referred to as a "discovery service" in Envoy parlance. These discovery services provide a bi-directional streaming gRPC connection to report newly discovered network services.


Here are just some of the many kinds of possible services and their responsibilities:


- **CDS** : Cluster Discovery Service provides configuration for clusters that represent services that Envoy communicates with. For Depot, a single builder is a single cluster.
- **EDS** : Endpoint Discovery Service provides each cluster's list of endpoints (e.g., IP address and ports).
- **LDS** : Listener Discovery Service defines how Envoy should handle incoming connections. We use` tcp_listener` to filter via SNI and send traffic to the correct cluster.
- **ADS** : Aggregated Discovery Service multiplexes several xDS services into a single stream to simplify implementation.


## Depot Envoy discovery service


We were able to essentially build our own custom discovery service by following the[Envoy go control plane example code](https://github.com/envoyproxy/go-control-plane/tree/43e37d5101a366f972e52e95f1b7a4ee85e4b006/internal/example) .


However, we had to make two main changes. First, we need to quickly find new builders that can be connected to and remove old builders that are no longer running builds.


To accomplish that, we start a long-running go routine that queries and parses DNS every 100ms to find new builders and remove old ones. This is the discovery of BuildKit builders. We query Fly's` instances.internal` and generate an Envoy configuration snapshot from the response.


```text
go   func  () {
ticker:   =   time.  NewTicker  (  100   *   time.Millisecond)
for   range   ticker.C {
machines, err:   =   fly.  LookupMachineIDs  (ctx)
if   err   !=   nil   {
log.  Errorf  (  "failed to lookup machine IDs: %v"  , err)
ticker.  Reset  (  1   *   time.Second)
continue
}
l.  Infof  (  "setting snapshot with %d machine IDs"  ,   len  (machines))
snapshot, err:   =   GenerateSnapshot  (machines)
if   err   !=   nil   {…}
if   err:   =   cache.  SetSnapshot  (ctx, nodeID, snapshot);
err   !=   nil   {
log.  Errorf  (  "snapshot error %q for %+v"  , err, snapshot)
}
ticker.  Reset  (  100   *   time.Millisecond)
}
}
```


Once we have the snapshot, we generate a configuration that is slightly different than the` go-control-plane` example, as we need to filter traffic at Layer 3/4 rather than Layer 7.


To handle that, we change the cluster discovery type to static, as it means that the socket address is an IP that does not need to be resolved:


```text
ClusterDiscoveryType:   &  cluster  .  Cluster_Type  {Type: cluster.Cluster_STATIC},
```


With that in place, we need to set up the listener to filter based on SNI. We do this by configuring the listeners` FilterChains` to match the SNI and then route to the correct cluster by its name:


```text
func   makeFilterChain  (  clusterName   string  ,   serverNames  []   string  )   *   listener.FilterChain {
tcpProxy, _:   =   anypb.  New  (  &  tcp.TcpProxy {
StatPrefix: clusterName,
ClusterSpecifier:   &   tcp.TcpProxy_Cluster {
Cluster: clusterName
},
})


return   &  listener.FilterChain {
FilterChainMatch:   &  listener.FilterChainMatch {
ServerNames: serverNames,
TransportProtocol:   "tls"  ,
},
Filters: []   *   listener.Filter {
{
Name:   "envoy.filters.network.tcp_proxy"  ,
ConfigType:   &   listener.Filter_TypedConfig {
TypedConfig: tcpProxy,
},
},
},
}
}
```


Then all that remains is returning the correct listener protobuf with the filter chains:


```text
func   makeTCPListener  (  filterChains   []  *  listener  .  FilterChain  )   *  listener  .  Listener   {
tlsInspector, _:   =   anypb.  New  (  &  tls.TlsInspector {})


return   &  listener.Listener {
Name:   "tcp_listener"  ,
Address:   &   core.Address {
Address:   &   core.Address_SocketAddress {
SocketAddress:   &   core.SocketAddress {
Protocol: core.SocketAddress_TCP,
Address:   "0.0.0.0"  ,
PortSpecifier:   &   core.SocketAddress_PortValue {
PortValue: ListenerPort,
},
},
},
},
ListenerFilters: []   *   listener.ListenerFilter {
{
Name:   "envoy.filters.listener.tls_inspector"  ,
ConfigType:   &   listener.ListenerFilter_TypedConfig {
TypedConfig: tlsInspector,
},
},
},
FilterChains: filterChains,
}
}
```


With the dynamic discovery service, we're able to launch and terminate Fly machines and have the Envoy proxy route traffic to the correct BuildKit instance based on SNI. The service queries new machines every 100ms and updates the Envoy configuration to route traffic to the correct machine. So, when you run` depot build` , you're connected to the correct BuildKit instance instantly.


## What's next?


We're excited about where this new discovery service will allow us to run secure BuildKit instances. We are far less constrained by the need to have a public IPv4 address for each machine and can now run BuildKit instances behind a single public IP address.


This is still early days, but we have some other ideas we can implement on top of Envoy that could unlock new use cases, not just for Docker image builds, but for GitHub Actions Runners.


If you're interested in this, we're always game to talk about ideas and topics in ourCommunity Discord . If you're new to Depot and want to try it out, you can start a 7-day free trial with no credit card required:[depot.dev/start](https://depot.dev/start) .


Chris Goller


Principal Software Engineer at Depot
