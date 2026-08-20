---
schema_version: "1.0.0"
document_id: "723a7f61cdc8b4e8eeeafb8942376a4af0d74a086cd0961b0f32ba51c54ebeb4"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-rss-c7494104b4c4"
canonical_url: "https://www.semrush.com/blog/schema-markup/"
published_at: "2026-07-30T10:21:00+00:00"
first_seen_at: "2026-07-30T12:56:11.678780+00:00"
fetched_at: "2026-07-30T12:56:12.342715+00:00"
content_hash: "sha256:3a48526049e830fba4eb5a6fc7c6babf47aad239220b07964f797831f09b08f9"
---

# What is schema markup? And how to add it to your site

Google, Bing, and AI platforms like ChatGPT all need to understand your content before they can surface it to users. They've gotten better at this over time but can still benefit from explicit signals that remove any ambiguity.


Schema markup is one way to give search systems explicit, machine-readable clues about the entities on your page, like a product, review, event, or local business.


This guide walks through what schema markup is, the most useful types, and how to implement it step by step.


## Add schema to win rich results, not to game visibility


Schema markup's proven value is in making your content eligible for rich results like star ratings and prices, which may lead to higher click-through rates (CTRs). Schema markup is not a direct ranking factor for Google, and it’s also not something you can manipulate to increase your visibility in AI systems.


The pages that benefit most from schema are the ones for which there is the potential to earn rich results. These include:


- Product pages
- Reviews
- Recipes
- Articles
- Local business pages


Add schema markup to your website because it helps search engines display rich results, not because you’ve been told it’ll improve your search or AI visibility.


## What is schema markup in SEO and why is it important?


