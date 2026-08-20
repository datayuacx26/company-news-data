---
schema_version: "1.0.0"
document_id: "c6f417fafa6dc99428606374eb109fc3198778ae3b03fb3566223c9e683bd883"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/new-in-product-february-2026"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:433e29c501d9e88acaee8e792bca0d5214431dbdea6a3c06769dd64bd288459d"
---

# What's new in product: February 2026

New tools released this February help sellers create higher-quality, more accurate product visuals faster to build trust and drive conversions. Discover 4K Virtual Models, instant AI Ironing, the new Shopify Product Catalog, and even more innovations coming to the API.


Table of content:


1.


[Recently launched](https://www.photoroom.com/inside-photoroom/new-in-product-february-2026#recently-launched)


2.


[API updates](https://www.photoroom.com/inside-photoroom/new-in-product-february-2026#api-updates)


3.


[Coming soon](https://www.photoroom.com/inside-photoroom/new-in-product-february-2026#coming-soon)


## **Recently launched**


### **Virtual Model: higher quality, higher conversion**


Introducing higher quality for[Virtual Model](https://www.photoroom.com/tools/virtual-model) , now available in three tiers: Standard (1K), Advanced (2K) – for Max users, and Premium (4K) – for Ultra users. Our highest-resolution models meet marketplace and print requirements while showcasing your brand at its best. With enhanced realism, lifelike skin, improved textures and lighting, and precise preservation of product details like logos and patterns, you get sharper accuracy, greater control over quality, and higher-performing visuals that build brand trust and drive conversions.


**Platform:** Web, iOS and Android


**For:** Max and Ultra users


### **Perfectly pressed in seconds with AI Ironing**


Instantly transform wrinkled clothes into perfectly ironed, professional-looking pieces without steaming or manual ironing. Ideal for fashion sellers who used to spend hours prepping garments, the new Ironing tool uses AI to deliver ready-to-sell results in seconds. Save time, eliminate the extra effort, and move products online faster with polished visuals that help you sell quicker.


**Platform:** Web, iOS and Android


**For:** Pro, Max and Ultra users


### **Launch faster on Shopify with Product Catalog**


Great news for Shopify users! You can now create and publish product listings faster than ever.[Product Catalog](https://www.photoroom.com/product-catalog) lets you connect your Shopify store, giving you one seamless workflow from product photo to published listing.


What you can do:


-


Create high-fidelity AI images with virtual models, backgrounds, and shadows.


-


Manage product visuals and apply batch changes at scale.


-


Generate AI-written product descriptions and alt text optimized for SEO.


-


Publish listings directly to Shopify. No switching tools.


**Platform:** Photoroom web app + Shopify app


**For:** Max and Ultra users, Shopify sellers


## API updates


The three updates below are built for teams processing images at scale. Before diving into the details, watch our product walkthrough to see how each capability fits into your production workflow.


### Edit with AI


Edit with AI is now available via API for precise product edits using text instructions. Upload an image, describe the change, and only the requested area is updated while the rest of the product stays intact. Use cases include changing product colors, removing unwanted objects, placing products in lifestyle scenes, converting flat captures into ghost-mannequin style visuals, generating flat lay shots, or creating a different product angle. Built for marketplace and retail catalogs where SKU-level accuracy matters.


Self-serve API uses a pre-set standard model optimized for speed and quality. Set` editWithAI.mode` to` ai.auto` and use` editWithAI.prompt` to describe the edit. To reduce randomness, set a fixed` editWithAI.seed` using the same image, prompt, and seed returns similar results for consistent outputs at scale.


View examples and try it in the API[here.](https://docs.photoroom.com/image-editing-api-plus-plan/alpha-edit-with-ai)


**Platform:** also available on web, iOS and Android.


### Subject outline


Add a colored outline around the subject of any image with full control over color, width, and blur. Set` outline.color` to any hex code or supported color name, adjust thickness with` outline.width` (0–0.1), and apply a soft glow or feathered edge using` outline.blurRadius` (0–0.025). To preserve the original background, set` removeBackground` to` false` . Use cases include product stickers, promotional banners, social content, and catalog images where subject separation needs visual emphasis.


View examples and try it in the API[here](https://docs.photoroom.com/image-editing-api-plus-plan/subject-outline)


**Platform:** also available on web, iOS and Android.


### Background blur


Blur the background of any product image using two modes:` bokeh` for a natural depth-of-field effect with soft circular highlights, and` gaussian` for a smooth, even blur that reduces background detail without distortion. Control blur intensity with` background.blur.radius` (0–0.05). Set` removeBackground` to` false` to keep the background visible while isolating focus on the subject. Built for marketplace and retail use cases where clean subject focus matters without full background removal.


View examples and try it in the API[here](https://docs.photoroom.com/image-editing-api-plus-plan/background-blur)


**Platform:** also available on web, iOS and Android.


## **Coming soon**


### **Bulk edit with AI tools to speed up workflow**


You will soon be able to use Batch on the web app alongside our e‑commerce AI tools: Ghost Mannequin, Virtual Model, Recolor, Flat Lay, Beautify, Product Staging, and Edit with AI, to bulk edit and generate product variations in minutes.


**Platform:** web. Coming soon to mobile


**For:** Pro, Max and Ultra users


### **Save and reuse custom models for consistent brand look**


Save your own model in your Brand Kit and instantly apply it across visuals for future campaigns.


**Platform:** web, iOS and Android


**For:** Max and Ultra users


##


### Video generator


Video generator via API is coming soon. Create product videos from existing assets by entering a short prompt and defining the scene. Video templates are coming to iOS and Android too. Stay tuned.


**Platform:** available on web, iOS and Android, API coming soon


**For:** Max and Ultra users


### Virtual model


API access for virtual model is coming soon. Generate on-model imagery with support for custom poses, backgrounds, and flexible output sizes. The API will also deliver higher quality output options: Standard (1K), Advanced (2K), and Premium (4K).


**Platform:** available on web, iOS and Android, API coming soon


**For:** Pro, Max and Ultra users
