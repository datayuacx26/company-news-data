---
schema_version: "1.0.0"
document_id: "d967c48dbd39891872025247802367abfabcc5a815af6b68ceee18a096d2f2b4"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/using-jpa-specification-interface-to-implement-dynamic-filters-ea5d022b67f1"
published_at: "2021-12-10T07:24:44+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:b17ba9e5ec863740442671e0cc37e40a4a914885426db3c7564545b91fc89d8e"
---

# Using JPA Specification Interface to Implement Dynamic Filters

Spring Boot


Jpa


Hibernate Jpa


Dynamicfilters


# Using JPA Specification Interface To Implement Dynamic Filters


[Kailas Nath](https://medium.com/@kailas.nath?source=post_page---byline--ea5d022b67f1---------------------------------------)


4 min read


·


Dec 10, 2021


--


Press enter or click to view image in full size


Photo by[Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


For almost all web applications out there, you’d have at least one requirement for listing some items based on some user-facing filters. The usual way to implement this in a back-end application built on Spring and JPA would be to either write native SQL queries, use the out-of-the-box Spring Data JPA / CRUD Repository methods or write criteria queries. Dynamic filters can be tricky to implement. You might need to do joins and add where clauses on the fly. This article is about using the JPA Specification interface to create maintainable, flexible code that can easily handle the above-mentioned issues.


## Scenario


Say we have two entities in our system — User and EducationDetail as shown below. To keep things simple, we consider User to EducationDetail as a` One To One` relation implemented by an education_detail_id column in the user table.


Let’s say our use case requires us to expose an API to give the List of Users. Let’s define a` FilterDTO` class that contains the query params accepted by our API. Keep in mind the fact that all the parameters are optional and the number of parameters can grow.


If we choose to use SQL queries to get the list of users satisfying these criteria, it might look like shown below.


We would need to use` coalesce` with every parameter as these parameters are optional. From a programming point of view not having type safety in the queries is a concern. With more parameters and relations, this would become difficult to maintain.
Let’s say we choose out-of-the-box JPA Repository methods to do this. This would become a mess as the number of query parameters increase. You might think a couple of new repository methods would do the work. But if the number of query parameters is more than two, it will become a tangled mess of if-else conditions to choose which repository method to call based on the presence of the request parameters.


This wouldn’t scale at all.


## Using JPA Specification API


If you are familiar with the Criteria API of JPA, you might move in the direction of writing criteria queries to solve the problem of having varying numbers of where clauses and joins. This works, but the problem with writing criteria queries is that the re-usability of queries that you write is limited (you need to set up Root, Criteria Builder, and Criteria Query and this is hard to externalize). Specification Interface solves exactly this. You can define a specification as a predicate over an entity. This means you can mix and match specifications using` and / or` if you create atomic specification object for a where clause that you’d want to get added to the final generated SQL query.


The above piece of code when used in a repository method adds a where clause like given below.


` where (coalesce(:userStatuses) is null or (u.userStatus in (:userStatuses)))`


Let’s see how JPA Specifications provide a cleaner way to solve our whole use case. To solve our use case using JPA Specification API, we’ll first define a helper class to get the specification object required for us to execute our query.


> Notice how instead of doing something like root.get(“age”) we use static meta models to do root.get(User_.age) which is type-safe.


` createSpecification` method combines the three other specifications using` and` operator. Each function in this class can stand alone and can be used independently as required. They return a specification object that can be used in the` findAll` interface method exposed by` JpaSpecificationExecutor` .


```text
List  < T  > findAll( @Nullable    Specification  < T  > spec)
```


To avail this method in our repository class we define the UserRepository as given below.


> Entity Graph allows us to load just the relations we want when queries are executed. It helps avoid N+1 queries.


Now from the service method, we can just do the below to get the list of users satisfying the filters.


## Outcomes


Let’s look back at what using JPA Specifications allowed us to do.


- Adding a new query parameter would be just adding a new function to our helper class and chaining that to our final specification using an` and` predicate.
- We don’t need to care about parameters that are null/empty. The functions return` builder.conjunction()` which ignores that particular where clause with a` 1=1` in the generated SQL query.
- The JPA static meta-model helps you to create type-safe queries. They are named after their counterpart JPA entity with the` _` symbol as a suffix. (` User` entity would have its corresponding` User_` class). Any change in field names in the entity will give errors at the time of compilation.
- Reuse of specifications. Say you want to find users with a grade greater than a certain value` Y` and age greater than a certain value` X` , we can reuse the functions in our helper classes like below.


## Further Reading


- QueryDSL is a much more friendly, more readable, open-source version of JPA Specification. Both solve the same problem, but in different ways.


[Advanced Spring Data JPA - Specifications and Querydsl In my last blog post I introduced the basic feature set of Spring Data JPA. In this post I'd like to dive into some… spring.io](https://spring.io/blog/2011/04/26/advanced-spring-data-jpa-specifications-and-querydsl/?source=post_page-----ea5d022b67f1---------------------------------------)


- ` JpaSpecificationExecutor` also supports methods for sorting and pagination.


[JpaSpecificationExecutor (Spring Data JPA 2.6.0 API) Interface to allow execution of Specification s based on the JPA criteria API. docs.spring.io](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaSpecificationExecutor.html?source=post_page-----ea5d022b67f1---------------------------------------)


- Adding static meta-models for type safety.


[JPA Static Metamodel Generator The structure of the meta model classes is described in the JPA 2 (JSR 317) specification, but for completeness the… docs.jboss.org](https://docs.jboss.org/hibernate/orm/5.4/topical/html_single/metamodelgen/MetamodelGenerator.html?source=post_page-----ea5d022b67f1---------------------------------------)


- EntityGraphs in JPA for performance optimizations and avoiding N+1 queries.


[JPA Entity Graph - Vlad Mihalcea In this article, I'm going to explain how you can fetch an entity association using a JPA Entity Graph and how you can… vladmihalcea.com](https://vladmihalcea.com/jpa-entity-graph/?source=post_page-----ea5d022b67f1---------------------------------------)
