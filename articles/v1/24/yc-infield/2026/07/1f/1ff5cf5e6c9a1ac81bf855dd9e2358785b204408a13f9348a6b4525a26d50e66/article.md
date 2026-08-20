---
schema_version: "1.0.0"
document_id: "1ff5cf5e6c9a1ac81bf855dd9e2358785b204408a13f9348a6b4525a26d50e66"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/how-to-stop-punting-upgrades"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:c7162340be68c374fe1175a5f1eb9831ec4e11233af2e85808681616aacdf757"
---

# How To Stop Punting Upgrades

For fast-growing companies, major dependency upgrades oftentimes find themselves struggling to move from the parking lot to the roadmap. The main reason why this happens is that growing companies (correctly) focus on shipping new, customer-visible features over the remediation of accumulated tech debt. In our experience, we’ve found it helpful to put upgrades in the same context as new features, and talk to the organization about the benefits of upgrades for both employees and customers.


#### **Security**


Security is probably the “easiest sell” for any company that has a security team or compliance obligations, it can ill afford to be on an unsupported version of a core dependency. Looking historically at attacks that caused catastrophic damage like Log4Shell (CVE-2021-44228), Drupalgeddon (CVE-2018-7600), and Ghostcat (CVE-2020-1938), the effects of these vulnerabilities lingered because many companies needed to perform major upgrades to patch them. You probably already know whether your software is EOL’d, but in case you don’t check out a tool like[endoflife.date](https://endoflife.date/) .


#### **Performance**


Updates often include performance optimizations that can significantly enhance the efficiency of your application. These optimizations could mean faster response times, reduced resource consumption, and a more seamless user experience. If your company is focused on application performance metrics, you’ll want to look at the changelogs for your core dependencies and see if an upgrade gets you access to these improvements.


For example,[Python 3.12.0](https://www.python.org/downloads/release/python-3120/) brought performance improvements such as PEP 709 and the BOLT binary optimizer, which delivered an estimated 5% performance boost to upgraded applications.[Ruby 3.3.0](https://www.ruby-lang.org/en/news/2023/12/25/ruby-3-3-0-released/) brought the YJIT compiler, and optimizations in garbage collection and thread management​​​​, which boosted application performance up to 10%.


#### **Developer Experience**


If your company is making a big push on hiring engineers, you should make it clear that talented engineers don’t want to be working with unsupported versions of core frameworks. Not only is this a bad signal for the organization's commitment to great developer experience, it also can also contribute a ton of friction to general engineering velocity. Newer versions of packages are generally easier to maintain and extend, and allow development teams to leverage the latest features and paradigms. They’re also generally more feature-rich, increasing the number of building blocks available to your team for new features.


#### **Final Thoughts**


In isolation, the impact of skipping a few updates seems negligible, but the cumulative opportunity cost of not upgrading should be evaluated in the same category as major product features with associated revenue opportunities. Additionally, engineering teams should consider that the effort associated with most individual upgrades increases at an accelerating rate, which makes upgrades even more important to prioritize sooner rather than later. We’ll blog about this aspect of upgrade effort soon(er rather than later).


If you need help prioritizing a major upgrade or scoping one to ensure it's done safely,[contact us](https://www.infield.ai/contact) and we’ll advise- free of charge.
