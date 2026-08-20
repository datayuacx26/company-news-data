---
schema_version: "1.0.0"
document_id: "69d1bf41879e595a1a88bad8bfec2c40d2d1b26f388343d3c490aa75acbed953"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-aebf23be7403"
canonical_url: "https://www.docusign.com/blog/developers/fluidlabs-docusign-extension-apps-customer-workflows"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-28T07:11:32.719779+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:711da803b9721f8c0a7b354ac0030366e06e920779333340b883fa0c0cbf9cf3"
---

# How Fluidlabs Built Docusign Extension Apps for Real Customer Needs: Process and Lessons Learned

[Fluidlabs opens in a new tab](https://fluidlabs.com/) is an ISO 27001-certified development partner that helps startups and enterprise teams solve complex software product challenges. They center their approach with understanding the client’s business goals and context, then planning and architecture to support them through the entire lifecycle of discovery, architecture, user interviews, execution, and launch.


This customer-focused approach helped Fluidlabs build extension apps that:


-


Address a small number of high-impact scenarios


-


Reflect real customer workflows rather than hypothetical use cases


-


Align with Docusign’s product, platform, and marketplace strategy


Fluidlabs and Docusign have worked together since 2024 to develop and implement solutions for Docusign eSignature and contract management products. When Docusign introduced[Intelligent Agreement Management](https://www.docusign.com/intelligent-agreement-management) (IAM), Docusign commissioned Fluidlabs to build extension apps that connect agreement workflows with key business platforms.


**The Middesk Extension app** enables real-time business identity verification within Docusign envelopes using[Middesk's Business Verification API opens in a new tab](https://docs.middesk.com/verify-business) and Docusign’s[Connected Fields extension](https://developers.docusign.com/extension-apps/extension-apps-101/supported-extensions/connected-fields/) .


**The Smartsheet Extension app** enables Docusign integration with the[Smartsheet collaborative work management platform opens in a new tab](https://www.smartsheet.com/platform) for document workflows and data synchronization.


Both extension apps are available in the[Docusign App Center](https://apps-d.docusign.com/app-center) .


## Real-time business identity verification with the Middesk extension app


The Fluidlabs extension app is a type of[Connected Fields extension](https://developers.docusign.com/extension-apps/extension-apps-101/supported-extensions/connected-fields/) that enables customers to verify business names, tax identification numbers (TINs), and office addresses in real time from within Docusign envelopes.


The Middesk extension app uses the Connected Fields API to map custom field validation requirements in Docusign eSignature. During signing, when a user enters text into custom fields designated as business names, TINs, and office addresses, the app validates business names, tax identification numbers, and office addresses against[Middesk’s Business Verification API opens in a new tab](https://docs.middesk.com/verify-business) in real time. It also checks the API periodically to keep the verification status current.


## Automatically sync your Workflow Builder data with the Smartsheet extension app


The Fluidlabs Smartsheet extension app integrates with[Smartsheet opens in a new tab](https://developers.smartsheet.com/) APIs to automatically synchronize your data between Docusign[Workflow Builder](https://developers.docusign.com/docs/maestro-api/) workflow documents and Smartsheet.


It provides three new actions that you add and build into your Docusign workflows:


-


**File Write to Smartsheet** : Store documents as row attachments


-


**Writeback to Smartsheet** : Update or create data in Smartsheet


-


**Read from Smartsheet** : Retrieve row data for workflow steps


To support these actions, this extension app dynamically converts rows in a Smartsheet spreadsheet into the Concerto data type definitions used by Docusign and validates the generated types against the Concerto schema.


## Building the apps that customers need


Fluidlabs began each extension app with a discovery and evaluation phase. Working with Docusign’s ISV Partnerships team, the company identified and prioritized the customer scenarios each app needed to support. The goal was to scope the apps around real workflows that could drive long-term adoption.


This partnership with Docusign enabled them to connect directly with customers and identify platform constraints in an iterative, and scenario-driven process. This enabled them to define a set of directional guidance for scoping and sequencing feature development, including:


-


Common and important workflows which benefit from automation, such as Vendor onboarding, and Commitment Orders


-


The data objects and fields each extension app needed to support


-


Operational pain points worth automating


-


Key Workflow Builder and extension app capabilities, including complexities around authentication and idempotency requirements


Fluidlabs then worked with the technical and product teams at partner companies, including Middesk, to:


-


Clarify business identity verification capabilities and constraints.


-


Identify realistic, high-value verification scenarios.


-


Align on how verification results should be surfaced within Docusign workflows


After gathering these requirements Fluidlabs, moved into development. The team continued to refine the scope as new technical considerations, customer feedback, and platform constraints emerged.


## Technologies and architecture


Fluidlabs built each of the apps using the same set of technologies. Fluidlabs chose to use TypeScript, Express.js, and Node.js for their backend stack. They used AWS DynamoDB for their data layer and AWS EKS (Elastic Kubernetes Service) for deployment, migrating away from AWS Lambda due to the drawbacks associated with cold starts, which could cause latency when starting the app that impacted user experience.


Both extension apps and their shared common library are organized as an NPM workspace in monorepo, managed from a single root package.json. This ensures that there is a single source of truth for idempotency logic, provides consistent error handling, reduces the bundle size, provides easier testing, and simplified maintenance.


One of Fluidlabs’ biggest technical challenges was building a robust idempotency solution to prevent duplicate requests from creating duplicate changes or corrupting data. Because the apps operate across multiple authenticated systems and may process concurrent requests, the team needed an approach that was not covered by any single platform’s documentation.


Fluidlabs used DynamoDB to support duplicate-request detection, concurrent request handling, caching of 2xx and 4xx responses, automatic retries for 5xx errors, and a 48-hour time to live for cleanup.


## Lessons learned and advice for developers


The most important takeaway from Fluidlabs’ success is their holistic view of the apps they create. To Fluidlabs, the final product isn't an extension app, it’s a flow for a customer to solve a problem.


Before finalizing the scope, Fluidlabs spoke directly with Docusign customers to understand their needs, pain points, and existing workflows. This helped ensure that the apps addressed real problems and provided enough value to support long-term adoption.


Another important lesson that helped Fluidlabs design a more seamless experience was experimenting with and developing around a lot of different Maestro workflow triggers, planning exactly how the app will be found and launched from external systems in order to minimize the number of manual steps.


It isn’t enough to design a workflow that customers want and that meets their technical needs; your extension app exists in the context of an external UI, with its own usability and discoverability requirements that you must consider. Reducing friction around using the workflow every day and making the customer experience easy is vital.


Another key lesson was to understand the capabilities and limitations of Docusign IAM, Agreement Manager, eSignature, and the available extension types before beginning development. Because the extension types selected for an app determine the functionality it can support and where that functionality can be used, it is important that you choose the correct extension types to use and that you are able to fully implement them.


For example, when developing the Middesk extension app, Fluidlabs spent several days implementing Data IO to verify data for a form which, as a Docusign eSignature form, turned out to already support this verification. Fluidlabs recommends studying and testing the supported extension types rigorously to understand what they do, where they are used, and what their limitations are.


Finally, Fluidlabs recommends using the[Docusign Developer Center](https://developers.docusign.com/) documentation throughout the development process. Example apps and implementation guides can help developers understand common patterns, evaluate the available extension types, and identify proven approaches before designing their own solutions.


## Develop your own extension apps


The Middesk and Smartsheet extension apps show how developers can connect Docusign agreement workflows with the systems customers already use. Their development also illustrates the importance of starting with a focused customer problem, validating requirements early, and understanding the capabilities and limitations of each extension type.


The Fluidlabs extension apps for Docusign IAM are now available in the[Docusign App Center](https://www.docusign.com/products/platform/app-center) , providing Middesk and Smartsheet users new ways to simplify and streamline their work.


Check them out and start exploring the possibilities of extension apps yourself using IAM’s extension framework, Developer Console, and APIs. Visit the[Docusign Developer Center](https://developers.docusign.com/) to get started or see[Build an extension app](https://developers.docusign.com/extension-apps/build-an-extension-app/) for in-depth documentation on how to plan and build your own app.
