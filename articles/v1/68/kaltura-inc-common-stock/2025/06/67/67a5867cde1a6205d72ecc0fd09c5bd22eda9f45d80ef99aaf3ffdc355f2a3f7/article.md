---
schema_version: "1.0.0"
document_id: "67a5867cde1a6205d72ecc0fd09c5bd22eda9f45d80ef99aaf3ffdc355f2a3f7"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-rss-8a80d100aa25"
canonical_url: "https://medium.com/kaltura-tech/istio-rate-limit-hands-on-lab-part-1-4dccb9885e64"
published_at: "2025-06-09T07:01:48+00:00"
first_seen_at: "2026-07-20T23:18:51.911067+00:00"
fetched_at: "2026-08-20T01:13:13.966489+00:00"
content_hash: "sha256:73f8df20e3bb76eaafd95890f1688cc3d02fb285cd1d310f4dd7a5d7dbd5a95c"
---

# Istio rate limit - Hands on Lab Part 1

### Introduction


I’ve always appreciated how versatile Istio is — not just a traffic router, but a full-fledged service mesh handling security, observability, retries, failovers, and more — all baked in.


At[Kaltura](http://www.kaltura.com/) , we take proactive measures to safeguard our systems against potential traffic anomalies. Istio’s built-in rate limiting plays a key role in helping us maintain stability and performance under unexpected conditions.


This is the first of a two-part series on how we use Istio rate limiting in production. In this hands-on lab, I’ll Walk through setting up route-level and per-user limits using request headers, explain advance rate limiting with **multiple domains** , and keep it sharp and practical — just like Istio.


*Part 1 : Setup and simple Rate Limit* <- **You are here**
[Part 2 : Advance Rate limit scenarios](https://medium.com/p/1e8b5fdad217/edit)


The code for the lab can be found[here](https://github.com/tshaiman/istio-ratelimit-blog)


Photo by[Markus Winkler](https://unsplash.com/@markuswinkler?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/a-red-and-white-speed-limit-sign-next-to-a-tree-EbqlnKG6iEw?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)


### Recap on Istio Rate Limit


Istio rate limiting is an external, pluggable component in the service mesh that enforces traffic quotas to protect services from overload. It typically runs as a separate deployment and relies on a fast datastore like Redis to track request counts and limits. This decoupled design makes it scalable, flexible, and mesh wide.


Rate limiting in Istio happens entirely in the **data plane** , enforced by Envoy proxies.


When a request hits the gateway, it passes through a **filter chain** (e.g., CORS, AuthN/RBAC, Rate Limit, Route).


Envoy then queries an external **Rate Limit Service** , which checks usage against a **Redis** store. If within limits, the request continues through the proxy to the service. If not, Envoy returns a 429 immediately, **without routing the traffic to the backend service** — a major advantage that helps reduce load on upstream systems.
othewise if no over-limit is met, the request is routed to the side-car proxy of the application, and then the backend handles it and return the actuall response for the requiest ( 200/400/500 ,etc)


### Setup


we will be using a local kind cluster for simplicity. grab the code here and head into the 0-setup folder.
from there just run ./setup.sh and wait for everything to be installed.
The script does the following:


- Spin up kind cluster
- Enable Cloud Provider KIND — an open-source project allow us use balancer in kind. more details[here](https://github.com/kubernetes-sigs/cloud-provider-kind)
- istio installation using the shortest way without customization (for demo purposes). you will need istioctl for this to work.


for Linux :


```text
curl -LO https://github.com/istio/istio/releases/download/1.25.0/istioctl-1.25.0-linux-amd64.tar.gz  tar -xzf istioctl-1.25.0-linux-amd64.tar.gz  sudo mv istioctl /usr/local/bin/
```


for mac: brew install istioctl


let's verify what we have:


```text
kubectl get all -n istio-system
```


load balancer is pending


we notice few interesting things:


- we got Istio ingress gateway as part of the installation
- our Istio ingress gateway is in pending state. that is because we have not run the cloud provider kind yet. open another terminal window and run cloud-provider-kind utility. after doing so, you should have
istio-ingressgateway service with valid external IP address.


### Demo Application installation


Next, let’s deploy a demo application that will serve as our web backend, on which we’ll apply rate limiting.


Navigate to the 1-Application directory and execute app-setup.sh.


Here’s a quick overview of what the script does:
It deploys a simple echo server, sourced from the official Istio documentation.


We then define the following **Gateway** :


```text
apiVersion: networking.istio.io/v1  kind: Gateway  metadata:    name: istiogw    namespace: istio-system  spec:    selector:      istio: ingressgateway    servers:    - port:        number: 80        name: http        protocol: HTTP      hosts:      - "httpbin.example.com"
```


And this corresponding **Virtual Service** :


```text
apiVersion: networking.istio.io/v1  kind: VirtualService  metadata:    name: httpbin    namespace: default  spec:    hosts:    - "httpbin.example.com"    gateways:    - istio-system/istiogw    http:    - match:      - uri:          prefix: /status      - uri:          prefix: /delay      route:      - destination:          port:            number: 8000          host: httpbin
```


Nothing complex here, but take note: we’re using a dummy DNS (httpbin.example.com). You’ll need to simulate that when testing.


The app-setup.sh script fetches the external IP and port of the Istio ingress gateway. For example, if your gateway IP is 172.18.0.3 and the port is 80, you can test it with:


curl -s -I -HHost:httpbin.example.com “http://$INGRESS_HOST:$INGRESS_PORT/status/200"


### Basic Rate Limit


We will start by installing the rate limit service. for this to work we need Redis service to store the rate limits value, and some EnvoyFilters.


> Envoy is the data plane used by Istio, providing high-performance proxying for service-to-service communication.


EnvoyFilter is a powerful Istio custom resource that allows fine-grained customization of the Envoy proxy configuration. It lets users insert or modify Envoy configuration at different stages of the proxy lifecycle.


head to the 2-ratelimit-basic folder and apply its content.
we will now review what we you have applied:


- A Redis deployment and its service
- The Envoy Filters (see below the explanation)
- The configuration which tells Istio what the configuration for the limit itself are (how many requests per minute and on which route)


**Why Two Envoy Filters?**
We need two EnvoyFilter resources because each has a distinct role. The first (filter-ratelimit) connects and enables the rate limit service in Envoy. The second (filter-ratelimit-svc) defines what traffic to limit using descriptors like STATUS, which can be any custom key.


> *STATUS* serves here is a keyword for the match criteria. we could use any name we want


and finally let's examine the configuration for the rate limit which is being deployed as config map:


```text
apiVersion: v1  kind: ConfigMap  metadata:    name: ratelimit-config    namespace: istio-system    labels:      app: ratelimit  data:    config.yaml: |      domain: global      descriptors:        - key: STATUS          value: "/status/200"          rate_limit:            unit: minute            requests_per_unit: 1        - key: STATUS          rate_limit:            unit: minute            requests_per_unit: 4
```


notice the domain definition (domain:global)field here which also appears on the EnvoyFilters file. we will get back to domains in the next section.


another interesting point to look at the envoy Filters is this definition


```text
enable_x_ratelimit_headers: DRAFT_VERSION_03
```


This enables Envoy to inject X-RateLimit-* headers into responses, which inform clients about their usage limits and remaining quota. These headers include:


- X-RateLimit-Limit: max allowed requests
- X-RateLimit-Remaining: requests left in the current window
- X-RateLimit-Reset: time until limit resets


This is helpful for clients to implement retries or backoff strategies.


**It's time for test drive.**


Photo by[Koushik Pal](https://unsplash.com/@koushikpal?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/aerial-photography-of-vehicles-yBhOcUr4TVY?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)


**Test drive**


Now that everything is set up, let’s test the rate limit.


We’ll send repeated requests to our HTTP demo server and observe the response on the 5th request — since we’ve allowed only 4 requests per minute. Use the following commands to retrieve your host IP and port, and then simulate the call with a custom Host header:


```text
export INGRESS_NAME=istio-ingressgateway  export INGRESS_NS=istio-system
```


```text
export INGRESS_HOST=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
```


```text
export INGRESS_PORT=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
```


```text
curl -s -I -HHost:httpbin.example.com "http://$INGRESS_HOST:$INGRESS_PORT/status/200"
```


non-rate limiting. yet


You’ll notice the x-ratelimit-remaining header — it tells us how many requests we have left. After 4 requests, the 5th is blocked, and we’ll need to wait about 51 seconds for the limit to reset.


**Summary**


In Part 2, we’ll explore a more advanced scenario, applying multiple rules and fine-tuning their behavior.


Don’t hesitate to check it out!


---


[Istio rate limit - Hands on Lab Part 1](https://medium.com/kaltura-tech/istio-rate-limit-hands-on-lab-part-1-4dccb9885e64) was originally published in[Kaltura Technology](https://medium.com/kaltura-tech) on Medium, where people are continuing the conversation by highlighting and responding to this story.
