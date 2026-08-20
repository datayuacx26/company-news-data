---
schema_version: "1.0.0"
document_id: "bde0049bc0c305ae3f4f938e8b4ef47bb6a6f6df60a6732e32b533a29413dfee"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/creating-a-single-entry-point-for-ifcs-multi-brand-design-system-iceberg"
published_at: "2024-12-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:60b0be85d2a61da958e4e0853acb455033afc1895e6c5ff52b2624f621e2fb50"
---

# Creating a Single Entry Point for IFC’s Multi-Brand Design System, iceberg

As the largest provider of property and casualty insurance in Canada and a leading provider of specialty insurance in North America,[Intact Financial Corporation](https://www.intactfc.com/) (IFC) needs a design system that flexes and supports their many insurance brands and products, in order to deliver a second-to-none customer experience.


That’s where iceberg comes in.


iceberg, IFC’s design system, started in 2019 with a simple library of components, supporting a few small teams. Now, it’s an expansive white-label ecosystem — including design variables, component code library, multilingual copywriting, and accessibility guidelines — that supports more than 500 users across multiple brands, products, and platforms.


### How IFC structures their multi-brand design system


The design system team at Intact takes a modular approach to iceberg so that each unique product can white label the UX components and elements provided by the system. Following the[atomic design methodology](https://atomicdesign.bradfrost.com/chapter-2/) , iceberg have found a unique balance of centralization and empowerment owning level 1 (atoms) and 2 (molecules), while product teams own level 3 (organisms) and 4 (templates) so they have more flexibility to create and innovate design solutions.


“Product development teams can build on average 60% of a screen with elements from iceberg, and then the other 40% is custom code created mostly with iceberg elements,” commented Simon Bélanger, iceberg Design System manager.


iceberg has its own brand agnostic identity, so it provides brand-less components and elements for internal product teams to use. This means designers and developers don’t have to think about the brand at first and can focus on resolving design problems, and then apply the appropriate brand at the end using foundational design libraries.


These design libraries have 3 layers of Figma variables. Simon Désilets, Senior Systems Designer, explains, “We have Primitives which are hidden variables that only the Iceberg team manages. Those include primary, secondary, and neutral colors. Then we have Semantics which contain our component-based variables. Those are linked to Primitives. And lastly, we have Utilities. They represent numbered variables like border-radius, icon size, font definition, etc.”


On the development side, iceberg provides base code libraries for HTML/CSS, React, Angular, iOS, and Android. These libraries are based on the design library and cover simple, pre-made elements.


### Connecting design and engineering information with Supernova


Now that iceberg satisfies all of a product designer’s baseline needs, the team has shifted to innovating and creating new resources and pushing the envelope of what a design system can be.


During a bi-annual survey, they uncovered an opportunity to make it easier to find information about the design system and wanted a website where everything is gathered in one place.


That’s when Désilets started looking into solutions and found[Supernova](https://www.supernova.io/documentation) . They wanted a tool that would allow them to connect their design and engineering libraries and have everything be easily seen, found, and understood.


Their goal was to create one main entry point for iceberg to house onboarding videos, copywriting and accessibility guidelines, design variables, component guidelines, and more.


> *"No more sending links through team channels. Designers, developers, product managers all go directly through the documentation platform to access all possible information.” – Simon Bélanger, iceberg Design System manager*


The team migrated their existing documentation from Figma over to Supernova in just 3 months. “Migrating our existing documentation from Figma was a breeze! We overestimated the time required to do this,” Désilets added. “We built a reusable hidden template within \[Supernova\] that helped us a lot keeping that consistency and efficiency.”


Bélanger emphasized that Supernova enabled them to create “a complete and sophisticated documentation site,” so they could scale up their design system and manage all the different information for their users, without hiring 2-3 dedicated developers to code a website and maintain it.


> *"We have a lot of elements, a lot of components, a lot of documentation, a lot of evolution. We would need two or three devs full time to just maintain that documentation website if we weren’t leveraging Supernova.” – Simon Bélanger, iceberg Design System manager*


Now that all of iceberg’s documentation is migrated, the team is looking at how they can further enhance collaboration between design and development and streamline design system changes.


They plan to explore[Supernova’s end-to-end code automations](https://www.supernova.io/code-automation) to operationalize foundational changes so the team can make changes to variables, push the hook, and send the PR to the lead developer, instead of adding each change to sprint planning.


### Ongoing impact of iceberg


The impact of iceberg is clear. When IFC adds a new product to its portfolio, it’s embedded into the Intact ecosystem in a matter of weeks, thanks to the design system.


“It takes two weeks or a sprint to create the new brand and make those components available,” Bélanger explained. “We duplicate the base code, apply the brand layer, and product teams can create their designs. When a customer accesses an IFC brand, the application dynamically fetches iceberg assets to skin itself in real-time.”


> *"This is also why we’ve invested in the system: to make sure we’re able to grow as a business.” – Simon Bélanger, iceberg Design System manager*


In fact, in their 2023 ROI report, the iceberg team found that on average 85% of designers and developers can save up to 10 hours a week by using the design system. In addition, 70% of developers said using iceberg had a high or very high impact on enabling code consistency.


When asked what advice they’d give to other teams looking to support a design system, Bélanger and Désilets strongly emphasized the need for documentation and overcommunication.


“Document, document, and did we say document?”


“Over communicate, and then communicate more – this will facilitate change management.”
