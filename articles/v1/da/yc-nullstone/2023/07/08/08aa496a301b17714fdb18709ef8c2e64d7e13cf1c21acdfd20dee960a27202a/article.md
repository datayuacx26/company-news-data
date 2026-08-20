---
schema_version: "1.0.0"
document_id: "08aa496a301b17714fdb18709ef8c2e64d7e13cf1c21acdfd20dee960a27202a"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/composable-developer-platforms"
published_at: "2023-07-06T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:cc6974fed79c5d7387cb7cf71fbba814c219bc488ea1d913aa5f9dabf3eaa93e"
---

# Why Composable Developer Platforms are the Future of Developer Platforms

## Introduction


Software teams face a challenge when selecting from the wide range of choices for developer platforms available today. Often, the available solutions require teams to adopt the entire platform regardless of whether it aligns with their specific requirements. In such cases, many growing teams abandon developer platforms and opt for cloud providers for their security and reliability, which comes at the expense of developer productivity. The need for a composable developer platform (CDP) is more crucial than ever. CDPs provide software teams of any size with the flexibility and control over infrastructure required for different scenarios and use cases, thereby delivering exceptional developer experiences while maintaining an easy-to-use developer experience. This article delves into the reasons why a CDP is an ideal solution for developers to achieve their goals and deliver value to their customers.


## Moving Beyond Platform-as-a-Service (PaaS)


