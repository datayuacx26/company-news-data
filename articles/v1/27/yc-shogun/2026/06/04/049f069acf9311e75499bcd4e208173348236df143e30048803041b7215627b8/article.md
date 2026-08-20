---
schema_version: "1.0.0"
document_id: "049f069acf9311e75499bcd4e208173348236df143e30048803041b7215627b8"
company_key: "yc-shogun"
company: "Shogun"
source_id: "yc-shogun-news-import-7a2a36e8ea0a"
canonical_url: "https://getshogun.com/learn/shopify-shipping-testing-feature-shogun-ab-testing"
published_at: "2026-06-24T14:16:36+00:00"
first_seen_at: "2026-07-24T00:37:25.911184+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:dc2de30aaa3f77353efda0ba180e64e2c3daacc43a28ddc6322c64e8cb0d3dbd"
---

# Introducing Shipping Tests in Shogun A/B Testing

Shipping is often the final decision point between a completed order and an abandoned cart. A shopper who has spent time browsing, added items to their cart, and begun checkout can still drop off the moment they encounter unexpected shipping costs or confusing delivery options.


Despite its obvious impact on conversion, shipping has historically been one of the hardest parts of the customer journey to experiment on — until now.


Today, we’re introducing Shipping Tests within[Shogun A/B Testing](https://apps.shopify.com/shogun-ab-testing) , a purpose-built way for **Shopify Plus, Advanced and Annual Grow merchants** to run controlled experiments on their shipping rates, delivery options, and messaging — all without writing a single line of code.


## Why Shipping Matters


Shipping sits at a unique intersection of conversion and profitability. Offer free shipping and you may lift conversion — but squeeze margins. Charge for shipping and you protect profitability — but risk checkout abandonment. The frustrating truth is that neither option is universally “right.” The right answer depends on your audience, your products, your margins, and a dozen other variables that differ from store to store.


Questions merchants routinely wrestle with:


- Does free shipping outperform flat-rate shipping for my customers?
- What order-value threshold unlocks free shipping without destroying margin?
- Are shoppers more likely to convert when they see one clean option or multiple choices?
- Do shipping preferences differ significantly between my domestic and international markets?


In the past, answering these questions meant making permanent changes to your store, waiting weeks to accumulate enough data, and hoping you hadn’t inadvertently hurt revenue in the process. Shipping Tests changes that. You can now run a properly controlled experiment, let real customer behavior tell you what works, and ship the winner with confidence.


## What Can You Test?


Shipping Tests gives you control over the delivery options shoppers see at checkout:


- Test free shipping thresholds — find the order value sweet spot between conversion lift and margin impact.
- Compare flat-rate pricing — see how different fixed shipping prices affect completed orders and revenue per visitor.
- Experiment with order-amount-based rates — show tiered shipping that unlocks free delivery at a certain spend.
- Show or hide specific shipping methods — test whether fewer options reduce friction or whether more choice increases satisfaction.
- Adjust delivery time messaging and descriptions — find the wording that builds the most shopper confidence.


## How to Set Up Your First Shipping Test in Shogun


Setting up a Shipping Test takes just a few minutes. The walkthrough below uses a real-world example to show you exactly how it works.


The test we’re running is called “Flat Rate vs. Free on $150+” — and the premise is straightforward: right now, all domestic customers pay a flat $10 for standard shipping regardless of how much they spend.


The question we’re asking is whether offering free shipping on orders above $150 would motivate shoppers to add more to their cart to hit the threshold, ultimately increasing average order value.


If the lift in AOV outweighs the cost of absorbing shipping on larger orders, the variant wins. That’s exactly the kind of hypothesis Shipping Tests is built to answer.


### **Step 1: Choose “Shipping” from the Dashboard**


Open Shogun A/B Testing from your Shopify admin. On the main A/B Tests dashboard, you’ll see test types across the top: Page, Split URL, Template, Theme, Product details, Price, Shipping, and Checkout.


You can hover your mouse over the “Shipping” test option to reveal more details about the test type, along with some ideas you can implement with shipping tests. Next, click “Start a shipping test” in the expanded panel to get started with your test.


### **Step 2: Name Your Test and Add a Hypothesis**


You’ll land on the Shipping test setup page. Give your test a clear, descriptive name — something like “Flat Rate vs. Free on $150+” works well because it tells you at a glance exactly what’s being compared.


Use the Notes field to capture your hypothesis: what do you expect to happen, and why? This context is especially valuable when you’re reviewing results weeks later.


Below the name and notes fields you’ll see the full test configuration: shipping profile selection, traffic split, test goal, and audience. We’ll walk through each one.


### **Step 3: Select Your Shipping Profiles**


Under “Configure your shipping rates,” you’ll see your store’s existing shipping profiles. Check the box next to any profile and zone you want to include in the test. In our example, we’ve selected “Domestic (3 rates)” from the General profile.


You can include multiple zones if you want to test across several regions simultaneously.


Once you’ve made your selection, click “Next” to proceed to the rate configuration screen.


### **Step 4: Configure Your Variant Rates**


This is where the test is actually defined. You’ll see your current rates on the left under “Original” and an editable version on the right under “Variant.” Any rates you set in the Variant column replace the original rates for shoppers bucketed into the variant group.


Shoppers in the Original group see nothing different — your store behaves exactly as it does today.


In our example, the Original has a “Standard” set as a flat rate of $10.00 with 3–5 business day delivery. For the Variant, we’re switching the rate type to “Order amount” and configuring two tiers:


- $0.00 – $150.00 → $10.00 shipping, 3–5 business days
- $150.01+ → Free shipping, 3–5 business days


The Express rate ($20.00 flat, 3–5 business days) is kept identical in both Original and Variant — isolating the Standard rate as the only variable being tested.


**The hypothesis:** offering free shipping on orders above $150 will encourage shoppers to add more to their cart to hit the threshold, increasing average order value without sacrificing conversion.


### **Step 5: Set Your Traffic Split and Test Goal**


The Traffic Split section defaults to 50/50 — half of qualifying visitors see the Original, half see the Variant. For most tests, 50/50 is the right starting point. It gets you to statistical significance as fast as possible.


Next, choose your Test Goal — the primary metric you’re trying to move:


- Conversion Rate — maximize the percentage of visitors who complete a purchase
- Add to Cart — measure impact on shoppers adding products to their cart
- Revenue per Visitor — optimize for total revenue generated across all sessions
- Average Order Value — find the configuration that drives larger baskets


For a test like “Flat Rate vs. Free on $150+,” Revenue per Visitor is often the most meaningful goal — it captures both the conversion effect and the basket-size effect of a threshold-based free shipping offer in a single number.


### **Step 6: Define Your Audience**


The Audiences section controls which visitors are eligible to enter the experiment. By default, “All Shoppers” is selected, meaning every visitor to your store could be enrolled. You can narrow this to specific segments if you’d like to segment further.


Audience targeting is particularly powerful for shipping tests because shipping economics vary so much by geography and customer type.


### **Step 7: Review and Launch**


When everything looks right, be sure to hit “Save” within the black bar at the top of the page and the center of the screen to save your progress.,


Then, click “Review” in the top-right corner of the screen.


A “Prepare to launch test” confirmation modal will appear with a few key reminders:


- Your test will go live for customers in your chosen audience — the more traffic, the faster you’ll reach significance.
- Shogun will monitor performance and surface recommendations on next steps.
- You can end the test at any time by returning to the test page.


Confirm the test name, click “Start test,” and you’re live. Shogun will immediately begin routing traffic and collecting data.


## **International and Multi-Market Brands**


Shipping expectations vary dramatically across markets. What converts well in the US may fall flat in Canada or Western Europe. Delivery speed expectations, carrier preferences, and price sensitivity all shift by geography — which means a shipping strategy that works perfectly for one segment can actively hurt another.


Shipping Tests are built with this in mind. Combined with Shogun’s audience targeting, you can run market-specific experiments independently and learn what works where — rather than applying a single global strategy and hoping for the best. For merchants using Shopify Markets, this is a meaningful step toward truly localized checkout optimization.


## **Measure What Actually Matters**


Every Shipping Test is tracked against the business metrics that matter: completed orders, revenue per visitor, and average order value. Results are displayed clearly so you can see not just which variant won, but by how much — and whether the difference is statistically meaningful.


Shogun will also surface recommendations as your test accumulates data, so you’re not left staring at a dashboard trying to figure out what to do next.


## **Start Testing Today**


The most effective shipping strategy isn’t always the most obvious one. A slightly higher free-shipping threshold might actually increase AOV without hurting conversion. A simplified rate structure might outperform a more complex one. A small change to delivery time messaging might be all it takes to tip a hesitant shopper into completing their purchase.


The only way to know for certain is to test. With Shipping Tests, you can now run those experiments on one of the highest-impact moments in the entire checkout journey — and let real customer behavior tell you what works.


Ready to get started? Get on the[Advanced or Unlimited Plan within Shogun A/B Testing](https://getshogun.com/pricing?product=ab) and **create your first Shipping Test today.**


* *Shipping Tests are available for Shopify Plus, Advanced, and Annual Grow merchants.*
