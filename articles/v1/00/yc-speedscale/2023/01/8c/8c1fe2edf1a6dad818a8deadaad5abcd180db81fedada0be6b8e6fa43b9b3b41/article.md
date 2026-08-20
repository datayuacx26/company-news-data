---
schema_version: "1.0.0"
document_id: "8c1fe2edf1a6dad818a8deadaad5abcd180db81fedada0be6b8e6fa43b9b3b41"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/traffic-mirroring-options-kubernetes/"
published_at: "2023-01-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:22a0c06b347cd37fc62e5f20d553074b69bbc42066b896f0ef0f267e5beeb920"
---

# Traffic Mirroring Options Kubernetes Guide

[Testing in production](https://www.globalapptesting.com/blog/testing-in-production) carries a lot of risk, like possibly causing downtime for users. However, the advantages of using real user traffic are many, which has led to the popularity of traffic mirroring.


Mirroring can be implemented as part of[pre-deployment testing](https://mattermost.com/blog/pre-and-post-deployment-testing-for-ci-cd/) , as well as other parts of the developer experience like the development itself. But, how do you get started with it?


Many companies have embarked on this journey, and the solutions range from ones trying combining multiple third-party tools, to building out a complete home-grown solution.


Some of the simplest solutions will try to replay traffic with tools like[GoReplay](https://goreplay.org/) , then using other tools like[Diffy](https://diffy.website/) to compare the traffic.


Those that choose to build out their own solution are most often those with big engineering teams. Some notable mentions are[Uber](https://www.uber.com/en-DK/blog/migrating-functionality-between-production-systems/) and[Meta](https://engineering.fb.com/2021/10/20/developer-tools/autonomous-testing/) . As you can see, even the best companies are using traffic mirroring, however it takes a lot high-skilled engineering to accomplish it successfully.


In this post you’ll get to see four different ways of setting up traffic mirroring in Kubernetes, along with the advantages and drawbacks of each approach, ultimately providing you with enough information to help you make an informed decision.


## Why Use Traffic Mirroring in Kubernetes?


Traffic mirroring isn’t unique to Kubernetes, and can be implemented in any infrastructure where you have some control of the network.


However, the nature of how Kubernetes works makes it optimal for traffic mirroring, given how it handles Ingress, Services, Load Balancers, etc.


In a traditional infrastructure based on virtual machines (VMs), if you’re lucky, your network infrastructure may support traffic mirroring by itself (like[AWS Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) ). If it doesn’t—or you’re not satisfied with the implementation—you will have to configure and manage a third-party tool. Most likely this will require additional VMs that you will then have to manage.


With Kubernetes, you get to utilize your existing infrastructure, by deploying new containers inside your existing Kubernetes clusters. Yes, containers are also additional resources that have to be managed, but it can be argued that managing containers adds a lot less cognitive load than managing VMs.


### Pros and Cons of Traffic Mirroring


As with any technology, it’s important to consider the pros and cons. Even though traffic mirroring seems like an obvious technology to implement, it may not be the right choice for everyone.


Starting with the advantages, traffic mirroring can allow you to:


- Preview changes during pre-deployment, given that you can accurately fork traffic;
- [Validate scale](https://speedscale.com/how-to-test-kubernetes-autoscaling/) ;
- Systematically perform A/B tests;
- Ensure realistic tests by using realistic data, thereby increasing the reliability of your tests.


There are many more advantages to using traffic mirroring, but most can be summed up by the simple fact that you are using real user data to test your services.


By manually generating test cases you can achieve very high code coverage, but you’re unlikely to test all edge cases. By using real traffic, you can be certain that you’re testing your application exactly the way that users are interacting with it.


That being said, there are some things you need to be aware of:


- You may impact business operations, especially if you’re writing to databases;
- You need to take care not to make any persistent changes;
- Traffic mirroring cannot run any actual transactions;
- [On-the-fly PII redaction](https://speedscale.com/preventing-pii-in-test-environments/) is a necessity, as regulations like\[


It’s worth noting twice that using any identifiable information in testing is illegal in many places around the world, which means that you’ll have to be careful when implementing traffic mirroring. Luckily there are tools that can help do this automatically, which you’ll see later on in this post.


## Ways to Implement Traffic Mirroring


Now that you have at least a general idea of why traffic mirroring is useful in Kubernetes, it’s time to take a look at how you can actually get it implemented.


Please note that this post intends to give you a high-level overview of what the available options are, providing you with a starting point. All options shown here will include resources for further reading, which you can then use to conduct further research on your own.


### Traffic Mirroring in Service Meshes


\[To get an idea of how service meshes can be used for traffic mirroring, let’s use\]([https://www.sqlshack.com/using-production-](https://www.sqlshack.com/using-production-) alt=)[Istio](https://cloud.google.com/learn/what-is-istio) —one of the most popular service mesh options—as an example. The example here is borrowed from the[official Istio documentation](https://istio.io/latest/docs/tasks/traffic-management/mirroring/) , where you can also read more about the specific implementation.


In this example, let’s make a couple of assumptions first:


- You’ve got two Deployments running:` httpbin-v1` and` httpbin-v2` ;
- These Deployments receive traffic through a single Service called` httpbin` .


In Istio, you can configure the Service to send all traffic to the` httpbin-v1` deployment, by implementing the following manifest:


```text
$   kubectl   apply   -f   -   <<  EOF
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
name: httpbin
spec:
hosts:
- httpbin
http:
- route:
- destination:
host: httpbin
subset: v1
weight: 100
---
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
name: httpbin
spec:
host: httpbin
subsets:
- name: v1
labels:
version: v1
- name: v2
labels:
version: v2
EOF
```


This will deploy a` VirtualService` and a` DestinationRule` - both services specific to Istio. The` VirtualService` defines that the destination for 100% of the traffic is the` httpbin` Deployment, specifically the` v1` subset.


The` DestinationRule` defines the subsets used in the` VirtualService` .


At this point, the` httpbin-v2` deployment is just sitting in the cluster unused, but now you can modify the` VirtualService` to mirror all traffic to the` httpbin-v2` Deployment:


```text
$   kubectl   apply   -f   -   <<  EOF
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
name: httpbin
spec:
hosts:
- httpbin
http:
- route:
- destination:
host: httpbin
subset: v1
weight: 100
mirror:
host: httpbin
subset: v2
mirrorPercentage:
value: 100.0
EOF
```


As you can see, this is done quite simply by adding a` mirror` field to the yaml manifest.


### Pros and Cons


The biggest advantage of using Istio for your traffic mirroring is that it’s easy to implement. As you just saw, all it took was an additional five lines of yaml.


However, that’s only if you’re planning to mirror traffic within the same cluster. This does have valid use cases, for example when using[blue/green deployments](https://www.redhat.com/en/topics/devops/what-is-blue-green-deployment) .


But when you’re planning to utilize production traffic in[staging environments](https://www.techtarget.com/searchsoftwarequality/definition/staging-environment) , most organizations want to maintain a clear separation of production and staging/development environments, which makes it a bit tougher to implement.


Back in 2020, Trivago implemented cross-cluster traffic mirroring with Istio, and judging by their[writeup of the experience](https://tech.trivago.com/post/2020-06-10-crossclustertrafficmirroringwithistio/) , it seems to work fairly well for them.


However, in their blog post they admit that setting it up was a bit challenging. Plus, there was one glaring disadvantage: They needed to establish a direct connection between the clusters.


Granted, this network connection from the production cluster to the staging cluster was done within their own private network, meaning no traffic was accessible from the public internet, but it still meant that the production cluster had an open connection to the staging cluster.


More importantly, the staging cluster had a direct connection to the production cluster, which can present a security risk. This can, of course, be solved by implementing strict network rules, but it’s still something to keep in mind if you plan to use Istio.


All in all, Istio is a good solution if you’re already using it, as it’s fairly easy to set up traffic mirroring. But Istio might not be the best choice if you only want to mirror traffic.


### Using AWS VPC for Traffic Mirroring


First of all, this solution is only viable if your resources are running in AWS. Second, the mirroring is configured outside of your Kubernetes cluster, and instead is configured as part of the VPC.


Getting[traffic mirroring set up within a VPC](https://docs.aws.amazon.com/vpc/latest/mirroring/what-is-traffic-mirroring.html) requires a bit more effort than configuring Istio. The main thing to note is that you cannot refer to any specific service within your Kubernetes cluster, as your VPC does not know about your Kubernetes services.


Instead, choosing the source and target for the traffic is done by selecting either a network interface, a network load balancer, or a gateway load balancer endpoint.


It’s far from impossible to get traffic mirroring implemented in Kubernetes with a VPC, but you should expect to have to do a fair amount of digging yourself, as there are few resources on how to implement it.


### Pros and Cons


The biggest advantage of using AWS VPC for traffic mirroring is that it’s running natively on AWS, which can be an advantage for AWS users.


Aside from that, some may also find it advantageous that you can interact with the traffic directly on the network layer, manually controlling network interfaces and VXLANs. However, this is where the list of advantages in relation to VPCs begins to fall short.


To clarify, there are definite use cases for using VPC traffic mirroring; however, whether it’s the optimal solution for Kubernetes clusters is harder to say.


### Traffic Shadowing with Ambassador


[Ambassador](https://www.getambassador.io/) offers traffic mirroring as well, although within their ecosystem it’s referred to as “traffic shadowing.”


To implement traffic shadowing with Ambassador, you only need to add a single resource to your cluster. This assumes that you’ve already got Ambassador configured in your cluster to handle network traffic in the first place.


To configure mirroring in Ambassador you have to add a` shadow` field to your manifest file. This field will tell Ambassador where it needs to mirror the traffic. Here’s an example of what a regular mapping resource may look like:


These examples have been borrowed from the[official Ambassador docs](https://www.getambassador.io/docs/edge-stack/latest/topics/using/shadowing) .


```text
apiVersion  :   getambassador.io/v3alpha1
kind  :    Mapping
metadata  :
name  :    myservice
spec  :
hostname  :   '*'
prefix  :   /myservice/
service  :   myservice.default
```


When you want to shadow any traffic going to the` myservice.default` , you’ll need to copy the manifest file and make some minor changes:


- Change the name of the resource;
- Change the name of the service;
- Add the` shadow: true` field.


```text
apiVersion  :   getambassador.io/v3alpha1
kind  :    Mapping
metadata  :
name  :    myservice-shadow
spec  :
hostname  :   '*'
prefix  :   /myservice/
service  :   myservice-shadow.default
shadow  :   true
```


Notice the changes to enable shadow to` true` with the new service. This is all it takes to implement traffic mirroring (shadowing) in Ambassador.


### Pros and Cons


As with Istio, the above explanation only covers how to set up mirroring within a single cluster. But unlike Istio, there aren’t any apparent resources to aid you in setting up mirroring to other environments, like staging.


Ambassador does offer[multi-cluster support](https://www.getambassador.io/learn/multi-cluster-kubernetes) , which seems to be the most plausible way to get cross-cluster mirroring to work. However, that again presents the issue seen with Istio, where you need a direct connection between clusters in different environments.


All in all, Ambassador is far from a bad choice when implementing traffic mirroring, but whether it’s right for you will heavily depend on your intended use case.


### Traffic Mirroring with Speedscale


Now for the first option on this list that natively supports PII redaction while mirroring traffic, letting you test with production traffic, without having to worry about leaking personal data.


Other than that, Speedscale resembles both Istio and Ambassador, in the way that it works natively with Kubernetes. However, while both Istio and Ambassador provide traffic mirroring as an extra feature, traffic is the primary focus for Speedscale.


You can read the[full Speedscale tutorial](https://speedscale.com/kubernetes-load-test-tutorial/) for details on how to set up and use Speedscale, but the general idea is that you install an[Operator](https://docs.speedscale.com/setup/install/kubernetes-operator/) into the clusters you want to use traffic mirroring.


With the operator installed, you can instrument your services with the[Speedscale sidecar](https://thenewstack.io/sidecars-are-changing-the-kubernetes-load-testing-landscape/) , either by manually adding annotations to your manifest, or by using the` speedctl` CLI tool.


Once the[sidecar is installed](https://docs.speedscale.com/setup/sidecar/annotations/) , it will act like a proxy for both incoming and outgoing requests (which can be configured). Speedscale will then capture all traffic for the instrumented services and save it to Speedscale’s database.


This is where you’ll see the biggest differentiator between Speedscale and other traffic mirroring solutions. The focus of the other three options on this list is on **mirroring live traffic** , while Speedscale focuses more on **replaying traffic** .


### Pros and Cons


The obvious disadvantage of Speedscale is its inability to replicate live traffic; however, there is good reason for this. By storing traffic and allowing you to replay it later, Speedscale allows you to:


- Modify traffic before replay, i.e., replicating the load, performing PII redaction, introducing chaos, etc.;
- [Transform the traffic](https://docs.speedscale.com/concepts/transforms/) to ensure compatibility with your test environment, like modifying auth headers;
- [Filter the traffic](https://docs.speedscale.com/guides/creating-filters/) in case you only want part of your application tested.


Those are just some of the advantages you get from having the incoming traffic recorded and replayed; however, Speedscale also records the outgoing traffic. Because of this, Speedscale can use the sidecar proxy to intercept outgoing requests, instead using the recorded response from your production environment, essentially creating automatic mocks for you.


In the end, whether Speedscale is the right choice for you comes down to your use case. If you need traffic to be replicated as it’s happening, then Speedscale perhaps isn’t the right choice.


One last thing to mention is that Speedscale prevents the need to have a direct connection between your production and staging/development environments, as captured traffic is stored by Speedscale.


While you may worry about a third-party storing your data, Speedscale makes sure to only store desensitized, PII-redacted traffic. With its single tenant, SOC2 Type 2 certified architecture, you can be certain that your data is safe and compliant.


## Choose the Best Option for You


Traffic mirroring can provide an array of benefits, and get you close to testing in production, without the inherent risks. It does require some up-front effort, but in many cases you’ll see a big ROI on the effort.


Once you’ve made the choice to implement traffic mirroring, it’s time to choose the right tool. There’s never a clear answer to this question, but hopefully this blog post has helped you make a more informed decision.


If you’re still curious to know more about this concept as a whole, take a look at how[Kubernetes and traffic replication go hand in hand](https://speedscale.com/kubernetes-production-traffic-replication/) .
