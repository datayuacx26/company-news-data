---
schema_version: "1.0.0"
document_id: "43c96333a8d954a4f2b933f97f516dc7977ee636369b5d5a0b49fd8c1ec8fa07"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/move-fast-without-breaking-things-c7d3407ee8d6"
published_at: "2019-04-16T21:56:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:c917c1a1b6f7273f1166d9b350953b0ffdf4d5e0147e85d196b6063699d25793"
---

# Move fast (without breaking things)

As GraphQL usage increases in an organization, it’s important for teams to practice[agile schema development](https://principledgraphql.com/agility#5-use-an-agile-approach-to-schema-development) and iteratively evolve their schema over time. One of the most common concerns we hear from teams is that they want to move fast with GraphQL, but they also want to make sure that the changes they’re making to their schema are safe to deploy to production.


This is why we’re excited to announce the official release of **schema validation** for teams on the Apollo platform! 🚀 Schema validation compares proposed schema changes against an organization’s production API traffic to ensure any changes won’t break existing clients. The schema validation workflow integrates with existing CI/CD pipelines and the GitHub pull request workflow to give teams the confidence to evolve their GraphQL API safely while maintaining a lean schema.


Take a look at the workflow in action:


## Schema validation workflow


Schema validation with the Apollo platform has three components: the **CLI** command, the change summary **UI** , and the **GitHub status** integration. Together, they create a cohesive workflow that ensures any schema changes your team makes are compatible with existing clients.


### CLI


Schema validation starts with a CLI command that runs during continuous integration (CI) or continuous deployment (CD). We recommend adding the` apollo service:check` command to your team’s CI/CD configuration to run on every commit or before every deploy. When` apollo service:check` runs, it outputs a summary of schema changes and a non-zero error code if there are breaking changes, isolating the important information from the noise of a source code diff.


Teams often have many different deployments (for example, staging vs. production) that should be independently validated.` apollo service:check` accepts a tag configuration that specifies the target environment. A common pattern, which we use internally to build the Apollo platform, is to run validation against multiple environments in parallel.


Output from running the CLI command


### UI


Every` apollo service:check` execution generates a unique page in Apollo Engine that displays a list of all schema changes. When a change breaks an existing operation, the UI separates these changes out with a summary detailing which clients are affected. The breaking change is compared against the affected client’s production traffic in order to determine the impact of the change and facilitate communication between team members who own different aspects of the system.


Schema validation UI


### GitHub status


Schema validation seamlessly integrates with your team’s existing GitHub workflow to display the results of the check and a link to the change summary UI right inside a Pull Request. When the status fails, indicating dangerous code, the merge can be delayed until it passes additional scrutiny. The following screenshot could be acceptable, for example a type rename that requires clients to regenerate types and mocks, while maintaining the same fields.


GitHub status checks


## Get started 🚀


We’ve been using schema validation internally at Apollo for a while now, and we’ve been impressed with the productivity boost it has given our teams. Testing every change against live production traffic ensures that we can maintain a lean schema without breaking existing clients. When breaking changes are proposed, schema validation surfaces the impact and fosters conversations so teams can proactively agree on a solution.


Schema validation is available for users on a paid Apollo platform plan. To try it out,[start a free Teams trial](https://engine.apollographql.com/?overlay=UpgradePlan) and follow the[setup instructions](https://www.apollographql.com/docs/platform/schema-validation#setup) . If you’d like to learn more about the Apollo platform, please contact our sales team atsales@apollographql.com .


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
