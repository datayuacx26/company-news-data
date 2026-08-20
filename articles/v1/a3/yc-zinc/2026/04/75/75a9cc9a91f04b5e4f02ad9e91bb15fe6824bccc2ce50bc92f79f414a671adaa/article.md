---
schema_version: "1.0.0"
document_id: "75a9cc9a91f04b5e4f02ad9e91bb15fe6824bccc2ce50bc92f79f414a671adaa"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/how-to-build-ai-shopping-agent"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:52.140847+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:419c5a757021b00245b8bf3ff05a5222d7a90d5a900034d5869b96f575175018"
---

# How to Build an AI Shopping Agent: Step-by-Step Guide

Most "AI shopping agent" tutorials stop at product recommendations. You prompt a model, it returns a list, the user goes elsewhere to buy. That's not a shopping agent - that's a search engine with a nicer interface.


This guide walks through building an actual shopping agent: one that takes a user's intent, finds products, and places a real order that gets shipped to a real address. You'll use Claude (or any LLM) as the reasoning layer, Model Context Protocol (MCP) tools for function calling,[Zinc](https://www.zinc.com/) as the execution layer for 50+ retailers, and[MPP](https://www.zinc.com/blog/what-is-mpp) for payment.


By the end, you'll have working code for an agent that can buy something on Amazon, Walmart, or Target and track it to delivery.


---


Zinc Agent


Give your AI agent commerce powers


Connect an agent to Zinc to search products, place orders, track shipments, and handle returns across top retailers.


[Try Zinc Agent](https://agent.zinc.com/)[Read the docs](https://www.zinc.com/docs)


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready


## What you'll build


A command-line shopping agent that:


1. Accepts a natural-language shopping request (` "I need a 6ft HDMI cable under $15"` )
2. Searches across retailers using Zinc's product search
3. Presents options with pricing and reviews
4. Confirms intent with the user
5. Places a real order via Zinc (optionally paying via MPP)
6. Tracks the order and reports status updates


The same pattern works as a Slack bot, a web chat interface, a WhatsApp integration, or an MCP-compatible tool inside Claude Desktop or any LLM client.


**Reference implementation** :[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) is an open-source shopping assistant built on this pattern ([GitHub](https://github.com/zincio/zinc-gpt) ). Clone it for a working starting point.


---


## The architecture


The stack breaks into four pieces:


**1. Model / reasoning layer** The LLM that interprets the user's intent, decides which tools to call, and generates responses. Claude 4, GPT-5, or Gemini 2 all work. We'll use Claude for this tutorial.


**2. Tools (MCP server or direct function calls)** The agent needs tools:` search_products` ,` get_product_details` ,` place_order` ,` track_order` . We'll expose them as MCP tools so the same server works with Claude Desktop, custom agents, or any MCP-compatible client.


**3. Execution layer (Zinc)** This is where the real commerce happens, and it's more than just "place the order." The same Zinc API covers the whole retail stack across 50+ retailers - product search and details, order placement, tracking, and returns - and it absorbs the messy parts (retailer accounts, checkout, payment) behind one interface. All four tools above (` search_products` ,` get_product_details` ,` place_order` ,` track_order` ) are the same API. When the agent decides to buy, Zinc transacts at Amazon, Walmart, Target, or wherever the product lives.


**4. Payment layer** Zinc gives you three ways to fund orders; pick based on how your app moves money:


- **Prefunded wallet.** Top up a Zinc balance and each order draws down against it. Simplest to start.
- **Pay-per-transaction via Stripe Connect.** No prepaid balance - add a` payment` object with` mode: "connect"` and Zinc charges your end-customer's card for the order cost, your margin, and its fee. Every order funds itself.
- **Pay-per-transaction via[MPP](https://www.zinc.com/blog/what-is-mpp) or[x402](https://www.zinc.com/blog/mpp-vs-x402) .** Agent-native: the agent pays inline over[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) with stablecoins or a Stripe SPT, no account provisioned up front.


(See our[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) if you're deciding which protocol to accept.)


---


## Prerequisites


- Node.js 20+ or Bun 1.0+
- An Anthropic API key (or OpenAI/Google equivalent)
- A Zinc API key from[app.zinc.com](https://app.zinc.com/) (or use MPP for keyless payments)
- A Zinc Wallet top-up via Stripe, or a USDC-funded Tempo wallet for MPP


Check your setup:


```text
export     ANTHROPIC_API_KEY  =  "sk-ant-..."
export     ZINC_API_KEY  =  "zn_live_..."
```


---


## Step 1: Set up the MCP tool server


MCP (Model Context Protocol) is Anthropic's standard for exposing tools to LLMs. Building your tools as an MCP server means they work with Claude Desktop, custom agents, and any MCP-compatible client.


Create a new project:


```text
mkdir   shopping-agent   &&     cd   shopping-agent
bun init -y
bun   add   @modelcontextprotocol/sdk axios
```


Create` server.ts` :


```text
import     {     Server     }     from     '@modelcontextprotocol/sdk/server/index.js'  ;
import     {     StdioServerTransport     }     from     '@modelcontextprotocol/sdk/server/stdio.js'  ;
import     {
CallToolRequestSchema  ,
ListToolsRequestSchema  ,
}     from     '@modelcontextprotocol/sdk/types.js'  ;
import     axios     from     'axios'  ;


const     ZINC_API_KEY     =   process  .  env  .  ZINC_API_KEY  !  ;
const     ZINC_BASE_URL     =     'https://api.zinc.com/v1'  ;


const   zinc   =   axios  .  create  (  {
baseURL  :     ZINC_BASE_URL  ,
headers  :     {     Authorization  :     `  Bearer   ${  ZINC_API_KEY  }  `     }  ,
}  )  ;


const   server   =     new     Server  (
{   name  :     'shopping-agent'  ,   version  :     '1.0.0'     }  ,
{   capabilities  :     {   tools  :     {  }     }     }
)  ;
```


---


## Step 2: Define the tools


The agent needs four tools. Add them to` server.ts` :


```text
server  .  setRequestHandler  (  ListToolsRequestSchema  ,     async     (  )     =>     (  {
tools  :     [
{
name  :     'search_products'  ,
description  :
'Search for products across Amazon, Walmart, Target, and 50+ other retailers. Returns structured product data with prices.'  ,
inputSchema  :     {
type  :     'object'  ,
properties  :     {
query  :     {
type  :     'string'  ,
description  :     'Natural language product query'  ,
}  ,
max_price  :     {
type  :     'number'  ,
description  :     'Maximum price in dollars'  ,
}  ,
}  ,
required  :     [  'query'  ]  ,
}  ,
}  ,
{
name  :     'get_product_details'  ,
description  :
'Get detailed info for a specific product URL (images, description, variants, reviews).'  ,
inputSchema  :     {
type  :     'object'  ,
properties  :     {
url  :     {
type  :     'string'  ,
description  :     'Full product URL (e.g., Amazon ASIN link)'  ,
}  ,
}  ,
required  :     [  'url'  ]  ,
}  ,
}  ,
{
name  :     'place_order'  ,
description  :
'Place a real order at a retailer. Ships to provided address. Deducts from Zinc Wallet.'  ,
inputSchema  :     {
type  :     'object'  ,
properties  :     {
product_url  :     {   type  :     'string'     }  ,
quantity  :     {   type  :     'number'     }  ,
max_price  :     {
type  :     'number'  ,
description  :     'Maximum total price in cents'  ,
}  ,
shipping_address  :     {
type  :     'object'  ,
properties  :     {
first_name  :     {   type  :     'string'     }  ,
last_name  :     {   type  :     'string'     }  ,
address_line1  :     {   type  :     'string'     }  ,
city  :     {   type  :     'string'     }  ,
state  :     {   type  :     'string'     }  ,
postal_code  :     {   type  :     'string'     }  ,
country  :     {   type  :     'string'     }  ,
phone_number  :     {   type  :     'string'     }  ,
}  ,
}  ,
}  ,
required  :     [  'product_url'  ,     'quantity'  ,     'max_price'  ,     'shipping_address'  ]  ,
}  ,
}  ,
{
name  :     'track_order'  ,
description  :     'Check the status of a previously placed order.'  ,
inputSchema  :     {
type  :     'object'  ,
properties  :     {
order_id  :     {   type  :     'string'     }  ,
}  ,
required  :     [  'order_id'  ]  ,
}  ,
}  ,
]  ,
}  )  )  ;
```


---


## Step 3: Implement the tool handlers


Map each tool to the matching Zinc API call:


```text
server  .  setRequestHandler  (  CallToolRequestSchema  ,     async     (  request  )     =>     {
const     {   name  ,   arguments  :   args   }     =   request  .  params  ;


if     (  name   ===     'search_products'  )     {
const     {   data   }     =     await   zinc  .  get  (  '/products/search'  ,     {
params  :     {   q  :   args  .  query  ,   max_price  :   args  .  max_price     }  ,
}  )  ;
return     {   content  :     [  {   type  :     'text'  ,   text  :     JSON  .  stringify  (  data  ,     null  ,     2  )     }  ]     }  ;
}


if     (  name   ===     'get_product_details'  )     {
const     {   data   }     =     await   zinc  .  get  (  '/products/details'  ,     {
params  :     {   url  :   args  .  url     }  ,
}  )  ;
}


if     (  name   ===     'place_order'  )     {
const     {   data   }     =     await   zinc  .  post  (  '/orders'  ,     {
products  :     [  {   url  :   args  .  product_url  ,   quantity  :   args  .  quantity     }  ]  ,
max_price  :   args  .  max_price  ,
shipping_address  :   args  .  shipping_address  ,
}  )  ;
return     {
content  :     [
{
type  :     'text'  ,
text  :     `  Order placed. ID:   ${  data  .  id  }  . Status:   ${  data  .  status  }  .  `  ,
}  ,
]  ,
}  ;
}


if     (  name   ===     'track_order'  )     {
const     {   data   }     =     await   zinc  .  get  (  `  /orders/  ${  args  .  order_id  }  `  )  ;
}


throw     new     Error  (  `  Unknown tool:   ${  name  }  `  )  ;
}  )  ;


const   transport   =     new     StdioServerTransport  (  )  ;
await   server  .  connect  (  transport  )  ;
```


Your MCP server is now complete. Run it to verify:


```text
bun server.ts
```


---


## Step 4: Wire up Claude as the reasoning layer


Create` agent.ts` that orchestrates Claude + the MCP tools:


```text
import     Anthropic     from     '@anthropic-ai/sdk'  ;
import     {   spawn   }     from     'child_process'  ;
import     readline     from     'readline'  ;


const   anthropic   =     new     Anthropic  (  )  ;


const     SYSTEM_PROMPT     =     `  You are a helpful shopping assistant. When a user wants to buy something:


1. Use search_products to find options across retailers
2. Present 2-3 good options with price and key features
3. Ask the user to confirm their choice and shipping address
4. When confirmed, use place_order to complete the purchase
5. Return the order ID and expected delivery


Always confirm the order details with the user before calling place_order. Never place an order without explicit user confirmation.  `  ;


async     function     runAgent  (  userMessage  :     string  )     {
const   response   =     await   anthropic  .  messages  .  create  (  {
model  :     'claude-sonnet-4-5-20250929'  ,
max_tokens  :     4096  ,
system  :     SYSTEM_PROMPT  ,
tools  :     [
{   name  :     'search_products'     /* schema from server.ts */     }  ,
{   name  :     'get_product_details'     /* ... */     }  ,
{   name  :     'place_order'     /* ... */     }  ,
{   name  :     'track_order'     /* ... */     }  ,
]  ,
messages  :     [  {   role  :     'user'  ,   content  :   userMessage   }  ]  ,
}  )  ;


for     (  const   block   of   response  .  content  )     {
if     (  block  .  type     ===     'tool_use'  )     {
const   result   =     await     callMcpTool  (  block  .  name  ,   block  .  input  )  ;
return   anthropic  .  messages  .  create  (  {
model  :     'claude-sonnet-4-5-20250929'  ,
max_tokens  :     4096  ,
messages  :     [
{   role  :     'user'  ,   content  :   userMessage   }  ,
{   role  :     'assistant'  ,   content  :   response  .  content     }  ,
{
role  :     'user'  ,
content  :     [
{   type  :     'tool_result'  ,   tool_use_id  :   block  .  id  ,   content  :   result   }  ,
]  ,
}  ,
]  ,
}  )  ;
}
}


return   response  ;
}
```


This is the core loop. The user sends a message. Claude decides which tool to call. The tool runs via MCP. The result goes back to Claude. Claude either calls another tool or responds to the user.


For production, handle multi-turn tool calls, error recovery, and user confirmation flows carefully. Orders should never happen without explicit user intent.


---


## Step 5: Add agent-native payments with MPP


The setup above assumes the agent has a pre-provisioned Zinc API key. That works for a single-tenant app where you control the billing.


For a more dynamic setup where any agent can pay per-order with no pre-existing account, use[MPP](https://www.zinc.com/blog/what-is-mpp) . It rides on top of[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) , so the flow is: agent posts an order, gets a 402 back with the price, pays inline, and the order is placed.


Replace the` place_order` handler with an MPP call:


```text
if     (  name   ===     'place_order'  )     {
const   response   =     await     fetch  (  'https://api.zinc.com/agent/orders'  ,     {
method  :     'POST'  ,
headers  :     {     'Content-Type'  :     'application/json'     }  ,
body  :     JSON  .  stringify  (  {
max_price  :   args  .  max_price  ,
shipping_address  :   args  .  shipping_address  ,
}  )  ,
}  )  ;


if     (  response  .  status     ===     402  )     {
const   challenge   =     await   response  .  json  (  )  ;
const   paymentToken   =     await     payMppChallenge  (  challenge  )  ;


const   retried   =     await     fetch  (  'https://api.zinc.com/agent/orders'  ,     {
method  :     'POST'  ,
headers  :     {
'Content-Type'  :     'application/json'  ,
'Payment-Method'  :     'tempo'  ,
'Payment-Authorization'  :   paymentToken  ,
}  ,
body  :     JSON  .  stringify  (  {
/* same body */
}  )  ,
}  )  ;


const   data   =     await   retried  .  json  (  )  ;
return     {
content  :     [  {   type  :     'text'  ,   text  :     `  Order placed via MPP. ID:   ${  data  .  id  }  `     }  ]  ,
}  ;
}
}
```


The` payMppChallenge` helper uses the Tempo CLI or SDK to sign a stablecoin payment. Read the[MPP guide](https://www.zinc.com/blog/what-is-mpp) for the full flow.


To see this live without writing a line of code,[agent.zinc.com](https://agent.zinc.com/) is a working MPP playground.


---


## Step 6: Handle tracking and order updates


Orders don't complete instantly. You need to track them. Two approaches:


### Polling


Every few minutes, call the` track_order` tool for any active orders and report status changes to the user.


```text
async     function     pollOrder  (  orderId  :     string  )     {
const     {   data   }     =     await   zinc  .  get  (  `  /orders/  ${  orderId  }  `  )  ;
return   data  .  status  ;
}
```


### Webhooks (recommended for production)


Configure Zinc to send webhooks when order status changes. Your agent receives push notifications and can immediately respond in the user's chat.


```text
app  .  post  (  '/webhooks/zinc'  ,     async     (  req  ,   res  )     =>     {
const     {   type  ,   data   }     =   req  .  body  ;


if     (  type   ===     'order.shipped'  )     {
await     notifyUser  (  data  .  client_notes  ,     {
message  :     `  Your order has shipped. Tracking:   ${  data  .  tracking_numbers  [  0  ]  .  tracking_number  }  `  ,
}  )  ;
}


if     (  type   ===     'order.delivered'  )     {
await     notifyUser  (  data  .  client_notes  ,     {
message  :     'Your order has been delivered.'  ,
}  )  ;
}


res  .  sendStatus  (  200  )  ;
}  )  ;
```


` client_notes` is a field you populate when placing the order (e.g., store the user's chat session ID there) so you can route the webhook back to the right conversation.


---


## Step 7: Production considerations


**Order authorization.** Always confirm orders with the user before placing them. The agent should display what's about to be bought, for how much, shipping where, and wait for explicit approval. Never auto-execute without confirmation.


**Spending limits.** Set a per-agent-session spending cap. If your agent is running on a customer's behalf with a delegated budget, enforce it in your application layer, not just in Zinc's` max_price` field.


**Address validation.** Before calling` place_order` , validate the shipping address (format, deliverability). Zinc returns clear errors for bad addresses, but catching them up front saves a failed order cycle.


**Error handling.** Orders can fail for real reasons: out of stock, price spike above` max_price` , retailer outage, card declined. Return those errors naturally in the chat so the user can decide what to do.


**Logging and audit.** Every order should be logged with who authorized it (which user, which session, which conversation). Agents that spend money need a paper trail.


**Rate limits.** Zinc's API has no hard rate limit, but large volumes are queued. For bursty workloads (Black Friday, flash sales), batch or stagger requests.


---


## What to build next


Once the basic flow works, good extensions:


**Multi-product carts.** Users often want multiple items shipped together. Zinc supports multi-product orders in a single` POST /orders` call. The agent should build a cart conversationally before checkout.


**Cross-retailer comparison.** For the same product on multiple retailers, compare price, shipping speed, and seller reputation. Present the best option or let the user pick.


**Recurring orders.** Subscriptions, reorder-on-threshold, anniversary gifts. Cron-driven flows that reuse the same agent logic for non-interactive runs.


**Merchant-native checkout.** If your catalog includes Shopify merchants, integrate[ACP](https://www.zinc.com/blog/what-is-acp) alongside Zinc so your agent can check out natively at ACP-compatible merchants and fall back to Zinc for the rest.


**Chat integrations.** Deploy the same MCP server to Slack bots, Discord bots, WhatsApp, or embed it in a web chat. The core agent logic doesn't change.


---


## Bottom line


A real AI shopping agent has four layers: model, tools, execution, payment. Most tutorials cover the first two. The second two are where the magic actually happens.


- **Model + tools** (Claude + MCP): Well-documented, lots of tutorials, easy to start.
- **Execution** (Zinc): The hard part, and the whole retail stack - search, ordering, tracking, and returns across 50+ retailers, with managed accounts and checkout behind one API. Don't build this yourself.
- **Payment** (Zinc wallet, Stripe Connect, or[MPP](https://www.zinc.com/blog/what-is-mpp) /[x402](https://www.zinc.com/blog/mpp-vs-x402) ): prefund a wallet and draw down, charge your end-customer's card per order via Connect, or let the agent pay inline over HTTP 402.


The complete pattern is what separates a chatbot from an agent. Start with the[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) reference implementation ([GitHub](https://github.com/zincio/zinc-gpt) ), clone it, adapt the prompts and tools to your product, and ship. Validate flows in[test mode](https://www.zinc.com/blog/test-mode) before you place live orders.


For a live demo of an agent actually placing an order via MPP, try[agent.zinc.com](https://agent.zinc.com/) or read[Zinc MPP Integration](https://www.zinc.com/blog/mpp) . For retailer-specific execution, see the[Amazon Shopping API guide](https://www.zinc.com/blog/amazon-api) . For the full context on where this fits in the broader agentic commerce picture, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) .


[Get started with Zinc](https://app.zinc.com/) or read the[quickstart docs](https://www.zinc.com/docs/quickstart) to place your first test order in under 15 minutes.


---


## Frequently Asked Questions


### What's the difference between an AI shopping agent and a product recommendation chatbot?


A recommendation chatbot suggests products; a shopping agent completes the purchase. The difference is whether the system can actually place an order that gets shipped. Most "AI shopping" demos today are just chatbots. Building a real agent requires an execution layer that can order at retailers on the user's behalf.


### Do I need MCP to build an AI shopping agent?


No. You can use direct function calling with Claude, GPT, or Gemini. MCP makes your tools portable across clients (Claude Desktop, custom apps, any MCP-compatible host) but direct function calling works fine for single-app deployments.


### How does the agent actually place an order at Amazon or Walmart?


It doesn't place the order directly. It calls[Zinc's API](https://www.zinc.com/) , and Zinc handles the retailer-specific checkout, login, and payment. Your agent sees a single` POST /orders` call. Zinc handles 50+ retailers behind that single endpoint.


### Can the agent pay without me pre-loading a Zinc Wallet?


Yes - two ways. With **Stripe Connect** , you skip the prepaid balance entirely: add a` payment` object with` mode: "connect"` and Zinc charges your end-customer's card directly for each order (order cost + your margin + fee). With **[MPP](https://www.zinc.com/blog/what-is-mpp) or[x402](https://www.zinc.com/blog/mpp-vs-x402)** , the agent posts an order, gets HTTP 402 back with a price, and pays inline with stablecoins or a Stripe SPT. Either way, no prefunded Zinc balance is required.


### How do I prevent the agent from placing unauthorized orders?


Always confirm orders with the user before calling` place_order` . Enforce spending limits in your application code. Use Zinc's` max_price` field to cap the maximum total.


### Which LLM is best for shopping agents?


Claude 4, GPT-5, and Gemini 2 all work. Claude tends to be better at multi-step tool use and honest uncertainty handling. GPT handles complex conversations naturally. Gemini is strongest on visual product inputs. Pick based on your team's familiarity and what else your app uses.


### Is there an open-source reference implementation?


Yes.[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) is an open-source shopping assistant built on the pattern described in this article ([GitHub](https://github.com/zincio/zinc-gpt) ). Clone it, adapt it, and ship faster than building from scratch.


Zinc Agent


Give your AI agent commerce powers


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready
