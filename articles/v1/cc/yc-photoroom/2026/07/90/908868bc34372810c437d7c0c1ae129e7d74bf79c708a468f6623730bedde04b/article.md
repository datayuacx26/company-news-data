---
schema_version: "1.0.0"
document_id: "908868bc34372810c437d7c0c1ae129e7d74bf79c708a468f6623730bedde04b"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-16e4c55287d6"
canonical_url: "https://www.photoroom.com/customer-stories/selency"
published_at: null
first_seen_at: "2026-07-23T22:00:21.424777+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:d1479f0f66b4a9e3c0ba8183a6a83ab7aa0ce1995429e2870aaf9f56f0fd2f05"
---

# How Selency cut image processing costs by 89% with Photoroom's API

*Selency, one of Europe's largest online marketplaces for secondhand furniture and home decor, integrated Photoroom's Background Remover API to automate product image editing across its catalog of more than 700,000 items. The Photoroom API replaced an external vendor that took 24 hours to process each image batch, cutting that time to seconds and reducing image editing costs by 89%.*


### **Life before Photoroom**


-


External outsourcing took 24+ hours per image batch


-


Image editing costs too high for growth plans


-


Manual process couldn't scale with the catalog


### **Life with Photoroom**


-


Listings processed in seconds instead of 24 hours


-


Image editing costs reduced by 89%


-


80% of images processed fully automatically


-


One API handles recentering, resizing, and background editing


> "The Photoroom API is so efficient that we use it for all our marketplace's images."
>
>
> — Vincent Paulin, Chief Technology Officer, Selency


[Selency](https://www.selency.co.uk/) started as an online destination for people who love secondhand furniture with character. Today, it is one of Europe's largest[marketplaces for home and furniture](https://www.photoroom.com/industry/furniture) , connecting professional antique dealers and individual sellers with buyers looking for unique vintage and pre-owned pieces.


For a marketplace that relies on discovery for sales, how quickly a new piece reaches buyers matters. Every item on Selency is one of a kind. A mid-century sideboard or a vintage brass lamp won't wait around for a slow listing process. If it takes too long for items to go live, a buyer looking for such pieces never sees them and a seller may list them elsewhere.


That listing speed depends on the editing speed and quality of product images. Clean, consistent photos help buyers browse without fatigue and trust what they're looking at. And at Selency's scale, producing high-quality, consistent images for every new listing was becoming the bottleneck.


## **The challenge: a lengthy image process for a large marketplace**


Before Photoroom, Selency outsourced all image editing to an external vendor. The vendor used a combination of automated and manual processes, transmitting images through a legacy File Transfer Protocol (FTP) system. Each time sellers added new listings, the entire editing cycle took at least 24 hours to complete.


24 hours is a long editing timeframe for a[marketplace](https://www.photoroom.com/industry/marketplaces) with more than 700,000 items, where new pieces arrive daily from thousands of professional and non-professional sellers across Europe.


This editing model also increased image processing costs. As Selency's catalog grew, so did the editing bill, and the process offered no clear path to getting faster or cheaper at scale.


The team needed a way to process listing images quickly enough to match the pace of new inventory, without sacrificing the photo quality that Selency's buyers expect from a curated marketplace.


## **How Selency replaced a 24-hour process with one API call**


After researching multiple photo editing vendors and testing image samples with their top choices, Selency's team found that the[Photoroom Background Remover API](https://www.photoroom.com/api/remove-background) produced the best results and fit their workflow.


The team integrated Photoroom's API by making a few updates to their back office, and their image processing workflow now requires two simple steps:


1.


**Listing:** When a seller lists a new product, a single call to the Photoroom API automatically[resizes the image](https://www.photoroom.com/tools/image-resizer) , re-centers it, removes the background, and adds a clean[white image background](https://www.photoroom.com/tools/black-white-background) .


2.


**Validation:** The Selency team then validates each listing by reviewing and comparing the original photo with the edited version before deciding whether to publish.


When customized image templates presented a challenge during setup, Photoroom's team worked directly with Selency to design a solution that worked for their catalog.


The result is a workflow that replaced a 24-hour FTP-based process with near-instant image editing, without requiring Selency to build or maintain any image processing infrastructure.


> The ability to perform all these operations automatically, with one API, and to get the result in near real time is really an excellent addition for us."
>
>
> — Vincent Paulin, Chief Technology Officer, Selency


*A seller photo processed with Photoroom's Background Remover API. Source: Selency*


## **Results: Selency's process went from hours to seconds, at a fraction of the cost**


Since switching from their external vendor to Photoroom, Vincent and his team have seen immediate improvements across their listing operations:


-


**Listing time dropped from 24+ hours to seconds.** New products reach buyers the same day instead of waiting in a processing queue overnight.


-


**Image editing costs reduced by 89%** compared to the previous FTP-based vendor, making the process sustainable as the catalog continues to grow.


-


**80% of images are processed automatically** by the API, with the remaining 20% requiring only basic manual review before publishing.


The integration covers all of Selency's marketplace images, handling[product listing photography](https://www.photoroom.com/use-cases/product-listing) across the full catalog of 700,000+ items.


> "The additional plus of the Photoroom API is its optimal price point for us."
>
>
> — Vincent Paulin, Chief Technology Officer, Selency


*Clean, consistent product listings on Selency's marketplace. Source: Selency*


Photoroom processes billions of images a year for e‑commerce and marketplace platforms, including[Mercari](https://www.photoroom.com/customer-stories/mercari) and[Depop](https://www.photoroom.com/customer-stories/depop) , where the API integration drove over 1 million seller listings. Selency's results, a 24-hour editing cycle reduced to seconds at 89% lower cost, fit a consistent benefit of automating product photography: marketplaces that embed automated image processing in the seller listing flow speed up time to market and spend less money doing it.
