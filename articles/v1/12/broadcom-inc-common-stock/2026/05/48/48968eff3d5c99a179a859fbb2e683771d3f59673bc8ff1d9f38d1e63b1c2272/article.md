---
schema_version: "1.0.0"
document_id: "48968eff3d5c99a179a859fbb2e683771d3f59673bc8ff1d9f38d1e63b1c2272"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/mainframe-software/managing-python-versions-on-the-mainframe"
published_at: "2026-05-04T15:39:40+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T22:12:49.473456+00:00"
content_hash: "sha256:c128ebac38ecb9df42e8043021822abd32b4ccd42426336e42c61f31843b1e53"
---

# Managing Python Versions on the Mainframe

-
-
-


As Python adoption on the mainframe accelerates, one truth becomes clear: success isn’t just about making Python *available* , it’s about making it *manageable* . Unlike compiled languages such as COBOL, Python isn’t “baked” into a self-contained load module. Every execution depends on the Python runtime environment, and managing those runtimes is where platform engineering earns its stripes.


Our job as platform engineers isn’t just to install Python. It’s to create predictable, repeatable outcomes that balance stability with agility. And when it comes to Python on z/OS, that starts with managing multiple runtimes.


**Why Python Version Management Is Different**


With COBOL, once you compile and link-edit, you can tuck the compiler away. The executable has everything it needs. Python is different:


` /usr/lpp/IBM/cyp/v3r12/pyz/lib/python3.12/bin/python someprogram.py`


Every time you run a Python program, it’s interpreted by a specific runtime. That runtime becomes part of the application’s execution contract.


IBM currently ships micro versions of Python four times a year (for example: 3.11.1, 3.11.5, 3.11.7, 3.11.11). Official support covers the current version plus the two prior minor releases (N, N-1, N-2). Meanwhile, the open-source community provides broader, longer-term support. But IBM’s supported builds are what enterprises live and die by.


​​And while Python typically maintains backward compatibility at the micro level, minor releases (3.11 → 3.12) often introduce deprecations and breaking changes. Because IBM’s implementation ports the open-source runtime, not every behavior carries over exactly. When versions drift without oversight, small differences can turn into real risk.


**Platform Engineer Mindset: Why Multiple Versions Matter**


From a platform perspective, the answer isn’t to force everyone to stay on the latest release. That might make our environment “clean,” but it ignores business reality. Today’s development teams must:


- Build and test against a specific runtime
- Expect consistency across environments
- Move at different speeds, depending on business priorities


Not every app can, or should, migrate in lockstep. Some workloads become “abandonware,” serving a temporary purpose before fading away. Others may run on unsupported runtimes because the business accepts that risk.


As platform engineers, our role is to **enable choice without chaos** . Supporting multiple runtimes means:


- **Predictability** : Teams know their tested runtime will remain available.
- **Business alignment** : Application lifecycles can align with business priorities, not arbitrary upgrade schedules.
- **Risk management** : Teams can migrate when they’re ready, while we provide safe coexistence.


In practice, platform teams must plan for N-4 (and sometimes N-5) Python support to deliver true stability. This isn’t technical debt or lax governance, it’s an intentional strategy that acknowledges application lifecycles, testing realities, and business risk tolerance. Anything less forces teams into premature upgrades, increases operational risk, and undermines trust in the platform itself.


**Immutable Runtimes: Protecting the Baseline**


One of the worst practices in distributed environments, and one we must avoid on the mainframe, is silently overwriting a runtime. By default, IBM installs under a generic path like **v3r11** . When a new micro release drops into that path, yesterday’s tested runtime is gone.


That’s a recipe for late-night outage calls.


The platform engineer mindset saysruntimes should be immutable. Every version should live in its own explicit directory:


` /usr/lpp/IBM/cyp/3.11.1/..`
` /usr/lpp/IBM/cyp/3.11.5/..
/usr/lpp/IBM/cyp/3.12.3/..`


