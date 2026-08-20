---
schema_version: "1.0.0"
document_id: "7c9d9e3545176eedf79f6f035c50508f0eed77b4cdf615b3a7a7e159cfdb9d4c"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/mainframe-software/a-second-chance-to-avoid-dependency-chaos-how-to-manage-python-packages-on-mainframe"
published_at: "2026-06-03T15:32:02+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:f2d3a40d409008f491e04f4d8042264c5471d6b99907c11ed88d633ea9041905"
---

# Avoid Dependency Chaos: How to Manage Python Packages on Mainframe

-
-
-


When enterprises adopt Python on the mainframe, one of the first challenges that surfaces isn’t syntax, performance, or tooling. It’s package management.


Python’s superpower is its ecosystem with thousands of libraries for AI, web development, automation, and beyond. But that same ecosystem, if unmanaged, quickly turns into what developers call **dependency hell.**


As platform engineers, our responsibility isn’t just to make Python available. It’s to make it predictable, stable, and consumable at scale. And that means having a deliberate strategy for managing Python packages.


**Why Global Packages Are a Trap**


Let’s start with what *not* to do: Installing global packages into the system-wide Python runtime.


In Python terms, these are called **system site-packages** . They live inside the base installation of Python, and every application that runs on that runtime inherits them. At first glance, this feels convenient: “If everyone needs Flask, let’s just install Flask globally.”


But here’s the reality:


- A modern AI toolkit might pull in more than **100+ dependencies** , each with its own release cadence


- Installing these globally forces every application to inherit them, whether they’re needed or not


- Different applications will require different versions of the same library. Globally, you can only have one version at a time.


That unleashed dependency chaos. Application A depends on` requests==2.28` , while application B depends on` requests==2.32` . With global packages, one of them is guaranteed to break.


Even worse, global installations undermine a core principle of mainframe workloads: **predictable, repeatable execution across environments** . When you “dirty the host” with ad-hoc global installs, you erode stability. What worked in your test environment may not work in production, and debugging becomes a nightmare.


**Virtual Environments: Great for Development, Risky for Runtime**


The common answer in the distributed world is **virtual environments** (venv). A venv creates an isolated Python environment where packages can be installed without polluting the global runtime.


For development, this is a great approach. Each developer can spin up a clean venv, install dependencies locally, and experiment safely.


But for runtime execution on the mainframe, venv introduces problems:


- **Lifecycle complexity** : Now you’re managing both the application’s lifecycle *and* the lifecycle of its` venv` . Application` A-v1` might use` venv-v1` , while` A-v2` requires updated dependencies in` venv-v2` . Do you update in place, or recreate from scratch? Either way, coordination becomes fragile.


- **External dependencies** : A` venv` typically requires running` pip install` at build or deploy time. What happens if PyPI is down? Or if a package has been pulled from distribution? Your deployment pipeline fails.


- **Network security** : On z/OS, giving LPARs outbound access to PyPI is rarely acceptable from a security perspective. The last thing most mainframe teams want is runtime deployments failing because of blocked internet access.


In other words,` venv` is fine for local development, but it’s not the right foundation for production deployments.


**The Artifact-Centric Approach**


A better model is to package **all dependencies directly into the application artifact** at build time.


Here’s the principle: When you build your Python application, you don’t just archive your custom code. You also include all the required libraries in a local directory inside the artifact.


This functionality is enabled by Python's module resolution order. Upon execution, a Python program references` sys.path` to locate libraries, with the current working directory always occupying the first slot. A significant advantage is the ability to use environment variables (e.g.` PYTHONPATH` ) to adjust` sys.path` to include additional directories. Consequently, if your application artifact contains a packages folder with the necessary libraries, Python can be directed to load these prior to searching other locations.


You can achieve this with` pip install -t` , which installs dependencies into a specified target directory:


` pip install -r requirements.txt -t ./packages`


Now your application structure might look like this:


` myapp/
├── main.py
├── packages/
│ ├── flask/
│ ├── requests/
│ └── numpy/`


When you tarball and deploy` myapp` , you’re shipping both the code and the dependencies together as a single artifact. No reliance on global packages. No reliance on runtime` pip install` . Just one artifact, built once and deployed consistently everywhere.


This aligns perfectly with the **immutable artifact pattern** common in DevOps: Build once, deploy many.


**Better Scalability**


This artifact-centric approach provides several key advantages:


- **Predictability** : The same code and dependencies run in development, test, and production. No surprises.


- **Resilience** : Deployments don’t depend on PyPI being online or accessible.


- **Security** : No need to punch outbound holes in firewalls for package downloads.


- **Team autonomy** : Each team owns its dependency set, without risk of colliding with others.


Yes, this does increase artifact size, but on the mainframe, stability and increased scalability is more important than shaving off a few megabytes.


**Beyond OSS: Supporting Custom Packages**


To truly unlock Python’s potential, organizations also need to support **custom package development** .


Python is often described as a “language of libraries.” The standard library is rich, but real-world development almost always involves installing additional packages such as CLI utilities (` click` ), web services (` flask` ), or HTTP clients (` requests` ).


Enterprises benefit when teams can create and share their own reusable packages as well. Maybe a fraud detection module, a mainframe data access layer, or a custom logging library. Sharing these as internal packages promotes consistency, accelerates delivery, and reduces duplication.


The way to distribute custom packages in Python is through **wheel files** (` .whl` ). These are binary distributions of Python packages that can be built once, tested, and stored in a private artifact repository (like Nexus or Artifactory). Teams can then consume them just like open-source packages, but with the added assurance of enterprise support and governance.


This creates a virtuous cycle: The more custom libraries an organization shares internally, the faster new applications can be built on top of them.


**Platform Engineer Mindset: Guardrails, Not Roadblocks**


At its core, package management isn’t about restricting developers. It’s about giving them freedom within guardrails.


From a platform engineer’s perspective, the guiding principles are:


1. **Never install global packages** : Keep the base runtime clean and stable


2. **Discourage runtime` venv` usage** : Great for development, risky for production


3. **Promote artifact-centric builds** : Include all dependencies locally in the deployed package


4. **Support custom package creation** : Empower teams to build and share internal libraries via wheels


These practices don’t just prevent breakage; they build trust. Developers know that when their app works in test, it will work in production. Operations knows that deployments won’t fail because of missing dependencies or blocked network calls. And the business benefits from faster, safer delivery.


**Stability at Scale**


Managing Python packages on z/OS isn’t about blindly copying practices from the distributed world. It’s about adapting them to the mainframe’s DNA: stability, predictability, and resilience.


By avoiding global packages, limiting` venv` to development, and adopting artifact-centric builds with support for custom wheels, enterprises can harness Python’s massive ecosystem without inheriting its chaos.


That’s the platform engineer mindset: designing the system so that developers move fast and the mainframe stays rock solid.


**Learn more about Python on the mainframe in these previous blogs:**


1. [Why Python on the Mainframe?](https://news.broadcom.com/mainframe-software/why-python-on-the-mainframe-a-platform-engineers-perspective)
2. [Managing Python Versions on the Mainframe](https://news.broadcom.com/mainframe-software/managing-python-versions-on-the-mainframe)


This blog post is provided to you by Broadcom as a courtesy and is for your informational purposes only. This blog post is intended to provide preliminary guidance in forming your plans and policies. You should consult your own legal, regulatory, and security advisors to confirm that the information in this blog post is correct and applies to your specific circumstances. Any reliance you place on the information in this blog post is solely at your own risk.
