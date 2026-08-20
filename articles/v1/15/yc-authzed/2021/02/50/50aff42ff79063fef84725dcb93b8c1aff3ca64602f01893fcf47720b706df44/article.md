---
schema_version: "1.0.0"
document_id: "50aff42ff79063fef84725dcb93b8c1aff3ca64602f01893fcf47720b706df44"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/new-enemies"
published_at: "2021-02-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:143cc5c99ac94b2e536fd55420179348a9f224fc0060289425a7c4ccc3209bf3"
---

# Enforcing Causal Ordering in Distributed Systems: The Importance of Permissions Checking

**NOTE:** This blog post makes use of


[Zanzibar terminology and namespace configuration](https://docs.authzed.com/concepts/terminology#equivalance-to-zanzibar-terminology)


. It will be updated in the future to match current Authzed terms and schema configuration


## Intern “Review” Party


It was finally the interns’ last day. My colleagues and I took away their computers, disable their logins, gave them some sage advice about how they wouldn’t be poor forever, and then gently pushed them out the door. Then came everyone’s favorite event of the year: intern review party! We broke out the libations and gathered around the projector to review some of the most hilarious bugs that the interns tried to sneak through code review during the course of the summer. Bill had just brought up Timmy’s particularly egregious lack of locking on a core data structure, and just as the room started to erupt into laughter, it happened.


Timmy had come back after realizing that he forgot his beloved ping pong paddle. His eagerness to join the party quickly turned to embarrassment and shame as he glanced at the projector. He grabbed his paddle and dashed out the door, leaving behind only a vow never to work with us again!


“How could this have happened?”, we asked ourselves. We aren’t bad people! We just wanted to relax, enjoy the conspicuously available ping pong table, and maybe pat ourselves on the back a little for stopping such hilarious bugs from getting into production! Besides, we revoked all of his access before he left. And that’s when we finally remembered:


**The door codes sync with the employee directory every night at midnight.**


## The New Enemy Problem


Thankfully, Timmy and Bill are made up characters, and we’ve never had such a party. But the underlying lesson from the story does teach us an important lesson about permissions checking in distributed systems. In our example, Timmy was able to witness our off-color celebration because there was a replication delay which broke the[causal ordering](https://scattered-thoughts.net/writing/causal-ordering/) requirement between revoking the interns’ access and the start of the party.


Formalized by Google in the[Zanzibar paper](https://research.google/pubs/pub48190/) as: “\[a failure\] to respect the ordering between ACL updates or when we apply old ACLs to new content.” In our case, we applied the old ACL, where Timmy and his friends had access to the office, to the new content of our tasteless party.


One way we could have prevented this problem would be to wait a week before throwing the party. This would give the door controller plenty of time to update, and would have given Timmy plenty of time to head back to school. While this solution would work theoretically, we need to know exactly when the door controllers will update, or exactly when all of the interns are guaranteed to be safely out of town. A failed update or a missed flight could easily break our ordering guarantee.


Another way we could have prevented the problem, would be to actually check that the doors had updated before we began our party. We could have tasked the least junior person on the team to continually check the door with an intern badge until it started rejecting the access requests.


Finally, and most realistically, we could have guaranteed that the new ACLs were in place immediately. We could have simply taken their badges on the way out. This works in the case of a single intern pool and a single office, but how can we guarantee that interns from the other office all had their badges revoked?


This is starting to feel vaguely familiar.


## Latency in Distributed Systems


Anyone who is familiar with the[CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) can tell you that all distributed systems will have to make a tradeoff between **C** onsistency and **A** vailability at some point. In our case, the door controller and the directory service formed an AP distributed system, which is to say they give up some consistency for higher availability. By only syncing once per day, our directory could freely process access revocations without having to first talk to every single door (some of which might be offline) and our doors could continue to grant access without checking with the directory first.


Most popular web applications that are built to scale use some kind of distributed system for their storage layer. Some use replicated SQL databases: AP systems that allow for some replication delay between updates to a single source of truth and a fleet of read replicas. Still others use CP systems that are built on consensus and voting, such as etcd and modern versions of Dynamo. These systems can be made to be seen as fully consistent to any outside observer, but can get into a state where enough faults will cause them to become unavailable.


In all cases, there is some time in between when some new data is proposed, and when that data will be seen by all callers.


In many applications, permissions and the data they protect form an ad-hoc distributed system. Maybe policies are distributed to each application server by a central policy repository. Maybe permissions are checked in an entirely separate service, such as our service[Authzed](https://authzed.com/) . Even when permissions are stored directly alongside the data in a way where they can be mutated atomically, the policy that evaluates them usually lives in code or comes from a policy server. If any of the policy evaluation machinery is out of date, you can again introduce a new enemy!


As you can see, it is very difficult to get permissions checking right in any system with more than one component.


## Content is King


Now that we’ve established that pretty much everything is some sort of distributed system, and that synchronization between parties is our most immediate enemy, what can we do about it? Thinking back to our intern party example, it was actually the party itself that forced a causal ordering requirement. If we hadn’t planned a party, it wouldn’t have mattered if the interns still had access. A party also wasn’t the only thing that could have manifested a causal ordering requirements. Maybe there was a physical security audit happening that evening. Maybe the IT team was coming to install a dashboard which prominently displayed sensitive financial information to which only full-time employees were privy.


In all cases, it was a change to the circumstances of the space to which the interns were purportedly no longer allowed access, which created a causal ordering dependency. Perhaps we can draw a parallel with application data?


It turns out that nobody should really care if someone is given access to an exact copy of something that they once had access to. There is no new information to be gained. If they had saved a copy, or had photographic memory, the end state would end up no different. Leveraging this new flexibility, we can create a system to generically enforce causal ordering between mutations to data, and the permissions that should be used to check that specific version of the data.


## I Did It All for the Zookie


In our system Authzed and the Zanzibar paper on which it is based, there is the option to get an opaque token to a snapshot of the permissions as evaluated at a single point in time. This token is called a Zookie (a portmanteau of Zanzibar and cookie). By atomically combining a token which represents the exact permissions used to protect a specific version of the content, and the content itself, we can make sure that the permissions we use to check access to that content in the future is **at least as fresh** as the permissions when the content was created.


Concretely, Authzed allows one to pass a Zookie when making a permissions[check request](https://docs.authzed.com/concepts/check) , and guarantees that the policy and individual relationships used to compute the answer will be at least as fresh as the Zookie presented requires. Now, our code follows approximately the following pseudocode convention every time there is an update to content:


py


1


2


3


4


5


6


```text
def    write_content  ( user, content_id, new_content  ):
is_allowed, zookie = authzed.content_change_check(content_id, user)
if   is_allowed:
storage.write_content(contentd_id, new_content, zookie)
return   success
return   forbidden


```


py


1


2


3


4


5


6


```text
def    write_content  ( user, content_id, new_content  ):
is_allowed, zookie = authzed.content_change_check(content_id, user)
if   is_allowed:
storage.write_content(contentd_id, new_content, zookie)
return   success
return   forbidden


```


And when accessing the data, we use the following pseudocode:


py


1


2


3


4


5


6


```text
def    read_content  ( user, content_id  ):
content, zookie = storage.get_content(content_id)
is_allowed = authzed.check(content_id, user, zookie)
if   is_allowed:
return   content
return   forbidden


```


py


1


2


3


4


5


6


```text
def    read_content  ( user, content_id  ):
content, zookie = storage.get_content(content_id)
is_allowed = authzed.check(content_id, user, zookie)
if   is_allowed:
return   content
return   forbidden


```


Now we have a mechanism for enforcing that we will never give access to a version of the content to which the user has had their access revoked!


## No, You May!


You may have picked up on the fact that there are probably some inconsistencies that can be introduced by using a version of the permissions that are at least as fresh, but not always the exact current permissions. So what are they?


Let’s say at some point a user is granted access to a document in a way that doesn’t cause the document to store a new zookie. It may take some time for that access grant to propagate everywhere, and we may issue some **false negative** responses to check requests. This is an explicit choice to improve the performance of the system, while always guaranteeing that no false positives are ever issued.


Because permissions mutations in Authzed also return a Zookie, if you can easily identify the content to which permission is being granted, you can optionally update the zookie on the content when the new permissions are granted. This will enforce a causal ordering between the permissions change and the next access request! This can alleviate the problem with false negatives, by trading off higher load to the datastore which stores the content. This will make sense for some use cases, but not for others.


## Shameless Plug: Onboarding Now


By presenting a generalized solution to solving the new enemy problem in a distributed permissions checking system, Authzed may be the perfect fit for your application! Nobody wants to have to implement Yet Another Permissions System, and our platform can help you avoid that pain, while also giving you a powerful platform that will set your application up to scale, both geographically and with traffic!


If you would like to learn more, we’re onboarding design partners now!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Intern “Review” Party
- The New Enemy Problem
- Latency in Distributed Systems
- Content is King
- I Did It All for the Zookie
- No, You May!
- Shameless Plug: Onboarding Now
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
