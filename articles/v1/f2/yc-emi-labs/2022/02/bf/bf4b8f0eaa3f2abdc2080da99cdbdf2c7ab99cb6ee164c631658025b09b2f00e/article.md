---
schema_version: "1.0.0"
document_id: "bf4b8f0eaa3f2abdc2080da99cdbdf2c7ab99cb6ee164c631658025b09b2f00e"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/all-things-microservices-part-ii-a0deadaf5f03"
published_at: "2022-02-17T18:31:12+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:3d9a6e3a1caed5cd424e96f2313b3aba59085dd5734bed2460e059122e005544"
---

# All things microservices (Part II)

# All things microservices (Part II)


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--a0deadaf5f03---------------------------------------)


3 min read


·


Feb 17, 2022


--


by[Marina Huberman](https://www.linkedin.com/in/marinahuberman/) &[Federico Sánchez](https://www.linkedin.com/in/fede-sanchez/)


In our previous post, we covered the bird’s-eye view of our microservices architecture including the challenges, resilience, and tracing and how they all together interact when designing and building through this pattern.


In this Part II, we’ll walk through Configurations interfaces, open the “service template vs. exemplar projects” debate, and have a look at the linting / style fixing tools we use everyday at Emi.


## Configurations


We use[Node-config](https://github.com/lorenwest/node-config) . As it states in the doc:


> *It lets you define a set of default parameters, and extend them for different deployment environments (development, qa, staging, production, etc.).*


Node-config offers a declarative configuration interface, making our code easier to test and extend.


For example, your code doesn’t need to *know* if the port number to be used is coming from an environment variable, a JSON file, or any other source. It only needs to read the Node-config object and doesn’t care how its values were set or overridden. Since these values can be set in many ways, it’s easy to fake them in your config for testing, while production code doesn’t change.


` config/default.js`


` config/custom-environment-variables.json`


Somewhere in your code:


## Service Template vs Exemplars


Discussing and deciding which are the best practices to follow is a hard job, but that is not enough. A good adoption model should provide a great developer experience. There are two techniques to make it easy for developers to do the right thing when starting a new service: Template projects and exemplars projects.


Right now, we are maintaining a Service Template that developers can clone when creating a new service, and then follow some simple steps to customize it for their own needs.


But we know this is no silver bullet. This template ends up in the scope of a single team, and this is not recommended because defining the best practices should be a collective activity. Also, there still are risks of this template getting outdated.


Exemplar Projects are a selection of your best real-world services, rather than isolated services that are just implemented to be perfect examples. As stated, this approach might allow some more control and help decrease the risks compared to the Service Template, and developers can use these exemplars as an inspiration for their own services, following team guidelines.


## Linter and style fixer


We use[standard](https://github.com/standard/standard) because it’s one of the most used linters, there is almost no configuration needed, and integrates well with every editor.


For devs using VS Code, this` settings.json` is provided to enable standard and set up a de-facto convention for the used tab size:


It’s also a good idea to run standard before running tests (` package.json` ), this way pull requests with invalid styling couldn't be merged 👌:


## Further reading


Among Emi Engineering team, we leverage our past experiences and already gathered knowledge when coding, for which the book Building Microservices: Designing Fine-Grained Systems from Sam Newman (available on[Amazon](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems/dp/1491950358) ) deserves a special mention. We strongly recommend it for further reading.


Press enter or click to view image in full size


## Wrap-up


Now that you’ve read this post, you have grokked some basic concepts for working with microservices in the Node.js ecosystem, which can be really helpful when designing for reliability in complex systems.


Have tips of your own for building microservices? Share them with us in the comment section below. We’d love to hear from you!


Last, we hope you have enjoyed the ride and, if you read up to here, it might be a good idea to have a look at our[current job openings](https://jobs.lever.co/emilabs) . We are looking for passionate and curious people to join in!
