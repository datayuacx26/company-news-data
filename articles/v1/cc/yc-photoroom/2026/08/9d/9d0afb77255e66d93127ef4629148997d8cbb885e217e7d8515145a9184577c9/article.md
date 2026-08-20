---
schema_version: "1.0.0"
document_id: "9d0afb77255e66d93127ef4629148997d8cbb885e217e7d8515145a9184577c9"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/new-in-product-july-2026"
published_at: null
first_seen_at: "2026-08-03T18:16:18.983363+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:854002835d6e32061807ca4c6ff5b322a929f3d03dcab2047b2aa0a965a0cb12"
---

# What's new in product: July 2026

Product fidelity is whether an AI-generated image actually matches your real product, not just whether it looks convincing. In July our research team measured it properly: across 3,400 AI generations of 850 real products, the four leading AI image editing models got the product wrong about three quarters of the time. Logos redrawn. Buttons missing. A colour that shifted just enough that your buyer notices when the box opens.


Nobody returns a photo. They return the product. That's why[Photoroom is declaring a fight on product fidelity](https://www.photoroom.com/inside-photoroom/nobody-returns-a-photo) , and why we are committed to being the best at product fidelity for e‑commerce brands.


If you've had an AI image get your product wrong,[show us](https://ai-infidelity.lovable.app/) , we'll tell you publicly what went wrong and how we'd fix it.


