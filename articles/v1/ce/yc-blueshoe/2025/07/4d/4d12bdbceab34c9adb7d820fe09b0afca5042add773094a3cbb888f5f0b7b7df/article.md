---
schema_version: "1.0.0"
document_id: "4d12bdbceab34c9adb7d820fe09b0afca5042add773094a3cbb888f5f0b7b7df"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/varnish-website-performance-with-kubernetes/"
published_at: "2025-07-28T10:00:00+00:00"
first_seen_at: "2026-07-24T11:21:12.712803+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d7bc41cbec8c56842b20fb810404204cd98c0c77a1fd7ae138d65fc1c5b8ebba"
---

# Varnish - The Secret Hero of Website Performance with Kubernetes | BLUESHOE

Intermediate


## Table of Contents


- Why Varnish Transforms Your Website
- Let's Get Started: What You'll Need
- Advanced VCL techniques for practical use
- Ingress Configuration for Domain Routing
- Monitoring and Debugging
- Best Practices for Optimal Caching
- Varnish in comparison: Choosing the right caching strategy
- Frequently asked questions (FAQ)
- Conclusion


---


## Meet the Author


[Jonathan Kölbl](https://www.blueshoe.io/team/jonathan-koelbl/) Yes, Varnish Cache is a software with fascinating features and is used worldwide by large websites.


Last updated:


2025-10-08


---


Kubernetes


Varnish


Development


Operations


Performance


---


2025-07-28


# Lightning-Fast Cache for Your Kubernetes Application


Varnish - The Secret Hero of Website Performance with Kubernetes


In modern web applications, performance isn't just a feature - it's essential. Users expect fast, responsive experiences, and slow load times can lead to high bounce rates and lost engagement. This is where Varnish Cache comes in. By placing it in front of your web application, you can serve cached content at lightning speed, dramatically reducing the load on your backend. This article guides you through setting up Varnish in a Kubernetes environment to make your website significantly faster.


## Prerequisites


You should have the following knowledge to use the article optimally:


- Kubernetes Basics
- Docker Basics
- [HTTP Caching](https://en.wikipedia.org/wiki/Web_cache) Basics
- [Varnish HTTP Cache](https://varnish-cache.org/)


If you have any questions or if anything is unclear, you can use the comment function below the article.


## Why Varnish Transforms Your Website


Performance is everything on the web. When your users have to wait, they bounce. Varnish elegantly solves this problem: As an HTTP cache, it sits in front of your application and delivers content lightning-fast from storage. The result? Up to 90% less load on your backend and significantly faster loading times.


## Let's Get Started: What You'll Need


Before we begin, here are the four files you'll need for a simple setup:


- ` default.vcl` : Your Varnish configuration
- ` Dockerfile` : To build your Varnish image including VCL
- ` varnish.yaml` : Deployment and Service for Kubernetes
- ` Ingress.yaml` : Optional, to make your website accessible via a domain name


### Understanding and Adapting the VCL File


Let's first look at the individual parts of the` default.vcl` in detail:


```text
backend default {
.host = "website-svc";
.port = "3001";
.first_byte_timeout = 300s;
}


```


The first block defines your backend.` website-svc` is the name of the Kubernetes service that Varnish will access. The port` 3001` is the port of your application (e.g., Django) which is accessible within the cluster. With` first_byte_timeout` you set how long Varnish waits for the first response from the backend.


```text
sub vcl_recv {
if (req.method == "GET" && req.http.Cookie !~ "sessionid") {
unset req.http.Cookie;
}
}


```


` vcl_recv` is called for every incoming request. Here you decide whether cookies are removed. In this case, Varnish removes all cookies from GET requests that do not contain a` sessionid` . This makes caching more effective because different cookie values would otherwise create different cache objects.


```text
sub vcl_backend_response {
if (bereq.url ~ "^/de-de/aktuelles/.*") {
set beresp.ttl = 2h;
} else {
set beresp.ttl = 12h;
}
}


```


` vcl_backend_response` is called when Varnish has queried the backend and received a response. Here you define how long content should be cached. News pages get 2 hours, everything else remains in the cache for 12 hours.


## Advanced VCL techniques for practical use


**Grace Mode & Saint Mode (bridging backend failure):**
With Grace and Saint Mode, you can deliver outdated content when the backend is unavailable. This greatly increases reliability.


```text
sub vcl_backend_fetch {
if (beresp.status == 500 || beresp.status == 503) {
set beresp.saintmode = 5m; // Mark backend as “down” for 5 minutes
return (abandon);
}
set beresp.grace = 2h; // Allow this content to be served 2 hours beyond its TTL
}


```


**Adjust cache keys:**
Manipulate the cache key to ignore unimportant query parameters, for example, and thus increase the hit rate.


```text
sub vcl_hash {
// Only consider 'id' and 'lang' for the cache key
if (req.url ~ "\?") {
set req.hash += regsuball(req.url, '^.*\?((?:id|lang)=[^&]+).*$', "\1");
}
// ...
}


```


**Secure purging via API:** Set up a secure HTTP endpoint for targeted cache purging.


```text
acl purge {
"localhost";
"192.168.1.0"/24;   // Only allow purging from this network
}
sub vcl_recv {
if (req.method == "PURGE") {
if (!client.ip ~ purge) {
return (synth(405, "Not allowed."));
}
return (hash);
}
}


```


### Dockerfile: Embedding VCL into the Image


```text
FROM   varnish:stable
COPY   default.vcl /etc/varnish/


```


This ensures that your VCL is directly included in the container.


Next. You can attach your image with:


```text
docker   build   -t   varnish:latest   .


```


### Deployment and Service Definition


The Deployment ensures that your Varnish Pod runs permanently and Kubernetes automatically restarts it when it crashes. In our example, only one instance runs (replicas: 1), but for production environments you can also enter multiple replicas.


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   varnish
spec  :
replicas  :   1
template  :
metadata  :
labels  :
app  :   varnish
spec  :
containers  :
-   name  :   varnish
image  :   varnish:latest
ports  :
-   containerPort  :   80
---
apiVersion  :   v1
kind  :   Service
metadata  :
name  :   varnish
spec  :
selector  :
app  :   varnish
ports  :
-   port  :   80
targetPort  :   80


```


The associated Service makes your Pod accessible in the cluster. It connects the Ingress or other Pods with your Varnish container. It's important that targetPort and containerPort match.


` Super!` Now your Pod is reachable in the cluster.


## Ingress Configuration for Domain Routing


The Ingress is the last link in the chain. It ensures that requests from the internet reach your cluster and are forwarded to the correct service, in this case your Varnish. This allows you to centrally manage domains and TLS certificates.


A simple example looks like this:


```text
apiVersion  :   networking.k8s.io/v1
kind  :   Ingress
metadata  :
name  :   varnish-ingress
annotations  :
cert-manager.io/cluster-issuer  :   "letsencrypt-prod"
spec  :
ingressClassName  :   nginx
rules  :
-   host  :   "example.com"
http  :
paths  :
-   path  :   /
pathType  :   Prefix
backend  :
service  :
name  :   varnish
port  :
number  :   80


```


Pay attention that the service name and port exactly match your previously defined service. This way, all web traffic is processed through Varnish and your application benefits from caching.


## Monitoring and Debugging


Here are a few practical commands to monitor your cache and Varnish server:


```text
varnishstat


```


Shows live cache statistics, for example hits and misses.


```text
varnishlog


```


Shows exactly why requests were cached or not.


```text
varnishadm   ban   "req.url ~ .*"


```


This clears the cache and forces new content.


```text
varnishadm   ping


```


Checks if your Varnish admin is reachable.


```text
varnishadm   status


```


Shows the current status of your Varnish process.


## Best Practices for Optimal Caching


### 1. Define Cache Strategies


- Cache static content longer (CSS, JS, images)
- Cache dynamic content shorter
- Handle session-based content individually


### 2. Set Up Performance Monitoring


- Regularly check cache hit rates
- Monitor storage utilization
- Monitor backend health


### 3. Plan Cache Invalidation


- Automated processes for content updates
- Targeted invalidation instead of complete clearing
- Health checks for backend servers


## Varnish in comparison: Choosing the right caching strategy


Varnish is extremely powerful, but it is not always the only solution. How does it compare to other caching mechanisms?


Caching solution Strengths Weaknesses Ideal for...


**Varnish Cache** **Maximum flexibility** thanks to VCL; caching of entire HTTP objects; features such as Grace/Saint Mode. **Greater complexity** than simple caches; no native TLS support (requires a proxy in front of it). ...complex websites (e.g., e-commerce, news portals) with dynamic content that require fine-grained cache control.


**Nginx Caching** **Simple configuration** ; integrated directly into the web server; very high performance; can terminate TLS. **Less flexible** cache logic than Varnish; primarily for static assets and simple responses. ...simpler websites and applications where an uncomplicated cache for static files and API responses is sufficient.


**CDN (e.g., Cloudflare)** **Globally distributed** (low latency worldwide); protection against DDoS attacks; easy setup. **More expensive** ; less control over cache invalidation; "black box." ...globally operating websites that want to benefit from a distributed cache and additional security features.


## Frequently asked questions (FAQ)


### How can I check whether a page is being loaded from the Varnish cache?


Check the HTTP headers of the response in your browser's developer tools. Varnish typically adds headers such as` X-Varnish` (internal transaction ID) and` Age` (how long the object has been in the cache). An` Age` value greater than 0 is a sure sign of a cache hit.


### Why is my cache hit rate low?


The most common reasons are:


1. **Cookies:** By default, Varnish does not cache requests with cookies. Adjust your` vcl_recv` to remove unnecessary cookies.
2. **` Set-Cookie` header from the backend:** If your backend sends a` Set-Cookie` header, the response will not be cached.
3. **` Cache-Control` header:** Watch out for` Cache-Control: private` or` max-age=0` from the backend.


### How do I protect the Varnish admin port in Kubernetes?


The admin port should never be accessible externally. Make sure that your Kubernetes service for Varnish only opens the HTTP port (e.g., 80). Access to` varnishadm` should only be via` kubectl exec` in the pod.


### How much RAM or memory do I need for Varnish?


That depends heavily on your usage profile. A RAM configuration between 1 GB and 16 GB is recommended, combined with SSD storage.


### Can Varnish cache without query parameters?


Yes, you can remove query parameters (e.g., using RegEx in VCL) to cache only one object:


```text
sub vcl_recv {
set req.url = regsub(req.url, "\?.*", "");
}


```


### How can custom error pages be configured?


Using` vcl_error` , you can create your own HTML pages with a synthetic response. Example:


```text
sub vcl_error {
set obj.http.Content-Type = "text/html; charset=utf-8";
synthetic {"…"};
deliver;
}


```


## Conclusion


Now you're completely done. Your setup from` default.vcl` to Ingress is complete, and Varnish now ensures your site loads significantly faster. With the right configuration, Varnish becomes the secret hero of your website performance.


[Learn more about Varnish](https://varnish-cache.org/)


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development Develop locally, test in the cluster - without CI/CD wait times. Gefyra promises the holy grail of Kubernetes development: full-speed coding with real cluster context. In our case study, you’ll learn why this tool is a game-changer for your daily operations and how to efficiently integrate it into your workflow by Carl-Christian Neundorf](https://www.blueshoe.io/blog/gefyra-hands-on/)


[django-o365: A Modern Mail Backend for Django and Exchange Online](https://www.blueshoe.io/blog/django-o365-a-modern-mail-backend-for-django-and-exchange-online/)


[django-o365](https://github.com/Blueshoe/django-o365) is an open-source mail backend for Django that enables sending emails via Exchange Online using OAuth 2.0. Born from real-world project requirements, this package is aimed at teams using Microsoft 365 who want to implement their email dispatch securely, maintainably, and without workarounds. This article provides an overview of the motivation, features, and usage.


by Robert Gutschale


[Mastering Django Caching: The Ultimate Guide from Cachalot to the Low-Level API Is your Django application slower than it should be? Long loading times not only frustrate your users but are also penalized by search engines. The solution often lies in a smart caching strategy. But where do you start? by Jonathan Kölbl](https://www.blueshoe.io/blog/django-caching-cachalot-performance-guide/)


[Handoff from Designer to Developer: What We Need for Frontend Implementation The transition from design to development is a critical moment in any frontend project. A well-thought-out handoff can save weeks of development time, while an incomplete handoff leads to endless questions, delays, and compromises. by Lukas Rüsche](https://www.blueshoe.io/blog/designer-to-developer-handoff/)


[Why Our Maintenance Reports Are a Key to Successful Collaboration For us, a maintenance report is more than a table of numbers: it is a tool for communication, planning, and trust. It shows what we have done, where potential lies, and what topics are on the agenda for the next quarter - so our customers know their systems are in good hands. by Robert Gutschale](https://www.blueshoe.io/blog/quarterly-maintenance-reports/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
