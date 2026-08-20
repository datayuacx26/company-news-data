---
schema_version: "1.0.0"
document_id: "de7bbd099b115cde220e953b842b77407302ef4c10cc57425d4e0c8d4373fc0d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/permissions-management"
published_at: "2023-11-09T10:50:03.199+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:ba820471be1e290ea0e866ea3b8c5818fdb38d76dc479196b8860ad36b7ab8c7"
---

# What you need to know about Permissions Management

## What you will learn


### The problem with permissions management, especially RBAC


- RBAC examples and challenges
- How companies start permissions management


### Authorization models applied by companies


- Custom code Permissions Management: example, pros and cons
- Policy-based Permissions Management: example, pros and cons
- Google Zanzibar: example, pros and cons


### Google Zanzibar open-source implementations:


- SpiceDB, Google Zanzibar’s most mature open source implementation
- Google Zanzibar vs Ory Kratos vs Hydra vs Oathkeeper
- How does SpiceDB stack against Google Zanzibar?
- Scalability and Compatibility in Permissions Management
- Open FGA vs SpiceDB


### How to choose your permissions management system


## Transcript


I'm Jake Moshenko, CEO of a company called AuthZed, and AuthZed is an authorization company.


So, why should you listen to me? Why do I even know anything about this topic? First of all, as I already mentioned, I'm a co-founder and CEO of an authorization company. In the past, I also founded a company called Dev Table, and one of the things that came out of that company was something that we created called Quay. Quay was the first private Docker registry before Docker Hub allowed you to store private images. Quay was acquired by CoreOS. Then at CoreOS, I was head of engineering for that same division.


Eventually, CoreOS was acquired by Red Hat. At Red Hat, I worked as a senior manager in the service delivery organization. You can think of that as like platform engineering and SRE all wrapped up into one. And prior to that, I also have some experience at some companies you've probably heard of like Google and Amazon. But outside of that CV, the real reason that you should listen to me is because I've actually lived the problem that I'm going to be talking about today.


Through my work at CoreOS, through my work at Quay, and even in the past at Google and Amazon, I've experienced this problem firsthand. So in today's talk, I'm going to talk about the problem that I'm referring to, a little bit about the solution space, and what some people are doing in this area.


I'll talk about ReBAC, and how Google Zanzibar is one example of ReBAC. ReBAC stands for Relationship-Based Access Control. I'll talk about some of the pros and cons of the different authorization approaches that we'll discuss in the solution space, and then I'll explain why we're all in on the Zanzibar model, which is what Google is doing internally.


To frame the problem, I've got here two graphical representations of permissions models, and I'm encouraging you to think about what these models have in common.


On the left side, we have what I call a straightforward RBAC system where user Suzy is an editor of document one. And then on the right, we've got this much more convoluted, much more interesting graph where user Suzy has a role within the context of an org, a role has dynamic grants based on what you're trying to do and the object types you're trying to do it to.


What do these models have in common? Well, they're both called RBAC. One of the challenges in the space of authorization is that everybody has different definitions of what these different acronyms stand for, not necessarily for what they mean in practice. And maybe you've heard other authorization terms like ABAC, so there's all kinds of work done around defining the problem and sort of giving it names in the space, but we often don't start here.


So companies will often start very simply when they're imagining their authorization or they're deploying their first authorization system for their application. Most of the time, they start by building their own. This could be as simple as storing a few pieces of data in a database, loading that up, and then interpreting it in code. At some point, that stops working. That point could soon or it could be very far in the future- but at some point, it stops working.


Maybe it stops working because you have too much traffic now. So the system was built to work for 100 queries per second, and now you're at a thousand or 10,000 queries per second. Or it was built to support 10 users, and now you have a thousand or 10,000 or a million users. So scale can be one of the reasons.


Another reason it stops working is that customers themselves start requesting things that you didn't anticipate. An example of this would be the model on the right in the past slide. I'll give an example of that from my past as well.


