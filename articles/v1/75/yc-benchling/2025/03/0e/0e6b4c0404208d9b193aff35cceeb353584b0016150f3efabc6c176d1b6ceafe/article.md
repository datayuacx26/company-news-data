---
schema_version: "1.0.0"
document_id: "0e6b4c0404208d9b193aff35cceeb353584b0016150f3efabc6c176d1b6ceafe"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/unlock-it-productivity-with-benchling-developer-platform"
published_at: "2025-03-06T08:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:92c44919beb5e4fcbc56dec76928188b1f4c00946164452220e3710eb3a7d75a"
---

# 5 ways to unlock IT productivity with the Benchling Developer Platform

The challenge in front of biotech IT teams has never been greater: there is an explosion of data, a surge in new AI technologies, and a proliferation of apps being used to drive cutting-edge R&D. If you’re an IT professional, developer, admin, or data scientist, you’re likely experiencing pressure to consolidate applications, improve user experiences, and increase the utility of scientific data. One way to alleviate those pressures is by selecting R&D technology platforms that prioritize interoperability, composability, and extensibility.


In that spirit, we’d like to share 5 ways to help unlock IT productivity using the Benchling Developer Platform. But first, just a little bit of context…


## The magnitude of the challenge for IT teams


When we speak with IT leaders across global biotech innovators, the sense of urgency is palpable. Chief Information Officers want to deliver on the power of AI and provide seamless experiences for their R&D organizations, but they are saddled with a large number of legacy applications that have entrenched workflows tied to them. Benchling surveyed 300 biopharma experts in our annual[State of Tech in Biopharma Report](https://www.benchling.com/state-of-tech) and found that R&D IT teams at companies with 1,000+ employees are typically supporting 10+ scientific software applications, and for many, over 50 applications!1


This creates tremendous overhead for IT teams to both manage the applications and ensure that data and experiences are connected across the portfolio. Working through the change management is no easy feat either: Gartner reports that 34% of digital initiatives have failed to meet expectations.2 The two best weapons against this are: (1) R&D platforms that fulfill many of the required workflows, and (2) strong developer tools that ensure these platforms can be composed and connected in ways that meet your business needs.


## That’s where the Benchling Developer Platform comes in...


The Benchling R&D Cloud provides essential capabilities that support a wide range of workflows spanning discovery, development, in vivo, and more. The Benchling Developer Platform underpins this with a suite of developer tools, integrations, and frameworks that enable IT teams to build connected ecosystems across R&D.


Here's a high level overview of the Benchling architecture:


1.


**Benchling Cloud Application and Data Warehouse:** These are the primary products that scientists interface with such as[Bioresearch](https://www.benchling.com/bioresearch) ,[Bioprocess](https://www.benchling.com/bioprocess) , and[In Vivo](https://www.benchling.com/in-vivo) . They serve as the system of record and execution for lab work, and capture structured data that is served into the Data Warehouse (a PostgreSQL database).


2.


**Benchling Developer Platform:** A suite of tools designed to help you integrate and extend Benchling within your broader app ecosystem. The platform includes an SDK, which simplifies development tasks such as handling API rate limits, as well as Benchling’s REST APIs and Events for deeper integrations. Additionally, the App Framework enables connectivity with custom and third-party applications.


OK… now that we have that context, let’s explore 5 ways to put Benchling’s developer platform to work for your IT team.


## 1. Connect 3rd party software systems with out-of-the-box integrations


Many R&D teams are standardized on a similar set of software and data platforms. Benchling has worked with our customers and industry partners over the past 10+ years to build a large (and growing) library of connectors to common systems that you can apply out-of-the-box and help move data back and forth. Here is a sampling of 3rd party apps where a Benchling integration is available:


## 2. Automatically ingest instrument data with Benchling Connect


Biopharma is experiencing an exponential growth of data, with nearly 90% of our survey respondents expecting annual data generation to at least double in the next year.1 As a result, manual data transfer and lack of real-time visibility into data puts a drain on time, resources, and accuracy. Benchling Connect was introduced last year to address this challenge head on.


Benchling Connect provides out-of-the-box integrations to a large range of commonly-used laboratory instruments. These connectors, built on the Allotrope Simple Model (ASM), automate data ingestion, transformation, and processing natively within the Benchling interface. Developers can access instrument data or data transformation steps via Connect APIs. Benchling Connect also enables SQL access to the Benchling data warehouse, allowing developers to build dashboards using third party tools on Benchling data.


We should note that Benchling Connect is actually a separate offering you can use in conjunction with your existing Benchling products, so it’s not technically part of our developer platform. But we think it’s a really important part of the developer’s toolkit and felt obliged to include this in the list.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## 3. Use Benchling’s open APIs to programmatically access your R&D data


R&D teams often require extensibility within their technology platforms such as a bespoke integration or the automation of data entry. The Benchling API is the most flexible way to build an integration with Benchling. The API provides CRUA (Create Read Update Archive) access to almost all data in Benchling, through REST endpoints that represent that data as JSON. Here are some actual examples of things our customers have built using the Benchling API:


## 4. Create custom interactive user interfaces with App Canvas


App Canvas gives developers the ability to create custom, interactive UI in what will feel like a native Benchling experience. Interactive Benchling apps can use Canvas “blocks” to create custom UI experiences in the context of a scientist's workflow, allowing users to interact directly with the app. Best of all, Canvas UIs update in real time, so feedback from a Benchling app is immediate.


Here’s an example of a Canvas UI that helps the user search for chemicals in PubChem, and then allows them to view and register in Benchling.


## 5. Deploy integrations at scale with Shareable Apps


Often, an integration will need to be used across multiple Benchling tenants. Benchling recently introduced Shareable Apps, which was an enhancement over the existing App framework that now allows developers to deploy one instance of an integration and share it across many tenants. It also allows 3rd party partners and developers to deploy their apps across many different Benchling accounts. Shareable apps are the foundation of an open, interoperable ecosystem — once an integration is created, it can be deployed to any Benchling customer. Developers create a single app definition that can be managed and updated in one place, then delivered to any Benchling tenant using a single install link.


Here’s an example of how an app developer would take the same app mentioned above and share this with another tenant. A unique URL will be generated that can then be used by others to install the app in their own tenant.


## Ready to try the Benchling Developer Platform?


There are lots of ways to get started implementing your ideas. If you are a current Benchling customer, you can learn more about the[developer platform](https://docs.benchling.com/docs/developer-platform-overview) , get inspired on the[Benchling Community](https://community.benchling.com/) , and reach out to your Customer Experience representative to discuss your goals. If you are at a software company interested in developing apps and integrations for Benchling, you can learn more about our[Partner Program](https://www.benchling.com/partnerships) andreach out .


**References:**


1. Benchling:[2024 State of Tech in Biopharma Report](https://www.benchling.com/state-of-tech)


2. Gartner: 2025 CIO Agenda Insights for Life Sciences
