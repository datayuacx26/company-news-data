---
schema_version: "1.0.0"
document_id: "c7907be59248bc95ffcfefda9f16c4e33b22990037596202248e87045195e894"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/nobody-returns-a-photo"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:a508c64253549e4f460e8e64d643d653b955bb2f5c2a4dd201f32561bdaae10a"
---

# Nobody returns a photo. They return the product.

#### AI made beautiful product images free. It did not make them true. We are picking a fight for product fidelity, and this is what we are doing about it.


The listing showed a sage green ribbed knit. What arrived was mint, and smooth.


[Product fidelity](https://www.photoroom.com/glossary/fidelity) is when AI matches your product exactly to show the right detail, realism, and accuracy. Lack of product fidelity results in a return, and worse, it is a buyer who will hesitate next time they see your name. US retailers will absorb $850 billion in returns this year, and the most common reason given is that the item did not match the description.


So here is the fight we are picking, and we will be spending the next year on it.


AI made beautiful product images accessible to all. It did not make them true. Beautiful is now table stakes, available to anyone, and it stopped being an advantage the moment it became free. What has not arrived is accuracy. And in e‑commerce, an image that is not accurate is not a design flaw. It is a return, a dispute, a delisting, and increasingly a regulatory problem.


The enemy is not Google, OpenAI, or any competitor. We sometimes use these models too and they are extraordinary. The enemy is "good enough": an industry that got comfortable publishing images nobody checked and that create zero emotion.


Photoroom processes 7 billion seller images a year, so we see this problem from the inside every day. And we know how bad it is because we measured it.


Research ran by Photoroom Machine Learning team checking product fidelity on Virtual Try-on across models.


## We measured the whole field, including ourselves


Our research team built the[Photoroom Product Fidelity Benchmark: 850 real products through the four leading AI image-editing models](https://www.photoroom.com/blog/top-editing-image-models-maintain-product-details-only-28-of-the-time) , 3,400 generations, ten trained annotators. One flagged issue and the image failed, because buyers do not grade your images. They either trust them or send the box back.


Across those 3,400 images, the models got the product wrong about three quarters of the time.


And the best of them still only passed 29%. Not the worst model. The best. Even the strongest passes fewer than one in three checks.


The most common failure is the logo, distorted or turned to gibberish in 20% of all generations. Your brand name, invented. Which is the clearest illustration of what is actually going on: these models are not reproducing your product, they are producing something that resembles it.


And the three frontier models finished close enough together that, as our research team put it, the narrow gap between them suggests that choosing a slightly better base model alone will not solve the problem. This is not one bad model. It is the state of the art.


So we built something else. The Photoroom Fidelity Layer is a reasoning-based correction system that sits on top of an image-editing model: it compares the generated image against the original product, reasons through the fidelity issues, localises the problem areas, and uses that analysis to guide a corrected generation. Applied to Nano Banana 2, it raises the pass rate from 29.0% to 38.2%, a relative improvement of roughly one third.


It is also nowhere near good enough, and we would rather say so than let you find out yourself. Thirty-eight percent is a minority. We are measurably the best at this and we are not close to finished.


At catalogue scale, that minority becomes a liability. Publish 200,000 listings a month at these rates and you are not managing image quality, you are generating disputes, chargebacks and delistings, each with someone's name against it. Which is the real reason marketplaces have not adopted AI at scale, and it is not the one people expect: the blocker is not capability, it is that nobody will take accountability for the output.


## Why this happens, and why a better model will not fix it


It is not that the models are careless. It is how they are built.


Almost every AI image model compresses your photograph into a smaller representation, does its work on that compressed version, then rebuilds a full image from it. That final step reconstructs the picture rather than restoring it. It fills in detail from what it has learned to expect, which as our research team puts it means the model ends up *"generating what it thinks should be there rather than restoring what was actually there."*


For a background tree, nobody notices. For a logo, a label, a stitched seam or a packaging colour, that is the difference between a listing and a return.


So we went after the cause. Our machine learning team is working on[PRX Pixel](https://www.photoroom.com/blog/prx-pixel-photoroom-s-open-source-pixel-space-image-model) , an image model we trained from scratch that works on raw pixels with no compression step at all. No rebuild, so no reconstruction error. It is research rather than a product feature today, it is text-to-image rather than editing, and we have open-sourced the whole thing under Apache 2.0: weights, training recipe, code. Anyone can run it, and anyone can tell us we are wrong and help us improve the models for e‑commerce.


We are doing that because pixel-level editing is where we think fidelity might eventually win, and because a company arguing for accuracy should be willing to show its working.


## What we actually mean by fidelity


The industry has not pinned this down, so we will. A product image has to get three things right at once, and almost every AI image gets at least one wrong.


> Product fidelity. The generated image shows *your* product. The right colour, the right pattern, the right buttons, your actual logo. This is the one we have benchmarked so far. This is the part we’re mostly talking about here.


> Visual realism. It does not look like AI made it. Photographic integrity: light that obeys physics, no melted hands, no uncanny gloss that tells a buyer this was generated. This part is being heavily worked on across models to avoid the 6th finger. However sellers still tell us all “AI looks too much like AI” so we’re not there yet either.


> Brand fidelity. It looks like *you* , and it keeps looking like you across the whole catalogue. Your aesthetic, your styling, your world, not the generic average of everyone else's. Sellers say all images look the same now, that’s brand issues we need to fix.


Miss the first and you get a return. Miss the second and the buyer does not believe you. Miss the third and you look like every other store, which is its own kind of loss.


You need all three, together, before AI is genuinely useful to a seller rather than merely impressive. That combination is what we are building towards, and it is why we treat this as a fight rather than a feature.


Generation of four virtual models wearing a light blue flowery pair of jeans show only 1 in four visuals get a similar product. In some the AI turned a pair of jeans into a skirt, sometimes darker tones, sometimes different patterns.


## The fight shows up in everything we build


This is not a campaign with a product attached. It is the way we shape the future of AI for e‑commerce.


We build our own models. Most of this industry takes someone else's foundation model and adds a product layer. We train ours and publish them openly, because when an output is wrong we want the whole stack to debug rather than a support ticket. As[Matthieu our CEO](https://www.photoroom.com/inside-photoroom/brand-refresh) puts it: *"The question isn't how to make beautiful images, it's how to make images that convert without breaking product fidelity."*


We put the fix in the app. Automated Fixer checks a generated image against your reference product, finds what is off, and corrects it in about half a second. No brushing, no re-prompting. Sellers want the problem gone, not a workflow. Check it by trying your product on our[AI Fashion model](https://www.photoroom.com/tools/virtual-model) to see the magic happen. You can also fix AI generations you’ve made on other tools,[read more](https://www.photoroom.com/tools/product-fixer) .


We put the check in the pipeline. For teams running catalogues through our API, our[Visual Agents](https://www.photoroom.com/enterprise/visual-qa) are structured workflows that ensure product fidelity. The agents analyse whether a product even suits the treatment, gates what passes, and rates every output against the original. They run in loop doing new generations when needed so only what passes goes live.[Talk to our sales team](https://www.photoroom.com/contact-sales) to book a demo on your catalog.


And for enterprise customers who want it, we will put fidelity in the contract. We call this[the Enterprise Guarantee](https://www.photoroom.com/enterprise/guarantee) . This shows how much we believe we can win this fight. Talk to our sales team to get a guarantee on fidelity.


This is just the beginning and across the next months we’ll show you how we pick up this fight and win it.


## Show us yours, join the fight


We want to fully understand the problem to create the best solution. So join us by sharing a generated product image from your own catalog that came back wrong. Post the AI version and the original on[our public gallery](https://ai-infidelity.lovable.app/) . We will run it through our detectors and tell you publicly what went wrong and how we would fix it. No account needed.


We are building a wall of them, because a pass rate is an abstraction until it is your product and we want you to see and understand the problem so you can also see how it’s fixed.


We are only getting started, until we have AI unlock all its potential for sellers we won’t stop because we believe AI can help sellers sell more.