Another time it might fail is if you start entering new geographies, and the underlying data store that you're building this on top of is not a distributed data store. So imagine you built a permission system, it works super well, and everything is headquartered out of Amazon US East, and now you need to expand into Frankfurt or somewhere in APAC. If you need a single unified view of permissions data, often the homegrown solution won't follow you on that journey.


Now I want to talk a little bit about my lived experience developing permissions for Quay at CoreOS. This was the very first permissions model that we built when we created and launched Quay. We thought we were copying the GitHub model, I'll put it that way.


In general, everything was centered around the concept of a repository. A repository is where you store your container images, and then users could have various relationships to a repository. So a user could be a reader of the repository, a writer of the repository, or an admin. And then if you see the arrows pointing to the right between the different roles, those are saying that when you're considering who is a reader, you should also consider all writers as readers, and you should consider all admins as writers and therefore also readers. So if someone just has admin, we don't want to prevent them from reading the repository.


So this was actually what we shipped with. Within a week, everyone said, "It's great that you copied GitHub's permission system, or you think you did, but we also need all of the nifty organizational support that GitHub offers in their permission system."


So within a month, we built and shipped this much more complicated system. Everything that's new, I have there in red. And what we had to do is we had to build some concept of organizations. We had to nest repositories under organizations. Organizations had teams, teams had roles within an organization, and then teams could be directly given access to any of the other roles that a repository already had. So that's what that graph is meant to represent.


The real takeaway here is that it was a lot more complicated than we initially anticipated. We built and launched that, and the users who asked for it were happy for a little while. But what we failed to realize is that not even GitHub nailed it, and what the users wanted was the ability to have multiple namespaces under an organization.


Imagine you're like me and you used to work at Red Hat, and Red Hat had different business units, or maybe even Red Hat itself is a business unit of IBM now. They wanted to be able to nest namespaces and they wanted to be able to nest teams. They wanted to be able to break down and model teams according to the org chart, and they wanted to be able to nest repositories under a top-level parent company/business unit/eam/repository. And they wanted to be able to hang permissions off of any of those various places in this tree that they wanted to build up.


For example, if you had a top-level organization, maybe the admin for it was some superuser in the operations department, the COO at the organization. And then as you started to descend through the tree, the permissions would become more fine-grained and more granular and be federated out on a team-by-team or a person-by-person basis.


We never actually built and shipped this version of the authorization model. And the reason that we didn't do that is because we were somewhat naive in our early implementation.


We did our authorization, probably similar to how many of you are doing, by storing data in a database and then doing a bunch of SQL joins to figure out what you have access to. At the time, we were built on top of MySQL. MySQL didn't support recursive CTEs, so we couldn't do a recursive query to jump through, to walk through and gather up these permissions as we descended or ascended through the namespace tree or through the team hierarchy.


We were never actually able to ship this feature. It was one of those things that was always on the backlog, something that we knew we needed to do, but it was going to require a major refactoring of the way we were doing authorization. That's the problem, as we see it, in a nutshell.


And now let's step into some of the ways that companies are currently authorizing things. The most common thing that we see by far is that companies are using embedded custom authorization code in their applications. They're just writing some code to interpret some data, they're doing it directly in their codebase, and then they're making authorization decisions based on that.


If people have decided that this is a bad idea, sometimes they'll adopt a policy engine, and I'll talk a little bit about that as well. And then finally, the new paradigm or the new kid on the block is the Zanzibar paradigm that Google put forward.


First up, this is the embedded code in applications. The specifics of the code aren't really important, but what is important is that you can see that we're reaching out to the database every time a user is trying to handle a request for an object. We reach out to the database and we load who are the authorized users for the object, and then we usually have to reach back out to the same database again to get the data to send back to the user. And then we've got a little authorized method there. It's already abstracted out into its function, so it's not like a big if-else block embedded in the code, but it is still something that exists within our monolithic application.


