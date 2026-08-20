---
schema_version: "1.0.0"
document_id: "75f0cc4ef3ef8118ca1cb6041679d0c418c70b868e10146b5b5edbcb78bbe995"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2024-07-08-streamlining-graphql-service-testing-with-karate/"
published_at: "2024-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:f95489bc43b241baf65848d1392c0cbf3d6d91c502ed7440cffe281f0ea7fcfe"
---

# Streamlining GraphQL Service Testing with Karate

## Introduction


Over the last year trivago refactored the existing GraphQL monolith and moved to a microservice architecture, in what is also known as a federated setup. Federated GraphQL, as championed by Apollo, represents a significant shift in how companies architect and scale their GraphQL ecosystems, especially when transitioning from monolithic designs to microservice-based architectures. In a federated setup, each microservice maintains its own GraphQL schema, relevant to the domain it serves. These individual schemas are then seamlessly stitched together into a unified gateway. This gateway serves as the single access point for clients, enabling them to query data across multiple services as if it were coming from a single, monolithic GraphQL API. Such change involves also a completely new approach to the test and delivery, with the goal of empowering developers to quickly and safely deploy to production autonomously.


## Understanding GraphQL Services and our challenges


As per[official website](https://graphql.org/) , GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL gives clients the power to ask for exactly what they need and nothing more and there lies its main advantage. We will not go into details, assuming you already know GraphQL and considering that rich documentation is available on the web.


Testing the GraphQL services brings **a few challenges** :


- GraphQL allows clients to construct complex queries to fetch deeply nested data in a single request
- We need to ensure that the GraphQL schema correctly validates queries and mutations according to type definitions and constraints. This includes testing for query syntax errors, type mismatches, and ensuring that queries respect the schema’s structure.
- GraphQL can return nested data structures based on relationships between types. Testing must account for these relationships, ensuring that the server correctly resolves them and that the data integrity is maintained across nested queries.
- GraphQL handles errors differently than REST, returning a status code 200 (“OK”) in most of the cases, even if there are errors at application level. It can return partial data along with errors in the same response. Testing needs to ensure that the server correctly reports errors and that the client can handle these mixed responses.
- The GraphQL schema serves as the contract between the client and server. Testing must ensure that changes to the schema do not break existing queries and mutations, necessitating a strategy for versioning or evolving the schema without causing disruption.


To address all that, a combination of unit, integration, and end-to-end tests can be employed, besides good practises like schema validation.


After the migration from monolith, all of our GraphQL services are stored in a monorepo. We have Continuous Deployment (CD) strategy in our monorepo, which means that any change merged to the main branch should be deployed to the Edge, Stage and Production environments. We already had the in code unit and integration tests, but they didn’t cover the actual deployed application. That was very stressful for our developers since a faulty change for one of our GraphQL Services could break the whole website. This gap is filled by karate.


We will now illustrate what solution we adopted to ensure a more reliable deployment strategy.


## Introduction to Karate


Some testing layers like the unit tests, plus end-to-end tests in the client applications, be it web, native mobile apps etc., were already existing when we started introducing the new test and release process of the microservices. The missing part to address the points previously listed was a good layer of integration tests. As several teams in trivago successfully use the[Karate](https://www.karatelabs.io/) framework for API automation since many years, our choice has been to integrate that into our GraphQL services release process. It simplifies the process of writing tests by allowing testers and developers to define test scenarios mostly in plain English, as Karate’s syntax is inspired by[Gherkin](https://cucumber.io/docs/gherkin/) , thus enabling users to write clear and concise tests. In addition, a whole set of JSON path expressions, Karate’s specific shortcuts and keywords or JS and Java functions can be employed. While it was originally focusing on REST API testing, its capabilities have expanded to include more, like GraphQL services.


Some relevant key features of Karate:


1. **Readable Syntax** : Karate uses a domain-specific language (DSL) that is easy to understand and write, even for non-programmers. This facilitates collaboration across teams.
2. **Built-in Assertions and Matchers** : Karate includes powerful assertions and matchers out of the box, allowing for complex validations with minimal code.
3. **Support for Multi-threading** : Tests in Karate can run in parallel, significantly reducing test execution time, which is crucial for continuous integration pipelines.
4. **Native Support for JSON and XML** : Given that API responses are often in JSON or XML, Karate provides first-class support for handling these formats, including validation and transformation.


Besides the advantages listed above, Karate is specifically well-suited for testing GraphQL services due to several reasons:


- **Flexible Query Testing** : Karate’s ability to handle dynamic JSON and XML responses makes it ideal for testing GraphQL’s flexible and nested query responses.
- **Readability and Maintainability** : The Gherkin-based DSL allows for writing clear and maintainable tests, which is beneficial for documenting and testing the myriad possible queries and mutations in GraphQL.
- **Custom Assertions** : GraphQL responses can be deeply nested and complex. Karate’s powerful assertion library can handle these complexities, allowing testers to validate even the most nested data structures.


Here is an example of a test written in Karate for a` Destinations` service, specifically for a query providing Destination details. In our feature files we import the required GraphQL query, that is separately defined and hence also reusable, and assign it to a variable. The request payload is also defined in a JSON file and imported the same way.


We then rely on Karate’s assertions to validate the responses. As you can see, in some cases we want to test specific expected values, while in other cases we can easily iterate through arrays and check fields types, specify if they are nullable etc. For further details about how to use Karate we suggest to check the[KarateLabs](https://www.karatelabs.io/) official page, the[Karate project on GitHub](https://github.com/karatelabs/karate) and if you want to dive deeper into the topic, also check out the excellent book[Writing API Tests with Karate: Enhance your API testing for improved security and performance](https://www.amazon.com/Writing-API-Tests-Karate-performance/dp/1837638268) by our fellow trivago colleague[Benjamin Bischoff](https://www.softwaretester.blog/) .


## Leveraging Justfiles for Testing


Justfile is associated with Just, a task runner that is often used as a modern replacement for` make` . Its primary purpose is to save and run project-specific commands. Just aims to be handy for saving and executing commands needed during development, such as building, testing, or deploying your software. A Justfile is a simple way to document these commands in a readable, straightforward syntax, making it easier for developers to remember and use them. Parameterization and cross-platform compatibility are some of its advantages. By using` justfile` we could abstract some complex tasks in some` targets` . Those` targets` can be used on both local environment and` CI/CD workflows` . This approach which we called local-first let us troubleshoot issues on` CI/CD workflows` and also react faster to incidents by running a target locally and revert the buggy changes.


As already mentioned, all of our GraphQL Services are in a monorepo. We considered a bounded context for each service. GraphQL service (API) and all services related to that bounded context will be stored in that directory.


```text
advertisers -> Bounded Context for advertisers
├── advertiser-i18n-transformer -> service transformer
└── api -> GraphQL service
```


Justfile offers a useful feature that let us use just one shared Justfile for all services and GraphQL services, so we have only one Justfile in the root of repository with all the required Targets. When we run a target in the service directory (for instance advertisers/api), Just finds that target in the Justfile in the root of repository.


We have multiple targets in our shared Justfile which can be used to build, deploy and do integration tests for a service on a specific Tier (Edge, Stage, Prod) . We offer two targets for Integration tests. The first one is` integration-tests-dev` which is used to run integration tests against local environment and is very useful for developers. The second one is` integration-tests` which can be used to run integration tests against any Tier and Region. This target can be used locally and on CI/CD Workflows. It lets our QAs and developers to run integration tests against our Prod or Stage during developing tests.


Relying on Just means that also locally the tests can be run with a command for which we just need to specify environment, and optionally region, as easy as` just integration-tests stage` or` just integration-tests prod europe-west-4`


## Integrating Karate with Docker and Justfiles


To achieve the smoothest and lightest setup for the test execution, given the dozens of services we needed to cover, we opted for having the following:


- A main Karate Docker image that gets built and pushed only when necessary, e.g. if we want to use a newer version or add functionalities.
- A Docker image for each service, that is based on the main image.
- Folders with` features` ,` queries` and` payload` for each service, that get copied to the corresponding service container.


The illustration further below explains it all. Such approach provides a highly maintainable, highly efficient setup for our use case.


We have two Docker Images for our Karate tests. The first one is Main Karate image which has Karate framework and features that are needed for tests. The second one is the continuous deployment Karate image which uses Main Karate image as base image and has all tests for that service.


As we mentioned the Main Karate image is the base image for all GraphQL Service Karate images, that means that whenever we update the Main Karate image we would need to update more than 50 Dockerfiles. But actually not, as thanks to Justfile we are sharing the version of the Main Karate image in the Justfile as a variable and during the build of GraphQL Service it will be injected and used to build the GraphQL Service Karate images.


We are using Justfile to build the main Karate image, at the moment it’s a manual process which can be done by quality assurance engineers or site reliability engineers. The output of the build will give us an image Tag that can be used to update the version of the Main Karate image in Justfile.


Even though Karate has its own fairly good reporting solution, our **test reports** are generated with the Cluecumber plugin that we use in other projects too and pushed to a GCS bucket, a solution mentioned[in a previous article too](https://tech.trivago.com/post/2023-02-15-how-we-improved-reporting-and-monitoring-of-test-automation-results) . Links to such reports are made available within comments on pull requests on GitHub, when the tests are running in the preview environment for a branch, or as links within Slack notifications when they run during the deployment to production.


## Our reliable release strategy


We started to think about a release strategy to add protection and let us detect a faulty change as soon as possible. We implemented our own Blue-Green strategy, which lets us test our GraphQL Services before deploying to Stage and Production environments.


Stage and Production environments are very important for us. For Production that’s obvious, since it serves our users, but Stage is important too, since developers are using it to develop new features and QAs are using it for End-to-End testing of those new features. To make it clear, our main website has a Stage environment that uses all GraphQL services on Stage environment, hence if we have a broken GraphQL service on Stage it can cause issues to develop and tests features, affecting other teams. Therefore, for us Stage is a proper replica of the Production environment and should also be highly reliable and available.


Before deploying changes to Stage and Production environments, we need to test those changes. First, we should deploy those changes onto our Stage and Production K8S Clusters. In order to do that, we introduced two new environments called PreStage and PreProd. PreStage and PreProd are temporary environments within our Stage and Production K8S Clusters.


For PreStage, we deploy the GraphQL Service and all of its infrastructure components (like Istio Virtual Service, etc. ) to a separate K8S Namespace on Stage K8S Cluster. For PreProd, we deploy the GraphQL Service and all of its infrastructure component to a separate K8S Namespace on all Prod K8S Clusters. Then we run integration tests against those environments. This approach lets us test both Infrastructure and Service together, since a faulty change on the infrastructure side will not let the service be accessible or even run.


When we change any GraphQL service, the following steps should pass successfully before we have that change deployed to Production:


1. Detect the GraphQL Services that are changed
2. Building Docker Images for GraphQL Service and its Karate tests
3. Deploy the Service to Edge environment
4. Deploy the Service to PreStage environment
5. Run Karate Integration tests against PreStage environment
6. Destroy PreStage environment
7. If Integration tests passed, Deploy Service to Stage environment
8. Deploy the Services to PreProd environments (all regions)
9. Run Karate Integration tests against PreProd environments
10. Destroy PreProd environments
11. If all Integration tests in all regions passed, Deploy Service to Prod environments


We were asked why we need to have PreProd environment, considering that we can test changes on PreStage environment. Our GraphQL services have dependencies to other services and components, so we wanted to ensure a working connection to those downstream services to then be able to test the Service and all of its dependencies, for which we might have different configurations between Stage and Production.


Besides integrations tests on PreStage and PreProd environments, we offer a Preview environment for each pull request. In the Preview environment CI/CD, after building and unit-testing, the service will be deployed onto K8S Cluster and the integration tests will be run against it. This way, our developers can merge their pull requests with more confidence.


In all integration tests steps, in both Production releases and Preview environments, we use our` just integration-tests` command, the same way it’s used locally.


## Conclusion


In summary, Karate’s combination of a readable DSL, robust validation tools, and comprehensive testing capabilities, including parallel execution, make it a strong choice for testing the complex and varied interactions inherent in GraphQL services. By adopting the appropriate combination of tools like Docker and Just, we also reduced complexity and improved the reliability and maintainability of a solution that now provides automated test coverage for dozens of microservices that get deployed multiple times a day. We managed to have multiple gates that can detect issues in our code before it reaches the production environments and negatively affect our users.