Platform-as-a-Service (PaaS) offerings such as \[Heroku\](https://www.heroku.com/) are renowned for delivering an exceptional developer experience, allowing rapid application deployment in a matter of minutes. This level of convenience is often sufficient for personal projects and early-stage startups to deliver value to their customers.


However, as companies experience growth beyond this initial stage, their software teams can become victims of their own success. To better support customers, they migrate their infrastructure to a major cloud provider. Governance constraints emerge, preventing the seamless deployment of new features that developers have been diligently working on. Teams find themselves spending more time putting out fires and ensuring customer satisfaction, which can feel like a futile step backward. This transition comes with chaos and a significant reduction in velocity.


Developer experience drives productivity, so why do teams sabotage themselves by abandoning a developer platform that had previously worked so well? The answer lies in the fact that as your company becomes mission-critical to your customers' daily operations, you assume the role of a steward. This stewardship entails having the necessary reliability controls to ensure uninterrupted services, safeguarding sensitive data that could potentially damage your customers' reputation, and avoiding any actions that might cause financial harm to them.


While it's true that most PaaS offerings are SOC2 compliant, which indicates that vendors have implemented essential controls, this compliance does not guarantee that your infrastructure meets the required standards. Moreover, highly regulated industries demand additional measures to protect consumer data in healthcare or maintain privacy controls in specific jurisdictions. If your company is publicly traded, this stewardship extends to the protection of shareholders, overseen by organizations such as the SEC or equivalent regulatory bodies.


Beyond compliance considerations, many teams genuinely care about their customers and take their stewardship responsibilities seriously. This necessitates the need for infrastructure flexibility and control. Different teams may require specific network topologies, with some comfortable using an SSH Bastion while others prefer a VPN to bridge their cloud infrastructure. Certain teams find immense value in leveraging Kubernetes, while others see it as a potential distraction. Depending on the circumstances, serverless architectures may be adopted to reduce operational costs or handle spikes in usage. In some cases, hosting applications on bare metal or virtual machines (VMs) offers the necessary level of control to serve customers effectively. Larger firms or government organizations often require extra controls over data provenance, including bringing their own encryption keys, dedicated tenancy, and employing various other techniques.


By moving beyond traditional PaaS solutions and embracing a composable developer platform, teams can achieve the flexibility and control needed to fulfill their stewardship responsibilities and cater to their customers' diverse requirements.


## The Limitations of Cloud Providers


With the limitations of a PaaS, it is evident why PaaS accounts for *****only***** \[1/5 of the public cloud market\](https://www.statista.com/topics/6425/platform-as-a-service-paas/#topicOverview). Cloud providers offer a compelling combination of security, reliability, and flexible controls, making them an appealing choice for teams aiming to mitigate business risks. Cloud providers have a natural draw due to their strengths in these areas. Despite these advantages, why aren't cloud providers the perfect solution?


When organizations transition to the cloud, they also expand their tooling and processes to incorporate third-party solutions. As a result, logs and metrics are spread across multiple cloud providers, log providers, and alerting/notification tools. Secrets are scattered across various locations, including cloud accounts, CI/CD tooling, and developer machines. Developers seek the ability to deploy applications using diverse patterns such as virtual machines (VMs), containers, serverless architectures, and static sites. Customers, internal stakeholders, and developers require real-time status updates on the health of the product. Separate tooling is set up for monitoring, alerting, notifications, and customer support. While cloud providers can offer some of this functionality, software teams often choose dedicated vendors for many of these specific needs. This introduces additional challenges as integrating these external systems becomes complex and error-prone. As teams grow to many environments (i.e. dev, staging, prod, and more), maintenance increases to keep these environments synchronized. This sprawl of vendors is necessary to compete, but becomes frustrating to software teams as it can feel like running down rabbit holes instead of serving the customer.


Moreover, granting developers direct access to cloud accounts, even for development purposes, \[has been proven to be risky\](https://www.fugue.co/blog/unsecured-dev-accounts-put-production-at-risk). Instances of unsecured developer accounts have resulted in substantial cloud bills or unauthorized escalation to production resources, highlighting the potential dangers. Even when security controls are correctly implemented, the developer experience provided by many cloud providers, including their "PaaS" offerings, can be painful. In fact, many software teams go to great lengths to restrict or remove console access for developers altogether.


While cloud providers offer notable benefits, these limitations drive software teams to seek alternatives that provide a more seamless and user-friendly developer experience, along with the flexibility to integrate various tools and services without compromising security or incurring unnecessary complexities.


## The Limitations of PaaS on Cloud Providers


One potential solution to address the challenges of developer platforms is to build a PaaS on top of major cloud providers like AWS. However, our experience with this approach yielded mixed results. Several years ago, we had the opportunity to migrate a large consulting firm's applications from private data centers to AWS. At that time, the DevOps movement was gaining traction, and we began offering consulting services in the areas of DevOps and Cloud Infrastructure. We developed an internal developer platform inspired by Heroku, granting developers automated access to cloud accounts, CI/CD pipelines, container hosting, and database hosting. This platform provided golden paths for hosting popular languages/frameworks like .NET, Python, Rails, and more. It was a resounding success, boosting developer happiness and productivity. Developers could deploy apps within minutes instead of waiting for months. The platform team maintained data security and controls, while developers enjoyed autonomy. However, this success was limited to trivial applications and dashboards.


When more complex requirements emerged, such as launching a data warehouse, leveraging serverless architectures, or hosting static assets through a CDN, our platform fell short. Similar challenges were faced by other platforms within the organization, as well as the flooded market of "PaaS on AWS" or "PaaS on Kubernetes" solutions. While these platforms performed admirably for small teams and nascent products, they struggled to accommodate the evolving needs of companies as they matured. Developers resorted to workarounds and found themselves circumventing the platform to meet their requirements.


## The Limitations of Developer Portals


Another approach that has gained popularity is the use of developer portals, such as \[Backstage\](https://backstage.spotify.com/) (popularized by Spotify), which offer dashboards and catalogs of tools, services, and predefined paths. While these portals excel in defining golden paths and providing visibility into service ownership, they come with significant implementation and maintenance costs. One of the primary reasons for this is the lack of seamless integration with the entire tech stack, including cloud infrastructure, CI/CD, source control, and other tooling. Platform teams still need to develop and orchestrate various components for infrastructure and software delivery.


The heavy investment required by developer portals often distracts platform teams from addressing the critical needs of their developers: a curated experience tailored to specific use cases. Frequently, the catalog of golden paths provided by these portals proves to be inflexible, difficult to discover, inadequate, or unable to solve developers' unique requirements. As a result, developers experience frustration as self-service options are available but fail to meet their needs effectively, hindering their success. For example, data teams may desire self-serve access to a Jupyter notebook but find that the resulting notebook lacks essential resources, doesn't support local IDE coding, or falls short in meeting specific compute and security requirements. This is a classic example of "\[bikeshedding\](https://en.wiktionary.org/wiki/bikeshedding)" - focusing on less critical issues while neglecting the most crucial problems. Consequently, the overall developer experience for the data team suffers, leading to a reluctance to adopt the portal.


While developer portals can enhance visibility within a software organization and enable self-service capabilities, they do not prevent the "garbage in, garbage out" scenario. They also fail to assist in the development of effective paths and patterns. Moreover, when used in isolation, they often contribute to a poor curation experience.


## The Emergence of Organic Developer Platforms


Developers are resourceful and pragmatic problem solvers. In many companies, an unofficial developer platform organically emerges as a result of automation scripts, infrastructure code, tooling choices, and vendor selections. This makeshift "platform" evolves through iterations and sometimes comes to fruition when major scaling challenges are encountered. While unplanned, this platform propels the company forward.


Even when teams initially opt for a PaaS, we observe these pseudo platforms taking shape to accommodate the growth of products and user bases. While effective in the early stages, they eventually become a drain on productivity as the team and product expand. Technical debt accumulates, inhibiting the rapid delivery of new features. Integrating new customer-facing or internal services becomes increasingly challenging. Setting up routine tasks, such as nightly jobs, becomes frustratingly difficult. Migrating to new logging and metrics platforms becomes a monumental endeavor. These teams inevitably find themselves deferring critical activities until they become overwhelming obstacles.


The emergence of organic developer platforms highlights the inherent need for a purpose-built composable developer platform that offers flexibility, control, and scalability to accommodate the evolving needs of software teams as they progress and mature.


## Embracing the Composable Developer Platform (CDP) Approach


A composable developer platform (CDP) offers a unique approach that allows developers to effortlessly compose their applications and projects, similar to ordering from a restaurant menu. This empowers developers with the flexibility they desire, without the unnecessary burden. Just like you can customize your meal or order fries separately at a restaurant, a CDP provides prebuilt patterns with the ability to easily swap components such as log providers, error reporting tools, CI/CD engines, and secrets providers. The key here is to enable developers to swap these components swiftly and effortlessly, without distractions or the need for extensive expertise.


A typical limitation of developer platforms is the initial setup process. To address this, a CDP should offer similar benefits to a PaaS out-of-the-box. This includes features like ease-of-use, self-serve capabilities for developers, and automated build and deployment of applications.


However, the real advantage of a CDP becomes apparent on Day 10, 100, or 1000. For example, when developers need to heavily optimize a performance-critical component of their product, a developer should have the flexibility to migrate to a Virtual Machine (VM) without re-architecting their solution or rebuilding their delivery process. A CDP ensures a seamless experience for different application patterns (e.g., containers, serverless) and platforms (e.g., Kubernetes, Nomad, Fargate). As another example, build functionality that suffices during the initial stages may not accommodate advanced testing requirements. This necessitates custom continuous integration and deployment processes. A CDP should readily enable quick and simple integrations with a CI/CD provider.


From our experiences, the best developers choose the right tool for the job, just as they would use a hammer for a nail and a screwdriver for a screw. In the same way, a CDP provides developers with a starting point to gain comprehensive visibility into their applications. This includes high-level metrics on dashboards and real-time status updates. However, when deep diagnostics are needed, the CDP should seamlessly integrate with external telemetry providers, offering direct links to custom dashboards, logs, and metrics. Additionally, the platform should provide resources for remote access, maintenance commands, and emergency procedures, ensuring developers have the necessary tools at their disposal.


In summary, we firmly believe that the optimal developer experience involves working alongside cloud providers, tooling, and processes, leveraging them to augment and amplify their capabilities. This stands in stark contrast to alternative solutions in the market that obscure these underlying platforms. By embracing the diversity of tooling options, developers can enjoy a productive experience from Day 1 and continue to thrive even on Day 100.


## Conclusion


As the landscape of developer platforms continues to expand, it is clear that existing solutions fall short in meeting the evolving needs of software teams. The limitations of all-in-one black box solutions, point solutions, PaaS offerings, developer portals, and organic developer platforms highlight the necessity for a composable developer platform (CDP).


A CDP represents a new philosophy that empowers developers to compose their applications by selecting and swapping components with ease. It provides the flexibility and control over infrastructure required for different scenarios and use cases, allowing developers to tailor their platform to specific needs. With a CDP, initial setup costs are minimized, and developers can swiftly optimize performance-critical aspects of their products. They can seamlessly navigate various application patterns and platforms, ensuring a seamless experience throughout their development journey.


By adopting a composable developer platform, software teams can achieve a balance between flexibility and productivity. They gain the ability to augment and amplify their cloud provider, tooling, and processes, rather than being limited or obstructed by them. A CDP empowers developers to deliver value to their customers while maintaining control over reliability, security, and compliance requirements.