No judgment if this is how your authorization code looks. Like I said, I've built and launched variations of this in the past, and it works for some scale.


So when we break down the pros and cons of writing custom code to do authorization, the biggest pro is it's infinitely flexible. You can do and express anything that your heart desires as long as you can figure out how to write the code for it. Another pro for custom code is once you've exceeded the single monolithic application and you start to break down your service into potentially other services or microservices, you can take that authorized method like I had on the last slide, and you can turn it into a shared library. You can share that library and compile that library into other services and other applications.


But if that code is living within a single service, one of the downsides is that you can't call it from other services. So one thing that we'll often see as a company is going through like a microservices or a service decomposition is that the authorization logic will get tied up into that monolith, and then they'll have to expose methods on for those other microservices to query it about authorization. This is obviously like a huge inversion of priority, right? You don't want microservices to be blocked on calling out to a monolith. This is not the ideal by a long shot.


Another disadvantage of this method is that if you split this out into a library and you happen to be a polyglot shop, which means you're writing your services in multiple different languages, you need an implementation of the library for all of the different languages that you're potentially implementing in. So for example, if you use JavaScript, and Node.js on the backend, maybe you use some Python, maybe some Java, maybe some Ruby, you need an implementation of that same authorization code for all of those languages. And that can often result in complicated rollouts as well. So if you need to update the authorization code, you need to make sure that you're updating it across all languages and rolling it out to services in such a way that the users are never given an inconsistent view or incorrect answer.


Another downside of this method is that you're going to your database, your main database, your source of truth, and fetching data on every single request that requires authorization. Not just data, but data specifically about authorization. This can add additional unwanted load to your database. This was something that we ran into for Quay. On Quay, we did those SQL joins as I mentioned, and I think the final version of the code, or the code that you can go see if you look at the open source right now, does a join with like 11 different tables. When we did some analysis, it turned out that we were spending far too much of our database CPU just joining on these same 11 tables over and over again. This could be like a tall tale; maybe every time I say it, the number of tables gets a little bit bigger, but it is open source, you can go check out how that works today.


And finally, authorization code isn't the kind of code that people like to open up and change all the time because it's tricky code. It's hard to verify that you haven't opened any security holes, and it's just not something that your average application developer is interested in getting down into the nitty-gritty and understanding the nuts and bolts of.


Moving on to the next solution, we have policy engines. So policy engines were kind of the hot new thing on the block probably about eight or nine years ago. So what about policy engines? Are they good? Are they bad? What do they do?


Often when people embark on adopting a policy engine, their goal is to abstract their authorization logic from their code, and this is a laudable goal. You can solve a lot of the same problems that we just talked about. You can write your policies in a robust logic language that's formally proven to be correct for a certain class of authorization operations.


Since it's usually a network request or you're running it in a sidecar, something like that, you can have one implementation of your policies for all languages, and then you just reach out and talk to those policies over the network. This works great when you already have all of the data ready.


Imagine if you're writing like a network appliance or an HTTP filter or something like that, and the data that you're making your request based on is already available to you in the form of the IP address of the caller or HTTP headers or the current date and the current time. If you already have all of that data that you want to feed to the policy engine, the policy engine can often give you an answer back in microseconds.


Some of the downsides of using a policy engine, though, are that you still have to fetch data from your main source of truth for every request and feed it to that policy engine. And sometimes you're reading a lot more data than you have to, depending on how you can constrain and how you can draw a boundary around the data that you need to feed to that policy engine. It can also cause a complicated rollout of new policies if you're using the sidecar model and you've baked the policy into the sidecar.


You need to make sure that you're rolling that out in a consistent way, or at least in a backward-compatible way, such that if two different services are evaluating the policy at two different times, they don't accidentally open up a security flaw in terms of having different data interpreted different ways at different places.


