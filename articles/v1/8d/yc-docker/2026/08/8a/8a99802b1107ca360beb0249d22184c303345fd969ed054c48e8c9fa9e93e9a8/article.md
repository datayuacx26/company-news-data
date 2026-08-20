---
schema_version: "1.0.0"
document_id: "8a99802b1107ca360beb0249d22184c303345fd969ed054c48e8c9fa9e93e9a8"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/make-zero-cves-your-new-default/"
published_at: "2026-08-17T13:00:00+00:00"
first_seen_at: "2026-08-17T14:33:52.793084+00:00"
fetched_at: "2026-08-17T14:33:54.003924+00:00"
content_hash: "sha256:ec15adad6720e0da103d5edbb7f2ca21329861584f7193d9b51cd694f75edefd"
---

# Make zero CVEs your new default

*Now in Docker AI Governance: a single searchable record of every policy decision your agents trigger, streamed to the SIEM your security team already runs, so you can show what your agents did and what your policy stopped.*


Today, Docker AI Governance now streams every policy decision in your organization into the SIEM your security team already runs, with a searchable record of all of it in Docker Cloud. You can see what your agents did, and what your policy stopped them from doing.


## Enforcement is step one


Somewhere in the past year, supply-chain attacks stopped being isolated incidents. The compromises now reach the tools the industry trusts to defend itself,[with Trivy and KICS among this year’s targets](https://www.docker.com/blog/trivy-kics-and-the-shape-of-supply-chain-attacks-so-far-in-2026/) . Mark Lechner, Docker’s Chief Information Security Officer,[called the latest wave “a permanent shift in the threat landscape”](https://www.docker.com/blog/defending-your-software-supply-chain-what-every-engineering-team-should-do-now/) , and nothing since has argued with him. Meanwhile the volume keeps climbing.[Over a quarter of production code is now AI-authored](https://www.docker.com/blog/docker-sandboxes-run-agents-in-yolo-mode-safely/) , and agents pull in dependencies at machine speed. If you run a platform team or a security program, you already know how this math feels. More code, more images, more dependencies, almost none of it written by your own engineers. And all of it becomes your responsibility the moment it ships.


None of this is news to us. Securing the software supply chain is the problem we’re here to solve, and our commitment to it is absolute. The latest round of updates widens the trusted foundation Docker is building under your supply chain, and tightens how it’s enforced. More of the software inside your images is now built and patched by Docker itself. Security coverage continues after software reaches end of life. Images get tailored to your environment without losing their guarantees. And policy enforcement now reaches every developer machine. The details are below. First, where all of this is headed.


## **A trusted foundation for the whole supply chain**


It all starts from one principle, and Docker Hardened Images was built on it. Security that doesn’t get adopted doesn’t secure anything. The entire catalog is free for every developer, because a secure baseline shouldn’t be a premium feature. Every image is compatible with Alpine and Debian, the distributions your teams already run, and Docker builds every one of them itself, from source. Adoption is a FROM-line change, not a migration project. And every image is independently verifiable, with signed SBOMs (software bills of materials) and SLSA Build Level 3 provenance, so your auditors work from evidence instead of vendor claims.


A year in,[the numbers make the case](https://www.docker.com/blog/why-we-chose-the-harder-path-docker-hardened-images-one-year-later/) . The[catalog](https://hub.docker.com/hardened-images/catalog) has grown past 4,000 hardened images, plus MCP servers, Helm charts, and ELS images. It draws more than 3.5 million pulls a week, with over a million builds running regularly to keep all of it patched, and open source projects like[n8n run production on DHI](https://www.docker.com/resources/how-n8n-uses-docker-hardened-images-webinar/) . The catalog grows the way it always has, driven by what customers request. But the goal was never just a catalog. The goal is one trusted foundation under your whole software supply chain, where the images you run, the packages inside them, the charts that deploy them, and the tools your agents call all carry the same provenance. Security becomes the default from day one, and it holds, without asking your teams to change how they work.


Docker is leading that charge. Here’s what that looks like in practice.


## Built from source, down to every package


The hardening keeps reaching deeper into the stack.[Docker Hardened System Packages](https://www.docker.com/blog/announcing-docker-hardened-system-packages/) take hardening below the image, to the packages inside it, across both Alpine and Debian, with every package built from upstream source, patched, and maintained by Docker in the same SLSA Build Level 3 pipeline that builds the images themselves. And the repository behind them is open to more than the catalog. DHI Enterprise customers can point apt or apk directly at Docker’s hardened package repository and bring the same packages into images they build themselves, extending the hardened supply chain beyond the images Docker ships to every image your organization builds.


The coverage keeps widening. What began with Alpine now spans Debian, with Python, the[catalog’s](https://hub.docker.com/hardened-images/catalog) most pulled image, among the first to ship fully hardened. The work compounds every week, and the[Debian](https://dhi.io/deb/debian/main/index.html) and[Alpine](https://dhi.io/apk/alpine/v3.24/main/index.html) package lists are public, so you can watch the catalog harden in real time.


If you’ve spent time chasing base-image CVEs, you know why this matters. System packages are notorious for slow fixes; a patch can sit waiting on the distribution’s next release for months or years. Docker doesn’t wait. We patch at the package level, ahead of upstream when it counts, and the fix lands in every image that uses that package, in one build wave instead of image by image. Entire businesses have been built on delivering community-distribution security updates faster than the community. With DHI, that speed is included.


The guarantees hold up under inspection, too. Packages you add through DHI customization, tailoring an image to your workloads, come from that same hardened repository, not an unverified public mirror, so they are hardened system packages in their own right and the SLA that covers the base image extends through everything you add. And because one vendor stands behind the image, the packages inside it, the CVE investigation, and the patch, your auditors get a single chain of signed provenance instead of a stack of vendor assurances.


Your distribution, meanwhile, stays your distribution. Building a hardened package ecosystem from source is a serious engineering commitment, and Docker made it twice, for Alpine and for Debian, so keeping your house standard never costs you your security posture.


## Patch past end of life


Production software has a habit of outliving its maintainers. Migrations wait on budgets, dependencies, and test cycles, and CVEs don’t wait with them. That’s the problem[DHI Extended Lifecycle Support (ELS)](https://www.docker.com/products/hardened-images/) exists for. It keeps end-of-life software patched, with SBOMs and provenance maintained, for up to five more years.


ELS isn’t limited to a set catalog, either. Docker watches the end-of-life calendar and builds coverage ahead of it, and anything you don’t see, you can request. MinIO is the newest addition. Upstream archived the project in February 2026, yet in the DHI catalog it lives on, patched and hardened, and your migration runs on your schedule instead of upstream’s.


## Customize at scale, manage as code


Nobody runs stock images in production. You add CA certificates, agents, and the packages your applications demand. The trouble is that in most of this market, the first change you make is where the vendor’s guarantees end, and everything after it is yours to carry.[DHI customization](https://docs.docker.com/dhi/how-to/customize/) works the other way around. You define what your images need, and Docker manages the full lifecycle of your customized images, rebuilding them through the same hardened pipeline on every upstream patch. The SBOM, the attestations, and the SLA travel with the customization instead of dying at it.


Customization operates at scale, too. Bulk customizations run through the UI, CLI, and API, with YAML configuration and GitHub Actions support, so you can tailor hundreds of repositories in one pass and let the rebuilds take care of themselves. And if your platform runs on Terraform, customization is code as well. The[DHI Terraform provider](https://registry.terraform.io/providers/docker-hardened-images/dhi/latest) mirrors and customizes hardened images with the same pull requests and reviews as the rest of your infrastructure.


The savings are real infrastructure, not a rounding error. Customers tell us they’ve shut off the CI pipelines that existed only to rebuild images, because Docker rebuilds for them. The blind redeploy cadence goes with those pipelines. You ship an update when a fix actually needs to go out, knowing exactly what changed, instead of rebuilding everything on a schedule and hoping QA catches what moved.


For organizations whose data-residency requirements keep images inside the EU, EU-hosted customizations arrive in September. Your customized images will live in Docker Hub’s EU region with the same SBOMs, attestations, and SLA as everywhere else. Residency stops being the reason your hardening program waits.


## Harden beyond base images


The same standard keeps moving up the stack. The catalog now carries fully supported Helm charts, so your Kubernetes deployments start hardened too. And it carries a growing set of hardened MCP servers, because the tools your agents call deserve the same scrutiny as the images they run on.


## Govern it all with Docker Scout policy


Scanning tells you what’s wrong. Policy is how you keep it from shipping. And enforcement is where most supply-chain programs quietly fail, because hardened artifacts only protect you when your teams actually use them. Developers move fast and default to what works, and the developer machine is exactly where the current wave of attacks aims.


[Docker Scout](https://docs.docker.com/scout/) policy closes that gap. It evaluates flexible, customizable policies from the CLI and inside CI, and it ships with the same policies Docker uses to verify every hardened image in the catalog. The policies are written in Rego, the industry standard, and they’re portable, so the same rules that gate a build in your CI travel with your teams to every developer machine in your organization. Gating at the registry matters, but it stops at the registry; developers can route around it all day. Policy that travels to the machine is how you hold every image you run, and every image your teams build, to the bar Docker holds itself to.


It’s an additive control. It works alongside the scanners you already run, and it’s already in the Docker subscription you have.


## The foundation is already in your stack


The supply-chain problem is not going to shrink. More code is coming, agents are becoming contributors, and the patch windows regulators expect keep getting shorter. Point tools won’t carry that weight. A foundation that’s secure by default will, backed by an ecosystem that keeps it that way. That is exactly what[Docker’s security portfolio](https://www.docker.com/solutions/security/) delivers. Hardened content on the distributions you already run, customization that keeps its guarantees, support that outlasts upstream, and policy you control, from one vendor accountable for all of it.


And none of it asks you to adopt something new. It’s all in the Docker you already run. Your builds, tools, and pipelines stay the same. Your CVE count doesn’t.


Browse the[DHI catalog](https://hub.docker.com/hardened-images/catalog) and pull your first hardened image today. And if you want the full story, how all of this works together, with your questions answered live, join our live webinar in early September. We’d love to see you there. \[webinar registration link\]
