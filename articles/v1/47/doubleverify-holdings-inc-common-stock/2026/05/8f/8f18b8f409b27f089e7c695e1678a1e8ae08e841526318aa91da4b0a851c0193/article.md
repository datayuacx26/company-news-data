---
schema_version: "1.0.0"
document_id: "8f18b8f409b27f089e7c695e1678a1e8ae08e841526318aa91da4b0a851c0193"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/modernizing-diverse-frontends-with-a-framework-agnostic-app-shell-568479d812e6"
published_at: "2026-05-28T08:57:18+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-08-20T03:52:32.830617+00:00"
content_hash: "sha256:8c3146eac87a6453325843009c5be140b5085fbf531614b80f9ea1fd2dd059e0"
---

# Modernizing Diverse Frontends With a Framework-Agnostic App Shell

*Written By:*[Ganesh Karthik Natarajan](https://www.linkedin.com/in/ganesh-karthik-natarajan-94013b3a/)


As DoubleVerify expanded, our frontend ecosystem naturally evolved into a more complex and diverse environment. This created an opportunity to further clarify ownership, streamline release processes, and elevate the overall user experience. What began as a unified Salesforce portal supporting multiple frontend codebases has since grown into a broader portfolio of products, driving the need for more consistent patterns, clearer accountability, and scalable approaches to building and delivering frontend applications.


**Figure 1:** High-level architecture of our legacy Salesforce frontend portal


Each product operated on its own subdomain and was built with a different frontend framework, such as React, Angular, or Vue, each with its own build process and design nuances. As the platform continued to grow, this diversity highlighted the importance of streamlining integration and establishing consistent patterns to support a more cohesive and scalable user experience.


### Scaling Opportunities Ahead


As our frontend ecosystem continued to grow, it revealed several opportunities to better support speed, consistency, and team ownership:


- Variations in design systems and layouts across applications created an opportunity to deliver a more unified and cohesive user experience.
- Releasing even small updates through the legacy portal highlighted the need to streamline and modernize deployment workflows.
- Sharing common capabilities, such as authentication and analytics, across applications highlighted the value of more standardized, reusable foundations.
- Authentication’s tight coupling with the legacy portal underscored an opportunity to enable greater independence and flexibility for newer applications.


Taken together, these areas pointed to a broader opportunity to simplify how teams build and deliver. As multiple teams and an expanding product portfolio continued to scale, establishing a more cohesive frontend foundation became key to improving ownership clarity, accelerating delivery, and enabling safer, more incremental modernization efforts, particularly in areas such as authentication and authorization.


### Our Solution: A New Architecture Built for Scale and Flexibility


To evolve beyond our legacy setup, we split the frontend into smaller, independent **micro-frontends** , enabling teams to move faster, reduce cross-app regressions, and deliver updates independently.


At the core of this setup are two key technologies:


- [Single-SPA](https://single-spa.js.org/) , a microfrontend framework that manages independent application lifecycles, allowing multiple frameworks to coexist seamlessly within a single page
- [Webpack Module Federation](https://webpack.js.org/concepts/module-federation/) enables these applications to share code dynamically at runtime. This approach reduces duplication, keeps bundles smaller, and supports incremental modernization without disrupting users.


### Deploying the New Architecture


We kicked things off by building a lightweight app shell that did not depend on any frontend framework. It handled routing and authentication, allowing teams to focus on their apps without worrying about the bigger picture. To maintain consistency in the user interface, we added a **layout micro-frontend** that manages shared layout and styling across all micro-frontends.


Using Single-SPA, we integrated each micro-frontend using lifecycle hooks: bootstrap, mount and unmount. This setup allowed teams to deploy independently and ensured everything worked smoothly together without tight coupling. During the rollout, the **toolkit micro-frontend** helped standardize how applications integrated with platform services such as authentication, feature flags and analytics. This gave teams a consistent way to plug into the platform and sped up onboarding.


### Implementing a Consistent Shared State


We implemented a consistent shared state across micro-frontends using the[Zustand](https://zustand.docs.pmnd.rs/getting-started/introduction) library. Zustand offers a clean interface and a minimalistic publish-subscribe model, allowing us to manage shared state efficiently without adding complexity. This setup handles authentication tokens, user profiles, and feature flags, making it easy for micro-frontends to consume the shared state while remaining independent.


To ensure a smooth migration, we configured smart ingress routing to direct traffic between the legacy Salesforce shell and the new app shell based on URL paths and authentication tokens. Requests to /new-app/* are routed to the new shell, while /legacy/* requests continue to the old portal. We replaced session-based authentication with JWTs to separate identity from the legacy system and allow modern applications to evolve independently.


To ensure a fast, reliable workflow, we established independent continuous integration and continuous delivery pipelines for each micro-frontend and built reusable starter templates for React and Angular projects. This automation made onboarding easier and helped teams consistently build micro-frontends.


Here is a high-level diagram that outlines the core components of our new micro-frontend architecture.


**Figure 2:** High-level overview of our micro-frontend architecture with the legacy monolith Salesforce portal


The diagram above shows how the legacy Salesforce portal and the new, framework-agnostic app shell coexist during migration. Key components include the app shell, the toolkit micro-frontend, and independently operating micro-frontends.


### Results: The New Architecture’s Measurable Impact


- We reduced build times from approximately 30 minutes to less than 10 minutes per micro-frontend.
- Deployment speed improved with independent daily or on-demand releases.
- More than half of our apps now use JWT authentication, and the migration is ongoing thanks to the hybrid shell setup.
- Users experience a consistent look and feel when switching between micro-frontends.
- We saw fewer cross-team regressions and faster issue isolation.
- Team productivity increased by about 30–40 percent.


#### A Future-Proofed Architecture


- The app shell supports whatever frontend frameworks we choose to adopt in the future.
- Onboarding new teams, especially those acquired, has become significantly easier.
- We can modernize incrementally without taking systems offline.


#### Organizational Benefits


- Product teams now have more autonomy.
- Teams are free to pick the frontend frameworks they’re most comfortable with.
- Our engineering culture has become more collaborative and open to experimentation.


### Final Thoughts


Looking back, moving away from a tightly connected frontend to a micro-frontend setup was a real game-changer for us. It helped us simplify our frontend, strengthen collaboration across teams, and build a foundation that can grow and adapt as the company evolves.


Looking ahead, our focus is now on scaling and standardizing. As more teams adopt this platform, we are prioritizing a better developer experience by clarifying contracts, improving version control of shared features, and enhancing observability across micro-frontends. We have introduced a shared React component library, reducing design differences across multiple products. However, some products built in other frameworks are still aligning, with full visual consistency remaining a core focus.


Ultimately, we want this architecture to be invisible to product teams, allowing them to onboard quickly, build confidently, and innovate freely. This is how we will continue to move forward quickly and sustainably as we scale.


---


[Modernizing Diverse Frontends With a Framework-Agnostic App Shell](https://medium.com/doubleverify-engineering/modernizing-diverse-frontends-with-a-framework-agnostic-app-shell-568479d812e6) was originally published in[DoubleVerify Engineering](https://medium.com/doubleverify-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