And finally, these languages that you write policies in are powerful. They're very powerful, and these can result in policies that are difficult to understand and eat up a lot of your budget in terms of latency, and difficult to write and difficult to maintain.


Just to show an example of one of those, I went to the OPA website and I just pulled a Rego script right off of the landing page. I'm going to give everybody about 30 seconds to read through this Rego script and try to have an idea in mind for what this script is doing or what this policy is doing.


When I look at this policy, I see eligible groups, something about locations, a lot of things about locations. And then we've got some kind of roles construct, and then we've got this block repeated three times which I'm guessing is binding a location to a role record.


So if you were able to figure it out, maybe you were, maybe you weren't, but what this is doing, is doing a role binding by location and group name. The first thing it does is it verifies that the groups are one of the supported groups, so that's group one and group two. And then it gives three different locations, and the locations have different roles for each group in each location. And then anything that doesn't match one of those locations or one of those groups results in an empty set of roles that you have access to.


So this table makes a lot of sense to me, but this policy was kind of hard for me to read through and for me to understand what exactly was happening.


As a result of having policies that are written in this domain-specific policy language, one thing that we're being told by our prospects and by our customers is that when you adopt a system like this, the platform or auth team usually ends up owning all of the policies.


We talked to a massive organization, and they told us that they were using a version of Prolog or a dialect of Prolog to do their authorization policy, and there was one person in this multi-tens of thousands of person organization who was capable and confident in writing their policies. This is usually kind of an anti-pattern for what we're trying to accomplish by bringing in an authorization policy engine.


Moving on to the next and final solution in the solution space is Google's Zanzibar. So in 2019, Google wrote a paper called Zanzibar: Google's Consistent Global Authorization System. The core concept of Zanzibar is let's build off on top of a globally distributed database. So internal to Google, there's a globally distributed database called Spanner. It's ACID, it seems to violate the CAP theorem and like the basic laws of physics, but it is a very solid thing on which to build an authorization system that might need to scale and that might be geographically distributed.


The whole paper is about 12 pages. It's an easy read, and I highly encourage you to go read it.


I will give an example of what adopting Zanzibar has allowed Google to do.If you've ever been in this case where you're writing an email, and in the email you include a link to a Google Document, and you think everything is great, and you go to hit the send button, and Google pops up a scary warning that says, "Hey, someone that you're trying to send this email to does not have access to the document that you've linked."


What is this magic? How does this work? How does Gmail possibly know what access my email recipient has to a document that I've merely linked?


The answer is that they use Google Zanzibar as a centralized authorization service for everything at Google. And Gmail is able to ask questions about permissions that exist ostensibly within the doc service.


A high-level overview of Google Zanzibar: it's a single authorization service for all of Google I mentioned. It deals primarily with storing relationships as edges in a directed graph, which is what I referred to earlier as ReBAC or Relationship-Based Access Control. Then they give their engineers a schema to interpret those relationships. So we're decoupling the relationships themselves from how they're interpreted to make authorization decisions.


The Google team spends about half the paper talking about performance, and it's throwing up some really impressive numbers. At Google, Zanzibar is doing 10 million queries per second at peak, and this is as of 2019, I'm sure it's grown since then. These are just the numbers in the paper. They're storing trillions of relationships, and they have a 95th percentile check query latency of 10 milliseconds.


If you know anything about writing applications that are human-facing, usually you want to keep those interactions under about 100 milliseconds. So 10 milliseconds isn't chewing up a lot of your budget in terms of latency. and maybe most impressively for this being a centralized service they've managed to keep 99.999% uptime, so five nines of uptime for this service which is super important for an authorization service which is usually in the line of fire for every request.


So, in an attempt to make this more concrete, and I hope I'm successful here, here's an example of using relationship-based Access Control to control policy decisions. I don't have access to a DSL if Google has one internally, but what I can show is how you express these things in our own Oso's DB schema language.


