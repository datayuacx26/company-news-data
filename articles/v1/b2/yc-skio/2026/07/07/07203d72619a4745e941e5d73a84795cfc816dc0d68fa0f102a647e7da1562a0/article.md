---
schema_version: "1.0.0"
document_id: "07203d72619a4745e941e5d73a84795cfc816dc0d68fa0f102a647e7da1562a0"
company_key: "yc-skio"
company: "Skio"
source_id: "yc-skio-news-import-cdeaee5c7d9f"
canonical_url: "https://skio.com/blog/build-a-box-subscription-setup-static-dynamic-stacked"
published_at: "2026-07-31T18:34:45.320+00:00"
first_seen_at: "2026-08-01T07:08:56.851472+00:00"
fetched_at: "2026-08-01T07:08:57.851324+00:00"
content_hash: "sha256:d503d787aa5941b3057fe4cfc1d3683077ca4b5816564ec300793f2e88710fe1"
---

# Build-a-Box Setup Guide: Static, Dynamic & Stacked Boxes Without Custom Dev | skio

Most brands add Build-a-Box because their marketing team saw a competitor doing it. That's the wrong reason, and it's why so many box programs quietly die six months after launch, buried under support tickets, fulfillment chaos, and margin erosion nobody planned for. Build-a-Box configuration is a retention decision, not a checkout decoration. Which type you choose determines your churn rate, your warehouse's sanity, and whether your support inbox stays manageable.


Static boxes lock product selection at checkout for predictable revenue. Dynamic boxes let subscribers swap products each cycle to reduce churn. Stacked boxes combine both for maximum flexibility. This guide covers how to configure each one in Skio, what breaks if you get it wrong, and when to skip Build-a-Box entirely.


## Why Build-a-Box Configuration Actually Matters (Revenue, Not Marketing)


Build-a-Box configuration determines whether you optimize for revenue predictability, churn reduction, or conversion rate — not all three at once. That distinction matters more than most operators realize when they're first setting it up.


A Static box gives your finance team something to model. Same products ship every cycle, COGS is predictable, and your warehouse can batch picks efficiently. The tradeoff is voluntary churn when subscribers get bored — but that's a solvable problem with Journeys, not a reason to default to Dynamic.


A Dynamic box solves a different problem. Subscribers who'd cancel because they're tired of Product A instead swap to Product B. The cancellation that was inevitable becomes a retention. But Dynamic boxes add fulfillment complexity, increase support volume by 15-20%, and require a SKU catalog deep enough that "choice" doesn't feel fake.


Stacked boxes are the hybrid play — Static at checkout to optimize conversion (browsers commit when they control the first box), Dynamic after the first order to optimize retention. More configuration upfront, but you're not trading conversion rate for churn reduction.


The choice you make at setup ripples through your fulfillment operation, your support queue, and your LTV curve. Getting it right means understanding what each type actually does, not just what it looks like on the product page.


## Static Build-a-Box: Lock Revenue, Eliminate Swap Chaos


Static boxes ship the same product selection every cycle — customers pick once at checkout, no portal swaps allowed.


This is the right starting point for most brands. The customer builds their box at checkout, selects quantities, and that exact bundle charges and ships on their chosen frequency. No portal logins to change selections. No "how do I swap my box?" emails landing in your support queue. No warehouse surprises when a surge of last-minute swaps hits on billing day.


**Static boxes work best when:**


-


Your SKU catalog is under 8-10 products (more than that and Dynamic starts making sense)


-


You're in a category with low variety fatigue: pet food, household staples, meal kits with fixed menus


-


Your fulfillment team needs predictability — same picks every cycle, batch efficiently


-


You want to launch fast without deep configuration (Static setup takes about 15 minutes)


-


You're testing Build-a-Box before committing to Dynamic complexity


