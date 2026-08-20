---
schema_version: "1.0.0"
document_id: "dbcf7c815dc89ebbc51d8d7a3b1e9cfc330a3fce1089a56081dd4fbc09729d0b"
company_key: "yc-integuru"
company: "Integuru"
source_id: "yc-integuru-news-import-ab81679661d6"
canonical_url: "https://www.integuru.com/blog/what-we-learned"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-22T00:13:16.768883+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:9c18153ca1e990a5dae351dcb104045deede202b3e9a99fac25e8fd0a41c3297"
---

# What We Learned Building Integrations for 2 Years

We at[Integuru](https://www.integuru.com/) have been building integrations for 2 years. We support engineering and product teams when official APIs aren’t available, accessible, or complete. Companies use us to integrate with electronic health records, transport management systems, property management software, financial platforms, and more.


Over time, we’ve seen too many teams make similar, avoidable mistakes with their integrations before they talk to us. That’s why we wanted to write this post: to help people avoid these mistakes upfront.


With integrations, even a tiny bit more thought upfront can save a lot of pain later on. We’ve seen real horror stories of companies getting hit hard in production once usage grows. A little planning here is worth it.


If you still have questions after reading, feel free toemail us .


### **1. Before building, ask yourself some questions to define the requirements**


Many teams rush to build before they’re precise about the requirements. Having clear requirements helps you select the right technical approach and set reasonable expectations.


Take a few minutes to ask and discuss the following questions:


1.


What’s the highest latency we can tolerate?


2.


What’s the minimum level of reliability we need in production?


3.


What volume do we expect now, in 3 months, and in a year?


4.


Do we need real-time and robust data sync, or is periodic caching (with some staleness) acceptable?


5.


How costly is downtime if something breaks?


These questions matter because they shape the right technical approach from the start. For lower requirements and more casual use cases, a team can and *should* choose browser automation / RPA because it’s the easiest to set up. For mission-critical scenarios, we recommend reverse-engineering the network requests, which is what we focus on at Integuru.


You should use network request connections if you *absolutely* need any one of the following: low latency, high reliability, or high throughput. Otherwise, stick with browser automation / RPA. For example, if you expect to use the integration at least a few hundred thousand times a month, which many companies we work with do, you likely should reverse-engineer.


(Note: The advice here is specifically for integrating with web apps.)


### **2. The most overlooked part is edge cases**


Edge cases are different branching logic, states, and paths that don’t appear during the build phase but appear in real-world usage across accounts and scenarios. Integrations that seem fine during testing can completely break in production.


Edge cases can be caused by things like:


-


different account states


-


conditionally appearing fields and pages


-


varying server behaviors


-


role-based differences


Before Integuru, testing edge cases had to be done manually. Developers must manually trigger as many variations as possible, including changing inputs, switching accounts to validate behavior across users, and stress-testing server limits, since frontend and backend validation differ. This process is often tedious and still can’t cover everything, but it’s significantly better than not testing and will surface some failure points before they hit production.


When you ship a manually tested integration to production, we strongly recommend easing into usage gradually, as you’ll likely run into unforeseen issues. Over time, you can cover an increasing percentage of cases, but stay alert, as these situations are unpredictable.


### **3. Main reasons why an integration breaks**


For manually reverse-engineered integrations, the main causes of breakage roughly look like this:


-


**~40%** come from the platform changing underneath. This could mean a page structure change (for SSR pages), an internal API change, an endpoint migration, or an auth flow change.


-


**~30%** come from niche API design edge cases. The content type may vary depending on what the server returns. For example, a failed request may still return “200 OK”, or an endpoint that previously returned JSON may suddenly return HTML.


-


**~10%** come from anti-bot or anti-automation systems starting to block traffic.


-


**~10%** come from other edge cases, such as hidden request dependencies or request-chain failures. In some cases, dependent requests fail under repeated calls because the browser normally triggers them once and caches the result client-side.


The remaining issues are more miscellaneous but still common in practice: race conditions across parallel requests, instability under higher throughput, and platform-specific server quirks, such as actions failing only at certain times of day.


Teams should keep these breakage points in mind, especially if they’re building integrations manually.


### **4. Reduce friction when asking for access**


Unofficial integrations often require asking customers for platform access. That can be an awkward and sensitive ask, and companies lose deals during this process.


Our advice is not to approach platform access as something you need to “convince” customers of. Instead, frame it around their goal and the benefit they get from the integration. Many companies want to adopt AI, but real automation usually requires cross-platform integrations, which need the right access. Help your customers understand this.


References help here, too. For instance, our customers often cite other companies that trust Integuru for integrations.


### **5. How we address these challenges at Integuru**


Initially, like everyone else, we struggled with handling edge cases manually. To solve this problem, we built an agent to automatically cover edge cases, including those that manual or frontend testing can never detect. Today, we use and provide this system for customers. This has significantly improved reliability and lowered upkeep.


We also built systems to counter other breakage points. In addition to covering edge cases, we handle most anti-bot systems and do custom work when a customer encounters a rarer anti-bot mechanism. We’re rolling out self-healing features that actively check each integration’s status and update it when needed. Together, these efforts have made our integrations reliable enough to support high-volume, mission-critical workloads that were previously impossible to support.


### **6. Other questions?**


If you have other questions tailored to your situation,email us . We love sharing helpful tips.


### **Our Services**


We offer both a self-serve product and a fully managed service. For the service, we fully build and maintain the requested integrations and provide a 24/7 on-call maintenance team. If you want to learn more about our self-serve product and/or managed service, schedule a call[here](https://calendly.com/d/cqb8-d9x-nbf/integuru) .