Each application then pins to the version it was tested against, with PATH and LIBPATH set accordingly. This is the same reason Open Container Initiative (OCI) images use explicit tags:


## **FROM python:3.11.1**


No serious engineer would risk:


## **FROM python:latest**


Immutable runtimes may feel like extra work, but they’re the only way to guarantee consistent outcomes. Stability is a feature.


**Developer Enablement: Declarative Versioning**


Of course, we can’t expect every developer to manually manage environment variables. That’s where platform engineering shifts from gatekeeping to enablement.


We should provide teams with a **declarative way** to specify their runtime. In cloud-native environments, this often lives in a Dockerfile or build manifest. On z/OS, a simple configuration file can serve the same role:


## **# .python-version (borrowing from pyenv)**


**3.12.10**


Our platform layer then interprets that declaration, sets the right environment variables, and launches the application against the specified runtime. A wrapper script or automation framework can make this seamless.


This approach reduces friction for developers, while keeping runtimes explicit and controlled. It’s not about policing; it’s about empowering developers while protecting the platform.


**Lessons From the Field**


Why so much emphasis on immutability and explicitness? Because the alternative hurts.


We’ve seen what happens when platforms allow “magical” version upgrades. A micro release slips in, and suddenly an application fails in production. Or worse, a minor release changes a default (remember when Java flipped TLS 1.2 to default and broke half the world’s integrations?).


These aren’t just technical headaches, they erode trust. Developers lose confidence in the platform, and the business loses confidence in IT.


The platform engineer mindset is all about building that trust and making the environment predictable, so development teams can innovate without fear of hidden traps.


**A Practical Roadmap**


To manage Python on the mainframe effectively, platform engineers should:


1. **Support multiple runtimes concurrently**


- Go beyond N-2. Plan for at least N-4 to give teams breathing room.


2. **Make runtimes immutable**


- Install each version in a separate directory. Never overwrite.


3. **Adopt declarative versioning**


- Provide developers with a simple manifest or config file. Automate their environment setup.


4. **Automate environment variables**


- Deliver scripts or tools that set PATH, LIBPATH, and PYTHONPATH automatically.


5. **Communicate the contract**


- Be clear with teams: *You choose the version, we guarantee stability* .


**Engineering for Stability and Agility**


Python on z/OS isn’t just another language runtime. It’s a new operating model, one that demands the rigor of platform engineering. Our role isn’t to chase the latest release or enforce arbitrary uniformity. It’s to create a foundation where developers can move fast, without breaking trust.


By supporting multiple runtimes, enforcing immutability, and enabling declarative versioning, we give teams both stability and choice. That’s the platform engineer mindset: Solving for predictability at scale, so the business can innovate with confidence.


**You can learn more about**[Python on the mainframe in the first blog](https://news.broadcom.com/mainframe-software/why-python-on-the-mainframe-a-platform-engineers-perspective) of this series.


*This blog post is provided to you by Broadcom as a courtesy and is for your informational purposes only. This blog post is intended to provide preliminary guidance in forming your plans and policies. You should consult your own legal, regulatory, and security advisors to confirm that the information in this blog post is correct and applies to your specific circumstances. Any reliance you place on the information in this blog post is solely at your own risk.*


**References**


1. [https://www.ibm.com/docs/en/python-zos/3.13.0?topic=configuration-installing-configuring-pax-format](https://www.ibm.com/docs/en/python-zos/3.13.0?topic=configuration-installing-configuring-pax-format)
2. [https://docs.docker.com/reference/dockerfile/#from](https://docs.docker.com/reference/dockerfile/#from)
3. [https://github.com/pyenv/pyenv?tab=readme-ov-file#understanding-python-version-selection](https://github.com/pyenv/pyenv?tab=readme-ov-file#understanding-python-version-selection)
4. [https://blogs.oracle.com/java/jdk-8-will-use-tls-12-as-default](https://blogs.oracle.com/java/jdk-8-will-use-tls-12-as-default)
