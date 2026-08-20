---
schema_version: "1.0.0"
document_id: "088cbf519849b4df9803424d1c519d9b9971a58976298de60ead0931aaf97fdc"
company_key: "yc-openbuilder"
company: "OpenBuilder"
source_id: "yc-openbuilder-news-import-e5bde0fe230a"
canonical_url: "https://www.easycode.ai/blog/get-best-ai-answer"
published_at: "2024-02-23T00:00:00+00:00"
first_seen_at: "2026-07-27T10:56:15.564414+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:47e85fedc8f8ffdcb92c8c966d9dc4d05baf70ff68bd89dd80a512249835ff61"
---

# How to Get the Best Answer from AI

**2 things can improve the quality of the AI answer:**


1. Provide specific (file level) context to the AI


1. Provide specific requirements and instructions


As an example, we will turn into a potentially bad prompt into a better prompt for a hypothetical e-commerce project.


**❌ Initial Prompt**


> Add discounts to products.


- the context is not specific.


- the requirements aren’t clear.


---


**✅ Final Prompt**


> ` /Implement`
>
>
> Add the ability to apply discounts to products that are in cart. Make sure that: - discounts are in percentages - discounts cannot be more than 100% - only one discount can be applied to a product
>
>
> ` @shopping.js`
>
>
> ,
>
>
> ` @shopping-service.js`
>
>
> ,
>
>
> ` @shopping-repository.js`
>
>
> ,
>
>
> ` @Cart.js`


- the context is specific.


- the requirements are clear.


## Specify context


Use the


` '@'`


system to tell AI


**exactly which files**


to consider as context. This produces the highest quality of answers, but requires you to know which files are relevant.


**Improved Prompt**


> ` /Implement`
>
>
> Add discounts to products.
>
>
> ` @shopping.js`
>
>
> ,
>
>
> ` @shopping-service.js`
>
>
> ,
>
>
> ` @shopping-repository.js`
>
>
> ,
>
>
> ` @Cart.js`


✅ the context is specific.


❌ the requirements aren’t clear.


**But what if you don’t know which files to include yet?**


Use


` /Explain`


and


` /Plan`


to quickly research which files are relevant.


### Understand codebase (skip if you know the codebase already)


You may first want to understand how products and carts interact with each other.


> ` /Explain`
>
>
> How does product get added to cart?


### Determine solution plan (skip if you already know how to do it)


EasyCode can offer suggestions on which files to change, and what changes to make.


> ` /Plan`
>
>
> Add the ability to apply discounts to products that are in cart.


## Specify requirements


Provide as much requirements upfront as possible. Imagine you are giving the task to another developer, the more clear you communicate what you want in the beginning, the less back-and-forth there will be.


> ` /Implement`
>
>
>
>
> ` @shopping.js`
>
>
> ,
>
>
> ` @shopping-service.js`
>
>
> ,
>
>
> ` @shopping-repository.js`
>
>
> ,
>
>
> ` @Cart.js`


✅ the context is specific.


✅ the requirements are clear.
