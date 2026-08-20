---
schema_version: "1.0.0"
document_id: "9accbde5c84da4f40c8cc903cf59f644cb080df9dfe95c91d65e4ae369b85bec"
company_key: "yc-hypotenuse-ai"
company: "Hypotenuse AI"
source_id: "yc-hypotenuse-ai-news-import-4c2c913250bd"
canonical_url: "https://www.hypotenuse.ai/blog/how-to-get-mentioned-by-ai"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-25T08:55:59.407814+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:1082bccfcb85992beda205a5e9cb02787ff900aef91985caea16cda547f5b7e8"
---

# The Ecommerce Playbook for Getting Mentioned by AI

More users are shifting from traditional search engines to AI-powered ones like ChatGPT and Perplexity.


And it shows[in the numbers](https://www.wsj.com/articles/ai-search-is-growing-more-quickly-than-expected-f75aa1ca) . In January 2024, AI platforms accounted for just 1.3% of search traffic. Last month, this number more than quadrupled to 5.6%.


In ecommerce, ChatGPT and Perplexity launched shopping features that recommends products.


Even on Google, there’s AI Mode and AI Overviews giving direct answers. If you’ve looked at your Search Console recently, you may see a huge divergence in impressions and clicks in the past month.


If you’re not already thinking of how to optimize for AI search, you should.


In this article, we’ll cover:


- How to optimize your product pages for AI visibility
- What else you can do on your website beyond product pages
- What other platforms to work on so LLMs can find, understand, and trust your brand


## **The core principle behind being mentioned by AI**


Your content needs to be easy for LLMs to understand and cite, include complete and trustworthy data so AI has enough factual detail to work with, and be published on platforms where AI models actually go looking for answers.


That boils down to three key areas:


- What you say about yourself (the content and structure on your own site)
- What others say about you (distributors, creators, forums, review sites)
- How consistently it’s being said (the more sources saying similar good things, the better)


We’ve previously written about[Generative Engine Optimization](https://www.hypotenuse.ai/blog/generative-engine-optimization-ecommerce) (or AIO, or LLMO — too many acronyms, but they’re essentially the same thing). That piece focused on strategy and what each platform like ChatGPT and Perplexity look out for.


This article is your step-by-step playbook. Let’s dive in.


## How to optimize your product pages for AI visibility?


There’re many things you can do to optimize for AI search. But if you had to start somewhere, make it your product pages.


This is where your product lives and breathes. Get this right, and you’re not just optimizing for AI search or SEO. Shoppers that land on your page are also more likely to convert.


It might feel daunting (there’s a lot to get right!). But most of it can be automated. And once your structure is in place, it runs like clockwork.


Here are 10 key actions to take when optimizing your product pages for AI visibility:


1. Use the right schema markup
2. Write relevant meta titles and descriptions
3. Structure your pages with clean headings
4. Use proper image alt text and file names
5. Use high-res product images that add context
6. Create product descriptions that match real shopper intent
7. Highlight specs in a structured, standardized format
8. Add FAQs that reflect real customer questions
9. Encourage reviews that mention specific use cases
10. Map your product attributes to match external standards


### 1. Use the right schema markup


Schema markup helps LLMs (and search engines) understand and cite your product pages more effectively. You can check your markup using the[Schema.org Validator](https://validator.schema.org/) or[Google’s Rich Results Test](https://search.google.com/test/rich-results) .


#### Choosing the right schema type


As an ecommerce brand, you should use:


- [Product schema](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) : for standalone products with no variants
- [ProductGroup schema](https://developers.google.com/search/docs/appearance/structured-data/product-variants) : when multiple variants, like size and color, are available on the same page (brands like Decathlon and Allbirds are already using this schema)


If you offer multiple colors and sizes on one page (e.g., with` ?size=` filters), wrap everything in` ProductGroup` and nest each variant as its own` Product` .


#### Minimum required fields


Even basic schema helps AI recognize your product:


- name
- offers.price
- offers.priceCurrency
- description
- image


#### High-impact optional fields


- aggregateRating / review (for star ratings)
- sku / gtin / mpn / brand (for unique identification)
- availability, itemCondition (for stock status)


From what we see, ChatGPT doesn’t feature products from your website if they’re unavailable. AI Mode does. That shows filling out the availability field is important.


#### **Dynamic schema in Shopify (or similar platforms)**


To scale your schema across thousands of products, use dynamic fields via Liquid:


- Conditionally select` Product` or` ProductGroup` based on variant count, here’s a trimmed down example:


` {% if product.variants.size > 1 %}
"@type": "ProductGroup"
{% else %}
"@type": "Product"
{% endif %}
`


- Pull in live product data, for example:


` "name": {{ product.title | json }},
"price": "{{ product.price | money_without_currency }}",
"currency": "{{ shop.currency }}",
`


This keeps your markup accurate and always up-to-date across your catalog


### 2. Write relevant meta titles and descriptions


Meta tags are what show up on Google search results pages. AI tools often use these to craft their answer snippets.


Having precise, intent-aligned metadata gives your product page a better shot at being pulled into relevant AI-generated responses.


Let’s say you’re selling a women’s hiking backpack.


Someone might ask the AI search engine:


**“Can you recommend a backpack for women? I’m going on a multi-day hike to Everest Base Camp.”**


There are a few cues in this query:


- **Backpack** – the product type
- **Hiking** – the activity
- **Women** – the audience
- **Everest Base Camp** – implies multi-day, cold-weather, and high-capacity needs


A good meta title might look like **70L Women’s Hiking Backpack** instead of just **Hiking Backpack** .


And your meta description should highlight use cases, benefits, and who it’s for. For example:


**“Durable and lightweight 70L backpack built for multi-day treks in cold weather. Designed for women seeking comfort, storage, and reliability.”**


The more specific and aligned your meta tag is to real-world queries, the easier it is for AI to pick it up.


### **3. Structure your pages with clean headings**


A clear page structure makes it easier for AI to understand what your product is about, who it’s for, and when to recommend it. It also improves readability for shoppers skimming your page.


Start with using proper heading hierarchy.


Use only one **H1** on every page — it should be your product name. Your H1 doesn’t need to match your meta title exactly, but should be close.


Use H2s and H3s to break down sections like “Features”, “Specifications”, “Frequently asked questions”, or “Reviews”.


These act as signposts for both AI and humans, improving comprehension and navigation.


### 4. Use proper image alt text and file names


When AI systems crawl your site, they don’t just look at your text — they also analyze your images.


Tools like ChatGPT, Perplexity, and Google AI Mode are increasingly multimodal. That means they take in not just text, but also voice and images to determine what to recommend.


Your image file names and alt text help these systems understand what your product visuals represent.


#### Therefore, use descriptive image file names.


Avoid default exports like` IMG_43110.jpg` or` dress43110.jpg` . Instead, name images based on the product and context.


Here are some good examples:


- womens-hiking-backpack-70L-sideview.jpg
- ceramic-planter-white-medium-front.jpg


Keep it consistent across your catalog. A simple format like` \[productname\]-\[color\]-\[angle\]` makes it easier to manage.


#### Write clear alt text


Alt text helps AI and screen readers interpret what’s shown in an image. It should describe the content and purpose, not just the object.


A good alt text would look like:


**“Front view of black 70L women’s hiking backpack with adjustable straps and rain cover”**


Avoid repeating the file name or stuffing keywords — just describe what’s visible and relevant. Or use[Hypotenuse AI’s image alt text generator](https://www.hypotenuse.ai/tools/ai-image-alt-text-generator) .


#### Align your image alt text and file names


For example:


- Image file name: womens-hiking-backpack-70L-sideview.jpg
- Image alt text: Side view of a 70L black hiking backpack for women, shown with padded straps and hip belt


This consistency gives AI more confidence to surface your product in visual or multimodal results. It’s also oddly satisfying.


### **5. Use high-res product images that add context**


Product images are often the first impression of your brand. Blurry, disproportionate photos can immediately signal low quality and turn shoppers (and AI) away.


Beyond resolution, what your product images show matters a lot, especially with more of us searching with image.


Here’s how to improve your images for shoppers and AI:


#### 1. Improve image quality and consistency


Make sure your images are crisp, well-lit, and not pixelated. They should hold up under zoom and look consistent across your catalog.


Crop and resize them so they’re aligned and proportionate. This keeps your product pages clean and professional.


#### 2. Show the product in context


Lifestyle or use-case imagery helps both AI and shoppers understand how the product fits into real-world scenarios:


- A mattress in a small bedroom
- A carry-on suitcase in an overhead bin
- A backpack worn on a hike


These images subtly answer questions like “Will it fit?” or “Is this made for travel?”


#### 3. Include multiple angles and variations


Display the product from different views to reduce ambiguity and give AI more visual data:


- Front, side, back
- Inside compartments
- Color and size variants


💡 Use[Hypotenuse AI’s image editor](https://www.hypotenuse.ai/features/ai-batch-editor) to upscale images, place products in context, and standardize your entire catalog in bulk.


### **6. Create product descriptions that match real shopper intent**


Just a few years ago, we were searching for products like a robot. We Google terms like:


- black shoes for running
- long dress for work
- large hiking backpack women


With AI search, we’ve become more human. Our questions are more conversational and way more specific.


Instead of “black shoes for running”, we ask:


**“Can you recommend running shoes for wide feet that are good for marathon training?”**


To get featured in AI-generated answers or ranked in relevant searches, your product descriptions need to reflect real-world use cases and language.


Here’s how to do that:


#### Mirror how people ask questions


Instead of “Made of nylon, fits 70L”, go for: “Made for multi-day treks, this 70L nylon backpack is water-resistant, breathable, and supports heavy loads without strain.”


#### Connect features to benefits


Don’t stop at “100% cotton, 300 thread count.”. Instead, say: “Soft 100% cotton with a 300-thread count — perfect for warm sleepers who want breathable comfort.”


#### Use bullets to surface key points


Not every detail fits naturally into a paragraph. Bullets help highlight your most compelling benefits. For example:


- Ventilated back panel keeps you cool on steep climbs
- Fits cabin-size requirements on most airlines


#### Include target audience and scenarios


Be clear about who your product is for, and when to use it: “Designed for active women tackling week-long hiking trips or international backpacking adventures.”


By writing like your ideal customer searches, you’re helping AI (and shoppers) connect the dots faster and recommend your product with confidence.


#### But don’t lose your brand voice in the process


Optimizing for AI is important. But don’t forget who you’re really writing for. Your descriptions should still sound like you. They should be clear, helpful, and aligned with your brand. AI might help shoppers discover your product, but it's still your voice that earns their trust and gets them to convert.


#### Bonus: Address the most pressing concern on the page


Every product has that one question people need answered before they buy.


Sometimes, it’s about fit, compatibility, or durability. Other times, it’s “Will this actually work for me?”


If you know the most common blocker to purchase, make sure your content addresses it clearly and early. Here’s a great example by Away, featuring a chart of all major airlines and whether their luggage fits.


💡 *Tip:* Use[Hypotenuse AI’s product description generator](https://www.hypotenuse.ai/tools/product-description-generator) to scale your content, stay aligned with your brand voice and guidelines, and hit your product launch timelines with ease.


### **7. Highlight specs in a structured, standardized format**


Specs have always mattered to shoppers. With AI, they matter even more.


In fashion, people look for sizing charts. In home & living, it’s about materials, dimensions, and warranties. In electronics, they care about battery life, ports, and compatibility.


Buyers today are savvy. They’re not swayed only by fluffy language — especially in more technical industries like tools, electronics, automotive, or industrial supplies. And neither is AI.


A shopper might ask:


“What’s a robot vacuum that fits under low furniture and works on both hardwood and carpet?”


If your product specs are buried in dense paragraphs or formatted inconsistently, AI may skip over your product.


Clear, structured specs give AI the data it needs to extract facts, make comparisons, and recommend your product confidently.


Here’s how to make that happen:


#### Use a consistent spec table or bullet format


Group all technical specifications in a single, scannable format, like a table or bullet list. It makes parsing easier for both AI and shoppers.


For example:


- Height: 7.8 cm
- Battery Life: 120 minutes
- Surface Compatibility: Hardwood, tile, carpet
- Dustbin Capacity: 600ml
- Connectivity: Wi-Fi, voice assistant compatible


#### Standardize units and terminology across your catalog


Don’t mix “cm” and “inches” across different products, or alternate between “battery life” and “runtime.” Consistent terminology increases AI accuracy in extraction and comparison.


#### Include specs AI can match to queries


Think about what people search for, and make sure your specs answer it:


- Size constraints (e.g., height to fit under furniture)
- Feature compatibility (e.g., works with Alexa, auto-return to dock)
- Certifications or ratings (e.g., Energy Star, HEPA filter included)


💡 *Tip:* Use[Hypotenuse AI’s product data enrichment feature](https://www.hypotenuse.ai/features/ecommerce-product-data-enrichment) to help clean, structure, and standardize specs at scale — especially useful if you manage large product catalogs across categories.


### **8. Add FAQs that reflect real customer questions**


AI tools like Google’s AI Mode, Perplexity, ChatGPT answer millions of product-related questions each day.


In particular, AI Mode uses a query fan-out technique. It takes the main query, generates a bunch of related sub-questions, searches for answers, then organizes them into a concise response.


That’s where FAQs come in.


Adding a FAQ section allows you to anticipate shopper concerns and directly answer these queries — questions that are too specific to cover in a general description, but are ones buyers (and AI) care about.


Outdoor brand Rumpl includes an FAQ section on their product pages, featuring a mix of store-wide and product-specific questions. These FAQs cover things like care instructions, warmth levels, materials, and warranty.


Here’s how you can create effective FAQs as well:


#### 1. Source real questions from:


- Customer reviews and support tickets
- Reddit threads, niche forums or Facebook groups
- Perplexity follow-up questions
- "People Also Ask" sections on Google


#### 2. Match how people phrase them:


Use the same natural phrasing shoppers would type or speak:


Instead of “Dual-surface compatibility inquiry” (nobody speaks like that), try “Will this vacuum work on both tile and carpet?”


#### 3. Provide concise, helpful answers


Get straight to the point while including key features or limitations:


“Yes, it automatically adjusts suction based on the surface. Works well on tile, wood, and low-pile carpet.”


And if possible, use[FAQPage schema](https://developers.google.com/search/docs/appearance/structured-data/faqpage) to help AI and Google extract and display your answers more easily.


### **9. Encourage reviews that mention specific use cases**


Detailed, scenario-based reviews help LLMs understand how your product performs in real-world context like “comfortable for 10-hour flights”, or “didn’t overheat even in 40º heat”.


It also adds social proof and enriches your page semantically. The result is getting more diverse phrasing and keywords on your page without flooding it with more bullet points.


If you don’t already collect reviews, start by adding a review section that updates automatically once approved. And trigger the request via post-purchase email flows.


But not all reviews are helpful. A review like “Great product. I love it!” doesn’t tell shoppers or AI much.


Here’s how to encourage better ones:


#### Prompt specific questions


When asking for reviews, guide your customers with simple prompts:


- What did you use this for?
- What stood out to you?
- Would you recommend it for a specific type of user or situation?


Here’s an example by travel brand, Away.


#### Feature strong examples


Showcase reviews that mention specific uses or outcomes. It signals the style you value and encourages others to follow suit.


If you don’t prompt intentionally, customers may skip the details that matter most. Every review is an opportunity. So tweak your review request flow until you start getting the kind that actually helps.


### **10. Map your product attributes to match external standards**


Google has their own taxonomy and data requirements. Amazon has theirs. Your[brand.com](http://brand.com/) has its own too.


You’ll need to make sure:


- Your products are categorized correctly according to each platform’s taxonomy
- Your attributes follow the required format (e.g., 5 bullets for Amazon, structured values for Google)
- All required fields are filled, and optional ones too — the more complete your data, the better


The cleaner and more compliant your data, the more likely it is to be approved, ranked, and featured in AI-generated results and marketplace filters.


💡 Tip: With Hypotenuse AI, you can automatically categorize products, tag attributes, enrich missing data, and format everything to match each channel’s requirements — all in bulk.


## What else you can do on your website (beyond product pages)


Product pages are a great starting point. But the other pages matter too.


LLMs evaluate the context of your entire site — what it talks about, how well-organized it is, and how fast it loads.


Here are 4 high-impact areas beyond product pages:


- Categorize products accurately
- Tag products based on how people search
- Create blog articles that target real shopper questions and intent
- Improve site speed


### **Structure your product categories around real shopper behavior**


How you categorize your products affects more than just navigation — it shapes how AI and shoppers understand your catalog.


If you accidentally list a frying pan under coffee machines, LLMs might assume it’s some kind of all-in-one breakfast device (which is pretty amazing tbh).


Use clear, intuitive categories that match how people browse. For example, instead of lumping everything under “Collections” or “Best Sellers”, organize by use case or product type:


- “Pet-friendly vacuums”
- “Cold-weather jackets”
- “Furniture for small spaces”


These can still appear under collections like “Best Sellers”. But they shouldn't be your primary categories.


In industries where shoppers follow trends — like fashion, home decor, or beauty — trend-based categories can drive both discovery and AI context. For example:


- “Coastal grandmother”
- “Dopamine dressing”
- “Y2K revival”


These categories don’t just help humans find what they’re subconsciously looking for — they also help AI connect your products to emerging searches and cultural moments.


MESHKI has a section on their navigation bar dedicated to trending categories.


💡 Tip: With Hypotenuse AI, you can identify trending themes, categorize products accordingly, and keep your taxonomy flexible as trends evolve.


### **Tag products to highlight features, use cases, and trends**


Tags help bring depth to your catalog without bloating your category structure.


They’re especially powerful for capturing:


- **Product traits** – waterproof, packable, rechargeable
- **Use cases** – carry-on approved, gym-ready, winter-friendly
- **Trends** – coastal, dopamine dressing, minimalist


Done well, tags act as soft filters that connect the dots between shopper needs and product attributes. They also give AI extra clues about what your product is suited for, even if it’s not in the main description.


Keep your tags consistent across your catalog. Not “fits 15-in laptop” and “fits 15-inch laptop”.


Avoid tag clutter like “fits 15-inch laptop” and “laptop compatible” unless they serve different filtering purposes. A tight, well-structured tagging system is easier to scale and parse.


💡 Tip: Hypotenuse AI can automatically categorize and[tag your products](https://www.hypotenuse.ai/features/automated-product-tagging) based on traits, use cases, and even emerging trends — especially helpful when managing large, fast-changing catalogs.


### Create blog articles that target real shopper questions and intent


Your product pages can’t cover every nuance or scenario. If they had to, they’d probably be 10 scrolls long. That’s where blog content comes in.


Blog articles are perfect for targeting **intent-rich, long-tail queries** that shoppers (and AI) are searching for, especially the ones that fall just outside the scope of a product page.


There are a few types of blog posts that work especially well:


- Best \[product\] round-up
- How-to guides
- Product comparisons


#### Create “Best of” \[product\] round-ups for real use cases


"Best of" round-ups are listicles that group your products by use case, need, or audience—just like how people search.


Let’s say you sell coffee equipment. You can create a blog article recommending different coffee grinders for specific scenarios:


- Best coffee grinder for beginners
- Best grinder for home baristas who love to experiment
- Best grinder for busy coffee lovers with no time to spare


This format lets you speak to different shopper intents, explain why each product fits that need, and cover a wider range of queries — all within one post.


It’s also easier for AI tools to associate your products with specific contexts like “best for travel” or “best for small kitchens”. And you’re more likely to be mentioned by AI for similar queries.


#### Write product-relevant how-to guides


How-to questions are evergreen. AI tools often answer these questions directly on the results page. That means users don’t always need to click. But that doesn’t make how-to guides any less valuable.


In fact, these articles still play a major role in how LLMs learn and cite content.


AI models often pull information from well-structured how-to guides to form their responses. If your content is clear, comprehensive, and helpful, it’s more likely to be cited — even if the user never clicks through.


Beyond citations, how-to guides are an opportunity to build trust and influence decisions early in the buyer journey.


Let’s say someone new to coffee asks: “How do I make cold brew at home?”


Here’s what a strong guide might include:


- What cold brew is and how it differs from iced coffee
- The ideal grind size and why it matters
- Equipment needed, with options for different budgets
- Step-by-step instructions with visuals or tips
- Troubleshooting common mistakes (e.g. too bitter, too weak)


Throughout, you can naturally introduce your products:


“This method works best with a coarse grind. Look for grinders that let you adjust the setting, like our \[Model X\].”


This lets you connect your product to the shopper’s need *without hard selling* . And when AI crawls your site, it picks up on those connections too.


#### Create product comparison posts


For higher ticket items, shoppers tend to compare multiple products before settling on one.


A blog post that breaks down the pros, cons, and differences between Product A and Product B helps them decide faster. It also gives AI structured information to reference when answering comparison queries.


Your comparison should include:


- A quick TL;DR chart
- Side-by-side spec breakdown
- Contextual recommendations (e.g., "better for beginners," "best for portability")
- Scenarios where one product wins over the other


If you’re unsure what to cover, try running the query on AI search tools. The way they answer can give you a clear idea of what to include.


### Improve site speed


This is not new. But it’s still one of the most important levers you can pull.


Site speed affects everything: SEO rankings, conversion rates, and yes, AI visibility too. If your pages take too long to load, AI tools may not crawl or extract them properly.


Run a check with[PageSpeed Insights](https://pagespeed.web.dev/) and follow the recommended fixes. Even small improvements can go a long way.


## What other platforms to work on so LLMs can find, understand, and trust your brand


### Distributors


If you sell through distributors, don’t just send over a generic product description or spec sheet.


Treat their listings like an extension of your own site. Provide well-written content that includes use cases, target audience, and complete product data (you can use Hypotenuse AI for this).


Some distributors have strong domain authority — which means their product pages are more likely to be cited by AI models. The better your content, the higher your chances of being mentioned.


### Reddit


Reddit shows up everywhere — like Perplexity, ChatGPT, Google. On AI Mode, there’s also a “Discussions and forums” section (which is often just Reddit).


That makes it one of the most visible sources for opinions and product recommendations.


The best part is it’s relatively lower effort since there’s no need for video production. Just text.


Set up Google Alerts or keyword triggers to surface Reddit threads worth jumping in on. You can also build agentic workflows that notify your team, assess the opportunity, and even suggest a comment draft.


For example, this is a great Reddit thread that Deuter or Osprey could've pounce on the moment it appeared in the wild (or perhaps Fun_Apartment631 is Osprey).


### YouTube


In AI Mode, most product results include a section for video content — and YouTube dominates that space.


If your brand is already active on YouTube, you’re one step ahead. If not, it’s worth partnering with creators in your niche.


Get featured in product round-ups, reviews, and “best of” lists. These videos are often cited directly in AI product cards. Even when they aren’t, they shape what LLMs learn through captions, reviews, and references across the web.


Rumpl’s YouTube influencer game is pretty strong.


### Instagram


As of July 2025, Google officially indexes Instagram. That means its content could start showing up more often in AI search results.


While we haven’t seen Instagram in product recommendations, they likely will.


If you already have an Instagram strategy, branch out to other topics.


Beyond the usual product shots, experiment with content that helps people decide — mini how-tos, product comparisons, and POVs from real users. You can also work with influencers to seed useful, specific reviews.


### Industry publications


Run product-related searches in tools like Perplexity or AI Mode and see which review sites and industry blogs show up often.


Those are the publications worth building relationships with.


Pitch to be featured in roundups, product reviews, or provide expert insights they can quote. It’s a good way to get your brand in front of both shoppers and AI.


Just look at this review of the sweet-sounding battery beast JBL speakers. Makes us want to get one.


### Other channels: Quora and Pinterest


You might also spot citations from platforms like Quora and Pinterest, especially when posts are helpful, specific, and evergreen.


If you’ve already covered the higher-impact channels, these can be worth testing to extend your reach. But it's still best to prioritize your top few channels and strengthen your presence there before moving onto the next.


## Final thoughts


The AI race isn’t about hacking a single channel or gaming a new algorithm. It’s about being genuinely helpful, in a format that’s easy for both people and machines to understand.


That means:


- Structuring your content clearly
- Speaking in the language your shoppers use
- Showing up on the platforms where people (and AI) look for answers, in the way you want your brand and products to be portrayed


Most importantly, don’t just optimize for AI. Optimize for discovery *and* decision-making. Because getting mentioned is only half the battle. The other half is winning trust when people land on your brand and turning them into happy, paying customers.
