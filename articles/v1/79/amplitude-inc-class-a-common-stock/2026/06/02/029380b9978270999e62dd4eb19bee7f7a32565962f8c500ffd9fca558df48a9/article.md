---
schema_version: "1.0.0"
document_id: "029380b9978270999e62dd4eb19bee7f7a32565962f8c500ffd9fca558df48a9"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/persisted-properties-acquisition-merchandising"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:1e4dca359af74c0546d07a1af76ea00ca05ddefa9929aa14a5f25b9c584c74bd"
---

# Beyond Last-Touch Attribution: Find Out Which Interactions Really Matter

Most analytics tools tell you where a buyer was when they clicked "purchase." That's not the same as what made them buy. User journeys don't work in single moments. The campaign that brought them in three days ago. The landing page that set the hook. The recommended product recommendation they added to their cart at the last second. These touchpoints provide critical information to optimize your acquisition, channel, web, and merchandising strategies.


We recently hosted a live session,[Digital Analytics with Amplitude: Tracing the Sale](https://amplitude.com/tracing-the-sale-jun-2026) , to discuss this very topic. Amplitude Head of AI Strategy, Jim Kultgen, and Principal Product Manager, Enzo Avigo, showed how to trace sales with Amplitude's new persisted properties to get a complete, accurate picture of the purchase journey from first click to conversion.


## Persisting event context tells you what works


Take a common marketing scenario: you're testing different landing pages across your email and ad CTAs. The question is which page actually drives conversions, and why.


Grouping conversions by page path shows you that purchases happened on the checkout page. True, but not very insightful. But persisting context, such as "first page visited in a session," enables you to see which entry pages actually drove that conversion.


Persisted properties are an event property type in Amplitude that retain context over multiple touchpoints as users move through their journeys. A search query, a recommendation click, a UTM parameter—each one can be tied to the specific downstream event it influenced.


You don't need to re-instrument your site to connect entry pages, referrers, on-site searches, and other interactions to a final conversion event. You can use[persisted properties](https://amplitude.com/docs/data/persisted-properties) to define what context to carry and for how long.


## Five attribution questions that growth teams can easily answer


With clean data and clear context, growth and marketing teams can understand customer behavior in a way that drives priorities, experiments, and next steps. Some critical performance questions for any e-commerce, financial services, B2B technology, media/streaming, and healthcare brand include:


Question


Scenario


How to fix it with persisted properties


Which campaign drove revenue, not just traffic?


A user clicks a sale email, browses, leaves, and converts three days later. Last-click attribution misses the sale email.


Persist your referring channel or UTM_source for a custom timeframe, such as 7 or 30 days.


Which landing pages convert best?


A user lands on a category page, browses several products, and checks out from a product page. Page path analysis just shows the checkout on the product page without the previous actions.


Create an entry page persisted property (“original” page path) that expires at the end of the session. Group your conversion event by entry page instead of page path.


Where are users dropping off and why?


The last page in a user’s session tells you whether they got what they needed. An exit from a confirmation page is expected, but an exit from a search results page means they’ve given up.


Create an exit page persisted property (“last known” page path), expiring at the end of the session. Group your sessions by exit page.


Which site features drive purchases, not just clicks?


A user adds a $100 item via search, then a $20 item from a recommendation carousel. Last-touch credits the carousel for everything. Linear attribution splits it 50/50. Neither is right.


Use[item-level attribution](https://academy.amplitude.com/persisted-properties-item-level-attribution) to tag each product with how it was discovered at add-to-cart. Search gets credit for the $100 item. The carousel gets credit for the $20 one. Revenue follows the item, not the last touch.


Which source drove your mobile app installs?


Mobile Measurement Partner (MMP) attribution events often arrive after the user has already started engaging. The install event fires without the right source attached.


Use “last known” allocation to persist the attribution value to previous events. It preserves the attribution value, so when the data arrives, it gets applied to the right events retroactively.


## Set up persisted properties: immediately, retroactively, and without breaking anything


It's never been easier to get the answers you need out of your data. Amplitude's persisted properties are:


- **Immediate** : You can create a rule and see the change in dashboards, data tables, and any other report instantly.
- **Retroactive** : Your rules are applied to historical behavior, so you can analyze everything you've been tracking instead of waiting for new data to come in.
- **Nondestructive** : Your original data is untouched, so there's no data loss even as you iterate on rules.


##### "A couple of incredible things about persistent properties. First, they're source agnostic. If you're sending data from your warehouse, it's much easier to control and achieve your desired outcome. Second, you can tightly control your time period and change it over time. If seven days is right for your company at one point in time, but you want to evaluate how results would look with a longer window, you can do that immediately, retroactively, and nondestructively."


Jim Kultgen


Head of AI Strategy, Amplitude


If you've been working around this problem with proxy data or engineering-driven property maintenance, you don't have to anymore. You can implement and update persisted properties faster and fearlessly with a no-code setup.


Learn more about[our latest marketing analytics features](https://amplitude.com/blog/tracing-the-sale-persisted-properties) or[sign up for Amplitude](https://app.amplitude.com/signup) to try it yourself.