In July, we stopped asking you to catch those mistakes yourself. Photoroom is the only tool that runs an automatic product fidelity check on every AI image generation with the[automated Product Fixer](https://www.photoroom.com/tools/product-fixer) to ensure images accurately match the product. Read on to discover other updates we launched this month.


Table of content:


1.


[Recently launched](https://www.photoroom.com/inside-photoroom/new-in-product-july-2026#1)


2.


[API updates](https://www.photoroom.com/inside-photoroom/new-in-product-july-2026#2)


3.


[Questions sellers ask](https://www.photoroom.com/inside-photoroom/new-in-product-july-2026#3)


## **Recently launched**


### AI product images that match your product, accurately and automatically on Photoroom


Photoroom's[Product Fixer](https://www.photoroom.com/tools/product-fixer) already let you repair what AI got wrong. The catch was that you had to spot it first. Across a catalog of hundreds of listings, the small errors are the ones that slip through, because they look like a photo and nothing is obviously broken.


Now a fidelity check runs automatically on every image transformation in Photoroom. Our rater model scores how closely the generated image matches your original product photo. If the score falls below the threshold, a localizer model works out exactly where the mismatch sits and asks whether you want it fixed. Say yes and the fixer repairs those areas on its own. No selecting, no prompting.


We also flag it when you upload a poor quality image, as our model is trained to rate reference image quality to ensure product fidelity and quality such as lighting and cropped objects. You have the option to upload a better image so your output images are better quality.


The result: product photos you can publish without inspecting every frame, and fewer listings that quietly disagree with what ships in the box.


**Platform:** web, iOS, and Android


**For:** everyone.


**[Learn how AI images match your product accurately](https://www.photoroom.com/tools/product-fixer)**


### Your catalogue arrives already organised in batch


###


Drag a folder onto an empty Batch, or use Upload folder, and Photoroom rebuilds your structure for you: one section per subfolder, named after the folder, with loose images dropped into a default section.


If your product shots are already sorted on disk by product, scene, SKU or campaign, that order carries straight into Photoroom. No re-sorting hundreds of images by hand.


**Platform:** web


**For:** everyone


### Manage your Shopify listings from Android


Good news! You can now manage your Shopify listings from Android, alongside iOS and web.


With **Photoroom for Shopify** , you can create, edit and publish product listings from one place. Improve product photos, update multiple listings at once, generate titles, descriptions and image metadata, then publish your changes back to Shopify with full control.


**What you can do:**


-


Connect your Shopify store and manage your listings from Photoroom


-


Create studio-quality product photos with AI backgrounds, virtual models and shadows


-


Update product visuals across multiple listings with Batch Edit


-


Generate titles, descriptions and image metadata automatically


-


Publish your changes back to Shopify whenever you're ready


Whether you're photographing new products, refreshing your catalog or making quick updates on the move, you can now manage your Shopify listings from anywhere.


**Platform:** Android, iOS and web


**For:** everyone


[Connect your store](https://app.photoroom.com/u/product-catalog)


### Pull in files straight from Google Drive


Your product shots do not all live on your desktop. Plenty of them sit in Google Drive, and getting them into Photoroom meant downloading first, then uploading again.


Insert View now has a Google Drive tab. Connect your account once and your Drive files become an input you can pull from directly, right next to your uploads, designs, AI images and products. No round trip through your downloads folder.


It is the same picker everywhere Insert View appears, including Batch, so wherever you add images, Drive is one of the options.


**Platform:** web


**For:** everyone


### AI Scenes: lifestyle product photos without the photoshoot


A bottle sitting in grass. A shoe on sand. A product held in hand. Shots like these used to mean a location, a photographer and a day you did not have.


AI Scenes is a new AI tool that places your product in an immersive real-world setting and relights it to match, so it looks genuinely there rather than floating on a backdrop. Open AI Tools, pick AI Scenes, describe the setting you want, and keep the results you like.


AI Scenes sits alongside AI Backgrounds rather than replacing it. Use AI Backgrounds when your product must stay untouched. Use AI Scenes when you want the shot to feel physically real.


Lifestyle imagery converts better than plain packshots. Now you can make it without leaving the app.


**Platform:** web, iOS, and Android


**For:** everyone


### Share links worth sending to your team


Sharing an AI image or video used to mean a raw file and a paragraph of explanation.


Share links now open a branded preview page: your image or video plays blurred in the background with the media itself in front. Paste a link into Slack, WhatsApp, iMessage or socials and it unfurls with a real thumbnail and title instead of a bare URL. Whoever you send it to can view it without logging in, and one click brings them into the app to try it on their own products.


Open any AI tool result, then the **...** menu, then **Share Image** or **Share Video** . The link lands on your clipboard.


**Platform:** web


**For:** everyone


# API Updates


### Visual QA: check every product image before it publishes


It took ten trained annotators, three passes each, to check 850 products for our benchmark. Nobody is doing that at 50,000 SKUs. So one bad frame goes live, and for a marketplace that means a misrepresentation risk, a dispute, sometimes a delisting.


Visual QA sits in the Photoroom API and does the checking for you. It analyses whether a product even fits the workflow you are about to run, gates what passes, generates, then rates the output against the original for fidelity. Only what clears the bar reaches your catalog.


Visual QA is the first workflow from Visual Agents, our orchestration layer for image pipelines. The full loop, including automatic regeneration until an image passes, follows later this year.


**Platform:** now available via API


**For:** Enterprise.


##


## Questions sellers ask


### Why do AI-generated product images get my product wrong?


Because the models were not built for product accuracy. In the Photoroom Product Fidelity Benchmark, published July 2026, ten trained annotators reviewed 3,400 generations of 850 real products across the four leading AI image editing models. Only 25.3% of generations passed with no fidelity issue flagged. The most common failure was logo and text distortion, which affected 20.1% of all generations, followed by missing elements at 12.5% and pattern changes at 11.4%.


### Can AI product images be made accurate?


Partly, and not yet fully. The strongest base model in our benchmark passed 29.0% of product fidelity checks. With the Photoroom Fidelity Layer applied on top, that rose to 38.2%, the highest score on the Product Fidelity Leaderboard, July 2026. That is a meaningful improvement and still a minority of images, which is why the fix runs on every generation rather than being something you have to remember to use.


### How do I fix an AI product image that does not match my real product?


In Photoroom, you no longer have to. The automatic fidelity check compares each generation against your original product photo and offers to repair any mismatch it finds. If you would rather work by hand, Photoroom Product Fixer lets you brush over an area and describe the correction yourself.


### What is the difference between AI Scenes and AI Backgrounds?


AI Backgrounds replaces the background and keeps your product unchanged. AI Scenes places your product into a real-world setting and relights it to match, allowing small adjustments so the shot looks physically real. Use AI Backgrounds for catalog packshots and AI Scenes for lifestyle imagery.


### Which Photoroom editing tools do not use AI credits?


Preset AI Shadows, Fill, Erase, Resize and Upscale are free utilities and do not consume AI credits, within a fair daily limit. AI credits are reserved for advanced AI generation.


### Can I manage my product listings from my phone?


Yes. Photoroom product catalog runs on web, iOS and Android. You can create products from your own photos, generate titles and descriptions, edit images in Batch, and publish to Shopify, all from your phone.


[Try Photoroom on your own product photos](https://app.photoroom.com/)


###
