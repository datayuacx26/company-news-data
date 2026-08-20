---
schema_version: "1.0.0"
document_id: "33a20a79d7842265e6294d12d9a8b715b148f0fb610d580454671a2339b221fa"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/over-1-million-q-and-a-cached-and"
published_at: "2025-05-01T14:16:14+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:1146351da1ffb27e4305cf1aed160bb322ca2f66a34060ac555c2775d29ffaba"
---

# 🚀 Over 1 million Q&A cached and counting

# 🚀 Over 1 million Q&A cached and counting


### The Universal Remote Cache now has over a million unique LLM prompts and responses. You can retrieve them at no cost whenever you replicate research at Expected Parrot.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


May 01, 2025


We’re excited to share a milestone:


Over 1 million unique questions and answers have been added to the Universal Remote Cache


You can view this and other metrics for the Universal Remote Cache at our


[cache stats page](https://www.expectedparrot.com/cache-stats) :


This page is updated daily to show total cache entries and total savings (costs avoided whenever a stored response was retrieved instead of resending an API call). The information is available for each language model used, so you can also tell at a glance which models are popular for research projects.


*Thanks for reading! Subscribe for free to receive new posts and support our work* .


## What is the Universal Remote Cache?


The Universal Remote Cache (“URC”) is a collection of all the unique prompts (user prompts and system prompts) that have been sent to language models via Expected Parrot, together with all the responses that were returned. It is a shared resource that is available to all users for free whenever you run a survey at Expected Parrot.


## How it works


When you run a survey at Expected Parrot your results draw from the URC by default. This means that if your survey includes any prompts that have been run before—by you or another user—the stored responses to those prompts are retrieved from the URC and included in your results, at no cost to you. A new response is generated for each of your prompts that has


*not* been run before, and added to the URC. If you want to generate all fresh responses instead you can turn this default off when you run your survey.


Your prompts and responses are shown at the


[Cache](https://www.expectedparrot.com/home/remote-cache) page of your account; details on costs (and costs avoided) in generating them are available at your


[Jobs](https://www.expectedparrot.com/home/remote-inference) and


[Transactions](https://www.expectedparrot.com/home/transactions) pages. To show how it works, below we run a simple question with some models and check the job, transactions and cache details. Then we re-run it and verify that the cached responses were retrieved and no credits were used.


Running a survey in notebook:


The survey job is added to the


**Jobs** page with information about credits used to generate the results:


When we re-run the question we can see that no credits were consumed for the second job:


At the


**Cache** page we can verify that 3 entries were created and then retrieved:


We can generate fresh responses by passing a parameter


` fresh=True` to the


` run()` method:


We can see that credits were consumed again to generate the new responses:


The cache has new entries (fresh unique responses to existing prompts are automatically indexed):


## **How remote inference works**


This caching of prompts and responses happens automatically whenever you run a survey using EDSL. The URC is used whenever you run a survey “remotely” at the Expected Parrot server; when you run a survey “locally” on your own computer your prompts and responses are cached on your computer instead.


To activate remote inference, navigate to the


[Settings](https://www.expectedparrot.com/home/settings) page of your account and confirm that the


*remote inference* option is toggled on. This allows you to run your surveys at the Expected Parrot server using your own


[API keys for LLMs](https://www.expectedparrot.com/getting-started/edsl-api-keys) and/or an Expected Parrot key. Learn more about how


[remote inference](https://docs.expectedparrot.com/en/latest/remote_inference.html#remote-inference) works.


## **Features**


Features of the Universal Remote Cache include:


-


**Free access:** The URC is free to use and available to all users, regardless of whether you are running surveys at Expected Parrot with your own keys for language models or an Expected Parrot key.


-


**Free storage & retrieval:** There is no limit on the number of responses that you can add to the URC or retrieve from it.


-


**Automatic updates:** The URC is automatically updated whenever a survey is run at Expected Parrot.


-


**Multiple responses:** If a fresh response is generated, it is automatically added to the URC as well, with an iteration index.


-


**No deletions:** You cannot delete entries from the URC.


-


**Only verified responses:** You cannot manually add entries to the URC. The only way to add responses is by running a survey at the Expected Parrot server.


-


**Privacy:** The URC is not queryable, and no user information is available.


-


**Sharing & reproducibility:** A cache for a survey is automatically attached to each results object, which can be shared with other users. A green checkmark at Coop indicates to other users that your results are verified and can be retrieved from the URC for free:


*Note:* The URC is not available for local inference (surveys run on your own machine).


## **Frequently asked questions**


**How do I add responses to the URC?** This happens automatically when you run a survey at the Expected Parrot server.


**How do I retrieve a stored response?** When you run a question you will retrieve a stored response by default if it exists. If you want to generate a fresh response you can use


` run(fresh=True)` .


**How do I know whether I will retrieve a stored response?** Results that were generated at the Expected Parrot server will show a verified checkmark ✓ at Coop. If you rerun a survey with verified results you will retrieve stored responses by default.


**Is the URC queryable? Can I check whether there is a stored response for a question?** No, the URC is not queryable. You will only know that a response is available by rerunning surveys with verified results.


**Can I see which user generated a stored response in the URC?** No, there is no user information in the URC.


**Can I add my local cache to the URC?** No, this is not allowed. The purpose of the URC is to provide a canonical, verified collection of responses to allow researchers to be confident in results and easily reproduce them at no cost. By only allowing responses generated at the Expected Parrot server we can verify the results that are reproduced.


**What if I want to run a survey remotely but do not want my responses added to the URC?** This is not allowed. Any new or fresh responses generated at the Expected Parrot server are automatically added to the URC. If you do not want to add responses to the URC you must run your surveys locally.


**Can I access the URC when I run a survey locally?** No, the URC is only available when running a survey remotely. However, you can pull entries from your own cache (responses that you generated or retrieved from the URC) to use them locally at any time.


**Can I delete a response in the URC?** No, this is not allowed.


**What happens if I delete my account?** Any remote cache entries that you generated will remain in the URC. All other account information will be deleted.


*Please send us other questions about how it works!*


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
