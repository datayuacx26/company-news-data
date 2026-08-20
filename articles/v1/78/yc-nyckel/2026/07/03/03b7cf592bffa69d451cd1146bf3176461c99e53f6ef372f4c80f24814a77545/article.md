---
schema_version: "1.0.0"
document_id: "03b7cf592bffa69d451cd1146bf3176461c99e53f6ef372f4c80f24814a77545"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/retailhub-product-tagging/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c50b4018a5ffe3ac5330fdddf89e2c58fc29447b9b4de62fb689499366fe8793"
---

# Retailhub auto-tags hundreds of thousands of products using Nyckel

"We tested out some not-great options for classification, and then found Nyckel. They were a perfect fit for our product tagging use case."


Roger Haugland


CEO, Retailhub


Retailhub auto-tags hundreds of thousands of products using Nyckel’s classification solution. Retailhub uses Nyckel to categorize and tag products across thousands of labels Works regardless of product type (clothing, machinery, home goods, etc.) Retailhub built this model with no machine learning experts in-house


## About Retailhub


[Retailhub](https://www.retailhub.no/) is a webshop platform for physical stores, currently serving 150 stores across Norway. Unlike other online store solutions like Shopify, vendors don’t have to manually upload any new images or descriptions themselves.


Instead, Retailhub integrates with the store’s POS system and automatically imports product images and descriptions from thousands of supplier image banks and websites. This saves stores a lot of time compared to manual image uploads.


Further, RetailHub enriches the products with detailed category information and product attributes like color, pattern, material, length. These category/attribute filters enable a stronger SEO presence, significantly improved Google Ads performance, and better webshop usability.


## The Challenge


To automatically tag products with relevant metadata, Retailhub must categorize a vast array of products (over 5,000 categories and counting) - ranging from clothing to knitting supplies to machinery parts.


And since physical stores use a variety of POS systems, many of which have incomplete or missing product metadata (such as item type or color), Retailhub had to find their own ways to categorize products.


"We knew what we wanted - a simple API for machine learning. We didn’t want to spin up the models ourselves. That would be a side step for our business."


Roger Haugland


CEO, Retailhub


Retailhub knew that machine learning classification was the right approach, but they had difficulty finding the right solution. Google’s dominant color feature, for instance, had just a 50% accuracy. They also knew that building this model themselves from scratch would be time and cost prohibitive.


## The Solution


Retailhub turned to Nyckel’s classification tool to address their challenges. Specifically, they built image classification models that ingest product images and tag them according to predefined categories. One model, for example, categorizes items into 5000+ categories including 600 different types of female clothing.


"Nyckel has helped us delight our customers while saving us from having to build these models ourselves."


Roger Haugland


CEO, Retailhub


Another automatically tags product images with their dominant colors. One challenge with color-tagging in this context is that product images often include colorful backgrounds or people/models which can confuse the color tagger. To get around this, RetailHub created an “isPackShot” classifier that separates out photos that just have the items themselves from the rest. Using these “clean” pictures to tag colors gave much higher accuracy. This isPackShot classifier was created in a few minutes using less than 200 training images.


## The Results


Retailhub has now run hundreds of thousands of images through Nyckel. This automatic tagging has helped them innovate the webshop platform space and delight customers. Additionally, by outsourcing the maintenance and management of the models, they have saved themselves hundreds of thousands from not having to hire an in-house ML expert.


"Fully-automated product tagging is the direction we’re heading in, and Nyckel has been a key part of that initiative."


Roger Haugland


CEO, Retailhub


Interested in exploring how Nyckel can support your business?[Try Nyckel for free](https://www.nyckel.com/contact) , orreach out to us with any questions.