We have three different object types: users, organizations, and documents. And within those object types, we have relationships, those are in the red boxes or relations. So we can say that a user can be an owner of a document, a user can be a reader of a document, and a user can be an administrator of an organization. And then I only really have one document-level permission modeled here, but you could imagine read, write, view, delete, and many different permissions.


But the way we're expressing this permission is we're saying in order to be able to view a document, you need to either be a reader of the document, you need to be an owner of the document, or you need to be an administrator or have the admin permission on the organization that the document belongs to.


I don't know about you, but for me, this seems a straightforward way to express this kind of authorization logic. But then to try to make it more visual, I have a graphical representation of that same policy on the right-hand side. We can see, that I tried to color coordinate it so that you can see relations and how they relate between users and documents and organizations. And then the interesting one is there's the dotted line that connects view to admin, and so that's just showing you that you sometimes need to traverse the graph, and go through different objects to be able to make the decision that you need to make. In this case, we're considering what a user's role is in the organization to decide whether they have access to the document.


And then down at the bottom, I just have some example relationships. So these relationships are the way to read this is that user Sean has reader permissions on document some document, user Fred has reader on the same document, user Jill has owner on the same document. So we're setting up edges in a directed graph where we're relating a subject, which is often a user, back to a resource which in this case is either a document or an organization. And then you'll also see that we explicitly bind the organization and the document together with this relationship down on the bottom of the relationship grid.


By combining these relationships and the policy which allows us to interpret them, we can express in a natural way very, very complicated authorization schemes.


So Google Zanzibar, what is it good for? I've already mentioned that it's very scalable with traffic, with requirements, and also across geographies because it's built on top of a distributed database. It's very flexible. So within relationship-based Access Control, you can model multiple other authorization paradigms, for example, ABAC, which is attribute-based access control, or RBAC, role-based access control. Facebook wrote a paper about how you can model all of ABAC in RBAC.


You also get a single view of permissions for all services. So this is how Google is accomplishing that thing that I showed earlier where Gmail can understand access that only sensibly exists in the document service.


One of the super powerful things you can do when you have a system based on a graph like this is you do reverse index questions, for example, what can this user access, right? So if user Jake is talking to my web app, what documents can Jake see? If you think about how you would implement that in your own code or a policy engine, you can see how that would be pretty technically complicated.


Finally, it gives you a natural way to express permissions as long as the permissions that you're trying to express are dealing with relationships between people and other people, or people and data, or data and other data.


One of the downsides to adopting something like Zanzibar is that it's hard to do with data that's only available at request time. So for example, if your permission decision needs to take into account the IP address of the calling user, you don't have the IP address to store as a relationship ahead of time, so you can't use that in a graph walk or a graph traversal.


We do have a solution to that that I'll get to in a little bit, but in stock Zanzibar, in core Zanzibar that they write about in the paper, that's difficult. And then finally, it's just yet another distributed system to run. So if authorization is very important to you and a very critical piece of your business, then you'll run that distributed system. But often you might say, well, you know, I'm kind of up to my ears already in distributed systems, so I don't want to bring this in as well.


If you're interested in learning more about Zanzibar, I gave a[full 60-minute talk](https://authzed.com/blog/what-is-google-zanzibar) just about the paper and the various sections of the paper for New York City's "Paper We Love" Meetup. There's a QR code there, but it's highly technical, and I break down all of the different distributed systems techniques that Zanzibar uses to get the performance and the uptime that we talked about a little bit earlier.


I'm an advocate of choosing the right tool for the right job. There's that phrase where if all you have is a hammer, everything looks like a nail. We don't just have hammers in authorization; we have other things. But just thinking about what the various things might be good for, I mentioned earlier that network-level or HTTP-level filtering because you have all of the data at request time is maybe a really good fit for a policy engine. And if you have to do a comparison of values, right, so if you're trying to say like is the content or the contents of this shopping cart greater than $100 and taking that into account when you do your permissions checks, that might also be more suited for a policy engine.


