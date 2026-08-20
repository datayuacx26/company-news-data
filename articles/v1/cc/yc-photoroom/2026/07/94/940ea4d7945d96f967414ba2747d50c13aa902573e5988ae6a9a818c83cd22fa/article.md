---
schema_version: "1.0.0"
document_id: "940ea4d7945d96f967414ba2747d50c13aa902573e5988ae6a9a818c83cd22fa"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/new-in-product-april-2026"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:9e5c2718ebc92a3d38dd2d3e8b20f6cd82c6d58312288dbb82b367f4764f767b"
---

# What's new in product: April 2026

Sell more with less effort this month. From turning a single photo into a full **AI Shot List** to generating **sales-ready videos** in one click, we’re automating the hardest parts of e‑commerce. Optimize your Shopify store with **Listing Scores** that show exactly where to improve, and fine-tune your realism with advanced **AI Shadow controls** . Whether you need 360° displays or professional lifestyle shots, April is all about getting your products market-ready in minutes.


Table of content:


1.


[Recently launched](https://www.photoroom.com/inside-photoroom/new-in-product-april-2026#recently-launched)


2.


[API updates](https://www.photoroom.com/inside-photoroom/new-in-product-april-2026#api-updates)


3.


[Coming soon](https://www.photoroom.com/inside-photoroom/new-in-product-april-2026#coming-soon)


## **Recently launched**


### **From inspiration to sales-ready video in 1 click**


Introducing a new video generator with 300+ templates designed for consistent, high-quality results—no prompts required. Choose from pre-built scenes and instantly apply your product to virtual try-ons with AI models or 360° displays, creating polished, sales-ready videos in minutes. Skip expensive shoots and long edit cycles entirely. With even more upcoming updates like smart template recommendations, multi-image references for greater accuracy, and advanced AI editing controls, sellers can go from idea to high-converting video in a single click.


**Platform:** web, iOS, and Android


**For:** Max and Ultra users


**[Learn more about video generator](https://www.photoroom.com/tools/video-generator)**


### Photoroom for Shopify


#### *Boost sales with the Listing Score ​*


The new Listing Score gives every product in your catalog a visible rating and progress indicator, so you can see at a glance what's performing well and what needs a little work before it goes live.


No more guessing, no more missed opportunities. Spot the gaps, fix them fast, and improve your sales.


**Platform:** Web


**For:** All users with connected Shopify stores


#### *AI Shot List: Professional product shots from a single photo ​*


Getting multiple visuals per product doesn't have to mean multiple shoots. AI Shot List generates a curated set of professional product shots — studio, lifestyle, ghost mannequin, flat-lay, virtual try-on, and more — from a single product photo.


The AI detects your product category and suggests the right shot types automatically. Shopify sellers can browse their recommended shots and add them directly to their listing. No manual photoshoot, no guessing what to shoot, just a complete set of images for every product in your catalog.


**Platform:** web


**For:** all users


**[Learn more about Photoroom for Shopify](https://www.photoroom.com/product-catalog/shopify)**


### More control for AI Shadows


Users have full control over the position of the shadow. You can now specify if an object is laying flat or standing upright for the shadow to generate at the correct angle, allowing products to look more real and accurate.


**Platform:** web app


**For:** all users


**[Learn more about AI Shadows](https://www.photoroom.com/tools/instant-shadows)**


## API updates


### AI shadows with full control


Generic AI shadows break catalog consistency. Teams processing thousands of product images need the same shadow direction, softness, and intensity across every SKU, not a different interpretation per image.


AI shadows now supports full control over how the shadow looks. Set` shadow.mode` to` ai.auto-with-overrides` and use five override parameters to fix the appearance:` shadow.softnessOverride` for hard or soft edges,` shadow.intensityOverride` for how dark the shadow is,` shadow.spreadOverride` for shadow length (` short` ,` medium` ,` long` ),` shadow.directionOverride` for direction relative to the subject (eight presets or degrees), and` shadow.subjectPoseOverride` for flat lay or upright subjects. Leave any override unset to let the model decide. Setting all five produces a fully deterministic shadow, which is what catalog teams need for consistent output at scale.


This is a preview feature. Enable it by adding the HTTP header` pr-ai-shadows-model-version: 2026-04-15` to your call.


View examples and try it in the API[here](https://docs.photoroom.com/image-editing-api-plus-plan/ai-shadows) .


**Platform:** now available via API. Also available on web, iOS, and Android.


### Video generator


Static product images don't convert the way motion does. Teams that want product videos are stuck choosing between manual video editing, production agencies, or stitching together multiple tools per SKU.


Video generator is now available via API through the` /v1/animate` endpoint. Upload an input image, specify animation parameters such as aspect ratio and mode, and the API returns a short animated product video. The endpoint supports free-form text prompts, so teams can cover custom scene or motion directions beyond pre-defined modes, and connect the API to their own LLM to dynamically generate prompts from image content and metadata.


Built for food delivery catalogs, marketplace listings, and e‑commerce teams scaling to thousands of product videos. Available on the Enterprise plan.


View the full parameter reference and request access[here](https://docs.photoroom.com/video-api-enterprise-plan/overview) .


**Platform:** now available via API Enterprise plan only Also available on web and Android.


### Multiple images to one video


Single-image-to-video covers most product videos but not all. Categories like cars, furniture, and large consumer goods need 360-degree product spins, which require multiple input images stitched into a single coherent video. Marketplace and classifieds catalogs scaling thousands of listings can't do this with single-image inputs.


The Video API now accepts **up to 7 reference images** alongside the main input on the` /v1/animate` endpoint. Pass them via` referenceImages.<name>.imageFile` (e.g.` referenceImages.back.imageFile` ,` referenceImages.detail.imageFile` ) to drive a single coherent video from multiple angles. When references are used, generation is prompt-driven — pair them with a free-form` prompt` to direct the scene, motion, or hero shot.


Built for marketplaces, classifieds, and e‑commerce teams generating 360-degree product spins and cinematic hero shots at scale. Async by design — submit the job, poll` statusUrl` , download from` resultUrl` when complete.


**Platform:** API Enterprise plan only*


### Edit with AI: multi-image input


A single input image limits what Edit with AI can produce. Teams often need to place a product from one shot into the scene of another, or transfer a specific object, style, or background across images, and one-image inputs don't cover it.


Edit with AI now accepts up to 4 additional reference images alongside the main input, for a total of 5 images per call. Set` editWithAI.additionalImages` to pass the extra references. The model can combine elements across the inputs in a single output, for example placing a product into a reference scene, transferring a style from one image to another, or pulling an object out of one image and into another.


View the full parameter reference and try it in the API[here](https://docs.photoroom.com/image-editing-api-plus-plan/alpha-edit-with-ai) .


**Platform:** now available via API.


## **Coming soon**


### **AI Tools in Batch on mobile**


You will soon be able to use Batch on mobile to edit with AI tools such as Ghost Mannequin, Virtual Model, Recolor, Flat Lay, Beautify, Product Staging, and Edit with AI. Apply AI to a single image or up to 250 at once, so you can save time, maintain the same polished look across hundreds of images, and manage all your product listing edits in one centralized place—no more switching between tools.


**Platform:** iOS and Android. Already live on the web app


**For:** Max and Ultra users


## **4k quality for more AI tools**


Since we’ve introduced higher quality for Virtual Model last month, we’ve rolled out even more quality upgrade across our AI tools: Ghost Mannequin, Edit with AI, Product Staging, Product Beautifier, and Flat Lay now all have export resolution up to 4k. With tiered resolution options—1k (Standard) for Pro users, 2k (Advanced) for Max users, and 4k (Premium) for Ultra users—you now have full control over higher-resolution outputs that ensure sharper accuracy, enhanced realism, and visuals that meet marketplace and print standards. The result: higher-quality images that build trust, elevate your brand, and drive more sales.


**Platform:** web, iOS and Android


**For:** Max and Ultra users


## Creating sections within batch


You can now create sections within Batch to group and manage thousands of images more efficiently, making it easier to streamline team workflows and quickly find the right assets for sale. Save time and keep your catalog structured exactly the way you need.


**Platform:** web


**For:** Pro, Max and Ultra users


## Auto generate your Shopify listing metadata on mobile


Generate file names, SEO-optimized titles and descriptions, and image alt text for your entire Shopify catalog in one click. Already live on web.


**Platform:** Web, iOS


**For:** All users
