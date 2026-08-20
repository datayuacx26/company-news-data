---
schema_version: "1.0.0"
document_id: "31289bc9554f3ec799b4bab1cf6c25de80611b10d256e3b64da34ed7c377329c"
company_key: "yc-mocha"
company: "Mocha"
source_id: "yc-mocha-rss-d0ffed2c2227"
canonical_url: "https://getmocha.com/how-to-scrape-data-firecrawl"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-24T11:28:42.148680+00:00"
fetched_at: "2026-08-20T02:22:16.718062+00:00"
content_hash: "sha256:0da77fabcff0061589ced894df3d8b72c6e14372796ee63fda86d2676c35df7c"
---

# How to Scrape Web Data with Firecrawl

import PromptBox from '@/components/PromptBox'; import firecrawlLogo from '@/assets/images/how-to-scrape-data-firecrawl/firecrawl-wordmark.png';


In this guide, we're going to step through the process of setting up a Mocha app that has web crawling capabilities for you to scrape data from the web and take actions based on those web crawls.


This is a high-leverage piece of functionality that can greatly increase the utility of your app. Using Firecrawl, we're going to set up a small pragmatic app that tracks the prices of products found online at stores like Amazon, Walmart, Target, and more.


## What is Firecrawl?


Firecrawl is a powerful web scraping service that turns websites into LLM-ready data. It's an open-source platform designed to help developers extract clean, structured data from any website, including JavaScript-heavy and protected pages.


Firecrawl provides several core capabilities:


- **Scrape** : Extract data from individual web pages in formats like Markdown, JSON, or screenshots
- **Crawl** : Automatically discover and scrape all pages on a website
- **Search** : Search the web and get full content from results
- **Map** : Get a comprehensive view of a website's structure


Built with developers in mind, Firecrawl is fast, reliable, and handles the complexities of modern web scraping—no proxy management or browser automation needed. It covers 96% of the web and delivers results quickly, making it perfect for powering AI applications and real-time data needs.