But where does Google Zanzibar shine?


Google Zanzibar shines when you're doing traditional role-based access control because you're able to associate the user with those roles and then interpret those roles all in a single place. It's great for RBAC with user-defined roles, so if you want to give your customers or your users control of what those things mean, you can do that with Google Zanzibar as well. We have an example of that in our playground.


If you want a single consistent view of permissions everywhere, Google Zanzibar is great for this, right? It's a network service, it's centralized, it has all of the data ahead of time, and it's great at being able to do that. Finally, if you need to feed billions or trillions of facts or relationships to your policy solution to make a decision, that's difficult to do with a policy engine. So if you try to check out what the research is on scaling Datalog to billions or trillions of facts, you'll find that this is actually cutting-edge Datalog stuff and isn't supported everywhere.


And then on the Zanzibar side, do have a little yellow warning sign there because Google Zanzibar isn't good at those things, but we've built a Google Zanzibar open-source service called SpiceDB, which is everything that the Zanzibar paper talks about plus a little bit more.


SpiceDB is our open-source love letter to Google Zanzibar. You can find it at[Authzed](https://authzed.com/spicedb) or[GitHub](https://github.com/authzed/) . It's fully open-source, it's Apache 2, which is a very liberal license.


SpiceDB has about 4,000 GitHub stars, and over 3,000 commits, we have 1,500 users on our Discord who are in there talking about authorization software, and we have over 40 contributors from multiple companies across the industry.


What I've been kind of alluding to is we have a thing called caveats which allows you to attach small bits of policy to individual relationships, and I have an example of that on the next slide.


We also have a few additional APIs that make it easier to work with this solution, giving you those reverse indexes that I talked about and giving you information about how permissions are changing over time. And we've also got a thoughtful, great schema language that we're constantly being told is sort of industry-leading.


And we want you to come to contribute, to use it, to do whatever you want to do, but just get involved in SpiceDB and the whole Google Zanzibar movement.


As promised, I have an example of caveats. So in this example, we just have a permission system where we're trying to decide who can unlock a car. In this case, the owner of the car can always unlock it, but the cleaner of the car might only be able to unlock it on certain weekdays. And then we have a single relationship where we say that user Jake, in this case, I'm the cleaner of the car, is only allowed to be the cleaner on Sundays. And then the car, the specific car that we're talking about in this case, is the Toyota Camry. I don't know why I picked that, I just did. So then we can see that as we're traversing this graph, we'll take into account what day of the week it's being called on.


This is really how we can fuse at scale. I can't talk about all of them because some of them are still in stealth mode or they haven't publicly disclosed that they're using us, but we have a few that are on our website under case studies. In terms of the metrics, we're very proud of our performance. We've done a lot of performance testing. We're not quite at the five nines that Google talks about with Zanzibar, but we're very, very close. And in terms of latency, we're often faster than what Google reports for Zanzibar, which we're very proud of.


We've also done a lot of work to make sure that SpiceDB is very operable, so it's very easy to run, it's very easy to scale. We have a lot of documentation about how to do that. And we're also very responsive on Discord if you run into any issues.


So, I think that's all the time I have. Thank you so much for your questions. If you have more questions, please come to[Discord](https://discord.com/invite/jTysUaxXzM) , please come to[GitHub](https://github.com/authzed/) , file an issue, or start a discussion. We're very, very active in the community, and we're always looking to help. So, thank you so much.


## Related reading


### [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)


### [Google Zanzibar’s annotated paper](https://authzed.com/zanzibar)


On this page


- What you will learn
- The problem with permissions management, especially RBAC
- Authorization models applied by companies
- Google Zanzibar open-source implementations:
- How to choose your permissions management system
- Transcript
- Related reading
- Understanding Google Zanzibar: A Comprehensive Overview
- Google Zanzibar’s annotated paper


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
