---
schema_version: "1.0.0"
document_id: "dece108bcddf5e760038abea4912f83f453411830de2d93481b4e027253be677"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/enforce-flexible-governance-in-graphos-using-custom-checks"
published_at: "2024-11-26T12:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:e58869b637224a067d19745c24f9b91b08fd39ebb0072b1fa1e7cf720cf985ed"
---

# Enforce Flexible Governance in GraphOS using Custom Checks

Apollo GraphOS now supports custom schema checks, a new feature that addresses API platform teams’ demand for enhanced schema governance capabilities. This functionality allows organizations to integrate their own business logic, security policies, and rules into Apollo’s existing schema check workflow, ensuring that every schema change adheres to company-specific standards. The result is a more robust, flexible, and secure approach to maintaining schema integrity, allowing platform teams to uphold organizational standards in an automated manner while adhering to stringent organizational requirements.


## API Governance


API governance encompasses the policies and standards that an engineering organization employs to secure, standardize, and maintain its APIs over time. Engineers utilize various methods to manage changes to their APIs, one of which involves centralizing a space where these policies can be enforced. Governance policies may cover non-functional requirements, like enforcing style guidelines and ensuring that all changes align with established standards, or more complex workflows, such as conducting automated testing and leveraging historical data to ensure backward compatibility.


GraphOS currently offers several tools that help automate API governance, including operation checks, schema linting, composition checks, and proposal checks.[Operation checks](https://www.apollographql.com/docs/graphos/platform/schema-management/checks#operations-checks) compare proposed schema changes against historical operations to verify whether the changes break any of your graph’s active clients.[Schema linting](https://www.apollographql.com/docs/graphos/platform/schema-management/linting) allows you to configure style guidelines and analyze proposed schema changes for violations of formatting rules.[Proposal checks](https://www.apollographql.com/docs/graphos/platform/schema-management/proposals/configure#configure-schema-checks) validate whether your proposed schema changes have matching and approved schema proposals, and finally,[downstream checks](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/contracts/create#run-contract-checks) ensure that proposed schema changes don’t break any downstream contract variants.


As organizations grow and APIs scale, there are going to be facets of governance that extend outside the realm of what is offered by GraphOS. Because of this, we built custom schema checks to offer a hook into the GraphOS schema check workflow to support the injection of custom business logic or rules. This allows users to implement their own checks and push results into GraphOS for viewing in Studio while maintaining a unified check workflow, acting as a safeguard within their continuous integration pipelines for schema modifications.


## How to set up a custom check


Custom schema checks require an HTTPS endpoint that is accessible over the public internet. We offer an[example implementation](https://github.com/apollosolutions/custom-check-examples/) for using an open-source linter, graphql-eslint, to lint the subgraph schemas modified by the check request. This sample is set up as a[Netlify function](https://www.netlify.com/platform/core/functions/) but could be adapted to run on any serverless architecture or containerized system.


After checking out the code and following the steps in the README to deploy it, we can enable it in Studio through the custom check configuration page. After that, we can test our integration with an empty payload by clicking the “Send test notification” button.


If everything looks good in the function logs, we can then run a schema check using the[rover CLI](https://www.apollographql.com/docs/rover/commands/subgraphs#subgraph-check) , and our endpoint will be called.


The schema information will be passed to our endpoint, which will lint the schema and return the results to GraphOS. These results from our custom check task will be stored and viewable in Studio.


The architecture we recommend for a custom check worker consists of two main components: one to receive the payload and the second to execute the business logic.


The first component is responsible for receiving the[webhook payload](https://www.apollographql.com/docs/graphos/platform/schema-management/checks/custom#webhook-format) containing the metadata of the check request. It places this information onto a worker queue and promptly responds with a 200 OK status. This response indicates to GraphOS that the check request has been received.


The second component retrieves check requests from the queue and executes the necessary business logic, such as downloading schemas, performing custom linting, or implementing other schema governance tasks, and then calls back to the GraphOS Platform API with the results of the custom check.


There is no need to perform a custom check synchronously because the state machine that manages schema check workflows will wait up to 10 minutes for the callback mutation to be received. If no callback is received, the custom check tasks will be marked as failed with a timeout.


## Getting started


With custom schema checks, API platform teams can integrate their own custom business logic, security policies, and governance rules into the schema check workflow. This feature improves an API governance posture by allowing organizations to enforce company-specific standards, automate schema validation, and maintain a unified, flexible workflow for schema modifications. Custom checks are now available in GA for GraphOS Enterprise plans. If you want to enable the custom configuration for your graph, head to the “Checks” tab in GraphOS Studio → Configuration → Custom checks. Or, if you haven’t set up schema checks yet for your graph, check out the[documentation](https://www.apollographql.com/docs/graphos/delivery/schema-checks) to get started.


Written by


Samuel Collard


[Read more by Samuel Collard](https://www.apollographql.com/blog/author/samuel)
