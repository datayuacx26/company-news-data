---
schema_version: "1.0.0"
document_id: "3169983d12d0e79ef7a0f4ab1f4ce005291e697f261451c03640d6383550310d"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/building-virtuous-pdf-how-gusto-replaced-a-deprecated-library-with-a-modern-pdf-microservice-b37c481eaa03"
published_at: "2026-08-10T14:55:30+00:00"
first_seen_at: "2026-08-10T19:30:04.502796+00:00"
fetched_at: "2026-08-20T01:03:50.245402+00:00"
content_hash: "sha256:fde4e9450dbcaa5aa80baa4dabcb70ba5e5641ae1a72b4c23bd0163b3331640f"
---

# Building Virtuous PDF: How Gusto Replaced a Deprecated Library with a Modern PDF Microservice

**The problem with our old setup**


PDF generation is a core part of Gusto’s business. To serve our customers, we generate reports, invoices, tax documents, employee handbooks, and more. Many of these features relied on a now-deprecated library: *wkhtmltopdf* .


That reliance had become a liability. Maintenance on the library had stopped, which meant no more security patches. It rendered HTML using WebKit, an engine that had drifted away from how modern browsers display the same markup, so our PDFs didn’t always come out the way we expected. And because PDF generation is memory-intensive by nature, it put pressure on the other services it shared resources with.


**Why build a new service?**


We decided to build our own PDF conversion service instead of swapping *wkhtmltopdf* for another library.


We wanted two things: an easy-to-use interface that let any team generate a PDF from HTML, and an operation that wasn’t coupled to the monolith. A standalone service gave us both. It could be scaled independently, and it took the memory pressure of PDF generation off of everything else. The new service uses Chromium to render the HTML and produce the PDF, the same engine behind the browsers our customers already use.


We built it as a Rails application that uses Ferrum to drive a headless Chromium instance, with a typical Gusto supporting stack: MySQL, background jobs, and ActiveStorage. We chose Ferrum because it speaks the Chrome DevTools Protocol directly from Ruby. Some popular alternatives we looked at (Puppeteer, or Grover, which wraps it) would have meant adding and maintaining a Node.js runtime alongside our Ruby stack, and we chose not to take on that dependency. We also shipped a client gem so other applications could drop our solution in with minimal effort.


**A note on the name**


The Ruby gem we’d been using to wrap *wkhtmltopdf* was called Wicked PDF. When it came time to name its replacement, we wanted the opposite of wicked, so we named our service Virtuous PDF.


**Migrating reports**


The first consumer for Virtuous PDF was reporting backend, the platform which supports documents customers download from Gusto such as payroll summaries or paystubs. This was a natural place to start as reporting was one of the largest producers of PDFs at Gusto.


We started by generating PDFs on Virtuous PDF in parallel with *wkhtmltopdf* . The service lets a consumer send along a comparison PDF (the original *wkhtmltopdf* output) so we could compare the two results side by side before showing anything to a customer. That gave us the confidence to roll the new conversion mechanism out gradually.


Matching *wkhtmltopdf* ’s output wasn’t always straightforward. Our *wkhtmltopdf* implementation leaned on a feature called “smart shrink” that scaled arbitrary HTML down to fit into a page. Convenient, but non-deterministic: a too-wide table could shrink a report’s text until it was barely readable. Chromium has no clean equivalent, so rather than rebuild one, we chose to render each report at a fixed width instead. That gave us pixel-perfect PDFs and real control over font size and spacing, at the cost of refactoring each report’s HTML to a fixed size.


Moving each report to a fixed width takes real refactoring time. Many reports share a template and can be moved in large chunks, but some require report-by-report work that we’re continuing to work on. Today, 75% of reports are generated on Virtuous PDF using just a handful of worker pods. This replaces work that used to be scattered across hundreds of pods in our monolith, every one of which needed enough memory headroom to handle PDF generation.


**Scaling with invoices**


Our first real scaling challenge came when we onboarded invoicing, the service that bills our customers each month. This implementation looked different from reporting. This time, instead of us doing the work, the invoicing team integrated with Virtuous PDF themselves using our client gem. It was the first real test of how easily another team could build on top of the service.


Invoices are unique because their traffic pattern is extremely bursty. We generate invoices for all of our customers at the end of each month: over 500,000 PDFs in a single day, ideally within a couple of hours. That created a risk: a spike that large could starve our reports service of resources during the same window.


Our early tests mostly went well. We could scale up quickly to meet invoicing’s demand while adding less than a second of latency to reporting. But a later production test surfaced a failure mode we hadn’t anticipated. Virtuous PDF runs its background jobs on SolidQueue. SolidQueue is the database-based queuing backend for Active Job and the default adapter with Rails. As the autoscaler brought new pods online, each one opened database connections for SolidQueue’s dispatcher and scheduler. Right around the time our service would reach the scale that invoicing required, our system crossed the database instance’s limit of 1,000 connections. From there it snowballed: pods failed to connect, queue latency climbed, and the autoscaler reacted to the rising latency by spinning up even more pods, which demanded even more connections. We’d built reporting behind a feature flag with *wkhtmltopdf* as a fallback, so we could flip it back the moment we noticed, so customers were never affected. After mitigating the immediate customer impact, we looked at longer term solutions: we upgraded the database instance to allow roughly 2,000 connections.


While our worker pods come online fairly quickly, there is still a small blip when the process first starts that increases the latency for generating reports due to the influx of invoicing jobs. To handle this, we built a workload reservation system. The invoicing service knows ahead of time when it will need to generate its PDFs, so we gave it a simple API to reserve that workload in advance. With this API, the client can send us the expected time and the expected number of PDFs to convert. We save this information and use it to emit a metric for desired capacity starting 15 minutes before the expected window. This allows the autoscaler to scale the service up to a baseline before the first invoice ever arrives. Once traffic starts flowing, our normal autoscaling takes over from there.


**What we learned**


Virtuous PDF started out as a quiet service behind a few reports. Today it handles hundreds of thousands of PDFs on our busiest days, and it does it without putting the rest of our systems at risk. A few lessons stood out along the way:


- Autoscaling can turn a resource limit into a feedback loop. When our worker pods hit the database’s connection ceiling, the resulting queue latency made the autoscaler add more pods which demanded more connections. It pays to know the finite, shared resources your scaling depends on: adding capacity in one place can starve another.
- Some migrations are gated by content, not code. Virtuous PDF was ready long before every report was. Moving each report to a fixed width takes real refactoring time, so getting from today’s 75% to 100% is steady, report-by-report work.
- A migration you can run in parallel is a migration you can trust. Comparing new output against the old, before any customer saw it, turned a scary cutover into a gradual, low-drama rollout.
- For predictable bursts, let the consumer declare the work. The invoicing service already knew when its spike was coming. Giving it a way to tell us was far simpler and safer than trying to react to the spike in real time.


*A big thank you to Ngan Pham for partnering with me to build Virtuous PDF from scratch.*


If you like building services that quietly hold up under big, bursty workloads,[we’re hiring](https://gusto.com/about/careers) .


---


[Building Virtuous PDF: How Gusto Replaced a Deprecated Library with a Modern PDF Microservice](https://engineering.gusto.com/building-virtuous-pdf-how-gusto-replaced-a-deprecated-library-with-a-modern-pdf-microservice-b37c481eaa03) was originally published in[Gusto Engineering](https://engineering.gusto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