The real downside is voluntary churn when subscribers exhaust their interest in the fixed selection. The fix isn't switching everyone to Dynamic — it's using[Journeys](https://help.skio.com/docs/understanding-skio-journeys) to auto-convert long-tenure Static subscribers to Dynamic after Order #6 or #8, when variety fatigue typically kicks in. You get the revenue predictability of Static during the honeymoon period and the retention benefits of Dynamic before they start considering cancellation.


**Setup:** Follow the[Static Build-a-Box setup guide](https://help.skio.com/docs/how-to-set-up-a-static-build-a-box) — you're creating the box in the Products tool, selecting eligible products, setting the discount structure, and publishing. Fifteen minutes if you have your product selection ready.


## Dynamic Build-a-Box: Let Subscribers Swap, Cut Churn by 30%+


Dynamic boxes let subscribers swap products in the portal before each charge — preventing cancellations caused by product fatigue.


The mechanics: you define a product pool (the full catalog of swappable items), set quantity or price thresholds (a box must contain X items or reach $Y value), and enable portal swapping. Subscribers log in before their billing date, swap whatever they want within the rules you've set, and the updated selection charges and ships.


The churn math is straightforward. In categories with variety fatigue — supplements, snacks, coffee, beauty — a significant portion of cancellations happen because subscribers are bored with their current selection, not because they've lost faith in the brand. Dynamic boxes intercept that cancellation intent before it becomes a cancel request. Instead of emailing support to cancel, they log into the portal and swap. Retention preserved.


What the optimistic feature pages don't tell you: Dynamic boxes require a deeper SKU catalog than most brands expect. If you only have six swappable products, subscribers will cycle through all of them within a few months and you're back to variety fatigue. A minimum of 10-15 actively available SKUs keeps the swap experience feeling genuinely flexible.


Factor


Static Box


Dynamic Box


Setup time


~15 minutes


20-30 minutes


Churn reduction


Moderate


High (30%+ in variety-fatigue categories)


Support ticket volume


Near-zero (swap-related)


+15-20% (mostly swap questions)


Fulfillment complexity


Low — same picks every cycle


Medium-High — real-time inventory sync needed


SKU catalog minimum


2-4 products


10-15 recommended


Best for


Predictable COGS, low SKU count


Supplements, snacks, beauty, coffee


Margin predictability


High


Medium (depends on swap rules)


**Critical configuration step:** Set swap limits. Without them, subscribers figure out that swapping to your highest-margin product every cycle costs them nothing extra. Cap total box value or restrict swap frequency to prevent systematic gaming. The[Dynamic Build-a-Box v3 setup guide](https://help.skio.com/docs/how-to-set-up-a-dynamic-build-a-box-v3-babv3) walks through where to set these limits.


## Stacked Build-a-Box: Static for Predictability, Dynamic for Retention


Stacked boxes start as Static at checkout for conversion, then unlock Dynamic swapping after the first order for retention.


This is the configuration most brands should eventually land on if they have the SKU depth for it. The sequencing matters: at checkout, a Dynamic box with full swap flexibility is actually a conversion liability. Browsers faced with "pick anything from our catalog, then change it whenever you want" often convert worse than browsers faced with "build your box now, choose these specific products." Paradox of choice is real, and checkout is the wrong place to flex your catalog depth.


After the first order ships, the calculus flips. Now the subscriber has committed, received their box, and formed an opinion about what they do and don't want next time. That's exactly when Dynamic swapping becomes valuable — it gives them agency over their second box before they've had time to decide whether to cancel.


> "We were seeing good conversion on our static bundles, but churn was killing us around month three. The stacked approach — lock them in at checkout, give them flexibility after — was the thing that actually moved the retention number." — Director of Ecommerce at a $20M DTC brand


**How to configure Stacked boxes:**


1.


Set up a Static box at checkout (same process as standard Static setup)


2.


Build a[Journey](https://help.skio.com/docs/understanding-skio-journeys) with trigger: **After Order #1 completes**


3.


Set the Journey action: **Convert Static subscription to Dynamic**


4.


Activate — subscribers automatically unlock swap access after their first shipment


The Journey handles the conversion automatically. No manual migration, no customer-facing communication required (though a "good news, your box is now customizable" email is a nice touch for the onboarding experience).


**When Stacked makes the most sense:** High AOV brands where checkout conversion friction is expensive, and categories with documented variety fatigue. If your average order value is $80+ and your subscribers historically churn around months 3-4, the Stacked approach is worth the extra configuration time.


## How to Actually Configure Each Box Type in Skio (15-30 Minutes)


### Setting Up a Static Box


1.


Navigate to **Products** in your Skio dashboard


2.


Select **Build-a-Box** → **Create Static Box**


3.


Name the box and set the minimum/maximum product quantities


4.


Select eligible products from your catalog


5.


Set the discount structure (percentage off, fixed amount, or tiered by quantity)


6.


Configure frequency options (monthly, every 6 weeks, etc.)


7.


Publish and add to your storefront


Total time: approximately 15 minutes. Full walkthrough in the[Static Build-a-Box setup guide](https://help.skio.com/docs/how-to-set-up-a-static-build-a-box) .


### Setting Up a Dynamic Box


1.


Navigate to **Products** → **Build-a-Box** → **Create Dynamic Box**


2.


Define your product pool — every SKU subscribers can swap into


3.


Set quantity thresholds (minimum 3 items, maximum 6, for example) or price thresholds ($50 minimum box value)


4.


Configure swap limits — max swaps per cycle or max box value cap


5.


Enable portal swapping and set the swap cutoff window (tied to your[Billing Window](https://help.skio.com/docs/billing-window) setting)


6.


Set inventory policy to prevent overselling on popular swap targets


7.


Publish


Total time: 20-30 minutes depending on product pool size. Full configuration in the[Dynamic Build-a-Box v3 setup guide](https://help.skio.com/docs/how-to-set-up-a-dynamic-build-a-box-v3-babv3) .


### Setting Up a Stacked Box


1.


Complete Static box setup above


2.


Navigate to **Retain** → **Journeys** → **Create Journey**


3.


Set trigger: **Order Completed** → filter to **Order Number = 1**


4.


Add action: **Convert to Dynamic Box** (select your Dynamic box configuration)


5.


Activate the Journey


Total time: 30-45 minutes including Journey setup. The Journey activates automatically — no manual intervention per subscriber.


**The mistake that breaks Dynamic boxes:** Skipping swap limits. Subscribers are smart. Within a few billing cycles, some will figure out that swapping to your highest-value SKU every time costs them the same as their original selection. Set value caps and swap frequency limits from day one — adding restrictions retroactively is much harder and will generate subscriber complaints.


## When NOT to Use Build-a-Box


-


**Single-SKU brands:** Build-a-Box is overkill. If you sell one product in one size, a standard subscription with frequency options is the right tool. Adding box complexity doesn't add customer value.


-


**Catalogs under 4 products:** Customers see through fake choice. A "build your box" with four options isn't a box — it's a product selector with extra steps. The novelty wears off fast and the configuration overhead isn't worth it.


-


**High-touch fulfillment:** Custom engraving, personalization, made-to-order products — Dynamic swapping breaks your fulfillment SLA because the final selection isn't known until the swap cutoff. Static might work here, but Dynamic almost certainly doesn't.


-


**When your AOV is already optimized:** If your bundles already convert well and churn is low, adding Build-a-Box complexity doesn't move the needle. The operational overhead isn't justified.


-


**When you're still finding product-market fit:** Don't layer subscription complexity onto an uncertain catalog. Nail your core product, then add box flexibility once you know which SKUs customers actually want.


For brands that don't need Build-a-Box,[Volume Discounts](https://help.skio.com/docs/volume-discounts) often accomplish the same conversion goal — incentivize multi-product purchases — with far less operational complexity. Worth evaluating before committing to box setup.


## Box Discounts: Pricing Strategy That Doesn't Cannibalize Margin


Box discounts should increase after the first order — lower discounts at checkout protect acquisition margin, higher discounts on renewals drive retention.


The most common mistake: operators set an aggressive flat discount (say, 25% off) to drive checkout conversion, then watch that same 25% get applied to every recurring order indefinitely. Acquisition cost is already baked into the first order — you don't need to give away 25% on order #8 to retain a subscriber who's already deeply habituated.


The smarter structure is tiered by order number, configured via Journeys:


Order Number


Recommended Discount


Rationale


Order #1


10-15%


Competitive at checkout, preserves acquisition margin


Orders #2-3


15-18%


Small increase rewards commitment, builds habit


Orders #4+


20-25%


Full loyalty discount — subscriber is retained, reward it


This approach means you're not subsidizing every order at your highest discount rate. The subscriber earns the deeper discount through tenure, which also creates a loss-aversion incentive for retention — canceling means losing the 25% discount they've built up to.


For supplements, where margins typically run 60-70%, a 25% flat discount is defensible. For food and beverage at 40-50% margins, it's painful. For beauty at 55-65%, it depends heavily on whether the box drives cross-category trial that increases LTV.


Build these discount escalations into Journeys — the[Journeys workflow](https://help.skio.com/docs/understanding-skio-journeys) handles the order-number triggers automatically, no manual intervention per subscriber.


## Fulfillment Impact: What Your Warehouse Needs to Know


Static boxes are fulfillment heaven. Same picks every cycle, same quantities, predictable batch processing. Your warehouse team can pre-stage picks days in advance. No surprises.


Dynamic boxes are a different conversation. Subscribers can swap right up to the billing window cutoff, which means your final pick list isn't confirmed until that cutoff passes. If your billing window closes 48 hours before charge date, your warehouse needs to understand they cannot start Dynamic box picks until that window closes. Communicate this explicitly — fulfillment teams that don't know about swap cutoffs will start picking early and end up with wrong orders.


Skio's[Billing Window](https://help.skio.com/docs/billing-window) setting controls exactly when swaps lock. Set it in coordination with your warehouse's lead time requirements. If your 3PL needs 72 hours from confirmed pick list to ship date, your billing window needs to close at least 72 hours before the scheduled charge.


A few other fulfillment considerations for Dynamic boxes:


-


**Inventory policy:** Configure Skio's inventory policy to prevent subscribers from swapping into products that are out of stock. Nothing creates support tickets faster than a subscriber who swapped to a sold-out SKU and gets an incomplete box.


-


**Daily swap reports:** Use Skio's[Data Exports](https://help.skio.com/docs/exports-guide) to pull daily swap activity and share with your fulfillment team. They need visibility into what's changing before the cutoff.


-


**Auto-Merge Billing:** If subscribers have multiple subscriptions,[Auto-Merge Billing](https://help.skio.com/docs/auto-merge-billing) consolidates them into one shipment — critical for Dynamic boxes where multiple subscriptions create shipping cost complexity.


## Support Volume: What to Expect (and How to Reduce It)


Dynamic boxes increase support volume 15-20% — Quick Actions and portal banners eliminate most swap-related tickets.


The ticket breakdown is predictable once you've run Dynamic boxes for a few months:


-


**"How do I swap my box?"** — subscribers don't know they can swap, or can't find where to do it


-


**"When's the cutoff to change my box?"** — deadline confusion


-


**"I tried to swap but it said it was too late"** — missed cutoff, now frustrated


-


**"Can I swap to \[out-of-stock SKU\]?"** — inventory confusion


Most of these are preventable. The highest-leverage fix is embedding[Quick Actions](https://help.skio.com/docs/getting-started-with-quick-actions) in your pre-charge email. Instead of sending a "your order is coming in 5 days" email that forces subscribers to log into the portal to swap, a Quick Action link lets them swap directly from the email — no login required. That single change eliminates the majority of "how do I swap?" tickets because the swap happens where subscribers already are.


For cutoff confusion, a swap deadline countdown banner in the Customer Portal (configurable in portal settings) answers the timing question before it becomes a support ticket. Combine that with a clear FAQ in the portal itself — embed swap instructions directly in the Customer Portal settings so the answer is surfaced when subscribers are actively thinking about it.


Static box operators can largely ignore this section. No swapping means no swap questions, and the near-zero support ticket profile is one of Static's most underappreciated advantages.


## Migration Notes: Moving Existing Subscribers to Build-a-Box


Migrate existing subscribers to Build-a-Box via opt-in campaigns, not forced changes — use Bulk Operations for product swaps and Journeys for gradual upgrades.


Forcing active subscribers into a new box structure without warning is one of the fastest ways to trigger a cancellation wave. Their subscription changes in ways they didn't choose, and for a meaningful percentage, "something changed about my subscription" is enough reason to cancel and re-evaluate.


The safer approach:


**For new programs:** Launch Build-a-Box for new subscribers only. Let the existing subscriber base continue on their current plan. Run the programs in parallel while you validate the box experience.


**For gradual migration:** Use a Klaviyo email campaign to offer existing subscribers an opt-in upgrade to the box program. Frame it as a benefit — "You can now customize your monthly box" — and let them choose to switch. Opt-in migration converts at lower volume but generates zero forced-change cancellations.


**For bulk product swaps:** If you're migrating subscribers from single-product subscriptions into Static boxes,[Bulk Operations](https://help.skio.com/docs/bulk-operations-guide) handles the product swap at scale. You can swap products across thousands of subscriptions without touching them one by one — use the product swap operation to move subscribers from the old SKU into the box configuration.


**For legacy subscriber upgrades:** Build a Journey with trigger **Order #3 or #4 completed** and action **Send offer to upgrade to Dynamic box** . Long-tenure subscribers are the most likely to benefit from Dynamic flexibility (they've already shown commitment) and the least likely to churn from the offer itself.


**Grandfather legacy pricing.** If existing subscribers are on a flat 20% discount and your new box program uses a tiered structure, don't automatically apply the new structure to legacy subscribers. They'll notice the change in their discount, and it reads as a price increase even if the math is technically similar.


If you're evaluating whether Build-a-Box is the right next move for your subscription program, the[Understanding Build-a-Box](https://help.skio.com/docs/understanding-build-a-box) doc is a good starting point before diving into setup. And if you're already running boxes and want to reduce swap-related support tickets, the[Quick Actions setup guide](https://help.skio.com/docs/getting-started-with-quick-actions) is the highest-leverage thing you can do this week.


## Frequently Asked Questions


**What's the difference between Static and Dynamic Build-a-Box?**


Static boxes lock product selection at checkout — same items ship every cycle. Dynamic boxes let subscribers swap products in the portal before each charge. Static optimizes for revenue predictability and fulfillment simplicity. Dynamic optimizes for churn reduction in categories with variety fatigue.


**Can I switch a subscriber from Static to Dynamic after they've already subscribed?**


Yes — use Journeys to trigger the conversion after a specific order number. The most common setup is Static at checkout, then a Journey converts the subscription to Dynamic after Order #1 completes. The subscriber gets an email explaining they can now customize their box, and portal swapping unlocks automatically.


**Do Dynamic boxes increase support tickets?**


Yes, by about 15-20%, mostly swap questions and cutoff confusion. The most effective fix is Quick Actions in pre-charge emails — subscribers swap directly from the email without logging into the portal. Adding a swap deadline countdown banner in the Customer Portal handles the cutoff question before it becomes a ticket.


**How do I prevent subscribers from gaming Dynamic boxes by always swapping to high-value products?**


Set swap limits in the Dynamic box configuration. You can restrict the total box value (so swapping to all premium SKUs exceeds the box cap), limit the number of swaps per billing cycle, or restrict which SKUs are eligible for swap. Configure these limits at setup — adding restrictions retroactively is operationally messier and generates subscriber complaints.


**Can I use Build-a-Box with prepaid subscriptions?**


Static boxes work cleanly with prepaid subscriptions. Dynamic boxes with prepaid get complex — swaps across multiple prepaid cycles require careful configuration to avoid billing discrepancies. If you want to combine Dynamic and prepaid, test thoroughly before rolling out broadly and document the expected behavior clearly for your support team.


**How long does Build-a-Box setup take in Skio?**


Static boxes take approximately 15 minutes. Dynamic boxes take 20-30 minutes, depending on product pool size and swap rule complexity. Stacked boxes (Static plus Journey for Dynamic conversion) take 30-45 minutes total. None of these require custom development — everything is configurable in the Skio dashboard.
