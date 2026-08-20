---
schema_version: "1.0.0"
document_id: "89968c091115c7e0200fcc456d60519c56811fc5184857e5d9074d6348abe658"
company_key: "robinhood-markets-inc-class-a-common-stock"
company: "Robinhood Markets Inc."
source_id: "robinhood-markets-inc-class-a-common-stock-rss-4d5f88ecf3b4"
canonical_url: "https://medium.com/robinhood-engineering/how-server-driven-ui-is-helping-frontend-engineers-scale-impact-c7dce0c20064"
published_at: "2023-05-25T20:57:47+00:00"
first_seen_at: "2026-07-25T21:41:09.986212+00:00"
fetched_at: "2026-08-20T03:51:12.506799+00:00"
content_hash: "sha256:96535c2e8c24dd105cfb3a7a08ed97865067532693295019249e66fe1555f43e"
---

# How Server Driven UI is Helping Frontend Engineers Scale Impact

*Robinhood was founded on a simple idea: that our financial markets should be accessible to all. With customers at the heart of our decisions, Robinhood is lowering barriers and providing greater access to financial information and investing. Together, we are building products and services that help create a financial system everyone can participate in.*


Authored by: Max Rabiciuc, Staff Software Engineer


Robinhood frontend engineers play a critical role in building user interfaces that are both intuitive and beautiful. To serve our customers and their 23.1M net cumulative funded accounts, we develop technologies that enhance developer experience and accelerate product velocity. In this blog we explore the technical implementation of our Server Driven UI (SDUI) platform and how it was used to build part of the crypto detail page in Robinhood’s apps.


**Problem**


Robinhood may need to change how we display information to our customers at any point in time due to customer feedback or regulatory constraints — so moving quickly is business critical.


To make a change to customer-facing information, it requires us to render a user interface (UI). The traditional approach to building that frontend UI is slow — it requires each individual client to fetch and transform a domain model. Client-side logic needs to be written and maintained across each platform that displays the UI, and each client must be individually updated. If changes were made on iOS or Android, this also necessitates releasing a new version of our apps.


The combination of making a change and releasing it was resulting in a weeks-long process involving multiple engineers just to make minor UI modifications. It becomes a week-long turnaround and a bottleneck to serving our customers.


**The Solution: Server Driven UI**


SDUI is an alternative approach to building UI that eliminates the need for client-side logic by fetching view models instead of domain models, allowing us to describe screens based on what they look like rather than what feature they belong to. SDUI enables us to make UI changes without modifying client-side code and having to re-release our apps. It also promotes the reuse of core design system components across platforms and features which allows us to build more with less engineering time. These advantages don’t come for free however; SDUI payloads are larger than traditional domain-model payloads, debugging can be challenging, and special care has to be given in order to ensure consistency across platforms.


While SDUI is not perfect for every task, it can save time, and enable great experiences with fewer engineers when applied correctly.


**SDUI at Robinhood**


Our SDUI platform needed to meet the following requirements:


1. It should be easy for *any* engineer to use SDUI, regardless of which platform they’re familiar with.
2. Edits to components and the introduction of new components must be guaranteed to be safe across client versions.
3. SDUI can be used to display an entire screen or just a section of a screen alongside native UI.


Let’s take a look at how these goals were realized by exploring how we built an SDUI section in Robinhood’s crypto detail page.


**Crypto Detail Page**


Robinhood’s crypto detail page is a native screen. It contains a section outlining your current position for any crypto that you own. SDUI allows us to change the structure and format of this section without ever having to release a new version of the app. It also ensures pricing and position information is consistently shown across all platforms without having to rely on client-side calculations.


**Requirement 1: Ease of Use**


We decided the best developer experience would be to define components directly in code and have the code be the source of truth. On the backend we use our existing API gateway, which is written in Python and leverages the[pydantic](https://github.com/samuelcolvin/pydantic) library with[mypy](https://mypy-lang.org/) for static type checking.


Here is a simplified definition of our base component type:


In order to add a new component, an engineer only needs to subclass UIComponent.


Here is an example of the DataRow component on the crypto detail page:


**Requirement 2: Safety**


The SDUI platform is meant to be self-serve, meaning teams can add new components on their own.


To ensure the backwards compatibility of UIComponent updates, we generate a json schema for all subclasses of UIComponent using[schema_json](https://pydantic-docs.helpmanual.io/usage/schema/) . When an update to a component is made, the new schema is compared to the existing one to guarantee backwards compatibility. We check to ensure no required fields have been removed, or enum values have been added.


In order to implement the design of the position section of the crypto detail page we had to make modifications to the DataRow component. We needed to:


1. Add an optional hyperlink to the title to display an education description upon click
2. Give the row the ability to render an accessory view next to its subtitle, in this case a CircleChart


In addition to backwards compatibility, we need to manage inconsistencies between different frontend platforms. To reduce inconsistencies, we generate client-side types that represent components. Generation takes place from the same schema that was created to validate backwards compatibility. This reduces the chance for error, and enforces that all fields have the exact same names and optionality on all platforms.


Below is the generated client-side type for DataRow.


**Requirement 3: Native Composability**


To render a UIComponent model on a screen, a translation is provided from each subclass of UIComponent to a client-side representation of a view. The implementation varies across the frontend platforms, transforming to a ViewModel on iOS, a Composable on Android, and a React component on the web.


Here is an example of how we transform an the DataRow to a ViewModel on iOS:


Once a UIComponent has been mapped to a ViewModel/Composable/React Component we can now treat it like any other frontend component and put it on screen like we do with all other native frontend components. This makes it very easy to embed the SDUI position section into the crypto detail page.


**Adding New Components**


As previously mentioned, SDUI gives us the ability to make structural changes to the UI without releasing new application versions. In the case of the crypto detail page it allows us to embed additional information into this section that was not included in the original design.


We can append an InfoAlert component to our payload above in order to render a new component on the screen. The SDUI platform ensures a component is not sent to an older client version that does not have a corresponding client side implementation.


**Looking Forward**


SDUI has enabled engineers to write once and ship to all platforms Robinhood can be found on. What previously took three engineers and required releases of our mobile apps can now be done in a matter of hours. Over 80 engineers have contributed components to the SDUI platform and it is now being used in 60+ screens.


We’ve also experienced drawbacks of SDUI. Modeling actions and screen updates can be difficult. We are exploring new ways of delivering SDUI to frontend platforms by leveraging kotlin/js to deliver executable code to clients in order to make SDUI more extensible and responsive. We hope to write about that in a future blog post!


*Robinhood means Robinhood Markets, Inc. and its in-application and web experiences with its family of wholly-owned subsidiaries. Cryptocurrency services are offered through Robinhood Crypto, LLC. All investments involve risk and loss of principal is possible.*


*We are always looking for more individuals who share our commitment to building a diverse team and creating an inclusive environment as we continue in our journey to democratize finance for all. Stay connected with us —*[join our talent community](https://www.gem.com/form?formID=1ebc409f-d44e-4696-ad94-583122ab6df8) *and*[check out our open positions](https://grnh.se/5db87ecd1us) *!*


*Robinhood and Medium are separate and unique companies and are not responsible for one another’s views or services.*


*© 2023 Robinhood Markets, Inc.*


2887728


---


[How Server Driven UI is Helping Frontend Engineers Scale Impact](https://medium.com/robinhood-engineering/how-server-driven-ui-is-helping-frontend-engineers-scale-impact-c7dce0c20064) was originally published in[Robinhood](https://medium.com/robinhood-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
