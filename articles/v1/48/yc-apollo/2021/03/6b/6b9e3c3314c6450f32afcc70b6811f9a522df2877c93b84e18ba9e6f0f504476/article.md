---
schema_version: "1.0.0"
document_id: "6b9e3c3314c6450f32afcc70b6811f9a522df2877c93b84e18ba9e6f0f504476"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-square-accelerates-product-development-with-apollo-graphql"
published_at: "2021-03-10T10:15:06+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:a9719fd913604a27ef46ef0f5baedcd454f7b1f0d44258b3657002df2eef6dfa"
---

# How Square accelerates product development with Apollo GraphQL

Square started their journey in 2009 with a credit card reader that plugged into the iPhone and iPhone app using a single Ruby on on Rail service. Since then, they have grown into an international company providing a suite of products and platforms to help businesses and individuals participate in the economy.


In[our webinar](https://www.apollographql.com/webinars/How-Square-accelerates-product-development-with-Apollo-Graphql/?referrer=blog) , Tech Lead on the Platform, Infrastructure, and Engineering Team, Lenny Burdette, discusses how Square works with Apollo to build a better experience for their customers.


### Ensuring a consistent user experience


At Square, they strive to provide an exceptional customer experience across the multiple touchpoints their customers may use during the day. To accomplish this, there are cross-functional organizations that consist of hundreds of designers, product managers and engineers. Each build and maintain back-end services that power the features of the front-end applications. In order to fetch the data they need, they have to integrate with services that are owned by different teams.


*“If I’m building a web app and I want to create a coherent experience that connects the wealth of Square’s data together and puts it in the customers’ hands right when they need it.”*


### Moving from a monolithic API towards a federated architecture


In the past, Square had used a monolithic API service. The API worked well for their front-end engineers but Square has been moving away from monoliths to microservices. With a global, decentralized engineering culture, they realized that a monolithic API was:


- Not scalable: Organizations within the company use different languages of the frameworks. A contributor from one organization would have to learn several new technologies to make changes in a monolith.
- Not feasible: Along with the operational overhead being expensive, if the monolithic service went down, this would also bring down the entirety of Square.


Lenny began his search for something better and came across GraphQL. He was looking for a flexible solution that would fetch all the data they needed in one request, as well as in a declarative language that was type-safe. He knew he did not want to build another monolithic API service and then discovered Apollo Federation.


With a system that works for both front-end applications and back-end services, Square saw multiple benefits by implementing Apollo Federation and GraphQL, including:


- The ability to reuse the same logic and behavior by making similarly shaped GraphQL requests.
- Backend teams declare the capabilities of their own services using smaller GraphQL APIs and the gateway automatically combines them into a unified graph for clients.
- Each team builds small GraphQL services that define the graph of data relevant to their own domain and each of the services stands on its own.
- They can automate their composition in a continuous integration system. Each time an organization changes their graph, it’s sent to Apollo Studio for validation and composition where it is composed into a Square wide GraphQL schema.


*“Square has a thousand plus engineers in many time zones, all working together to try to create coherent products. Federation makes it easier to run an automated, centralized API service. Which is really fantastic, but it really works well with Apollo schema management tools because that’s what makes the automation actually possible.”*


To learn more about how Square uses Federation,[watch the full webinar.](https://www.apollographql.com/webinars/How-Square-accelerates-product-development-with-Apollo-Graphql/?referrer=blog)


To speak to an Apollo GraphQL expert,[reach out to us!](https://www.apollographql.com/contact-sales/?referrer=blog)


Written by


Lyndsie Berens


[Read more by Lyndsie Berens](https://www.apollographql.com/blog/author/lyndsieberens)
