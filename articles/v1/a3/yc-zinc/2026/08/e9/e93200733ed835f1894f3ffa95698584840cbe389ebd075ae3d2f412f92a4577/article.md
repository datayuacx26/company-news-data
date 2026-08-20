---
schema_version: "1.0.0"
document_id: "e93200733ed835f1894f3ffa95698584840cbe389ebd075ae3d2f412f92a4577"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/ai-shopping-prompts"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T19:33:26.686126+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:db09603e1e835b7f033923adcb53b445803f02171c50eb56f5d6cdf668d9488b"
---

# 10 AI Shopping Prompts for Zinc API

**Zinc API prompts** are copy-paste instructions you give an AI agent so it can search retailers, place orders, and pull tracking back through one API.


Most shopping agents stop at product links. These **10 prompts** go further. They use the[Universal Checkout skill](https://github.com/zincio/skills) to compare prices across Amazon, Walmart, Target, and Best Buy, then checkout with a` max_price` cap.


That matters if you are building a personal shopper, a procurement bot, or an HR gift flow. You need prompts that end in a real order, not a recommendation.


Load the skill in Cursor, Claude Code, or OpenClaw via[skill.md](https://www.zinc.com/skill.md) or` npx clawhub@latest install universal-checkout` . Then paste any prompt below.


Here are 10 prompts to get you started.


1. Cheapest AirPods Pro under $220
2. Best 65-inch OLED under $1500
3. Buy 3x from a Target URL
4. Weekly price-drop auto-buy
5. 50 notebooks to 50 addresses
6. Sony WH-1000XM5 price shootout
7. Personal shopping agent, no questions asked
8. iPhone 16 Pro first-party under $1100
9. Back-in-stock sniper
10. New-hire welcome automation


## 1. Cheapest AirPods Pro under $220


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


AirPods emergency run


Left a bud in an Uber? This finds the cheapest Pro 3 under $220 with 2-day shipping from Amazon or Walmart.


[Setup guide](https://www.zinc.com/docs/v2/agent-skills/setup)[GitHub](https://github.com/zincio/skills)


You lost one bud. Or the case died. Either way you need replacements by Friday and you are not opening six tabs to compare Amazon vs Walmart shipping games.


This prompt makes the agent hunt both stores, add shipping to the math, and stop at $220. You approve before it spends a cent. Start here if you want to see search + checkout work on something small.


## 2. Best 65-inch OLED under $1500


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


OLED shortlist, human picks the winner


Big-screen shopping without blind checkout. Top 3 OLEDs under $1500, you say go.


A $1,200 TV is not an impulse buy. You want three real options with seller ratings, not one link the model picked because it sounded confident.


The agent shops Best Buy and Amazon, ranks offers, and waits. You pick. It buys. Your living room and your marriage stay intact.


## 3. Buy 3x from a Target URL


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Three of the same thing, one prompt


Facilities found the SKU. You need 3 to the office, max $89 each, no manual checkout loop.


Someone in ops already found the exact SKU on Target. They just need three units to the office without copy-pasting checkout three times or begging IT for a corporate card login.


Drop the URL, set qty 3, cap at $89 each. One idempotency key per order. Swap in a real address from your CRM instead of "on file."


## 4. Weekly price-drop auto-buy


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Monday morning deal hunter


Stop refreshing product pages. Weekly price check, auto-buy when it hits your number.


You have been manually checking the same product page for months like a weirdo. Let a cron job do it.


Zinc will not wake up at 9am on Mondays. Your scheduler will. It pings the agent, the agent checks Amazon, Walmart, and Target, and buys if price drops below your cap. Set` max_price` so a hallucinated "deal" cannot drain your card.


## 5. 50 notebooks to 50 addresses


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Swag day without the swag trauma


50 Moleskines, 50 homes, one bulk price hunt. Finance gets metadata on every order.


HR said "send everyone a nice notebook." They did not mean "spend your afternoon as a human checkout bot."


One SKU, fifty addresses, fifty orders, fifty idempotency keys. The agent finds the best price on one notebook first, then loops. Go slow, log every order ID, and tag metadata so finance does not hunt you down in Slack.


## 6. Sony WH-1000XM5 price shootout


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Headphone thunderdome


XM5s under $300. Three retailers enter, one receipt leaves.


Same headphones, three stores, three different "final prices" once shipping shows up. This prompt makes the agent fight it out: Amazon vs Best Buy vs Walmart, total landed cost and delivery window, winner under $300.


Check[integrations](https://www.zinc.com/integrations) before you assume your store is live. Headline price lies. Shipping tells the truth.


## 7. Personal shopping agent, no questions asked


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Say grab this, get a tracking number


The unhinged one. Drops a link, agent buys. Cap it or cry.


This is the prompt people paste at 11pm and regret at 11:04pm.


You say "grab this," drop a link, and the agent buys it with a price cap and an idempotency key. Demo magic. Production nightmare if you skip spend limits. Add a category allowlist and approval above $150 unless you enjoy explaining random Amazon boxes to your accountant.


## 8. iPhone 16 Pro first-party under $1100


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


No sketchy iPhone sellers


First-party Amazon, Prime, Desert Titanium, under $1100. Paranoia built in.


You are not gambling $1,000 on a random marketplace seller named "PhoneDeals4U_99."


The prompt insists on first-party Amazon + Prime shipping + Desert Titanium 256GB + hard cap at $1100. Still eyeball the seller in the approval step. Even good filters deserve a human glance at that price.


## 9. Back-in-stock sniper


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Out of stock until it is not


Daily SKU watch. Two units under $75 the moment it reappears.


The item has been "out of stock" for six weeks. You stopped checking daily but you did not stop wanting it.


Run this on a schedule. The second it comes back under $75 on any supported store, buy two and cancel stale pending orders from your own log. Zinc will not cancel orders it never placed. Your app needs to track that.


## 10. New-hire welcome automation


Agent Skill


[universal-checkout](https://www.zinc.com/docs/v2/agent-skills/overview)


Welcome box on autopilot


New hire hits Slack, box hits their doorstep. Physical swag only, not gift cards.


Someone joins` #new-hires` . A welcome box should show up at their door without you remembering to order it at 4pm on a Friday.


Zinc ships the physical stuff: snacks, mug, notebook, whatever fits $50. Gift cards are a different API (Prezzee, Tremendous). Wire Slack → webhook → agent → fixed product list. Full HR playbook in[corporate gifting](https://www.zinc.com/blog/corporate-gifting-api) .


## Before you paste these in prod


- Run in[test mode](https://www.zinc.com/blog/test-mode) first.
- Set` max_price` on every order ([Create Order](https://www.zinc.com/docs/v2/api-reference/orders/create-order) ).
- Use an[idempotency key](https://www.zinc.com/docs/v2/api-reference/introduction/idempotency) per logical purchase.
- Subscribe to[webhooks](https://www.zinc.com/blog/webhooks) for` order.placed` ,` order.failed` , and` order.tracking_received` .
- Add human approval on anything over your comfort threshold. Prompt #7 is fun until it is not.


## FAQ


### Do these work in ChatGPT out of the box?


No. You need an agent with the[Universal Checkout skill](https://github.com/zincio/skills) via[skill.md](https://www.zinc.com/skill.md) , or a custom[MCP tool](https://www.zinc.com/blog/best-mcp-servers-for-retail) that calls Zinc. Chat alone cannot checkout at Amazon.


### Can Zinc run scheduled price checks?


Not by itself. You schedule the job; the agent uses Zinc when it is time to search or buy.


### Which retailers do these prompts support?


Check[integrations](https://www.zinc.com/integrations) . Amazon, Walmart, Target, and Best Buy cover most prompts above. Availability changes; confirm before you hardcode a retailer in production.


Pick one prompt. Get it working end to end. Then try #7 on a day when your wallet can take it.
