---
schema_version: "1.0.0"
document_id: "e1611d33eb2ac9fdd7417dab66f405104e7073a49cef903ac72b8dbe3c9ef6f9"
company_key: "yc-openbuilder"
company: "OpenBuilder"
source_id: "yc-openbuilder-news-import-e5bde0fe230a"
canonical_url: "https://www.easycode.ai/blog/plan-and-build-features"
published_at: "2024-04-23T00:00:00+00:00"
first_seen_at: "2026-07-27T10:56:15.564414+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:9116a13f47e6a0ce41edbb80b768f446cb6c06dd4d7a7f4e6b4dd24126c2d0b4"
---

# Learn EasyCode: Plan & Build Features

> In this series, we will explore EasyCode features and specific use cases that developers use to build faster.


## Planning Features


You have a feature/task, but you don't yet know:


- *where to get started in the codebase - which files are relevant?*


- *what kind of changes to make - what does solution look like?*


"Plan A Feature" can be used to get


**a list of files**


that needs to be added/modified, and the


**type of changes**


to make within these files.


💡 Note, the goal is to get a high level understanding of solution shape, ie,


**a roadmap**


; NOT the exact code to implement the feature. For implementation, see the “Building Features" section below.


You can either provide very high level prompt to the AI such as:


*➡️ Add the ability to delete multiple items*


Alternatively, you can be more specific in what you'd like to see in the solution, such as:


*➡️ Add the ability to delete multiple items, re-use the logic in*


*` **@Delete.jsx**`*


Once you receive the first answer, quickly review it to make sure it's following the right direction. You may notice that EasyCode:


- 🔎 found some files that you may not have considered ->


**take a look at these files to enhance your understanding.**


- ❓ suggested a solution, but you don't understand ->


**ask for clarifications or explanations as a follow up question.**


- 🔪 suggested an incomplete solution, due to missing details or requirements


**-> add the additional details or requirements as follow up questions.**


- ↪️ suggested a valid approach, but you want to take a different approach


**-> Tell the AI the approach you want in follow ups.**


- 💯 suggested the right High Level plan


**-> start implementation.**


## Building Features


In general, 2 things improve the quality of the AI answer:


- detailed prompt with requirements and instructions


- specify (file level) context to the AI


**Quick tip**


: Use the '@' system to tell AI exactly which files to consider as context.


Imagine we want to add a new feature where we can bulk delete multiple items in a list.


❌


**Bad Prompt**


: Add ability to bulk delete items.


🆗


**Decent Prompt**


:


` **@Item.jsx**`


****


` **@Lists.jsx**`


****


` **@Delete.jsx**`


Add ability to bulk delete items.


**✅ Good Prompt:**
