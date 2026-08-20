---
schema_version: "1.0.0"
document_id: "728bce05816ca67e1efa6f88e937024cdb81968c85b851720c2791b6a96f8b65"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/introducing-apoxy-services"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T17:04:35.618820+00:00"
fetched_at: "2026-07-31T17:04:37.717754+00:00"
content_hash: "sha256:89cf1494b3a1964b69dc3e86ae37d1e6b533c6a0901a84f7a6b444794f671941"
---

# Introducing Apoxy Services

Today we're launching Apoxy Services, with workerd as the first runtime. Write a fetch handler against the workerd APIs, ship it to your Apoxy proxies or to the Cloudflare Workers® platform.


API authentication handlers, response shaping, geo-aware logic, an experiment splitter, a thin shim that joins two services — anything you'd normally hand to a small backend service can run as an Apoxy Service instead. No new process to babysit, no separate cluster, no extra hop for your traffic to take.


## Why we built this


There's a class of routing and policy decisions that belongs in front of your backend services. The conventional answer is to keep adding to the gateway's config language until it can express what you need.


We prefer a different path. Apoxy Services puts a battle tested runtime in the gateway, so you can write code instead of config. It can handle the obvious gateway work: auth, rewrites, routing shaped by your tenancy model. But it doesn't stop there. You can ship a complete backend service with nothing else to run.


## Write a fetch handler


An Apoxy Service is just a standard` fetch` handler:


$


terminal


TS


` export


default


{


async


fetch


(request


:


Request


)


:


Promise


<


Response


> {


const


url


=


new


URL


(request.url);


if


(url.pathname


===


"/health"


) {


return


Response.


json


({ status:


"ok"


});


}


const


upstream


=


await


fetch


(


"https://api.example.com/data"


);


return


Response.


json


(


await


upstream.


json


());


}


};


`


` fetch()` ,` Request` ,` Response` ,` Headers` are all the same web APIs you'd write against in a browser or any web-standards runtime. Nothing proprietary, nothing you'll have to rewrite if you want to move it.


## One command to deploy


If you wrote a` fetch` handler, you've already done the hard part. Now just deploy it — no registry to set up, no service.yaml to write first:


$


terminal


SH


` $


apoxy


deploy


--yes


Deploying


to


project


"acme"


(production)


✓


name


swift-hopper


(generated;


override


with


--name


)


✓


service.yaml


wrote


service.yaml


✓


build


1


module


(


s


)


→


.apoxy/build


✓


push


registry.apoxy.dev/


<


project-i


d


>


/swift-hopper@sha256:c9d2…


✓


apply


Service/swift-hopper


(digest


sha256:c9d2…


)


`


` apoxy deploy` builds your code into an OCI artifact and pushes it to your project's hosted private registry. On a first deploy it writes a minimal` service.yaml` for you, pins the pushed bundle digest into it, and applies the Service. The resulting artifact is immutable from the moment the build finishes.


Bringing your own registry still works: set` spec.source.oci.repo` in` service.yaml` , or pass` --repo` , and` apoxy deploy` pushes there instead.


## Wire it to a route


An Apoxy Service is simply another backend that any HTTPRoute can point at it:


$


terminal


YAML


` apiVersion


:


compute.apoxy.dev/v1alpha1


kind


:


Service


metadata


:


name


:


hello-world


spec


:


source


:


oci


:


repo


:


docker.io/yourorg/hello-world


digest


:


sha256:c9d2…


# pinned by \`apoxy deploy\`


---


apiVersion


:


gateway.apoxy.dev/v1


kind


:


HTTPRoute


metadata


:


name


:


api


spec


:


parentRefs


:


-


name


:


default


kind


:


Gateway


rules


:


-


matches


:


-


path


:


type


:


PathPrefix


value


:


/api/


backendRefs


:


-


group


:


compute.apoxy.dev


kind


:


Service


name


:


hello-world


`


## Run anywhere your gateway runs


Most platforms that let you deploy edge logic put it on their infrastructure. Your function runs on their POPs, billed at their rate, or it doesn't run easily.


Apoxy Services run on the Apoxy platform, where ever you deploy it. If your traffic is on our self-serve cloud, deploying a service will run it on our managed edge. To run a service inside your own cluster, you can deploy the gateway in your own cluster — which means either[self-hosting using our open-source repo](https://github.com/apoxy-dev/apoxy) , orworking with us to deploy and operate it in your VPC or colo with our team on-call 24/7.


## What's inside


Apoxy Services is powered by[workerd](https://github.com/cloudflare/workerd) , the open-source JavaScript and Wasm runtime behind the Cloudflare Workers® platform. workerd is Apache 2.0 and we run it as-is. Each of your Services runs as its own V8 isolate inside a workerd process dedicated to your project, wrapped in our[gVisor](https://gvisor.dev/) sandbox.


We package workerd. We don't extend it, fork it, or rebrand its APIs. This means that upstream improvements ship to you when they ship to workerd with the same date-based compatibility guarantees.


## What it doesn't do yet


Apoxy Services is in a public beta as of today's v0.21.0 release. There's a few things you need to know before pointing your production traffic at it:


-


**Tunnel upstreams are coming soon.** We know that being able to dynamically route to upstreams connected via Tunnels is the dream, so we're working on it!


-


**No durable storage yet.** You can call out to your databases and other services over` fetch` , but we do not have any built-in support for key-value stores or queues at this time. Secrets — API keys and other credentials — are supported today; see[Secrets](https://apoxy.dev/docs/guides/deploying-compute-services#secrets) .


-


**Adjustable limits.** We've limited execution time to 25ms for now, but if you need more time please let us know.


-


**Logs are basic.** You'll get a simple per-request log today. Structured tracing across service → upstream and detailed per-execution logs are coming soon.


If any of those are something you need right now then let us know and we'll try to get you early access.


## Get started


Apoxy Services is available today on our self-serve plans. Check out the[quickstart](https://apoxy.dev/docs/guides/deploying-compute-services) to get your first service deployed in about ten minutes.


[Try it out for free](https://apoxy.dev/) to start. No sales call required.


---


*Cloudflare, the Cloudflare logo, and Cloudflare Workers are trademarks and/or registered trademarks of Cloudflare, Inc. in the United States and other jurisdictions. Apoxy is not affiliated with, endorsed by, or sponsored by Cloudflare, Inc. workerd is open-source software licensed under the Apache License 2.0.*


Matt Ward


Co-founder & CEO


Matt is co-founder and CEO of Apoxy. He previously scaled infrastructure for YouTube and Mux. Outside of work, you can find him skiing down or pedaling up a mountain if he's not at home trying to be the kind of husband and father his family deserves.


[GitHub ↗](https://github.com/therealwardo)[X ↗](https://x.com/therealwardo)


[← Back to all posts](https://apoxy.dev/blog)
