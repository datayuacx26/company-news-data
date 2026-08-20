---
schema_version: "1.0.0"
document_id: "1e4f3d691c2cb738fad1788f735cc1675c79ef5c6ec89185d3894506f2a43d8f"
company_key: "yc-motion"
company: "Motion"
source_id: "yc-motion-rss-f628c3046538"
canonical_url: "https://engineering.usemotion.com/replacing-clickops-with-pulumi-d21f3e80b851"
published_at: "2025-06-04T00:51:42+00:00"
first_seen_at: "2026-07-20T23:21:05.244128+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:6b096cfd749f241ecefa33ecede22e46cba5f9c55f2c7a53f1d0867ae9df64e3"
---

# Replacing ClickOps with Pulumi

# Replacing ClickOps with Pulumi


[Sebastian Kowalczyk](https://medium.com/@sebkow?source=post_page---byline--d21f3e80b851---------------------------------------)


7 min read


·


Jun 4, 2025


--


Google would never[accidentally delete your entire GCP project](https://www.unisuper.com.au/about-us/media-centre/2024/a-joint-statement-from-unisuper-and-google-cloud) … right? But if they *did* hypothetically do that, how long would it take you to recover? Rare as it is, sometimes[entire regions go down](https://www.theregister.com/2023/05/10/google_cloud_paris_outage_persists/) . In such a case, could you forklift your entire business to a different region or cloud provider? If so, how long would that take? For years, Motion’s answer to these questions was effectively maybe *never* !


Like most startups, all our infrastructure at Motion was set up via “clickops” — developers would just click around the GCP dashboard and set up whatever services they needed. And that worked! For a while at least. But eventually, devs forget *exactly* where they clicked or in what order, and soon Motion’s infrastructure became a tangled mess that nobody really knew how to reproduce. Even worse, people became afraid to touch *anything* lest they break it.


Almost everyone in platform engineering recognizes clickops as an anti-pattern, but few have successfully managed a largescale transformation from the ball of tangled yarn to an organized IAC approach. Roughly one year ago, I joined Motion to help untangle this six year-old mess. And after a decade of wrangling Terraform, I felt it was time to give Pulumi a shot. If you’ve ever tried to keep Terraform configs in sync, you know the struggle: learning HCL, juggling modules, managing environment-specific quirks, dodging provider-breaking changes, and taming Kubernetes chaos.


Press enter or click to view image in full size


From chaos to clarity: bridging the gap from ClickOps to Infrastructure as Code


Why bother with proprietary formats and languages when we could just use a full fledged language like Typescript, which we already used elsewhere in our stack? For our small team, moving from the backend to the frontend to infrastructure without any context switching has been a huge win.


## Lift and Shift?


The “easiest” option is often to just translate *exactly* what exists in the AWS or GCP console to HCL or Typescript and call it a day. And while this has the benefit of codifying the ball of yarn, it does not actually de-tangle it. If your IP ranges are clashing, who’s to say you *could* even successfully deploy in a new region anyway? As an all-encompassing AI powered suite, we knew that businesses run their most critical day to day operations on Motion. Uptime is a necessity, so we needed our infrastructure to feel as polished as our application code and not a second class citizen.


We typically don’t operate this way, but infrastructure is *so* foundational and crucial to modern SAAS companies that we felt it was worth taking time to do it *right* . Hitting reset gave us the chance to rethink our architecture, clean up tech debt, and set a better foundation for the future. It let us leverage Pulumi’s full power, like proper code reuse, better testing, and cleaner abstractions, without dragging along years of technical debt.


> If you wish to make an apple pie from scratch, you must first invent the universe.


-Carl Sagan


(The apple pie was worth it!)


## Laying the Foundation


When starting one common temptation is to go for a single workspace or stack for all your infrastructure. While this approach might seem simpler at first, it can quickly lead to a series of problems as your infrastructure grows. Instead we chose to break down our infrastructure into common patterns.


- **Creating a Baseline:** Some resources, e.g., VPCs or IAM roles, are foundational and often shared across multiple stacks. By separating these common patterns into their own dedicated stacks, you avoid tightly coupling them to a single project’s lifecycle, ensuring they remain available even if one project is sunset while others still depend on them.
- **Blast Radius Impact:** With a single stack, the blast radius of a mistake or issue becomes significantly larger. If something goes wrong (e.g., a misconfiguration, failed resource deployment, or an accidental change to a shared resource), the impact can ripple through the entire infrastructure. This could cause downtime or disruptions across multiple services.
- **Increasing Velocity:** Move fast without breaking things. Smaller stacks let developers make changes without stepping on each other’s toes, reducing bottlenecks and freeing up teams to push updates without fear of disrupting the entire system. Infrastructure that changes frequently also benefits from being smaller, making it quicker and safer to apply updates without impacting unrelated resources.
- **Slow Deployments:** As your infrastructure grows, having everything in one stack can cause deployments to slow down considerably. A small change in one area could trigger a full redeployment, which could be time-consuming and inefficient.


By using smaller, modular stacks, you can limit the impact of any failure to a specific module or environment, making it easier to isolate, troubleshoot, reduce bottlenecks and fix issues without affecting unrelated parts of your infrastructure. This approach also enables better management of changes, faster deployments, velocity and easier scaling. With a more granular setup, you minimize conflicts, maintain a cleaner separation between components, and can scale your infrastructure more efficiently, all while improving overall stability and flexibility.


## Get Sebastian Kowalczyk’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


And it all starts with a well-structured repo. We mapped out all our existing infrastructure, first on a whiteboard and then in a more detailed document. This helped us visualize common patterns, identify shared resources, and understand how different resources relate to each other. We first broke things down by provider, then organizing resources into logical sections like networking, compute, IAM, and specific services e.g., ECR or GKE. This not only supports modular stacks but also makes it way easier to collaborate and keep your infra neat and tidy as you scale.


Here’s a simplified version of how our infrastructure repo is structured:


```text
pulumi/  ├── argo/  │   └── argocd/  │       ├── config.ts  │       ├── github-secret.ts  │       ├── helm-install.ts  │       ├── index.ts  │       ├── namespace.ts  │       ├── Pulumi.mgmt.yaml  │       └── Pulumi.yaml  ├── dragonfly/  │   ├── config.ts  │   ├── datastore.ts  │   ├── index.ts  │   ├── migration/  │   │   ├── README.md  │   │   └── riot.yaml  │   ├── package.json  │   ├── Pulumi.dev.yaml  │   ├── Pulumi.prod.yaml  │   ├── Pulumi.yaml  │   ├── sdks/  │   │   └── dfcloud/  │   ├── tsconfig.json  │   ├── vpc-peering.ts  │   └── vpc.ts  ├── gcp/  │   ├── gke/  │   │   ├── buckets.ts  │   │   ├── config.ts  │   │   ├── gke.ts  │   │   ├── index.ts  │   │   ├── k8-provider.ts  │   │   ├── kubeconfig.ts  │   │   ├── node-pool.ts  │   │   ├── Pulumi.dev.yaml  │   │   ├── Pulumi.mgmt.yaml  │   │   ├── Pulumi.prod.yaml  │   │   └── Pulumi.yaml  │   ├── iam/  │   │   ├── config.ts  │   │   ├── index.ts  │   │   ├── Pulumi.dev.yaml  │   │   ├── Pulumi.mgmt.yaml  │   │   ├── Pulumi.prod.yaml  │   │   ├── Pulumi.yaml  │   │   └── service-account.ts  │   ├── networking/  │   │   ├── config.ts  │   │   ├── index.ts  │   │   ├── nat.ts  │   │   ├── Pulumi.dev.yaml  │   │   ├── Pulumi.mgmt.yaml  │   │   ├── Pulumi.prod.yaml  │   │   ├── Pulumi.yaml  │   │   ├── subnets.ts  │   │   ├── vpc.ts  │   │   ├── vpc-peering.ts  │   │   └── vpn.ts  │   ├── project/  │   │   ├── config.ts  │   │   ├── index.ts  │   │   ├── project.ts  │   │   ├── Pulumi.dev.yaml  │   │   ├── Pulumi.mgmt.yaml  │   │   ├── Pulumi.prod.yaml  │   │   └── Pulumi.yaml  │   ├──  .../  ├── .../  ├── package.json  ├── README.md  └── tsconfig.js
```


This structure organizes your infrastructure by provider and resource types. Each service is encapsulated within its own directory, with separate configuration files and stack-specific YAML files for different environments.


## Putting it all Together


Now that the pre-requisites of modular stacks, organized repository structure, and code have been satisfied, we could finally tie it all together.


- **Stack Wiring and Dependencies:** We used[Pulumi stack references](https://www.pulumi.com/tutorials/building-with-pulumi/stack-references/) to link our various resources resources. In our case, the GKE stack needed to pull in subnet details from our networking stack and IAM roles from our identity stack. This keeps your infrastructure decoupled but still tightly integrated, reducing the blast radius if something goes sideways.
- **Environment-Specific Configs:** We kept our environments tidy by leaning on` Pulumi.dev.yaml` ,` Pulumi.prod.yaml` , and other stack-specific config files. This kept our codebase clean and made it clear what is environment-specific to easily avoid the temptation of jamming everything into a single stack.
- **Reusable functions and Shared Code:** The next core goal was reusability. We extracted common patterns, like network setups or IAM policies, into shared modules or functions which let us deploy new stacks extremely quickly, reduced duplication, and also made it more readable.
- **Modular Stacks for Flexibility:** With all of the above, we finally had smaller, more focused stacks with the power and flexibility to scale, update, and troubleshoot one part of the infrastructure without risking the whole system.


## “Never” to Thirty Minutes


At Motion, we’ve transformed our infrastructure into something that’s not only efficient but also scalable, repeatable and maintainable. If we ever needed to swap all of Motion to a new region, we could now do this in less than 1 hour in case the unthinkable should ever happen. It’s all automated and human-proof.


Thanks to Pulumi and our embrace of Typescript, we’ve built a system that allows *any* developer to jump into the codebase and understand what’s going on without needing a crash course in HCL or a deep dive into YAML sprawl.


With this setup, it takes just about 30 minutes plus time for DNS propagation to spin up our entire cloud infrastructure, from the cloud account to our application running in Kubernetes. That 30-minute timeline is largely thanks to how we’ve structured everything for speed and flexibility. However, it’s worth noting that roughly half of that time is just waiting for the Kubernetes cluster to be fully up and running. Despite that wait, everything else is already in motion and ready to go, with no bottlenecks elsewhere.


Press enter or click to view image in full size


Spinning up all of Motion in under ten minutes


This migration has given us faster development cycles, better control over resources, and a more resilient infrastructure. And while the road to this point wasn’t always straightforward, by starting from scratch and embracing Pulumi, we’ve laid a foundation that lets us evolve our infrastructure with ease, scale efficiently, and adapt quickly to whatever comes next. With this setup, we’re no longer just keeping the lights on. We’re building a platform that can grow with us while keeping things fast, flexible, and future proof.