Schema markup (a type of structured data) is code you add to your website to help search engines, and potentially AI systems, understand your content better. Schema markup is important for search engine optimization ([SEO](https://www.semrush.com/blog/what-is-seo/) ) because it can make your content eligible for rich results.


Schema markup looks like this:


Adding schema markup to your pages also makes your content eligible to have search engines like Google show enhanced search results known as[rich snippets](https://www.semrush.com/blog/rich-snippets/) (also called rich results). Like star ratings, prices, and whether a product is in stock.


When rich snippets appear, users can find valuable information directly within search results.


Getting your content to appear as rich results can lead to:


- **Enhanced click-through rates (CTRs)** : Rich results may see higher CTRs compared to standard blue links given users can preview key information directly in search results to better understand if it meets their[search intent](https://www.semrush.com/blog/search-intent/)
- **Improved SERP real estate** : Rich snippets take up more visual space on search engine results pages ([SERPs](https://www.semrush.com/blog/serp/) ), pushing competitors further down the page


Without schema markup, your page will probably only show as a blue link with a description below it. And if your competitors are using schema markup and getting rich snippets, then your link won’t stand out.


For example, compare these two pages ranking for “small microwave under $200” that appear right next to each other in the search results:


The Home Depot result is minimal. But the Target result stands out by showing a star rating, explicitly saying there are options between $18 and $200 (which reflects the search intent), and including mention of free delivery and returns.


Just know that adding schema markup doesn’t guarantee rich results. Ultimately,[it’s up to Google to decide](https://developers.google.com/search/docs/appearance/structured-data/sd-policies) whether to show rich formats.


### Does schema markup help rankings?


No, schema markup is not a direct Google ranking factor.[Google has stated](https://developers.google.com/search/docs/appearance/structured-data/sd-policies) that structured data doesn't influence how a page ranks in search results.


Schema markup can make you eligible for rich results. These enhanced listings can improve your click-through rate, which may lead to increased traffic even though your ranking position doesn't change because of the markup itself.


## How does schema markup work?


Schema markup works when crawlers scan your website’s HTML and see a script with tags from Schema.org’s standardized vocabulary clearly labeling aspects of your content in a way that search engines can understand.


For example, schema markup can explicitly tell search engines that "$299 is the price of this specific product." So, they don’t have to infer what $299 refers to.


The explicit communication from schema markup helps search engines categorize your content more accurately and determine which rich results it may be eligible for.


## Is schema markup important for AI visibility?


You do not need to add schema markup to be visible in AI responses.[Google says](https://developers.google.com/search/docs/appearance/ai-features) there’s “no special schema.org structured data that you need to add” to appear in its AI features like AI Overviews and AI Mode. But adding schema markup is an[SEO best practice](https://www.semrush.com/blog/seo-best-practices/) . So, it’s beneficial to add proper schema markup to your content where relevant anyway.


Schema markup gives search engines explicit information about entities, and this may reduce ambiguity and help the search engines better understand the page. But whether this markup does the same thing for AI systems is unclear, and including schema markup doesn’t guarantee AI retrieval or citation.


Google’s[guide to succeeding in AI search](https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search) does say you should “make sure your structured data matches the visible text on the page.” But this guidance only briefly mentions schema markup as being useful for sharing information in a machine-readable way that Google’s systems consider. And because the page frames these as the same foundational SEO best practices that apply to Google Search overall, there's no direct statement that using structured data helps specifically with AI visibility.


[Microsoft has a similar page](https://about.ads.microsoft.com/en/blog/post/october-2025/optimizing-your-content-for-inclusion-in-ai-search-answers) about optimizing content for inclusion in Bing’s AI answers. Here, the advice is more explicit:


“Schema is a type of code that helps search engines and AI systems understand your content.”


But it stops short of providing any clear explanation of the impact of schema on AI visibility.


We can’t definitively say schema markup helps with AI visibility. It’s also unclear if AI systems like ChatGPT or Claude use schema markup in the same ways that search engines do.


## Common types of schema markup


Google supports[dozens of schema markup types](https://developers.google.com/search/docs/appearance/structured-data/search-gallery) , but you don’t need to use them all.


In this section, we’ll explore some of the most commonly used types of schema.


### Organization markup


[Organization schema markup](https://developers.google.com/search/docs/appearance/structured-data/organization) tells search engines more about your business, such as its name, logo, address, and contact details.


[Organization schema](https://www.semrush.com/blog/schema-markup-for-company-corporations/) can also influence how your organization’s details appear in a knowledge panel in Google search results (if your business has a knowledge panel). For merchants, this can include details like returns policies and contact information.


### Product snippet markup


[Product snippet markup](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) is one of[two types of product schema markup](https://developers.google.com/search/docs/appearance/structured-data/product) and is used to provide search engines with extra product details for pages where users cannot make a purchase, like product review pages.


This type of markup allows search engines to display information in search results, such as:


- **Ratings and reviews** : Aggregated customer reviews and opinions
- **Pros and cons** : Key advantages and disadvantages of the product (only available for[editorial product review pages](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) )
- **Price and availability** : Basic pricing information without direct purchase options


Product snippet markup is particularly valuable for editorial websites, affiliate marketers, and ecommerce platforms showcasing products but not selling them directly.


If a tech review site publishes an in-depth review and adds product snippet schema markup to the page, Google can choose to display star ratings, review summaries, and pros and cons directly in the search results.


### Merchant listing markup


[Merchant listing markup](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) is the second type of product markup specifically for product pages, but it’s for pages where users can make a purchase.


Merchant listing markup can display key purchase-related details, such as:


- **Pricing and discounts** : Regular prices, sale prices, and promotions
- **Stock availability** : Indicates if the item is in stock, on back order, or out of stock
- **Shipping and returns** : Delivery estimates, shipping fees, and return policies


Ecommerce websites and online retailers that want to give potential buyers purchase-related information directly in search results can benefit from using merchant listing markup.


### Review schema markup


[Review schema markup](https://www.semrush.com/blog/review-schema/) can display star ratings, review summaries, and other review-related details in search results.


Google supports review snippets for[various structured data types](https://developers.google.com/search/docs/appearance/structured-data/review-snippet) , including:


- Product
- Local business
- Movie
- Book
- Software app
- Recipe


Review markup typically appears in one of two formats: a single rating and an aggregate rating.


A single rating shows just one reviewer’s rating along with their name:


An aggregate rating shows an average score based on multiple user reviews and the total number is indicated in parentheses:


Review and AggregateRating schema markup must describe an item you **don't** control the reviews for. Google won't show star ratings if the entity being reviewed is the same entity hosting the review.


This means a business can't mark up its own testimonials with Organization or LocalBusiness schema and expect to get star rating rich results.


### Article markup


[Article markup](https://developers.google.com/search/docs/appearance/structured-data/article) helps search engines understand news articles,[blog posts](https://www.semrush.com/blog/seo-blog-post/) , and sports articles and makes your pages eligible for a rich snippet in Google Search and[Google News](https://www.semrush.com/blog/google-news-seo/) .


When you use article markup, it can influence how article headlines, author names, and publication dates appear in search results. Like this:


### Local business markup


[Local business markup](https://developers.google.com/search/docs/appearance/structured-data/local-business) gives search engines key information about a physical business, including its address, phone number, and operating hours.


Brick-and-mortar shops, restaurants, service providers, or other businesses aiming to attract local customers can all benefit from local business markup. It helps confirm your correct business details, which supports how Google displays your business in local Search and Maps results.


***Further reading*** *:*[Local SEO: What Is It & How to Do It](https://www.semrush.com/blog/what-is-local-seo/)


### Breadcrumb markup


Breadcrumb markup tells search engines where a page sits in your site's hierarchy, like “Home > Laptops > Gaming laptops.”


Google uses this markup to build the breadcrumb trail it shows in search results, so users can see where the page fits in your site before they click. As with any structured data, Google doesn't guarantee the feature will appear.


## Schema markup examples


Here’s an example of article schema markup:


` {
"@context": "https://schema.org",
"@type": "Article",
"headline": "How to Add Schema Markup to Your Website",
"image": "https://www.example.com/images/schema-markup-guide.jpg",
"author": {
"@type": "Person",
"name": "Jane Doe"
},
"datePublished": "2026-07-01",
"dateModified": "2026-07-02"
}`


Here’s an example of product schema markup:


` {
"@context": "https://schema.org",
"@type": "Product",
"name": "Wireless Noise-Cancelling Headphones",
"image": "https://www.example.com/images/headphones.jpg",
"description": "Over-ear wireless headphones with active noise cancellation and 30-hour battery life.",
"sku": "WH-4521",
"brand": {
"@type": "Brand",
"name": "SoundCore"
},
"offers": {
"@type": "Offer",
"url": "https://www.example.com/products/wireless-headphones",
"priceCurrency": "USD",
"price": "149.99",
"availability": "https://schema.org/InStock"
}
}`


Here’s an example of[local business](https://www.semrush.com/blog/local-business-schema/) schema markup for a restaurant:


` {
"@context": "https://schema.org",
"@type": "Restaurant",
"name": "Bestia",
"image": "https://www.example.com/images/bestia-storefront.jpg",
"address": {
"@type": "PostalAddress",
"streetAddress": "2121 E 7th Pl",
"addressLocality": "Los Angeles",
"addressRegion": "CA",
"postalCode": "90021",
"addressCountry": "US"
},
"telephone": "+1-213-555-0100",
"openingHoursSpecification": {
"@type": "OpeningHoursSpecification",
"dayOfWeek": \["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"\],
"opens": "17:00",
"closes": "23:00"
},
"priceRange": "$$$"
}`


## How to add schema markup to your website


Here’s how to generate and add schema markup to your site:


### 1. Choose a schema type


Choose a schema type based on the main purpose of the page. Here are some of the most common page types and which schema type makes the most sense to use:


**Page type**


**Best-fit schema type**


Homepage or about page


Organization


Blog post or news article


Article


Product page where users can buy directly


Product (merchant listing)


Product review page with no direct purchase option


Product (snippet), with Review or AggregateRating nested


Physical business location page


LocalBusiness, or the most relevant specific subtype (e.g., Restaurant, Store)


More schema isn't automatically better. Don’t just mark up every possible entity on a page.


### 2. Generate your schema


Generate your schema markup manually, with an AI tool, with a plugin if you’re using WordPress, or with[Google’s Structured Data Markup Helper](https://www.google.com/webmasters/markup-helper/u/0/) .


#### How to generate schema markup manually


Generating your schema markup by hand gives you the most control over what’s included, but it does require a good understanding of what valid schema markup looks like. You add a <script type="application/ld+json"> block to your page's <head> (or <body>) with the properties for your schema type, using[schema.org](https://schema.org/) 's vocabulary as your reference.


This works well once you're comfortable with the syntax. It’s also easy to scale since you can use the schema you generate as a template across similar pages.


#### How to generate schema markup with AI tools


Generating structured data with AI tools like ChatGPT, Google Gemini, and Claude is as easy as entering a prompt, but it’s not foolproof and still needs to be validated before you implement it on your site.


To generate schema with an AI tool, just prompt the tool to generate it for a given URL.


For example, we asked ChatGPT to generate JSON-LD schema for our post about the[Google 3 Pack](https://www.semrush.com/blog/google-3-pack/) .


#### How to add schema markup with a WordPress plugin


Add schema markup to your WordPress website using a plugin like Yoast, Rank Math, or Schema Pro. These plugins let you add schema without having to touch any of your site’s code.


Here’s how to add schema markup to WordPress using Yoast:


First, install and activate[Yoast SEO](https://wordpress.org/plugins/wordpress-seo/) , then navigate to the plugin’s page. Then go to “ **Site representation** ” under “General.” On this screen, you can choose whether your site represents an organization or a person, along with the organization name(s) and logo. Yoast will automatically use these in your site’s schema markup.


To add schema markup to individual pages, go to the Yoast SEO widget in the WordPress editor and navigate to the “ **Schema** ” tab. Then update the page and/or article type using the drop-down menus. Yoast will automatically add the relevant schema markup for you.


#### How to add schema markup with Google’s Structured Data Markup Helper tool


Go to Google’s[Structured Data Markup Helper](https://www.google.com/webmasters/markup-helper/u/0/) , choose a schema type, enter your URL, and click “ **Start Tagging** .”


Your webpage will appear on the left. Data items for markup will appear on the right.


Using the tool, highlight the section on your page that you’d like to mark up.


For example, for an article, highlight the title and choose the “ **Title** ” data item from the menu that pops up.


The tool will then take the article’s title and place it next to “Title” on the right-hand side.


Continue adding relevant markup properties for that page.


When you’re done marking up your page, click on the “ **Create HTML** ” button at the top right of the screen.


You'll receive JSON-LD markup by default ([Google’s recommended format](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data#:~:text=In%20general%2C%20Google%20recommends%20using,less%20prone%20to%20user%20errors).) ), but you can switch to Microdata using the drop-down menu.


### 3. Add the schema markup to your website


Now that you have your code, add the schema markup to your webpage’s HTML, either in the <head> or <body> section.


You can easily copy the markup from the tool.


If you generated your schema with a WordPress plugin, the plugin will typically add it to your pages automatically.


### 4. Validate the markup


Validate the schema markup using Google’s[Rich Results Test](https://search.google.com/test/rich-results) .


Enter your URL or paste the generated schema markup into the tool, and it’ll confirm if your markup is valid.


Errors, warnings, and detected schema markup will show up on the right side of the screen:


If you need to fix any errors, edit your code directly on the left side of the page.


After you make changes, click the “ **Run Test** ” button at the bottom of the page:


Once you’ve validated the fix, update your code on the live page.


Use[Google Search Console](https://search.google.com/search-console/) ’s URL inspection tool to check for schema markup issues on individual live URLs.


Use the “ **Shopping** ” and “ **Enhancements** ” reports to find issues with schema markup across your site. Click into individual issues to see which URLs have issues.


## Schema markup best practices


Follow these best practices to ensure your schema markup is effective and compliant with[Google’s structured data guidelines](https://developers.google.com/search/docs/appearance/structured-data/sd-policies) :


- **Focus on pages that benefit from rich results** : Add schema to pages like product listings, reviews, articles, and local business pages, as these are more likely to show enhanced search results
- **Only use relevant schema** : Make sure the schema type accurately matches your page
- **Keep the markup up to date** : Regularly check and update your schema, especially for details that change over time, such as product prices or business hours
- **Add all the recommended properties for the types of schema you use** : If your schema type supports multiple details, fill out any that are relevant to you. For example, local business schema can include opening hours, accepted payment methods, and location details.
- **Ensure schema matches other online listings** : Make sure your schema details are consistent with similar details on your[Google Business Profile](https://www.semrush.com/blog/google-my-business/) , social media, and other websites
- **Use the most specific schema type** : Use the most precise schema subtype for organizations (e.g., Restaurant instead of just LocalBusiness)
- **Always test your structured data** : If your schema contains errors, Google may issue a[structured data manual action](https://support.google.com/webmasters/answer/9044175) , making the page ineligible for rich results. While this won’t affect[SEO rankings](https://www.semrush.com/blog/seo-ranking/) , it can reduce your visibility in search results.


## How to check your website’s schema markup


Regularly audit your site’s schema markup to ensure it remains accurate.


Use Semrush’s[Site Audit](https://www.semrush.com/siteaudit/) tool to find and fix schema-related issues at scale.


After you[set up a full audit of your site](https://www.semrush.com/kb/539-configuring-site-audit) , click the “ **View details** ” button under “ **Markup** .”


You’ll get a markup score that indicates how much of your schema data is valid or invalid. The higher your score, the fewer errors you have.


To see a full list of errors, scroll down to the “Structured Data Items” section. And click on the “ **View all invalid items** ” button.


Click on any entry in the “Affected Fields” column to see specific errors for a given page.


If any errors show up, regenerate the markup for those pages (by hand, with an AI tool, or your plugin), then validate it again with the[Rich Results Test](https://search.google.com/test/rich-results) .


This post was updated in 2026. Excerpts from the original article by Zach Paruch may remain.
