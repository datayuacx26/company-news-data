---
schema_version: "1.0.0"
document_id: "5e7ae6cee0c47282c718ff7bab423aacd6c4abf8d6e09f69e0ea3251c867e53f"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-06-10-crossclustertrafficmirroringwithistio/"
published_at: "2020-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:be80aff56447824e44f88b22b205503e10eb71e9c92909522adb9a7138fd6273"
---

# Cross-Cluster Traffic Mirroring with Istio

> The price of reliability is the pursuit of the utmost simplicity.
> — C.A.R. Hoare, Turing Award lecture


# Introduction


Have you ever enthusiastically released a new, delightful version to production and then suddenly started hearing a concerning number of notification sounds? Gets your heart beating right? After all, you didn’t really expect this to happen because it worked in the development environment.


This *“But it worked in the development environment!”* problem is uncomfortably common amongst software developers and has been attempted to be solved for many years. Fortunately, we’ve progressed significantly. One of the most consequential concepts to solve this problem is containerization, thus, “[Docker](https://www.docker.com/) ”. We now have immutable images, meaning the delta of the application codebase/configuration between the environments is *almost* 0. Emphasis on almost, because containerization doesn’t cover the whole problem.


Let’s specifically investigate one of the uncovered parts that we’ve faced recently. We developed a feature for one of the services and deployed it to the development environment. No issues were seen, thus, we proceeded with production deployment. Deployment went well, as expected. Yet, after ~30 minutes, we started to see alarms being triggered because the Kubernetes log volume was exceeded by a factor of two over the previous day. Can you see the problem here?


The issue was simple: There is traffic in the production environment, but not during development. Since the access log is populated with each request, you kind of expect this issue to happen in production. Of course, we could’ve unearthed this in the development environment by executing the same size of mock traffic -but is it the best solution?


Mocking the traffic means either you have to rely on a 3rd party tool such as[gock](https://github.com/h2non/gock) and[pook](https://github.com/h2non/pook) , or you have to code one by yourself. In both cases, you have the following disadvantages:


- You introduce one more dependency,
- You have to make sure that you mock the traffic as accurately as possible,
- You create a workload on the server that you generate the mock traffic.


There is a better alternative, eliminating these disadvantages. By mirroring traffic with Istio, you can develop features based on the actual, live traffic. No extra dependencies, the exact same traffic and no server to generate the mock traffic. Another beauty of Istio is that you can configure the mirrored traffic percentage easily, just by adjusting one parameter namely` mirror_percent` . All this means, we are reducing the delta between the environments even more.


# Cross-Cluster Traffic Mirroring with Istio


If you are using[Kubernetes](https://kubernetes.io/) with[Istio](https://istio.io/) , make yourself comfortable because Istio has a[traffic mirroring feature](https://istio.io/docs/tasks/traffic-management/mirroring/) and it’s really straightforward, *if you mirror traffic in the same cluster* . This feature gets a bit complex when you try to mirror the traffic between two clusters. For extra complexity, imagine these two are in different regions.


## Scenario


In trivago, we deploy services across multiple geographic regions to improve resiliency, high-availability and quality. However in this case, the service in question was special in that it was deployed and receiving traffic only in the US region and we wanted to mirror that traffic to our stage environment. Our stage cluster is only booted in the EU region to stay close to the trivago talents for them to work efficiently. So, we had to mirror traffic between clusters running in two different regions.


From now on, the word Prod Cluster is used interchangeably with source cluster and the word Stage Cluster is used interchangeably with target cluster.


## Additional Requirements


To achieve intra-cluster traffic mirroring, you create two different versions of` Deployment` for the` Service` , you create a` Destination Rule` to define subsets (versions) and you create a` Virtual Service` to mirror the traffic. Fairly easy. Assuming that you already implemented this –[Istio Traffic Mirroring Documentation](https://istio.io/docs/tasks/traffic-management/mirroring/) –, the following are the **additional** requirements to accomplish cross-cluster traffic mirroring:


- Deploying the services(i.e httpbin) that you’d like to mirror to Prod Cluster and Stage Cluster, separately.
- Making sure that the source cluster can connect to the target cluster — The good thing about GCP is that you can span single VPCs to different regions, thus, you can utilize internal Load Balancers with Istio Gateways using annotations to allow cross-region communication, further information[here](https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balancing#global_access) (Annotations) and[here](https://cloud.google.com/load-balancing/docs/internal/setting-up-internal#ilb-global-access) (Global Routing for Internal Load Balancers).
- You need to create a` Service Entry` inside the Prod Cluster to introduce Stage service, since Stage service is not a part of Prod Cluster’s mesh. This step is required if your[meshConfig.outboundTrafficPolicy.mode](https://istio.io/docs/tasks/traffic-management/egress/egress-control/#envoy-passthrough-to-external-services) configuration is set to` REGISTRY_ONLY` . By default, it’s set to` ALLOW_ANY` .
- You need to catch the mirrored hostname by the Istio` Ingress Gateway` inside the Stage Cluster.
- After catching the mirrored hostname by Istio` Ingress Gateway` , you need to forward the mirrored traffic to Stage Service by creating a` Virtual Service` inside the Stage Cluster.


## Connectivity


Before we get ahead of ourselves and try to implement traffic mirroring, let’s first test if we can connect to the target cluster from the source cluster. It’s best to progress one by one. I’d even encourage you to first try intra-cluster. Anyway, we need a` Service Entry` in Prod Cluster to introduce the external Stage` Service` , also` Gateway` and` Virtual Service` resources in Stage Cluster to catch and act on the incoming traffic as explained before.


**[Service Entry](https://istio.io/docs/reference/config/networking/service-entry/) (Prod Cluster)**


```text
apiVersion  :   networking.istio.io/v1alpha3
kind  :   ServiceEntry
metadata  :
name  :   httpbin-serviceentry
namespace  :   frontend
spec  :
hosts  :
-   httpin.frontend.stage.eu.trv.cloud
ports  :
-   number  :   80
name  :   http
protocol  :   HTTP
location  :   MESH_EXTERNAL
resolution  :   DNS
```


Above` Service Entry` introduces` httpin.frontend.stage.eu.trv.cloud` hostname to the service mesh of the source cluster.


**[Ingress Gateway](https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/) (Stage Cluster)**


```text
apiVersion  :   networking.istio.io/v1beta1
kind  :   Gateway
metadata  :
name  :   istio-ingress
namespace  :   istio-system
spec  :
selector  :
istio  :   ingressgateway
servers  :
-   hosts  :
-   '*'
port  :
name  :   http2-dev
number  :   80
protocol  :   http2
```


Notice the` hosts` section having ’*’ wildcard. It means, the target cluster is listening to all incoming traffic on` port 80` , regardless of` hostname` .


**[Virtual Service](https://istio.io/docs/reference/config/networking/virtual-service/) (Stage Cluster)**


```text
apiVersion  :   networking.istio.io/v1beta1
kind  :   VirtualService
metadata  :
name  :   http2-frontend
namespace  :   istio-system
spec  :
gateways  :
-   istio-ingress
hosts  :
-   '*.frontend.stage.eu.trv.cloud'
# - There will be another entry here for the mirroring
http  :
-   match  :
-   authority  :
prefix  :   httpbin.frontend
port  :   80
route  :
-   destination  :
host  :   httpbin.frontend.svc.cluster.local
port  :
number  :   8000
```


Above` Virtual Service` ensures that any traffic received with the` *.frontend.stage.eu.trv.cloud` hostname from` port 80` will be forwarded to` httpbin Service` on` port 8000` in the Stage Cluster.


After creating the resources, let’s try to connect to the` Stage Service` with the` sleep Pod` that we’ve created.


Voilà! We can connect from Prod Cluster to Stage Cluster. That means, mirroring should work if configured properly, but before continuing, I’d like to explain some potential non-successful response status codes that you might see at this point:


**502 - Bad Gateway**


This means that likely your` Service Entry` in the source cluster or` Gateway` in the target cluster is configured wrong. Please check them.


**404 - Not Found**


This means that likely your` Virtual Service` in the target cluster is configured wrong.


## Mirroring


There are two steps left to implement traffic mirroring. We need to create a` Virtual Service` inside the source cluster to configure the mirroring and we need to configure the` Virtual Service` in the target cluster. Here is what the configuration looks like.


**Virtual Service (Prod Cluster)**


```text
apiVersion  :   networking.istio.io/v1alpha3
kind  :   VirtualService
metadata  :
name  :   httpbin-virtualservice
namespace  :   frontend
spec  :
hosts  :
-   httpbin
http  :
-   route  :
-   destination  :
host  :   httpbin
port  :
number  :   8000
weight  :   100
mirror  :
host  :   httpin.frontend.stage.eu.trv.cloud
port  :
number  :   80
mirror_percent  :   100
```


This is similar to what we’d have with intra-cluster mirroring – the only difference is that we now give the exact hostname of the` Service Entry` as the mirroring target. It is worth mentioning that this is NOT a` host` rewrite, this is a service registry lookup.


Now, if you have the eagle’s eye, you’ll remember the comment I put in the` Virtual Service` of the target cluster before. The **crucial, vital, critical – you name it –** thing here is that the mirrored traffic has` -shadow` postfix in the` Host` header. That means, if you execute` curl httpbin.frontend.svc.cluster.local` command in the source cluster, the mirrored traffic will be sent to the target cluster as` httpbin.frontend.svc.cluster.local-shadow` . It is worth noting that the request yields a different` hostname` depending on how you` curl` . Knowing this, we have to configure the` Virtual Service` in the target cluster and add the host with` -shadow` postfix.


**Virtual Service (Stage Cluster)**


```text
apiVersion  :   networking.istio.io/v1beta1
kind  :   VirtualService
metadata  :
name  :   http2-frontend
namespace  :   istio-system
spec  :
gateways  :
-   istio-ingress
hosts  :
-   '*.frontend.stage.eu.trv.cloud'
-   '*.frontend.svc.cluster.local-shadow'   # Added the Host entry with -shadow postfix
-   '*.frontend-shadow'
http  :
-   match  :
-   authority  :
prefix  :   httpbin.frontend
port  :   80
route  :
-   destination  :
host  :   httpbin.frontend.svc.cluster.local
port  :
number  :   8000
```


After adding the` Host` entry, to see whether the mirroring works or not, we have to send requests to our` Prod Service` . We’ll use the sleep pod again to send some requests to the` Prod Service` in this case. We can say it works if we see the traffic **also** in the` Stage Service` .


This is the curl command that I’m using:


` kubectl exec -it httpbin-sleep-669b559684-6lm6l -c sleep -- sh -c 'curl http://httpbin.frontend.svc.cluster.local/headers -v'`


On the left terminal, I’m sending requests to` Prod Service` from the` sleep pod` . The right terminal is attached to the logs of` Stage Service` . You can see that the requests sent to` Prod Service` are also received from the` Stage Service` .


**Fun fact:** When we were implementing this at trivago, we couldn’t see the traffic mirroring happening even though there was a connection, because we weren’t catching the` Host` header with the` -shadow` postfix on it. To debug what was wrong, we had to use the[ksniff](https://github.com/eldadru/ksniff) tool and watch the` tcpdump` of the` sleep` pod. That way, we found that the mirroring was returning 404s, because the` Virtual Service` in the target cluster didn’t know anything about a` …-shadow` Host header.


# Conclusion


This is all great, now we can develop features on the actual traffic and expect little differences between environments when we release versions.


**BUT** again, is this the best solution? Probably, for now. For now, because if you look at this implementation from an architectural perspective, even though we reduced the element of surprise and accelerated the development, we actually have introduced complexity to our structure and steered a bit away from simplicity. The complexity comes from the fact that now we have to ensure the service in Stage Cluster doesn’t process the mirrored traffic as if it’s the live traffic and we have to ensure that the connection between US and EU is running smoothly. Needlessly to say, compared to the other solutions, the complexity is lower, but it’s there. If you remember what I quoted at the beginning from C.A.R. Hoare, our implementation somehow contradicts with the statement. We have traded simplicity, for quality.


The point is, there is still plenty of room for improvement with the software development industry and it seems that we still have a long way ahead of ourselves.