For more information, visit[firecrawl.dev](https://www.firecrawl.dev/) .


## Starting with the Right Prompt


The initial prompt you give Mocha is crucial for getting your project started on the right track. When building an app that uses web scraping, you need to be specific about:


- **What data you want to collect** : Clearly describe what information you need from websites
- **How the data should be used** : Explain how the scraped data fits into your app's functionality
- **Key features** : List the main features you want, including any data storage or display requirements
- **Technical requirements** : Mention any specific services or tools you want to use (like Firecrawl)


Here's the example prompt used to build the competitor price tracker app:


<PromptBox client:load prompt={\`Build me a competitor price tracker app.


I want to be able to add products I'm interested in tracking by pasting URLs from different shopping websites (Amazon, Walmart, Target, etc.). The app should scrape the current price from each URL and store it.


Features needed:


- Add a product by pasting a URL and giving it a name
- Display a list of all my tracked products with their current prices
- Show price history over time (when did the price change?)
- Highlight when a price drops below a threshold I set
- Ability to manually refresh prices or have them update on a schedule


Use FireCrawl for the web scraping. Keep the UI clean and simple — I want to see my products and prices at a glance.\`} />


Notice how this prompt includes:


- A clear description of the app's purpose
- Specific features broken down into bullet points
- Technical requirements (Firecrawl)
- Design preferences (clean and simple UI)


This level of detail helps Mocha understand exactly what you're building and sets a solid foundation for the rest of the development process.


This example will show you how to get the web crawling technology working and how to debug the outputs of that scrape so that the data is what you want and can be used for your app in its intended way.


## Setting Up the Firecrawl API Key


During the build, you should see a prompt to add a secret for the Firecrawl API Key. If you need a walkthrough for how to add this key, click the walkthrough button and follow the instructions.


By following the steps in the window that opens, you'll be able to find your API key after logging in when you arrive at the Firecrawl dashboard.


Copy and paste this API key into the secret field in the Mocha app.


Once you've set up the Firecrawl API key, the app will be able to use it to fetch web data from different addresses that you provide to the app.


Wait until the app is finished building its initial version. This may take several minutes to finish the first loop of implementations. When done, test it out by adding a product and seeing if the app can scrape the price from the product page.


## Debugging the Data Crawler


You may find that the data you get from the web crawl doesn't match what you expected. This is because the web page isn't structured in a way that's easy for the AI to understand. This is the point in the build where you need to debug the data being fetched from the web, and work with the AI to get the data you want.


Make direct and specific descriptions of what you're doing, what you should be seeing, and what you're actually seeing instead. Act as a quality assurance engineer, or a data detective, to help Mocha get to the bottom of the issue.


> **Example prompt to the AI:**
>
>
> "I just added a product to the app. The NVIDIA GeForce RTX 5080 which is on sale for 1399.97 on Amazon using the link https://a.co/d/bDfMS1a and yet, when the product was added to the app, the price that was recognized was incorrect. The app is currently showing 169.99 as the price of the product, even though the price from Amazon was 1399.97. Please debug the price discrepancy when fetching the price so that we see the correct pricing when items are added to the app."


In this example, we were seeing the wrong price getting populated into the app, so we asked the agent about it and the AI tries to fix the problem by creating its own solution:


> **AI solution:**
>
>
> "I've updated the price scraping logic with two improvements:
>
>
> 1. **Better detection** : Now looks for specific price labels like 'Price:', 'Product Price:', 'Total:', etc. that Amazon and other retailers use
> 2. **Fallback strategy** : If no labeled price is found, it takes the highest price on the page (the main product is usually the most expensive item, not accessories or related products)
> 3. **Debug logging** : Added console logs to help diagnose future issues
>
>
> Try adding the product again and it should capture the correct $1399.97 price."


But this solution doesn't work. After testing it, the price is still incorrect. So we need to debug the problem further.


> **Follow-up prompt to the AI:**
>
>
> "The total price it is finding now is actually $2,198.96 because it is finding the 'Price:' pattern from the 'Total price:' wording present in the 'Frequently Bought Together' section, which adds up 3 items frequently bought together for a total price to display. We need to nail down the Amazon price scraping feature because Amazon is going to be the most common price tracking source that gets added to this app. We are able to see the actual price of the item in several places on the page, but I need to know what the response from Firecrawl looks like so that I can consistently identify the correct price from the response. I do not see anything in the console when I do a refresh of the price. Maybe we can add a console log to the refresh action so that whenever a price is refreshed, we can see what we're dealing with in the Firecrawl response."


The AI will often try to use the console logs to help debug the problem. If it doesn't do so, you can explicitly request this option to help with the process. Open the logs in the Mocha interface by pressing ⌘ B (or Ctrl + B on Windows). There you'll see the logs that the AI is producing to help you with the fix.


### Finding the Right Heuristic


After examining the logs, we discovered something useful:


```text
All prices found: [
169.99,   16.99,    5000, 1399.97, 1399.97,
1399.97, 1399.97, 1399.97, 1399.97,  169.99,
16.99,  169.99,   16.99,    5000,   16.99,
5000,   16.99,    5000, 1377.77, 1377.77,
1399.97, 1399.97, 1399.97, 1399.97, 1399.97,
469,     469,  329.99,  329.99, 2198.96,
2198.96, 1199.99, 1199.99,  999.97,  999.97,
1359,    1359, 1299.99, 1299.99,     349,
349, 1409.99, 1409.99,   999.9,   999.9,
1249.9,  1249.9,    8.99,    8.99,    1799,
1799,   21.99,   21.99, 2758.99, 2758.99
]


```


Look at how frequently the correct price of 1399.97 appears! We can use the most frequently appearing price instead of pattern matching—at least that's what will work for Amazon links.


After implementing this method for finding the price of the item, we were able to get accurate pricing for items listed on Amazon, Target, and BestBuy. **But not for Walmart.**


### Handling Different Retailers


Walmart had its own price scraping logic:


```text
All prices found: [
1659.99,      85, 1399.97,
59,      77, 1399.97,
1262.98,    1262, 1262.98,
150, 1262.98,    1262,
1262.98
]


Walmart link detected - using first price: $1659.99
Extracted price: 1659.99


```


Walmart needed its own price scraping logic because the most frequently appearing price wasn't the one being displayed for the product. Walmart product pages have a unique structure that repeatedly lists related products multiple times throughout the page but only shows the actual price of the product once at the top of the page.


After realizing this feature of Walmart links, we needed to add a new rule to the price scraping logic that's exclusively applied for Walmart product pages.


> **AI solution:**
>
>
> "I'll update the price extraction to use different strategies based on the source. Walmart links will use the first price found, while Amazon and others will continue using the frequency-based approach.
>
>
> Now:
>
>
> - **Walmart links use the first price found ($1,659.99)**
> - **Amazon and other sites use frequency analysis (most common price)** "


After implementing this new rule, we were able to get accurate pricing for items listed on Walmart, as well as Amazon, Target, and BestBuy. It isn't perfect, and you're going to need to keep debugging for your specific use cases...


But that's the beauty of Mocha. You can keep debugging and iterating on the scraping logic in your app until it's perfect for your specific use case.


## Wrapping Up


In this guide, we walked through the process of setting up a Mocha app that has web crawling capabilities for you to scrape data from the web and take actions based on those web crawls.


We also walked through the process of debugging the data crawler and getting the data you want.


You can see the full source code for the app we built by cloning the[Firecrawl Example App](https://pricetracker.mocha.app/) .


---


**Ready to add web scraping to your app?**


- [Start building for free with Mocha →](https://getmocha.com/)
- [Read the Mocha documentation →](https://docs.getmocha.com/)
