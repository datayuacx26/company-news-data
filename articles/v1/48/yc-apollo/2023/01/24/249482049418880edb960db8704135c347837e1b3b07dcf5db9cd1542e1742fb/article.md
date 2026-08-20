---
schema_version: "1.0.0"
document_id: "249482049418880edb960db8704135c347837e1b3b07dcf5db9cd1542e1742fb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/new-apollo-federation-spec-compliance-tools-for-graphql-server-maintainers"
published_at: "2023-01-24T08:09:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:da51a085852ec1930e91899b664e95dcbd0036a225ec3584a7443867396bfa77"
---

# New Apollo Federation spec compliance tools for GraphQL Server maintainers

[Apollo Federation](https://www.apollographql.com/docs/federation/) is supported by over[20 different subgraph libraries across 12 different programming languages](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) . Have you ever wondered how we capture those compatibility results?


In order to ensure those libraries are compatible with the specification, we have a[testing repository](https://github.com/apollographql/apollo-federation-subgraph-compatibility) that verifies compatibility against their reference implementations. While it is great to have a testing suite verifying compatibility with the spec, up until recently, compatibility results were only available at the end of the library development lifecycle. Developers first had to implement the spec, release the library, create a reference subgraph implementation in the testing repo and then once the tests are run, they would finally get the compatibility results. We heard your pain and decided to streamline this process.


We are happy to announce that you can now easily run subgraph compatibility tests on your local machine and within your Github CI/CD pipelines! Compatibility test suite is now distributed as an executable[NPM script](https://www.npmjs.com/package/@apollo/federation-subgraph-compatibility) and a corresponding[Github Action](https://github.com/marketplace/actions/apollo-federation-subgraph-compatibility) !


Read below to see how you can now streamline your subgraph development process.


## Implementation Under Test


We first need to create a subgraph that will be used for testing. In order to be able to consistently verify federation features, all subgraphs have to implement the same[reference schema](https://github.com/apollographql/apollo-federation-subgraph-compatibility/blob/main/COMPATIBILITY.md) . Compatibility tests will be run against a supergraph which consists of your subgraph implementation, two additional reference subgraphs and[Apollo Router](https://github.com/apollographql/router) .


Compatibility Test Supergraph


Based on the test results, we will be able to generate a compatibility report.


Federation Compatibility Results


For additional details about the reference schema and the executed tests, please refer to our[documentation](https://github.com/apollographql/apollo-federation-subgraph-compatibility/blob/main/COMPATIBILITY.md) .


## Using NPX to run compatibility tests locally


Apollo Federation subgraph compatibility test script is published as an executable[NPM package](https://www.npmjs.com/package/@apollo/federation-subgraph-compatibility) and you can easily execute it by using[NPX](https://docs.npmjs.com/cli/v7/commands/npx) .


```text
npx @apollo/federation-subgraph-compatibility --help
```


When running the script, you have two options in how you can run and manage your test processes – you can either use[PM2](https://pm2.keymetrics.io/) or[Docker Compose](https://docs.docker.com/compose/) . If your subgraph is already running, you just need to specify the target endpoint:


```text
npx @apollo/federation-subgraph-compatibility pm2 --endpoint http://localhost:8080/graphql
```


Otherwise, you can provide a PM2 ecosystem file or Docker Compose file that specifies how to start your application. See our[script documentation](https://www.npmjs.com/package/@apollo/federation-subgraph-compatibility) for additional usage information.


## Integrating Github Action to your CI/CD pipeline


For your convenience, compatibility tests are also packaged as a[Github Action](https://github.com/features/actions) ![apollographql/federation-subgraph-compatibility](https://github.com/apollographql/federation-subgraph-compatibility) action uses Docker Compose to run your compatibility tests. Simply add the following step to your workflow file:


```text
- uses: apollographql/federation-subgraph-compatibility@v1
with:
# [Required] Docker Compose file to start up the subgraph
compose: 'path/to/docker-compose.yaml'
# [Required] Path to the GraphQL schema file
schema: 'path/to/schema.graphql'
# GraphQL endpoint path, defaults to '' (empty)
path: ''
# GraphQL endpoint HTTP port, defaults to 4001
port: 4001
# Turn on debug mode with extra log info
debug: false
# Github Token / PAT for submitting PR comments
token: ''
# Boolean flag to indicate whether any failing test should fail the script
failOnWarning: false
# Boolean flag to indicate whether any failing required functionality test should fail the script
failOnRequired: false
```


By default, action will post compatibility results to your job summary and also upload them as a workflow artifact. If you configure action with a Github Token that has PR write permissions, compatibility test action will also be able to post results as comments on your PRs. You can see an example integration in[apollographql/federation-jvm](https://github.com/apollographql/federation-jvm) repository ([example comment](https://github.com/apollographql/federation-jvm/pull/285#issuecomment-1374047449) ).


Please refer to our[Github Action documentation](https://github.com/marketplace/actions/apollo-federation-subgraph-compatibility) for additional details.


## Updating Subgraph Compatibility Results


[@apollographql/apollo-federation-subgraph-compatibility](https://github.com/apollographql/apollo-federation-subgraph-compatibility) is the source of truth for all the subgraph compatibility results that are displayed on our[supported subgraphs](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) documentation page. Since we need to be able to reproduce and verify the results, we can only report on the subgraph implementations that are included in the repository.


Once you are happy with the compatibility results, please open up a PR with the same example implementation against our testing repository. Make sure to check out our[subgraph contributor guide](https://github.com/apollographql/apollo-federation-subgraph-compatibility/blob/main/SUBGRAPH_GUIDE.md) for additional details. After your PR gets merged, your compatibility results will be displayed on our website.


## Conclusion


Apollo Federation Subgraph Compatibility tests allow us to consistently verify Federation support in a target implementation. With the introduction of an executable NPM script and a Github Action we hope to simplify and streamline the development process for all subgraph authors.


Apollo Federation is also constantly evolving! By introducing versioned artifacts, we can now more easily update or introduce new tests to verify new features. Make sure to follow our compatibility testing repository Github releases for latest test updates to the test harness!


We are also open for feedback so please[start a discussion](https://community.apollographql.com/) , join our[Discord server](https://discord.com/invite/graphos) or raise an issue ([NPM script](https://github.com/apollographql/apollo-federation-subgraph-compatibility/issues) ,[GH action](https://github.com/apollographql/federation-subgraph-compatibility/issues) ) for any new features you would like to see!


Written by


Derek Kuc


[Read more by Derek Kuc](https://www.apollographql.com/blog/author/derek)
