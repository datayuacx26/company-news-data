---
schema_version: "1.0.0"
document_id: "00dc70c82279c94cbb4937c15bd03e781dcfccf2235099441bdc0da774a1e4c0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety"
published_at: "2024-09-05T12:21:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:3c508834e04011dc9e558c577506fa73c2818896e483d4582428b919d496c556"
---

# Consistency is the Key to Performance and Safety

The[Zanzibar paper](https://authzed.com/zanzibar) (on which[SpiceDB](https://authzed.com/spicedb) is based) describes a novel and revolutionary approach to answering permissions questions: Unlike some solutions that came before it, Zanzibar’s focus was not simply on performance, scalability, or correctness, but rather a combination of all of these characteristics into one manageable, global solution (that’s why its formal title is: “Zanzibar: Google’s Consistent, Global Authorization System”).


Being a global solution intended to run as a single service across multiple regions, the Zanzibar team was confronted with a problem: *strong consistency would be key to ensuring correctness, but caching would be necessary for performance* . But consistency and caching are often diametrically opposed, with cached data often being stale and therefore potentially providing inconsistent results.


How then does SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact.


## Some staleness allowed


The first major realization of the authors was that, for the majority of permissions requests, some level of staleness is perfectly acceptable.


At first glance, allowing for **any** staleness in a security check seems wrong: if I ask whether a user can view a document, and their access was just removed, we should of course answer “no.”


However, the team had an important insight: it is not whether the answer to a permissions question is stale, but rather the **maximum** possible staleness that matters and on **which** permission.


Let’s take our previous example of a user viewing a document and modify it slightly: if the user lost access to the document **within the last few seconds** , then allowing them to view the document does not matter … so long as the contents of the document have not changed. After all, the user just had access to the document (and its contents) a few seconds ago, so letting them view the contents again does not cause a security incident.


What would happen, however, if the contents of the document **did** change since the user last viewed the document? The updated contents of the document could potentially leak sensitive information to the user, who should not have been able to view the new information. The paper labels the user in this scenario as a “ **new enemy** ”: while they were previously not an enemy, they have become one due to the change in the document’s contents.


At a glance, the solution to this case is straightforward: simply ensure the cached results for the` CheckPermission` are not returned for this case. However, that would entail either tracking on the server whenever **any** user’s access changed for the document, or it would remove the ability to use the cache for the permission entirely.


This was the second major realization of the authors: one solution to the new enemy problem is to simply change the **bounds** of the staleness of the cache.


To see how this works, let’s take the above example: let’s say the user had access at time` T` , lost access at time` T+1` , and the contents of the document were updated at time` T+2` . If the cache is at time` >= T+2` , it is perfectly fine to use the cache for checking whether the user has view access.


To ensure the cache could continue to be used, the concept of a` ZedToken` (known as a` Zookie` in Zanzibar) was added. A` ZedToken` encodes the **minimal point in time** at which the cache is considered “valid.” Whenever the **contents** of a document are changed, a ZedToken can be requested from the service and stored **alongside** the document. This ZedToken is then sent alongside the` CheckPermission` to tell SpiceDB that the caching used for the permission must take the updated contents into account by ensuring the result in the cache was created at (or after) the point in time at which the contents of the document were changed.


## Choosing your consistency


Using a minimum point-in-time for cache checking solves the new enemy problem for the **view** permission, but there was one other case necessary to solve: that of a different permission.


Take our previous example but change the permission to the ability to **delete** the document: if the user had their ability to delete the document revoked a few seconds ago, and tries to delete the document now, the operation should fail. If it does not, then the user was able to modify a resource for which they no longer have access.


This led to the third major realization: while higher consistency is necessary, it is only needed in **select circumstances** .


A key design was therefore added: permissions operations such as` CheckPermission` offer a **per request configurable** consistency level, to ensure that the **caller** can choose which level is most appropriate.


In SpiceDB, this option is exposed via the` consistency` block, which allows for a number of options:


shell


1


2


3


4


```text
bool minimize_latency = 1; // The default
ZedToken at_least_as_fresh = 2;
ZedToken at_exact_snapshot = 3;
bool fully_consistent = 4;


```


shell


1


2


3


4


```text
bool minimize_latency = 1; // The default
ZedToken at_least_as_fresh = 2;
ZedToken at_exact_snapshot = 3;
bool fully_consistent = 4;


```


SpiceDB always defaults to` minimize_latency` , as we’ve seen above that the majority of requests will be perfectly safe with the small amount of enforced maximum staleness (by default, ~5s).


For those requests where bounded staleness is required, the` at_least_as_fresh` option is used to specify to SpiceDB – via the` ZedToken` – the minimum point-in-time necessary for the cache to be valid.


For those requests where the caller wants to execute the call at the same revision as a previous call, the` at_exact_snapshot` option allows for specifying an exact point-in-time for making the request.


What then about` fully_consistent` ?


## Full consistency and you


Sometimes the safest approach to checking a permission is to require the **latest** and most consistent possible answer. For these scenarios, SpiceDB provides the` fully_consistent` option, which will force SpiceDB to load the most recent data stored when computing a result.


At first glance,` fully_consistent` seems the obvious choice to use whenever a permission must be checked at the latest version of data. However, it comes with a significant cost: **performance** .


` fully_consistent` calls bypass the cache in SpiceDB at **all times** , which can put significant load onto the service.


Thus, the recommendation for making fully consistent requests is: **do not do so** unless there is tolerance for **zero** staleness for the **particular** permission.


## What about lookups?


SpiceDB expands upon the list of APIs described in the Zanzibar paper by also supporting the ability to lookup **lists** of resources for a permission.


While these APIs are incredibly powerful, they raise a common question: how do we prevent the new enemy problem for a **set** of resources, when the ZedTokens are stored for **each** resource?


The answer is fairly simple: **don’t!**


As we saw above, the new enemy problem applies to the **contents** of a document that have changed since a user lost access to the document itself. While it is true that the title of a document **may** contain sensitive information added after the user lost access, it is unlikely to truly be a source of concern.


Thus, when listing resources, it is typically quite safe to make use of` minimize_latency` , as the only real risk is a user seeing the title of a document to which they previously had access a few moments ago. So long as the **view** permission of the document is checked (with a ZedToken) before the user can view the contents, there is no real risk of a new enemy occurring.


## Conclusion


Choosing the appropriate consistency is an important factor in getting the best performance for your SpiceDB integration. If you’re building an integration and want feedback on your design decisions, join our[Discord community](https://authzed.com/discord) . There are other community members actively building with SpiceDB!


On this page


- Some staleness allowed
- Choosing your consistency
- Full consistency and you
- What about lookups?
- Conclusion


## Related


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)


[Engineering Modeling Google Cloud IAM in SpiceDB We often get asked about how you would model Infrastructure as a Service (IaaS) permissions in our SpiceDB Schema Language. Since we know that Google Cloud IAM uses Zanzibar internally, it should be possible to use relationship based access control to get the desired effect. Jan 19, 2023 · 11 min](https://authzed.com/blog/google-cloud-iam-modeling)[Engineering Modeling Google Cloud IAM in SpiceDB We often get asked about how you would model Infrastructure as a Service (IaaS) permissions in our SpiceDB Schema Language. Since we know that Google Cloud IAM uses Zanzibar internally, it should be possible to use relationship based access control to get the desired effect. Jake Moshenko · Jan 19, 2023 · 11 min](https://authzed.com/blog/google-cloud-iam-modeling)
