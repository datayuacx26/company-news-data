---
schema_version: "1.0.0"
document_id: "163682449ddb0d2c139223bb3c9f9b75483f536d6ebed08580872ead86b401de"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-impact/"
published_at: "2023-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:e3fb4b20e673f8e7dd79582bbca1ed7ed87c015d36fb4d37954302712c8f75b2"
---

# 2023-03-08 incident: A deep dive into the platform-level impact

Laurent Bernaille


On March 8, 2023, Datadog experienced an outage that affected all services across multiple regions. In[a separate blog post](https://www.datadoghq.com/blog/2023-03-08-multiregion-infrastructure-connectivity-issue/) we describe how our products were impacted.


This event was highly unusual from the start. An outage that affects services across multiple regions is rare because our regions don’t have direct connections with one another, and because we roll out changes in one region at a time. An outage that affects services across multiple regions running on distinct cloud providers *at the same time* ? If you had asked us before March 8, we would have strained to give you a plausible scenario. Not anymore. We were able to retrace all the steps that led to this outage, and we are sharing our investigation with you in a series of posts, starting with this one that dives into the platform-level impact.


## Down the rabbit hole


The story really began in December 2020, two years before the outage, with two simple changes. In December 2020, systemd (v248) introduced a new[behavior](https://github.com/systemd/systemd/pull/17477#issuecomment-718350598) to systemd-networkd (a systemd component that manages network configuration on hosts): on start-up of v248, systemd-networkd flushes all IP rules it does not know about. Five months after this initial change, an additional[commit introduced in v249](https://github.com/systemd/systemd/pull/19287) made it possible to opt out of this behavior using a new` ManageForeignRoutingPolicyRules` setting. Both these changes were backported to systemd v248 and v247.


Systemd runs on pretty much every recent and widely used Linux distribution. It is a critical part of Fedora, Debian, Ubuntu, and many more. At Datadog, we have relied on Ubuntu for years. We were using Ubuntu long before our migration to Kubernetes, and when we did migrate we adapted our base images but kept the distribution. Our goal is to use the latest LTS (Long Term Support) version, but we usually wait a few months after a new release to start testing it so that it stabilizes. Ubuntu 22.04 was released in April 2022, and we started our tests in June 2022 before beginning a progressive deployment to production in November 2022. The previous LTS Ubuntu version (20.04) uses systemd v245, which did not get either of the changes mentioned in the previous paragraph. Ubuntu 22.04, however, uses v249, with the default configuration that manages foreign routing policy rules.


Proportion of hosts in production running Ubuntu 22.04 Proportion of hosts in production running Ubuntu 22.04


The new systemd-networkd behavior has been around on 22.04 hosts from the very beginning of the 22.04 rollouts. However, this new behavior only manifests when systemd-networkd starts, which in our infrastructure only happens when a new host is created, at which point we do not have any specific routing rules present.


### Patching for a good cause


On March 7, 2023, a patch for a CVE in systemd was[made available on Ubuntu repositories (249.11-0ubuntu3.7)](https://ubuntu.com/security/notices/USN-5928-1) . When such a patch is installed, all systemd components—including systemd-networkd—are restarted. Per Ubuntu’s publishing history for systemd, there has been no vulnerability that has required a systemd patch since at least September 2022, before we started deploying Ubuntu 22.04. This explains why we had not seen a systemd update leading to a systemd-networkd restart before. A[similar patch](https://launchpad.net/ubuntu/+source/systemd/245.4-4ubuntu3.20) for the same CVE was also released for systemd v245 (used in Ubuntu 20.04)—but in this version of systemd, a restart of systemd-networkd does not flush IP rules, and Ubuntu 20.04 nodes were not impacted.


Our servers were using the[Ubuntu defaults](https://wiki.ubuntu.com/SecurityTeam/FAQ#What_repositories_and_pockets_should_I_use_to_make_sure_my_systems_are_up_to_date.3F) for unattended (automated) upgrades:


-


Download package updates twice a day at 6:00 UTC and 18:00 UTC with a randomized delay of 12 hours (` apt-daily.timer` ):


```text
1   [Timer]    2   OnCalendar=*-*-* 6,18:00    3   RandomizedDelaySec=12h    4   Persistent=true
```


-


Run upgrades once a day at 6:00 UTC with a randomized delay of 60 minutes (` apt-daily-upgrade.timer` ):


```text
1   [Timer]    2   OnCalendar=*-*-* 6:00    3   RandomizedDelaySec=60m    4   Persistent=true
```


-


Only install security updates (which may pull in dependencies from the main release):


/etc/apt/apt.conf.d/50unattended-upgrades


```text
1   // Automatically upgrade packages from these (origin:archive) pairs    2   //    3   // Note that in Ubuntu security updates may pull in new dependencies    4   // from non-security sources (e.g. chromium). By allowing the release    5   // pocket these get automatically pulled in.    6   Unattended-Upgrade::Allowed-Origins {    7        "\${distro_id}:\${distro_codename}";    8        "\${distro_id}:\${distro_codename}-security";
```


This configuration minimizes potential changes from unattended upgrades by only allowing updates to packages published in` \\${distro_id}:${distro_codename}-security` and, if required for a security update, in the main archive (` \\${distro_id}:\\${distro_codename}` ), which only includes packages from the original release. Other updates are in` \\${distro_id}:\\${distro_codename}-updates` —which, in the default configuration that we use, is not part of allowed origins for unattended upgrades.


This configuration explains why all our hosts ran` apt-upgrade` between 06:00 and 07:00 UTC every day. The systemd patch did not impact 100 percent of our servers because:


-


The new systemd-networkd behavior only affects 22.04 nodes (90+ percent of our fleet as of March 8).


-


Not all nodes had downloaded the new version of systemd when the upgrade ran. For instance, we estimate that the new systemd version was available for Google Cloud nodes at 18:50 UTC on March 7, meaning that nodes that ran their 18:00 (+0-12h)` apt-daily` update before 18:50 UTC were not impacted.


We had never disabled unattended upgrades, as we have been using Ubuntu at Datadog for years without any issue. **However, we do not rely on** **` unattended-upgrades`** **to update our nodes.** We get and apply security patches as we automatically replace all nodes on a regular basis. This process gives us the ability to provide more than security patches as it also allows us to upgrade some Kubernetes components (such as kubelet or containerd) or the distribution we use. Our automated process is orchestrated to make sure we deploy progressively across regions and clusters. A typical workflow would look like this:


1.


Validate the update on experimental clusters


2.


Deploy the update on a single staging cluster


3.


Deploy to a few more staging clusters


4.


Deploy to all staging clusters


5.


Deploy to a production Kubernetes cluster running workloads in a single availability zone in our smallest region


6.


Deploy to a cluster in a second zone in the smallest region


7.


Deploy to all production clusters in the smallest region


8.


Repeat steps 5 through 7 in a second region, then a third, and so on


Between each of these steps, we let the change bake in (from one week to a month) to make sure we don’t have a regression. The graph “Proportion of 22.04 nodes” above is a good illustration of how we perform progressive rollouts as a matter of principle and process: start small and increase gradually as the risk of regression decreases.


## The host impact of the systemd-networkd restart


We have now solved the first puzzle of the simultaneous application of a change that disrupted instances across regions and cloud providers: restarting systemd-networkd as part of a security patch. But not all hosts around the world running Ubuntu 22.04 were disrupted, so there has to be more to it. We need to figure out what else came into play that turned an innocuous security patch into a global outage. For that, we need to share a few things about how we configure the network in Kubernetes.


On a Kubernetes node, pods run in their own network namespace, and each pod has a unique IP. To enable communication outside the host there are two options:


1.


Use an overlay. In this case, pod IPs are not part of the underlying network, and communication between pods running on different hosts requires encapsulation (usually VXLAN). This makes it possible to run Kubernetes on any underlying network without specific requirements, but it adds overhead and—because pod IPs aren’t part of the underlying network—it makes communication outside of the Kubernetes cluster challenging.


2.


Give pods IPs from the underlying network. In this setup, pods have full access to the network without overhead and can communicate with pods in other clusters easily. This is the most efficient setup and now a common Kubernetes networking choice. The main challenges in this setup are to have enough IP addresses for pods, to allocate IPs from the underlying network to pods, and to route traffic to and from pods.


In our environment, we have always used the second approach, and all of our pods get their IP addresses from the underlying cloud network (VPC). In this setup, for pods to have connectivity, Kubernetes networking plugins have to manipulate host routes and rules. Let’s look at exactly how it works in detail.


### Kubernetes networking at Datadog


We use[Cilium](https://cilium.io/) to manage pod networking. The setup is slightly different between cloud providers because their virtual networks have different behaviors. On AWS, we run Cilium in[ipam=eni mode](https://docs.cilium.io/en/v1.12/concepts/networking/ipam/eni/) , which means that:


-


Kubernetes nodes have additional elastic network interfaces (ENI) with multiple IPs used for pods.


The[Cilium operator](https://docs.cilium.io/en/v1.13/internals/cilium_operator/) is responsible for allocating additional ENIs and IPs to nodes to make sure that each node has enough available IPs for new pods.


-


The Cilium agent that runs on each node is responsible for managing routes and rules (as well as load balancing and network policies) so pods can have network connectivity.


The following diagram illustrates this:


In this example, the node uses two ENIs, identified as ens5 and ens6. The former enables traffic to and from the host network. The latter sends traffic to and from the pods on the host. In more detail, Cilium controls pod routing with the following set of rules and routes:


Traffic to pods is controlled in the main route table:


Terminal window


```text
1   $     ip     route     show     table     main   |   grep     lxc    2   10.132.194.9     dev     lxce64dbdfd41b6     scope     link    3   10.132.221.143     dev     lxc9008a40141ca     scope     link
```


Traffic from pods uses additional ENIs, which requires source routing. In our example, traffic using ens6 uses route table 11:


Terminal window


```text
1   $     ip     rule   |   grep     "lookup 11"    2   111:      from     10.132.221.143     lookup     11    3   111:      from     10.132.194.9     lookup     11
```


Route table 11 (uses additional interface ens6; main interface is ens5):


Terminal window


```text
1   $     ip     route     show     table     11    2   default     via     10.132.192.1     dev     ens6    3   10.132.192.1     dev     ens6     scope     link
```


However, we need to make sure that traffic between local pods is not routed outside the host, so we need additional rules before the 111 ones:


Terminal window


```text
1   $     ip     rule   |   grep     20    2   20:      from     all     to     10.132.221.143     lookup     main    3   20:      from     all     to     10.132.194.9     lookup     main
```


In addition, for advanced features, Cilium manipulates another set of rules and routes to control traffic to and from L7 egress and ingress proxies. These proxies are required for L7 network policies (for example, only allow traffic to a specific DNS domain, or only allow specific HTTP methods). To achieve this, Cilium uses the following rules and routes:


Terminal window


```text
1   $     ip     rule   |   grep     2004    2   9:      from     all     fwmark     0x200/0xf00     lookup     2004    3
4   $     ip     route     show     table     2004    5   local     default     dev     lo     scope     host
```


The above makes sure all traffic with a mark matching 0x200/0xf00 is delivered to the local stack. The mark is added by eBPF code for all traffic that needs to be sent to the proxy. It also requires matching TPROXY rules. See[the Cilium source code](https://github.com/cilium/cilium/blob/master/bpf/init.sh#L116-L120) for reference.


There are a few complex edge cases associated with the L7 proxy features that Cilium routing configuration needs to account for. For instance, Cilium has to be able to tell the difference between traffic leaving the proxy (which uses the host IP) and traffic from a service running on the host because traffic leaving the proxy has to be routed through the` cilium_host` interface. Cilium achieves this by adding a mark to packets leaving the proxy and including an IP rule to use a different route table for this traffic (route table 2005):


Terminal window


```text
1   $     ip     rule   |   grep     2005    2   10:      from     all     fwmark     0xa00/0xf00     lookup     2005    3
4   $     ip     route     show     table     2005    5   <cilium_host_IP>/32     dev     cilium_host    6   default     via     <cilium_host_IP>
```


There is an even more subtle edge case: traffic from a pod targeting a service binding the host IP. In that case, when traffic leaves the proxy, its destination IP is the host IP. This traffic still has to be routed through the` cilium_host` interface, but packets will never reach the rule matching the mark because it will first match this default routing rule:


Terminal window


```text
1   0:      from     all     lookup     local
```


Because this rule is priority 0, it effectively prevents Cilium from routing traffic from the proxy to the host IP through the` cilium_host` interface and instead routes it to the local network stack. To address this, Cilium lowers the priority of the local rule from 0 to 100, which allows the rule routing traffic from the proxy to be evaluated earlier because it has a higher priority (10).


If you want to dive into this setup, all the details can be found[in this Cilium commit](https://github.com/cilium/cilium/commit/f89a22ca779202e9180db40f389e2d677c767fab) .


Our setup uses endpoint routes, and so route table 2005 and matching rule are not used (i.e., traffic does not need to be sent to` cilium_host` ). This means that we don’t need to move the local lookup rule, but the Cilium configuration currently does not allow disabling this.


In the end, what really matters here is that Cilium replaces:


Terminal window


```text
1   0:      from     all     lookup     local
```


With:


Terminal window


```text
1   100:      from     all     lookup     local
```


So far we’ve described our AWS setup. Our Azure configuration is very similar: we have an interface dedicated to pods and run Cilium in[ipam=azure mode](https://docs.cilium.io/en/v1.12/concepts/networking/ipam/azure/) , which means we also need source routing (IP rules matching pod IP addresses and routing traffic with a table that uses the additional interface).


Our clusters running on Google Cloud are slightly different because the Google Cloud network supports adding an IP block to the main interface for pods with[alias IP ranges](https://cloud.google.com/vpc/docs/alias-ip) , and we run Cilium in[ipam=kubernetes mode](https://docs.cilium.io/en/v1.12/concepts/networking/ipam/kubernetes/) . This setup does not require source routing as we use a single interface for host and pod traffic. Cilium therefore does not need to install rules for pod egress traffic on Google Cloud, but it still has to move the` lookup local` rule.


The whole picture is starting to come together: we need pods to interact with their environment via host routing rules in order to communicate with the rest of the network, and that in turn requires processing routing rules in a specific order.


### The networking impact of the systemd-networkd restart


Getting back to the systemd upgrade: when the new version of systemd-networkd flushed routing rules on nodes, it had the following effects:


-


For AWS and Azure, pods use an additional ENI that requires source routing, so when the related rules disappeared (rules with priority 111 in our example), traffic from pods no longer used the proper interface and pods lost network connectivity. (This issue also[impacts the AWS CNI](https://github.com/aws/amazon-vpc-cni-k8s/issues/1600) .)


-


For all cloud providers, when the new, Cilium-created local rule with lower priority (100) was deleted, but the original priority-0 one was not added back, traffic to the node IP was not considered local anymore—and the node totally lost network connectivity.


To understand why nodes lost connectivity, let’s look at an example.


**Example**


-


node IP: 10.128.123.85


-


pod IP: 10.132.126.226


-


pod main interface: ens5


-


pod additional interface: ens6


#### Default Ubuntu host rules (without Cilium and Kubernetes)


This is what the routing rules look like for a fresh Ubuntu node.


Terminal window


```text
1   $     ip     rule    2   0:      from     all     lookup     local          <  =     local     route     lookup    3   32766:      from     all     lookup     main    4   32767:      from     all     lookup     default
```


#### Rules for a Kubernetes node using Cilium


Once Cilium and Kubernetes are running on the node, this is what its routing rules look like.


Terminal window


```text
1   $     ip     rule    2   9:      from     all     fwmark     0x200/0xf00     lookup     2004    <  =     L7     proxy    3   20:      from     all     to     10.132.126.226     lookup     main     <  =     route     to     pod    4   100:      from     all     lookup     local                <  =     moved     local     route    5   109:      from     all     fwmark     0x80/0xf80     lookup     main    6   111:      from     10.132.126.226     lookup     11        <  =     route     from     pod    7   32766:      from     all     lookup     main    8   32767:      from     all     lookup     default
```


#### Rules after systemd-networkd restart


These are the routing rules after the fateful systemd-networkd restart. Note how much shorter they look.


Terminal window


```text
1   $     ip     rule    2   32766:      from     all     lookup     main    3   32767:      from     all     lookup     default
```


In particular, the node is now missing:


-


**the rule for traffic from pods (AWS / Azure), breaking pod traffic.**


-


**the** **` lookup local`** **rule (AWS / Google Cloud / Azure), breaking host traffic.**


#### Route evaluations after rule flush


To visualize the effect on actual traffic, we’re going to resolve a few destinations and reason through the decisions that the network stack makes.


Pod traffic:


Terminal window


```text
1   $     ip     route     get     8.8.8.8     from     10.132.126.226     iif     lxc1c85157065ec    2   8.8.8.8     from     10.132.126.226     via     10.128.123.1     dev     ens5     10.132.126.226     iif     lxc1c85157065ec
```


With this command, we are performing a route lookup for traffic to destination IP 8.8.8.8 (a public IP of Google DNS resolvers outside of our network) coming from a pod with IP 10.132.126.226 through the[veth interface](https://man7.org/linux/man-pages/man4/veth.4.html) lxc1c85157065ec. Because this traffic is coming from a pod, it should be egressing the node via the pod interface, ens6. However, we can see that after the route flush, this traffic is now using the wrong outgoing interface (ens5) and so will be dropped by AWS or Azure software defined networks (SDN). **This means that pods lose network connectivity.**


Host traffic:


Terminal window


```text
1   $     ip     route     get     8.8.8.8    2   8.8.8.8     via     10.128.123.1     dev     ens5     src     10.128.123.85     uid     100    3
4   $     ip     route     get     10.128.123.85     from     8.8.8.8     iif     ens5    5   10.128.123.85     dev     ens5     src     10.128.123.85     uid     1001
```


Let’s now consider the effect the rule flush has on host traffic. The first route lookup shows that traffic from the host to an external IP is routed via ens5 and uses the host IP as source (10.128.123.85), which is expected. In other words, packets to 8.8.8.8 exit the node without problems. However, returning traffic arriving on interface ens5 with the host IP as destination should be routed to` dev lo` to be delivered locally. We can see that after the route flush, traffic to the host IP is instead routed back out through ens5 (the default route) and will be dropped by the cloud provider network. **This means that the host loses network connectivity.**


In summary: when nodes running Ubuntu 22.04—having downloaded the new systemd version—ran the unattended upgrade between 06:00 and 07:00 UTC on March 8, 2023, they lost host connectivity on all providers and pod connectivity on AWS and Azure.


## Cloud provider impacts


So far we have looked at the networking configuration we use at Datadog and what the systemd-networkd update changed. Next, we’ll cover in more detail what it means for a Kubernetes host at Datadog to lose network connectivity, as the effects differ across cloud providers due to their specific configurations. To be clear, any one cloud provider’s response to node connectivity loss is not better or worse than the others; we only highlight the differences here because it required a different response on our end.


### Google Cloud and Azure


When instances running in Google Cloud or Azure lose network connectivity, they continue to run without networking. We can see the impact of the incident on the total number of packets sent per second across our EU1 region. Traffic went from hundreds of millions of packets sent per second to almost 0 between 06:00 and 07:00 before increasing again as we were recovering and reaching twice the usual throughput as we were processing the backlog.


Sum of packets sent by instances in EU1 on March 8 Sum of packets sent by instances in EU1 on March 8


Google Cloud and Azure automatically repair instances when they experience a hardware or hypervisor issue. However, they do not do so by default for instances facing issues at the guest operating system level. You can enable this behavior by configuring HTTP-based health checks (see[Google Cloud documentation](https://cloud.google.com/compute/docs/instance-groups/about-repair#autohealing) and[Azure documentation](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-instance-repairs#how-do-automatic-instance-repairs-work) ). These health checks detect failures and replace instances.


For our clusters, we rely on the Kubernetes control plane to detect and replace failed instances, so we do not enable HTTP-based health checks. **Because instances had “only” lost network connectivity but were still running, Google Cloud and Azure did not detect failures and did not replace them, so we were able to recover instances by restarting them via their cloud APIs.**


### AWS


In addition to[HTTP-based health checks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-health-checks.html) that can be enabled for instances registered with Elastic Load Balancers, AWS also uses[status checks](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html) that verify if an instance is healthy and if it has network connectivity. If an instance in an Auto Scaling group fails its status checks, it is[terminated and replaced](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-health-checks.html#replace-unhealthy-instance) . When our instances on AWS lost network connectivity after systemd-networkd restarted, AWS detected them as unhealthy and started terminating and replacing them. The below graph illustrates this by showing the proportion of instances deleted with` TerminateInstances` calls from AWS between 06:00 and 08:00 UTC.


The percentage of instances in US1 terminated between 06:00 and 08:00 UTC The percentage of instances in US1 terminated between 06:00 and 08:00 UTC


This graph shows that more than 8 percent of our instances were terminated every 10 minutes between 06:00 and 07:00 UTC. By 08:00, AWS had terminated and replaced approximately 60 percent of the instances in the region, representing tens of thousands of instances.


This auto-healing feature is usually helpful because instance failures are detected fast, and Auto Scaling groups quickly replace the impacted instances. As we will see in a follow-up post discussing recovery, a large proportion of our workloads recovered quickly thanks to this feature. However, this behavior did create an additional impact for our services due to their specific architecture.


We heavily rely on local disks for data stores, coordination systems, and caches: a significant portion of them (whether open source or developed in-house) use local ephemeral storage because local disks have very good performance (throughput and latency) relative to costs, and the data stores are responsible for data replication. The data they store can always be reconstructed, but it can require reprocessing data or restoring from a backup (we did all three).


We use local disks on all providers, but instances on AWS were terminated, which means we lost all data stored on their local disks. In contrast, on Google Cloud and Azure, we could recover the instances and their disks by restarting the instance.


Given the difference in behavior, our initial assessment of the incident was that our Google Cloud and Azure regions were in a worse state than the AWS ones because many stateless services (e.g., serving web traffic) were down or severely impacted on Google Cloud and Azure, but were mostly okay on AWS (as AWS had quickly replaced the instances). However, we realized later that the AWS situation was much harder to recover from as we had lost a significant amount of data stored on local disks. In contrast, on Google Cloud and Azure rebooting the instances was the quickest path to recover the data still present on local disks.


## Conclusion


The mystery of what happened on March 8, 2023, between 06:00 and 07:00 UTC is now solved: A new systemd behavior introduced in Ubuntu 22.04 disconnected more than 60 percent of our instances from the network. This impacted multiple regions across distinct cloud providers. It also affected the regions hosting our CI/CD and automation tools.


Restoring connectivity required a different path based on the cloud provider. On Google Cloud and Microsoft Azure, we “only” had to restart thousands of instances. AWS automatically replaced thousands and thousands of instances at once, which delayed the recovery of data stores because they rely on local storage.


We had figured this all out within the first two to three hours of the incident, but that was just the beginning of a recovery that took many unexpected turns before we got everything back in shape. Curious about the next steps? You can read about our efforts to restore our platform[here](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-recovery/) .


-
-
-
