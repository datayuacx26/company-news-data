---
schema_version: "1.0.0"
document_id: "b20fa7dd2325be36cd1aaba0a9f1c7f8ce7f77982708b20ae3e0aafd2c2f9639"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/03/19/data-visualization-essentials"
published_at: "2019-03-19T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:990329d24d4cd6786ab3ee0f90dd5782ffb153c026cf70e63201d185158a4569"
---

# Data Visualization Essentials

The business world has been buzzing from the phrase “Big Data” for years. Marketing around the term makes it seem like a magical phrase that can solve any problem just as long as we have enough data. However, even though we can store petabytes of data, it does us little good if we don’t have some way to let that data tell us a story and allow us to ask questions of that story. This is the benefit of data visualization. Below are three essential techniques for getting the most from your data.


## Show Me the Numbers


A phrase I constantly hear is “I just want to see the numbers.” To demonstrate this approach, we’ll use a sample data set (below) and look at the image of Contributor Percentage Rate broken out by App, Contributor, and Category. Looking at this image, can you identify **how many are below a 25% acceptance rate?**


*NB: All data displayed in this post is fictional, generic, Tableau-provided data*


How many were you able to count? Was that too fast? Didn’t have enough time to check every column and row?


Now check out a different visualization of the same data.


Looks like Emily is the only Contributor below a 25% acceptance rate and it’s in the “Fashion” category. Sorry Emily, but we’re going to need you to step up your fashion game.


Why is it so hard to read all of that text? Our eyes have to scan each individual row and column and do a quick yes or no before moving on. We can perform this task but it’s not very efficient. A lot of mental gymnastics have to take place in order for our brains to digest so much information.


When provided a data visualization layer instead of raw text, our brains are able to see differences in shapes, colors, and sizes much more efficiently, making outliers pop off the page.


So instead of making reports that are full of nothing but numbers and text it’s much more effective to let our data tell its story using visuals.


## The Forgotten 10%


Color is an important element of data visualization. But when using color to communicate information we need to be careful that all actionable items are fully accessible to the widest audience possible, especially those affected by color blindness.


In this visualization we see the length of the bars as the sales of each category and the color of the bar represents the amount of profit. This is how someone with deuteranopia or red/green color blindness would see a visualization.


See anything in there that is at **negative profit** ?


How about now?


A world of difference. Around 90% of us can see this chart and immediately see that the “Tables” category is a problem and it’s losing us money, but the remaining 10% of the population are color blind in a way that makes these reds and greens very muted or even appear black. They’ll miss this important information about this category.


It’s essential to take this subset of the population into consideration when we are encoding by color. The best way around this issue is to use color-blind-friendly palettes like blue/orange instead of green/red:


This palette still keeps that cool/warm diverging scheme but a lot more people will be able to decipher the information easily.


Try not to use green and red colors in your visualizations to convey good/bad because our color blind friends need to be included.


## Pie Charts and Why You Should NEVER Use Them


Which category has the most sales?


Even if you looked at the legend for the appropriate color and came back to the pie chart, it’s still incredibly difficult to make this call based on the visual information you’re given. Why is that? It’s all about how our brains are wired. Humans have a really difficult time comparing things in an arc.


What if we just applied labels to this visualization? That would just add unneeded clutter to our screen making it even harder to understand the visuals.


What would be a better way of communicating this story to our audience? We can take this same information and communicate it in a way that our audience will get the point right away and be confident of what the data is telling them.


*Now* which category has the most sales?


We didn’t even need to sort this or add “Percent of Total Sales” as a header like the visualization is showing in order to see that the bar for “Telephones and Communications” is slightly longer than “Office Machines.”


A good data visualization is simple, clear, and concise. As Shakespeare wrote, “brevity is the soul of wit.”


## Conclusion


By using symbols instead of numbers where appropriate, readers can more easily compare figures. Preferring blue/orange to green/red allows those with deuteranopia to view visualizations easily. And there’s usually a simpler and more concise way to display data than with a pie chart. Our data has a story to tell—we just need to let it do so, by using the right tools.
