---
schema_version: "1.0.0"
document_id: "3557094e4d998302e87c579c7e3a172039aaf4fe82820c2ea21c00383d805136"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/challenges-building-ai-agent-shopify-mcp"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:0fbdd206403e13dc275b99cefd68b3fa4a336963e5b064f6823dc445bd21f3d8"
---

# The hard parts of building an AI Agent on Shopify MCP

## Introduction


MCPs are often described as **plug-and-play** . You simply give new real-world functionality to an AI Agent or an AI app. And it is true in a sense. It is straightforward to connect to an MCP and, once it is connected, the AI magically starts using it.


However, as is usually the case with AI apps, it is extremely straightforward to build an **almost-magical demo** with near-zero effort but then notoriously difficult to get to **production-level performance** . In other words, as soon as your demo is over and real people start interacting with your AI it turns out things don’t work as intended.


## What is MCP? A quick refresher


In this blog post, let’s take the[Shopify MCP](https://shopify.dev/docs/apps/build/storefront-mcp) as an example. Once the Shopify MCP is connected to your AI Agent, it becomes aware of the existence of several **tools** . What are MCP **tools** then?


A **tool** is telling your AI Agent: *look, here is a way for you to interact with the real world* .


## AI Agent for Ecommerce


Imagine you’re building a conversational AI Agent for Ecommerce. A very useful thing would be to be able to check if a particular product is available in a particular store. The Shopify MCP provides a tool for just that - it is called` get_product_details` .


Here is what the tool looks like to your AI Agent:


```text
{
"type": "function",
"function": {
"name": "get_product_details",
"description": "Look up a product by ID and optionally specify variant options to select a specific variant.",
"parameters": {
"type": "object",
"properties": {
"product_id": {
"type": "string",
"description": "The product ID, e.g. gid://shopify/Product/123"
},
"options": {
"type": "object",
"description": "Optional variant options to select a specific variant, e.g. {\"Size\": \"10\", \"Color\": \"Black\"}"
}
},
"required": [
"product_id"
]
}
}
}
```


` Description` tells your AI Agent what the tool **does** . This particular description is quite generic but you can infer that product availability and other details can be looked up this tool.


` Properties` tell us that we will be looking up not just products (by **product ID** ) but particular **variants** by specifying parameters such as size and colors. That’s very important for products such as clothes or home decorations.


> The above descriptions have been written by the Shopify team but there is nothing forcing you to use them when interacting with the MCP. Those would be the descriptions you use **by default** when interacting with the MCP. But there is nothing stopping you from editing them or creating your own versions of Shopify tools.
>
>
> I believe that’s an important detail showing that we shouldn’t expect MCPs to work out of the box for all use cases. Rather, it is up to the AI Agent builder to make them work for their use case.


## Use tools to do useful things - checking product availability


We connect MCPs to AI Agents so that they hopefully do **useful things in the real world** . As mentioned before, in an Ecommerce setup, being able to check if a particular product is available is a very useful thing to be able to do. Any AI experience that helps users buy products, must be able to check in real time if the product they’re about to recommend or add to user’s cart is still available and in stock.


Let’s say we want to check if *Private Soirée High Waist Skort* is currently available in size S in the[Miss Lola](https://misslola.com/) store. Let’s assume that we already know the` product_id` is` 687386257001` 9 (it was previously fetched using the` search_shop_catalog` tool which we’re not going to cover here).


What we want to do is call the` get_product_details` tool with` product_id` set to` 6873862570019` and` Size` set to` S` . During conversations with users, **the AI Agent would do that on user’s behalf** .


Below is the exact Python code that would be run in that case:


```text
import httpx


MCP_ENDPOINT = "https://misslola.com/api/mcp"


payload = {
"jsonrpc": "2.0",
"method": "tools/call",
"id": 1,
"params": {
"name": "get_product_details",
"arguments": {
"product_id": "gid://shopify/Product/6873862570019",
"options": {
"Size": "S",
}
},
},
}


headers = {"Content-Type": "application/json"}


with httpx.Client() as client:
response = client.post(MCP_ENDPOINT, json=payload, headers=headers)
print(response.json())
```


> The Shopify MCP (and API), just like any Shopify store, is available and free for everyone to use which means you can try and run the above snippet yourself.


Below is the response you get when running the above code (truncated for visibility). It is exactly the same response that an AI Agent would receive from Shopify when calling the` get_product_details` tool with those parameters.


```text
{
"product": {
"options": [
{"name": "Size", "values": ["S", "M", "L"]},
{"name": "Color", "values": ["Navy"]}],
"price_range": {"currency": "USD", "max": "19.99", "min": "19.99"},
"product_id": "gid://shopify/Product/6873862570019",
"selectedOrFirstAvailableVariant": {
"available": true,
"currency": "USD",
"image_url": "https://cdn.shopify.com/s/files/1/2723/4846/files/skirt-11.07_InStudio36802.jpg?v=1701103329",
"price": "19.99",
"title": "S / Navy",
"variant_id": "gid://shopify/ProductVariant/40128009437219"},
"title": "Private Soirée High Waist Skort - Navy",
"url": "https://www.misslola.com/products/private-soiree-navy-high-waist-skort"}
}
```


We can see the following information about the product:


- it is currently available
- it comes in three sizes:` S` ,` M` and` L`
- it comes in one color only:` Navy`


The information is compiled under the` selectedOrFirstAvailableVariant` key which could suggest that if the product we’re looking for is not available what we’ll see here is the *first closest available product* . Let’s check if that the case.


Let’s now call the` get_product_details` with the following payload:


```text
{
"product_id": "gid://shopify/Product/6873862570019",
"options": {
"Size": "M"
}
}
```


And we received exactly the same output as before (truncated even more for visibility).


```text
{
"selectedOrFirstAvailableVariant": {
"available": true,
"currency": "USD",
"price": "19.99",
"title": "S / Navy",
"variant_id": "gid://shopify/ProductVariant/40128009437219"
}
}
```


A natural conclusion to draw would be that the skirt **is not available in size M** . I wouldn’t blame anyone or any AI Agent for concluding that.


However, let’s try calling` get_product_details` one more time, this time with the following payload:


```text
{
"product_id": "gid://shopify/Product/6873862570019",
"options": {
"Size": "M",
"Color": "Navy"
}
}
```


And now the output we’re getting is this!


```text
{
"selectedOrFirstAvailableVariant": {
"available": true,
"currency": "USD",
"price": "19.99",
"title": "M / Navy",
"variant_id": "gid://shopify/ProductVariant/40128009469987"
}
}
```


Size M is available after all!


> Note the different` variant_id` returned.


What we discovered here is that for the` get_product_details` endpoint and its` selectedOrFirstAvailableVariant` response to work correctly, **all` options` values must be provided** . And that’s also in the case when one of the keys has *only 1 possible value* (as in the case of` Color` :` Navy` ).


That particular requirement would be very difficult to infer from tool descriptions, even with a powerful model such as GPT-5. As a matter of fact, if it did infer that, we would need to call it a **hallucination** because it would be making assumptions *beyond the context it was given* .


## Takeaways for AI Agent builders


The above example clearly shows that treating MCPs as **plug-and-play** is not an option for any implementations more complex than a simple demo. A useful initial question to ask yourself is this: for all scenarios I care about, are the following **unambiguously clear** based on MCP descriptions:


- which tools should be called,
- in what order,
- with what parameters,
- how their responses should be interpreted


I am not trying to say that there’s anything wrong with the Shopify API itself or that the MCP is incomplete or misconfigured. On the contrary, it’s one of the best MCPs to work with!


What I am saying is what is true of every AI implementation: you can create a great demo in 5 minutes but real engineering work is needed to make it production-ready across a wide range of scenarios way beyond a typical **happy path** .


> Sidenote:
>
>
> ` Color` :` navy` (lowercase) or
>
>
> ` Color` :` Blue` (synonym)
>
>
> also would have returned` Size S` as` selectedOrFirstAvailableVariant` .


## Our take


The example described above is just one of many small issues that must be solved by teams building AI Agents to be deployed **in the real world** .


A successful conversation between a human and an AI Agent might be many messages long where each response involves several tool calls. A single mistake, however subtle, might be enough for the user to be discouraged for good and conclude that AI Agents cannot be trusted yet.


What the above example hopefully illustrated is how much **old-school engineering work** goes into developing something as simple as an Ecommerce AI Agent capable of checking products’ capabilities.


Importantly, the reason why the engineering work is needed is not because LLMs are not capable enough. In order to interact with the real world, they need to interact with systems **designed by humans** with certain assumptions and context baked in. The engineering work is precisely to provide that context to the LLM in an unambiguous way.


## Our approach


What was described in this blog post was one of many issues we tackled with when testing and improving Quickchat’s[Shopping AI Agent](https://quickchat.ai/shopify) . How do we make sure we can identify and prevent a broad range of such issues?


While several other approaches to testing conversational AI Agents exist, **simulations + LLM as a judge** work best for us for the following reasons:


- they test conversations end-to-end - just like both our users and our users’ users do
- they test conversations from several different angles - just like both our users and our users’ users do
- being fully automated they allow for scale needed to identify broad ranges of subtle issues


Our work on these issues has resulted in an increase **from 5.9 to 6.8** on our internal **Shopify AI Agent benchmark** (on a 1-10 scale). Apart from product availability checks, our improvements also focused on:


- Contextual understanding – teaching the AI to pick up subtle hints and translate them into tailored product recommendations.
- Presentation layer – improving formatting, images, and colors so AI recommendations look beautiful.


You can spin up our Shopify AI Agent in 10 seconds for any Shopify store here:[quickchat.ai/shopify](https://quickchat.ai/shopify) .


We’re also on the[Shopify App Store](https://apps.shopify.com/quickchat-ai) .
