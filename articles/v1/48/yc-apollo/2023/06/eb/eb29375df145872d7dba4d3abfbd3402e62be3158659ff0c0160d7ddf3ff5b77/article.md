---
schema_version: "1.0.0"
document_id: "eb29375df145872d7dba4d3abfbd3402e62be3158659ff0c0160d7ddf3ff5b77"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/standardize-graphql-schema-linting-policies-with-graphos"
published_at: "2023-06-15T08:29:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:30e1da59c6817f0953986d7accb95446e1642aa3e8f5c8ba28fab1c0ab0b80f3"
---

# Standardize GraphQL schema linting policies with GraphOS

When it comes to writing code collaboratively, consistency is everything. APIs are no exception. In fact, APIs are perhaps the most important place to ensure consistency, since they act as a vein for collaboration across nearly every developer in an organization’s ecosystem.


For GraphQL APIs, the schema is at the heart of all collaboration and consumption, but ensuring consistency in a graph that spans many teams, tools, and workflows is no easy task. Today, we’re excited to make things a bit easier by adding[schema linting](https://www.apollographql.com/docs/graphos/delivery/schema-linter) to Apollo GraphOS!


As an API platform owner, linter checks in GraphOS will help you keep your schema syntax clean and consistent by validating that schema changes pass pre-defined formatting and naming rules like:


- Field names should be` camelCase`
- Input type names should be suffixed with` Input`
- The` @tag` directive should use a pre-approved value for its` name` argument


**Starting today, schema linting is available for all orgs on the GraphOS Enterprise and Serverless plans.** Linter checks will automatically run whenever new schema changes are published to GraphOS, but you can also run checks manually on a local schema or in a separate CI/CD pipeline using the Rover CLI.


## Consistency at every stage of graph development


GraphOS schema linting is designed to integrate seamlessly into your existing processes to ensure that your schema’s syntax stays consistent at every stage of graph development.


### Lint locally with the Rover CLI


No one wants to reread all of the code they just wrote to figure out where they miscapitalized one field. Using[the Rover CLI](https://www.apollographql.com/docs/rover) , devs can run schema linting on their local schema as they work to ensure that their changes pass when published.


To run schema linting locally, just[install Rover](https://www.apollographql.com/docs/rover/getting-started) , authenticate, and run[rover graph lint](https://www.apollographql.com/docs/rover/commands/graphs#graph-lint) or[rover subgraph lint](https://www.apollographql.com/docs/rover/commands/subgraphs#subgraph-lint) .


### Integrate into CI/CD


Linting locally helps individual developers work quickly, but keeping a cross-team code base consistent requires enforcing linter rules centrally. Similar to other linting tools you may use, GraphOS schema linting can be integrated directly into your existing CI/CD pipelines to ensure that every schema change is checked before being published, no matter who it comes from.


To run schema linting in a third-party CI/CD tool, just[install the Rover CLI as part of your pipeline](https://www.apollographql.com/docs/rover/ci-cd/) .


### Linter checks on published schemas in GraphOS


GraphOS gives you a central source of truth for all of your schemas and, if you have a[federated graph](https://apollographql.com/docs/federation) , acts as the final checkpoint for your schema before your graph router uses it in production.


Running linter checks for new changes published to your schema in GraphOS ensures consistency in all of your running graphs.


If you already have schema checks set up in GraphOS, linter checks will automatically be added to your checks workflow! You can review the status of linter checks on the checks page in GraphOS Studio.


## Configuring the linter in GraphOS Studio


The linter in GraphOS comes with a pre-defined list of rules that cover common best practices for writing GraphQL schemas. You can customize the severity of each rule independently to` Off` ,` Warn` , or` Error` in GraphOS Studio under Checks > Configuration > Linter.


When set to` Warn` , a rule violation will allow the checks workflow to pass but output a warning, whereas a violation of a rule set to` Error` will cause the checks workflow to *fail* outright.


You can also choose to have the linter ignore definitions marked with` @deprecated` or` @inaccessible` to prevent errors caused by outdated or private definitions.


## Start linting!


Starting today, linting is enabled for every organization on GraphOS Enterprise and Serverless plans. All you need to do to get started is publish a change to your graph! If you have schema checks enabled, you’ll automatically see linter checks as a new step.


If you want to change the linter configuration for your graph, head to the “Checks” tab in GraphOS Studio → Configuration → Linter. Or if you haven’t set up schema checks yet for your graph, check out[the documentation](https://www.apollographql.com/docs/graphos/delivery/schema-checks) to get started.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
