---
schema_version: "1.0.0"
document_id: "8f9786d4b69aea53f2d1d53e8bd18df2af0d8defa818181bd582f20e9e757a4d"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/google-ucp-feed-checklist"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:997f009d904d89b2fb2d9ab62b7631194f73b4826a672b2fb87d183f2132f68f"
---

# 6 things you need to know to charge up your visibility in AI checkout

Here's the shift nobody's ready for: Google's **[Universal Commerce Protocol](https://www.anglera.com/glossary/universal-commerce-protocol-ucp) (UCP)** turns an AI conversation into a checkout. A shopper asks AI Mode in Search or Gemini for a jacket under $100, and — if your catalog is ready — buys it on the spot, never loading your site, never seeing a competitor.


If you're not ready, you're not in the cart. You're not even in the conversation.


And here's the part that stings: **your[product feed](https://www.anglera.com/glossary/product-feed) decides everything.** Not your ad budget, not your homepage. The agent reads your feed to decide whether an item is eligible, what it costs, and which warnings it has to show. Get the feed right and you're the answer. Get it wrong and you're invisible.


These are the 6 things that put you in the cart — and the gaps that keep you out.


## 1. Flip the switch — checkout is OFF until you say so


UCP checkout is off by default. Set the` native_commerce` attribute (a single boolean) to` true` on every product you want eligible. Missing or` false` means the agent skips it. Push this through a **supplemental feed** , not your primary one, so a formatting mistake can't take down your core product data.


## 2. No return policy, no checkout — full stop


UCP makes you the Merchant of Record, and Google won't let an agent check out against a product with no return policy. Set return **cost** , **window** , and a **link to the full policy** — globally in Merchant Center, or per-product with the` returns` attribute right in the feed. Set your customer-support info too; it becomes the "Contact Merchant" link on the confirmation screen.


## 3. Skip a legal warning and the agent skips you


Anything with a regulatory warning — California Prop 65, safety disclaimers — needs a` consumer_notice` group with a` notice_type` and` notice_message` . These render prominently on the checkout screen. Multiple warnings on one SKU? Send each as its own repeated` consumer_notice` . Compliance is on you, and the agent will show exactly what you give it.


## 4. One mismatched ID and the sale dies after checkout


The` id` in your feed has to line up with the product ID your Checkout API expects. If they already match, you're done. If not, map them with` merchant_item_id` — it takes precedence over` id` in agentic requests. Mismatch here, and orders fail silently after the shopper has committed.


## 5. Know exactly what UCP refuses to sell


A long list of categories is ineligible: subscriptions, installments, personalized or engraved goods, pre-orders, refurbished and final-sale items, bundled warranties and installation, freight-shipped items, age-restricted goods, services, rentals, and digital/virtual items. Set` native_commerce` to` false` on these. Flagging an ineligible product as eligible is a broken checkout waiting to happen.


## 6. Eligible isn't enough — you still have to get *picked*


Eligibility gets you in the cart. Getting *picked* is a data-quality problem. The agent isn't moved by a campaign — it's convinced by structure that proves fit: detailed product types two to three levels deep, accurate price and availability, and a **[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number)** wherever you have one. (Retailers with correct GTINs see ~20% more clicks on average; an agent reasoning over your feed leans on them even harder.) Loyalty perks like free shipping or a discount only count if they're structured cleanly enough for the agent to read them into its math.


## The one thing all six come down to


Strip away the attribute names and every item on this list is the same demand wearing a different hat: **structured, complete, machine-legible product data across your whole catalog.** One perfect SKU moves nothing.[Agentic checkout](https://www.anglera.com/glossary/agentic-checkout) only pays off when your *entire* feed is eligible, accurate, and rich enough to be chosen.


That's the work — and it's exactly what[Anglera](https://www.anglera.com/) does. PIM stores the data; we get every SKU UCP-ready: attributes mapped, notices attached, IDs reconciled, gaps filled. So when a shopper asks an agent to buy, you're the one in the cart.
