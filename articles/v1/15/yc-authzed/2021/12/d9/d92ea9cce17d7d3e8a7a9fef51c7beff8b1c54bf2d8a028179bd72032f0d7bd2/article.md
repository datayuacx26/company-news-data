---
schema_version: "1.0.0"
document_id: "d92ea9cce17d7d3e8a7a9fef51c7beff8b1c54bf2d8a028179bd72032f0d7bd2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/unveiling-wildcard-permissions-in-spicedb"
published_at: "2021-12-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:24ac2c63cd25f99c4556cec5e3f829eea3307ccd86d33d1a26956e36b44d2e25"
---

# Unveiling wildcard permissions in SpiceDB

[SpiceDB](https://github.com/authzed/spicedb) is the open source, Zanzibar-inspired permissions system. Want to learn how you can contribute or get started?[Join the Discord](https://authzed.com/discord)


We’re excited to announce today the release of wildcard support in SpiceDB! Wildcard support allows` relation` s to include *all* subjects of a *particular type* , allowing a` relation` or` permission` to become *public* for checks.


To add wildcard support in your schema, simply declare that your relation supports it:


zed


1


2


3


4


```text
definition    resource    {
relation    viewer  :  user     |    user  :*   # < -  -   ` user  :*`   adds wildcard support for type `user`
permission   view  =   viewer
}


```


zed


1


2


3


4


```text
definition    resource    {
permission   view  =   viewer
}


```


A relationship can now be written between your resource and *all* users (since we specified type` user` in` user:*` ), making that relation and any permissions that use it *public* for any` user` :


sh


1


2


3


4


5


```text
> zed relationship create resource:someresource viewer user:*
aHR0cHM6Ly9iaXQubHkvM0piZ1hUMg==


> zed permission check resource:someresource view user:somegal
true


```


sh


1


2


3


4


5


```text
> zed relationship create resource:someresource viewer user:*
aHR0cHM6Ly9iaXQubHkvM0piZ1hUMg==


> zed permission check resource:someresource view user:somegal
true


```


## Why the need for public support?


Computations of permissions in SpiceDB are all about *relationships* : how a resource and subject are related to one another indicates the permissions for that subject. For example, the ability to view a document may be because a user is a` viewer` , because a user is a` writer` of the document, or even because a user is an` admin` of the folder which contains the document.


In[Check it out: How permissions are answered in Authzed](https://authzed.com/blog/check-it-out/) we discussed how these relationships are defined, and how permissions are computed as a result. Defining permissions by a combination of[schema](https://docs.authzed.com/reference/schema-lang) and these relationships can be incredibly powerful but has one major capability missing: the ability to indicate that a permission is granted not to a particular subject but to *all* subjects... i.e. that access for the permission is *public* .


Without direct support for public, developers must either create a specialized` allusers` type or create a relationship between a resource and *every* user, which simply does not scale.


## A public (de)tour of Zanzibar


[SpiceDB](https://github.com/authzed/spicedb) is heavily inspired by the[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) system by Google, providing us some guidance on possible solutions to these kinds of problems.


A key source of information is[the USENIX Zanzibar presentation](https://youtu.be/mstZT431AeQ?t=290) , which briefly mentions how Zanzibar supports public access:


In the above screenshot, we can see that public support was considered useful enough to have a special *system defined value* created to represent that a resource had a *public* relationship between itself and *all users* .


Given the importance of this capability, and the numerous use cases for it, public became[issue #1](https://github.com/authzed/spicedb/issues/1) for implementation in SpiceDB.


However, given SpiceDB’s evolution of Zanzibar, particularly with support for[schema](https://docs.authzed.com/reference/schema-lang) , we wondered: could we provide public support but with more power?


## Developing the proposal: Wild ideas and wildcards


Being open source, it is critical that all development of major features of SpiceDB take place *publicly* , ensuring feedback and buy in from all stakeholders of the project.


The Authzed team began the development process for public support by creating a[github issue](https://github.com/authzed/spicedb/issues/1) , outlining the background. We then followed up the background with a[proposal summary](https://github.com/authzed/spicedb/issues/1#issuecomment-933854997) , listing a set of various proposals and their pros and cons, to ensure a spirited discussion.


The major proposals considered were:


- Documentation and guidance for manual use only
- A special system defined value (like Zanzibar)
- Wildcard support


It was this last proposal, **wildcard support** , that appeared to cause the most (positive) discussion.


Wildcard support had a number of positive features:


- Type safety
- The ability to limit public access to a particular *type* of subject
- No need to add a new keyword
- Symmetry for possible support for[wildcard resources](https://github.com/authzed/spicedb/issues/346) in the future


After much back and forth, community members agreed that wildcard support would be most applicable, and we began implementation which culminated in[PR #336](https://github.com/authzed/spicedb/pull/336)


With the merge of[PR #336](https://github.com/authzed/spicedb/pull/336) yesterday, we are happy to announce that wildcard support is now available in SpiceDB and Authzed.com!


## Wildcard in practice


A working example of wildcard support can be found here, where a document supports being made publicly viewable:


Want to get started testing your permissions system with public support?[Start modeling your permissions system in our playground](https://play.authzed.com/) , or join us in our[Discord server](https://authzed.com/discord) to get answers to your questions. Authzed engineers and a community of users are there to discuss and help.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Why the need for public support?
- A public (de)tour of Zanzibar
- Developing the proposal: Wild ideas and wildcards
- Wildcard in practice
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
