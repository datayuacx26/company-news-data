---
schema_version: "1.0.0"
document_id: "29326cd99a41b28885a986ea8b5395013d561dd64010ec32a3433168de224b2d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/consul-at-datadog/"
published_at: "2016-08-11T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:33fc85ed8da260e9cbd102bdd7aa21f549ce4cb475ff96024c7968b7954f6556"
---

# Consul at Datadog

Darron Froese


We’ve been using Consul for about 18 months at Datadog and it’s an important part of our production stack.


It helps us primarily to:


1.


Distribute configuration across our cluster.


2.


Discover service endpoints for our microservices based architecture.


Here’s how it’s all connected together:


We’ve talked about[our journey with Consul](https://blog.froese.org/2016/04/08/srecon-running-consul-at-scale/) but want to post some of our most important recommendations here:


1.


Consul Servers like Beefy CPUs


2.


Fast Auditable Configuration Changes


3.


ACLs are your Friend


4.


Don’t DDoS Yourself - Use a Watch


5.


dnsmasq Lightens the Load


6.


Monitoring Consul is Not Optional


## Consul Servers like Beefy CPUs


Consul server nodes elect a Leader using the[Raft consensus protocol](http://thesecretlivesofdata.com/raft/) . They need a single leader to help them to agree as a distributed system.


If the non-Leader server nodes don’t hear from the Leader for 500 milliseconds, they kick that Leader out and elect a new one - this is called a` leadership transition` . If your Consul server nodes are[undergoing a large number of leadership transitions](https://www.consul.io/docs/internals/consensus.html) , the simplest thing to do is to give them more CPU power.


```text
1   Server Size Recommendations:    2   m3.large ~ 300 agent nodes    3   c3.xlarge ~ 500 agent nodes    4   c3.2xlarge ~ 800 agent nodes
```


We have some specific recommendation sizes[posted](https://speakerdeck.com/darron/running-consul-at-scale-journey-from-rfc-to-production?slide=76) , but the rule of thumb is: If you’re seeing leadership transitions every hour - or more - then increase the server’s CPU size until they are - at most - a daily occurrence.


Please note - most monitoring systems don’t have high enough resolution to see a 500 millisecond CPU spike - but this helps to minimize leadership transitions.


## Fast Auditable Configuration Changes


A great use of Consul’s Key Value store is to distribute configuration data around your cluster. Data stored here is available on any node via an[HTTP call](https://www.consul.io/docs/agent/http.html) or - when it changes - through a[Consul watch](https://www.consul.io/docs/agent/watches.html) .


Having this data available without an audit trail is a recipe for disaster - you don’t know who changed what or when the change was made. Use[git2consul](https://github.com/Cimpress-MCP/git2consul) to distribute the contents of a git repository.


We use git2consul for 60 second cluster wide configuration changes dozens of times a day.


## ACLs are your friend


Ever heard the saying: “Good fences make good neighbors?”


In the same way, use Consul’s[Access Control List system](https://www.consul.io/docs/internals/acl.html) to make sure that only authorized processes can remove or overwrite data that you’re placing into the Key Value store.


These ACLs can also help to protect against accidental mistakes by localizing the scope of the damage - any given token only has access to its own data and no more.


## Don’t DDoS Yourself - Use a Watch


Watch your read and write velocity and volume. Even though it can handle significant read and write loads, Consul isn’t designed to be accessed hundreds of thousands of times per second like Redis or Memcached.


[Consul watches](https://www.consul.io/docs/agent/watches.html) are a very powerful way to distribute and interact with Key Value data as it changes:


```text
1   {    2       "watches"  : [    3          {    4           "type"  :   "key"  ,    5           "key"  :   "/kvexpress/hosts/checksum"  ,    6           "handler"  :   "kvexpress out -k hosts -f /etc/hosts.consul -c 00644 -e 'sudo pkill -HUP dnsmasq'"    7          }    8        ]    9   }
```


Be aware that Consul watches can occasionally[fire too much](https://github.com/hashicorp/consul/issues/571) . We’ve been using[sifter](https://github.com/darron/sifter) to protect against watches firing when they’re not supposed to.


## dnsmasq Lightens the Load


If you’re using Consul for service discovery, and you’re using the[DNS interface](https://www.consul.io/docs/agent/dns.html) to find your services, there are several ways to help Consul scale.


First off, add a[short DNS TTL](https://www.consul.io/docs/guides/dns-cache.html) to Consul - we use 10s for most services.


Secondly, query dnsmasq instead of Consul directly. If dnsmasq doesn’t know the answer, it will ask Consul. There’s some example dnsmasq configuration and installation details available[here](https://github.com/darron/kvexpress-demo/blob/c0bd1733f0ad78979a34242d5cfe9961b0c3cabd/ami-build/provision.sh#L42-L56) .


Third, at extremely high velocities, you can cache the Consul services in an additional hosts file that’s loaded into dnsmasq -[see here](https://github.com/darron/kvexpress-demo/blob/c0bd1733f0ad78979a34242d5cfe9961b0c3cabd/ami-build/provision.sh#L51) . With this in place, we regularly serve more than 100,000 DNS requests / second using dnsmasq while only 400 requests / second are hitting Consul directly.


We’re getting stats out of dnsmasq and into Datadog using[goshe](https://github.com/darron/goshe#dnsmasq) .


## Monitoring Consul is Not Optional


If you want to deploy Consul - you really do need a way to monitor it.[We have blogged about monitoring Consul](https://www.datadoghq.com/blog/monitor-consul-health-and-performance-with-datadog/) in the past using Datadog but because of the[go-metrics library](https://github.com/armon/go-metrics) that Consul uses, there are additional alternatives.


The most important metrics to watch are:


1.


` consul.consul.leader.reconcile.count` - Do we have a Leader? Should be flat.


2.


` consul.serf.events.consul_new_leader` - When were the last leadership transitions? Lots of these are a sign of problems.


With those two metrics in a good state you can be reasonably sure that your Consul cluster is healthy.


You can be assured that your cluster is NOT healthy if you see this:


Other metrics to watch include:


1.


` consul.raft.leader.lastContact` - Time since the node has had contact with the Leader.


2.


` consul.consul.dns.domain_query.count` - How many DNS requests are hitting Consul directly?


3.


CPU on Consul server nodes.


4.


Networking on Consul server nodes.


-
-
-
