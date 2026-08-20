---
schema_version: "1.0.0"
document_id: "27240d76103d4b0385176e2990d7004c5d87b002c823e745f13ce304a4a7ff7e"
company_key: "whitefiber-inc-ordinary-shares"
company: "WhiteFiber Inc."
source_id: "whitefiber-inc-ordinary-shares-news-import-44ec15aad2d1"
canonical_url: "https://www.whitefiber.com/blog/how-we-build-software"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T13:51:15.419840+00:00"
fetched_at: "2026-08-16T13:51:18.150276+00:00"
content_hash: "sha256:66157a60a7d8d6d23ccf930fbe3327986068a6e446b6b717d7a75bd0a6742f5c"
---

# How We Build Software at WhiteFiber

We are, as the tagline states, The AI Infrastructure Company. But infrastructure without software is just expensive stuff in a fancy warehouse. While we talk a lot about the hardware we make available to customers. We don’t talk much about our software layer that provides orchestration capabilities through an API to simplify resource management. Nor have we talked much about how we approach software engineering and how this manifests as value for our customers. Today’s post dives into these topics and sheds light on our software development practices and philosophy.


‍


‍


## **IT STARTS WITH OPEN SOURCE**


‍


In our space, speed is everything. Many companies choose to build their orchestration stack from scratch. That can work but it’s slow or expensive because it has to be good. (Hot take: The ‘pick-two principle’ is a fallacy as ‘good’ should never be optional.) Instead, we’ve leveraged and customized hardened open-source tools to accelerate delivery.


‍


With a small team we delivered a fully functional orchestration API for bare metal in six months. That kind of speed would be impossible if we were reinventing every layer ourselves.


‍


We've built custom drivers, enhanced network automation, and tuned storage integrations; all without abandoning the open-source base that lets us move quickly and contribute back to the community.


‍


The ability to integrate best-in-class open tools means we can offer familiar, reliable abstractions across heterogeneous infrastructure: B200s, H200s, future GPU models. It also gives us a path to support orchestration frameworks like Kubernetes or Slurm-as-a-Service for specialized use cases without having to re-architect everything from scratch.


‍


‍


## **INFRASTUCTURE, ABSTRACTED**


‍


From day one we had a goal to make provisioning high-performance resources simple. Push a button, simple. Behind the scenes, that means abstracting a ton of complexity, from bare metal provisioning to network automation, without exposing users to any of it.


‍


> WhiteFiber’s orchestration layer is built to hide the "how" and deliver the "what": a simple, unified interface where customers can request and manage compute resources without needing to know what’s happening behind the curtain.


‍


We’ve built this using OpenStack and Ironic, tailoring it specifically for AI workloads. But crucially, we’ve done it in a way that’s modular and flexible, so we can swap out components in the future if something better comes along. To customers, it doesn’t matter if we’re using OpenStack, something else, or a stack we haven’t invented yet. The interface remains simple, familiar, and fast.


‍


‍


## **RETHINKING RECOVERY**


‍


One of the most impactful features we’ve built is volume-based booting via iSCSI as part of our API based orchestration layer. If a machine fails, the customer doesn’t lose their data. We can simply move the root volume to another node and bring it back online in minutes.


‍


> This approach eliminates the need to physically move drives between machines or reimage entire systems. We've eliminated that burden with smart software architecture, and the benefits to uptime, customer experience, and operational efficiency are huge.


‍


‍


## **COMMUNITY COMMITMENT**


‍


We see OSS as an accelerator and an ecosystem we intend to continue investing in.


‍


We’re actively engaging with the OpenStack community, writing plugins, and participating in IRC channels. Our plan isn’t just to consume open source. We plan to contribute meaningfully by pushing work up-stream, helping move the entire ecosystem forward. That’s a win for the community, for our customers, and the industry at large.


‍


‍


## **THE WHITEFIBER PHILOSOPHY**


‍


We're opinionated about performance. But we’re not precious about building everything ourselves. We believe:


‍


From flexible instance types to a unified API for both bare metal and VMs, from deep storage integrations to customer-first observability, our software exists to remove friction and unlock speed, scale, and control for the AI teams building the future.


‍


‍


‍


## Conclusion


We’re just getting started. But even now, the way we’ve chosen to build says everything about who we are: practical, performance-obsessed, and radically open.


If you're building complex AI workloads and want infrastructure that works as fast as you do, WhiteFiber is for you.
