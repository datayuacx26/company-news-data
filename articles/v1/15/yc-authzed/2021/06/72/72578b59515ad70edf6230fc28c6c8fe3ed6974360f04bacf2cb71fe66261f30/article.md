---
schema_version: "1.0.0"
document_id: "72578b59515ad70edf6230fc28c6c8fe3ed6974360f04bacf2cb71fe66261f30"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/naming-is-hard"
published_at: "2021-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:783bf760c7dd706e8a6c53422644fa951260082fe49aec34c85bf10bc2fddd1e"
---

# We spent hours naming this blog post

Have you ever participated in an engineering design session at one of the FAANG companies? It's a fairly unique experience because engineers do not need to consider many of the typical requirements that force making compromises at smaller companies or open source software. For example, you know that the production environment will be *reasonably* reliable and that there are ubiquitous and mature systems for storing data, securing service-to-service communication, authenticating users, checking users' permissions, and more. This is the major benefit of engineering at hyperscale: you have the most sophisticated foundation on which to solve your problem. Now all that's left is to focus on the problem at hand. What could possibly go wrong?


## Step into the Design Room


Put five of the smartest people in a room for an hour and tell them to solve a relatively simple problem: what kind of solution do you think they will have when they leave the room?


- the naive idea that solves the 80% use case
- the reasonably complex idea that covers 95% use case
- the most flexible abstract idea that covers all possible changes in scope


Every Xoogler (former Googler) I've ever met would tell you with confidence that the last option most often emerges at the end of the meeting. But, shouldn't this be a good thing? Not only is the problem solved, but there's also flexibility for the product to change and still be successful, right? The problem lies in the fact that these solutions were designed with engineering as the sole focus. However, to create a successful product outside of Google, there are more requirements to take into consideration than elegant software design.


## A rose by any other name would just be more vague and confusing


The names used to describe the data in your product are as important as the internal design of the product itself. The more "meta" the name, the more difficult it will be for users to understand. Are you still discussing the problem domain or are you now discussing "nodes and edges" or "objects and types"? Sometimes these terms *are* the domain of the problem, but often it will become clear that you've lost sight of the user's perspective. Consider the following hyperbole: you could describe "a particular monoidal property of the category ℕ" or you could simply say "addition". The former is only understood by mathematicians that already know "how the sausage is made" while the latter you could discuss with your coworkers' children. At the end of the day, software products provide value by abstracting away the complexity of the solution so that others can apply that solution without reinventing it themselves.


## Reversing the abstraction


At Authzed, we've built a product inspired by the[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) permission system at Google. Zanzibar has been[publicly documented](https://research.google/pubs/pub48190/) in a research paper and, as such, uses terminology that makes sense for engineers building such a system, but has very little focus on how an intuitive end-user experience could be created from such a system. Your intuition would lead you to think that a permission system should frequently have to mention its core concept: permissions. However, outside of the introduction and conclusion of this paper, the word permission is only used twice: both in the same example. The Zanzibar paper avoids using this term almost entirely because it represents permissions as paths through a graph that they call relations. Instead of permissions, the core API objects used in Zanzibar are called Tuples, which define a direct relation between two objects. It is extremely non obvious that graph databases are usually built on top of a concept called a "tuple store" and that relationships in graphs are stored as "Tuples" (or "Quads" when there's an enforced schema).


While our engineering team has previously worked on the internals of relational and graph databases, it is entirely unreasonable to require that our users also have that same level of understanding in order to design their own permissions systems. As a result, we are now in the process of updating our[terminology](https://docs.authzed.com/concepts/terminology) and iterating our API to make a more approachable system. We think this signals a level of maturity in our development that other Google-inspired software like CockroachDB (Spanner) and Kubernetes (Borg/Omega) have also acknowledged. Our goal is to have an experience that has the intuition of the 80% solution, but is also capable of the *[last 80%](http://www.matvoz.com/blog/2013/10/let-me-tell-you-why-the-last-20-of-work-takes-the-same-amount-of-time-as-the-first-80/)* for those that need it.


We invite everyone to review our[new terminology](https://docs.authzed.com/concepts/terminology) and help provide feedback. We take into consideration all user perspectives: you need not identify as a backend engineer or even an engineer at all. Afterall, solving our users' problems is what drives us to build a better product.


On this page


- Step into the Design Room
- A rose by any other name would just be more vague and confusing
- Reversing the abstraction


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
