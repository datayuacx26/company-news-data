---
schema_version: "1.0.0"
document_id: "0d61b3e031066c52b8cc466da21e641b86e8b9fb583d98c597b4962d4b61cbf5"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/building-an-api-in-less-than-30-minutes/"
published_at: "2017-11-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:574c45eb223df2a9e1c55283e41429f9edc9e4d2f336cbbe6e8757c13b44e92d"
---

# Building a full fledged API in less than 30 minutes in Symfony

We all have been there, done that. You want to build an API that allows you to manipulate your entities so you start checking which specification to use. Maybe[REST](https://en.wikipedia.org/wiki/Representational_state_transfer) or[JSON API](http://jsonapi.org/) or maybe no specification.


Once you have decided which way to go it’s time to create the controllers. Each controller needs at least 5 actions: one to get a single entity, one to get the collection and 3 others to create, update and delete. By the time you have finished writing the API for five entities, you end up with 5 controllers and 25 actions in total and hours lost taking care of validating the request, filtering, sorting and security and lots of other stuff to take care of.


## Finding the solution


3 months ago I started working on a project that needed an API to manage around 20 entities. I had 2 options:


1. Create 20 controllers and write the code for each entity.
2. Find a library that does the job for me.


As I prefer not to spend my time writing copy pasted code I went for the second solution so I started digging around. There were different libraries on GitHub which could do the trick but each of them was missing a big piece. One was easy to customize to your need but needed way too much work to get it running. Another one could be used out of the box but could not be customized. Another one had no documentation.


It was pretty frustrating that there was no solution that:


1. Worked out of box with minimal code
2. Could be customized easily
3. Had documentation
4. Followed a specification


## The third option


That’s when I decided to go for the third option and create a whole new library.


Introducing Jade which stands for[J SON A PI](http://jsonapi.org/) **D** octrine **E** xposer. A simple to setup and use and customize library for building APIs with[JSON API](http://jsonapi.org/) specification. Initially built for Symfony but can easily ported to other frameworks.


So let me tell you what features Jade can offer you:


- Lots of listeners! Jade has 6 types of listener interfaces.


- CreateListener: Before and after saving hooks.
- UpdateListener: Before and after saving hooks.
- DeleteListener: Before and after saving hooks.
- RequestListener: Similar to Symfony.
- ResponseListener: Similar to Symfony.
- ExceptionListener: Good for throwing your own exceptions and showing customized error messages.


- Documentation: Everything is explained with examples. You even have a simple bash command that makes a basic Symfony setup!
- Advanced filtering: Want to do (X AND Y) OR (Z AND A)? No problem! You can do it with Jade.
- Security: Control on resource level and attribute level. Please make sure you read security concerns in the library.


## So who should use Jade?


The library is not optimized for heavy load applications. But for any other application that has lots of resources to manage (like a CRM) Jade works amazingly well. You create the backend and put your focus on something that is not creating, updating or deleting an object.


## Who is maintaining the library?


Right now the library is being maintained by me but it is used in 3 internal projects at trivago. We are open to PRs from anyone who wants to contribute.


## What about tests?


Jade comes with unit tests for the most important classes. More tests (including functional tests) are coming soon.


## Versioning


Jade follows[Semantic Versioning 2](https://semver.org/) . So all the breaking changes will be kept for the next major versions.


## Where can I get the library?


You can find the complete documentation at[GitHub repository](https://github.com/trivago/Jade)
