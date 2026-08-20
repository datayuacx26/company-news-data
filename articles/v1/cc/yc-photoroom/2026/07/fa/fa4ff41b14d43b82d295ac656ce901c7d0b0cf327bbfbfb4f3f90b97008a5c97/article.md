---
schema_version: "1.0.0"
document_id: "fa4ff41b14d43b82d295ac656ce901c7d0b0cf327bbfbfb4f3f90b97008a5c97"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/new-in-product-may-2026"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:9d7c014374943c02456669729aca7c6e4a5a03027b0c7f162fea442aed6eec24"
---

# What's new in product: May 2026

This month is all about accuracy. When a shopper lands on your listing, the image has one job: show them exactly what they'll get. Anything off, a wrong logo, an invented detail, a product that doesn't quite match, and you lose the sale. In our recent **[The Hidden Cost of Product Photography for SMBs report](https://www.photoroom.com/industry-trends/product-photography-cost-for-smbs) ,** 59% of sellers said they'd lost sales to bad product photos.


So in May, we focused on product fidelity. A more accurate video generator with multiple reference images, a Product Fixer to match your original product image, plus improvements to Batch, Photoroom for Shopify, and the Photoroom MCP for Claude.


Table of content:


1.


[Recently launched](https://www.photoroom.com/inside-photoroom/new-in-product-may-2026#1)


2.


[API updates](https://www.photoroom.com/inside-photoroom/new-in-product-may-2026#2)


3.


[Coming soon](https://www.photoroom.com/inside-photoroom/new-in-product-may-2026#3)


## **Recently launched**


## More accurate product videos with multiple reference images


A product video only works if the product looks exactly like the one you're selling. Generating from a single photo meant the AI had to guess at the angles it couldn't see, and those guesses don't always match.


Now you can add multiple reference images to improve product fidelity. Show it the front, the back, and the details that matter, and it builds a video that stays true from every angle. The result shows your true product more accurately, which means fewer reshoots, fewer returns, and more “add to carts”.


**Platform:** web, iOS, and Android


**For:** Max and Ultra users


[Learn more about the video generator](https://www.photoroom.com/tools/video-generator)


[Learn more about the video generator templates](https://www.photoroom.com/tools/video-generator/templates)


[Watch our latest webinar How to Sell More with AI Video](https://www.youtube.com/watch?v=aQ7TVEld9MM&list=PL7VBorktZT7KMNKQFZprx5qPr8SNVmPl9)


## More accurate product images with product fixer


AI editing is powerful, but it can get your product wrong. On a listing, those small slips are the difference between a visual a shopper trusts and one that makes them hesitate.


Product Fixer now does a better job of preserving what makes your product yours. With 1 brush, our model corrects them based on your original product image, so the result stays faithful to your product so you get visuals you can publish with confidence. You can find it under AI Tools tab, or while editing inside Virtual Model, Product Beautifier, Product staging, ghost mannequin, edit with AI, AI ironing, Recolor, and Flat lay.


This does not consume any AI credits.


**Platform:** web. Coming soon to mobile


**For:** Pro, Max and Ultra users.


[Learn more](https://help.photoroom.com/en/articles/15289495-use-product-fixer-to-improve-image-fidelity-web-app)


## Streamline workflow with Batch mode


### ***1. AI Tools in Batch on mobile***


You can now use Batch on mobile to edit with AI tools such as Ghost Mannequin, Virtual Model, Recolor, Flat Lay, Beautify, Product Staging, and Edit with AI. Apply AI to a single image or up to 250 at once, so you can save time, maintain the same polished look across hundreds of images, and manage all your product listing edits in one centralized place. No more switching between tools.


**Platform:** iOS and Android. Already live on the web app


**For:** Max and Ultra users


### *2. Organize content easily with sections within batch*


You can now create sections within Batch to group and manage thousands of images more efficiently, making it easier to streamline team workflows and quickly find the right assets for sale. Save time and keep your catalog structured exactly the way you need.


**Platform:** web


**For:** Pro, Max and Ultra users


### *3. Resize easily with Expand with AI in Batch*


When resizing a batch, you can now decide what happens to each image inside the new canvas. After picking a size, choose between **Expand with AI** (extends the background naturally), **Fill image** (scales up and crops the edges), or **Fit image** (keeps the whole picture visible with transparent space around it). One click applies the choice to every selected image. No more reshooting or editing image-by-image to fit a new aspect ratio.


**Platform:** web


**For:** everyone


## Sell more on Shopify


### ***1. SEO metadata, in your store's language***


Your store ships globally, but translating titles, descriptions and alt text for every market is a chore, so our SEO metadata only in English was holding you down!


Connect your Shopify store to access auto generated produces titles, descriptions, file names and alt text in your store's language by default, on web and iOS. Optimised so you list perfect on ChatGPT, and Google!


Say welcome to international sales 🌎.


**Platform:** Web


**For:** All users users who connect their Shopify Store


### *2. See every weak listing at a glance*


Hundreds of listings, and no fast way to tell which ones are quietly under-converting. You only find the gaps after the sales are already lost.


We’re fixing this! Now when you add your Shopify catalog, the Listing Score shows on every product in your catalog grid, with a progress indicator per listing. Sort by lowest score to surface the weakest ones first.


You can now triage your catalog in seconds. Fix the listings hurting your conversions, instead of the ones you happen to notice.


**Platform:** Web


**For:** All users users who connect their Shopify Store


### *3. Run your Shopify catalog from your phone*


Mobile editing used to be limited, you couldn't really run a catalog from your phone, just touch up the odd image. So listings were stuck waiting for the next desk session.


Say welcome to a new Products tab on iOS surfaces your full Shopify catalog. Multi-image insert, the Publish button on the product page, and even the SEO image description generation, all native to mobile.


You can now capture in the stockroom, edit on the move, publish from your phone. The Shopify catalog now travels with you.


**Platform:** iOS


**For:** All users users who connect their Shopify Store


# API Updates


### Portrait-ready lighting in one parameter


AI Relight was tuned for products, not people. Run it on a portrait and it could over-smooth skin, flatten natural features like freckles, and skip the retouching that face shots actually need.


AI Relight now has a portrait mode on the API. Set` lighting.mode` to` ai.optimize-portrait` to switch it on. The mode preserves natural skin features like freckles, applies stronger blemish removal, and adds subtle teeth whitening, so portraits look enhanced rather than artificial. It is a single parameter on the existing AI Relight capability, so teams already calling AI Relight can adopt it without changing their integration.


View the full parameter reference and try it in the API[here](https://docs.photoroom.com/image-editing-api-plus-plan/ai-relight) .


**Platform:** now available via API. AI Relight is also available on the web app.


### Use the Photoroom API inside Claude, no code required


Using the Photoroom API meant writing code against the endpoints. Less-technical users and AI agents had no way to reach Photoroom's editing capabilities conversationally.


Photoroom now has an MCP connector you can add to Claude. Add the connector (` mcp.photoroom.com/mcp` ), enable the Photoroom API, and you can edit images and chain multi-step editing workflows through natural language directly in Claude. It runs on your existing API credits, so it plugs into your current subscription. Adding the connector requires admin rights in both Claude and Photoroom.


Learn more[here](https://www.photoroom.com/api/claude) .


**Platform:** now available via API.


## Coming soon


## **4k quality for more AI tools**


Since we’ve introduced higher quality for Virtual Model recently, we are rolling out even more quality upgrade across our AI tools: Ghost Mannequin, Edit with AI, Product Staging, Product Beautifier, and Flat Lay will all have export resolution up to 4k that ensure sharper accuracy, enhanced realism, and visuals that meet marketplace and print standards.


**Platform:** web, iOS and Android


**For:** Max and Ultra users


###
