---
schema_version: "1.0.0"
document_id: "6069f0a2e56a1da3f357f075516f94e315ef7430cb94b319a65044f153491854"
company_key: "yc-ai-tech-packs"
company: "AI Tech Packs"
source_id: "yc-ai-tech-packs-news-import-be47509359c4"
canonical_url: "https://aitechpacks.com/blog/how-to-write-prompts-for-ai-tech-packs"
published_at: "2026-04-02T20:48:51.867+00:00"
first_seen_at: "2026-07-21T05:35:53.930845+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:7d6ac75e1352b2016966d8707ebd22e73d649d9e2a293af3ed8f3c4b58867c4d"
---

# How to Write AI Tech Pack Prompts (Complete Guide)

*Can AI make tech packs? Yes — and how you write your prompt is the single biggest factor in getting a factory-ready result. This guide walks you through everything you need to know about prompting inside AI Tech Packs, from your very first tech pack to importing specs from existing styles.*


📺 **This post is based on our video tutorial.**


📊 Prefer slides?[View the presentation →](https://docs.google.com/presentation/d/1bZmBnIMohXs6J6oiLEDCjpwbL1Wygt29ZatY2hfcOdU/edit)


## What Are Prompts in AI Tech Packs?


When you use AI Tech Packs to generate a tech pack, your prompt is the set of instructions you give the system. It tells the AI what garment you're making, what materials to use, and what measurements to apply.


Your prompt drives two primary sections of your tech pack:


1. **BOM (Bill of Materials)** — the fabrics, trims, threads, hardware, and other materials that make up your garment.
2. **POM (Points of Measurement)** — the measurement specifications and size chart for your garment.


Everything else — flat sketches, construction details, callouts — gets built around these two foundations. The better your prompt, the less editing you'll need to do after generation.


### A Quick Note on Sizing


AI Tech Packs supports alpha sizing: S, M, L, XL, and so on. If your specs use numeric sizing (0, 1, 2), you'll need to convert those to alpha sizes before entering your prompt.


## The Two Types of Prompts


There are two ways to prompt inside AI Tech Packs. We call them **generic prompts** and **strict prompts** . Which one you should use depends on your experience level and whether you already have specs on file.


### Generic Prompts: Natural Language Instructions


A generic prompt is written the way you'd describe a garment to another person. You don't need to know fabric weights, stitch types, or measurement terminology. Just tell the AI what you want.


**Example generic prompt:**


> Make this hoodie a cropped hoodie for men, slim fit. Use some sort of cotton that is soft to the touch.


That's it. The AI reads your description, looks at the image you uploaded, and figures out the appropriate materials, measurements, and construction details.


Generic prompts are ideal if you're new to tech packs or new to garment development entirely. You don't need prior experience — just a clear idea of what you want.


### Strict Prompts: Defined Specs and Size Charts


A strict prompt is a structured list of specific materials and measurements. It's what experienced brands and design teams use when they already know exactly what they want.


A strict prompt typically includes:


- A clearly defined BOM table (fabric type, weight, composition, color, supplier, etc.)
- A clearly defined size chart or POM table (measurement names, values per size, tolerances)
- Optionally, grade rules instead of a full size chart


Strict prompts don't require every field to be filled in. You can provide partial information — for example, measurement names without all the values — and the AI will fill in what's missing. But you do need to provide at least a partial table structure.


### When to Use Each


**Use generic prompts if:**


- You're new to AI Tech Packs or tech packs in general
- You've never developed a garment before
- You want a fast starting point that you can refine


**Use strict prompts if:**


- You're an established brand with existing styles and specs
- You have tech packs in Excel sheets you want to import
- You want the system to use your exact specs with no guessing


## How Generic Prompts Work Behind the Scenes


Here's something most users don't realize: every generic prompt gets converted into a strict prompt before generation.


When you write a natural language instruction and click **Enhance** , the AI transforms your description into a structured BOM and POM. This is how we guarantee consistency — no matter how casual your input is, the system always works from a standardized spec format internally.


After the enhancement step, you'll see the strict prompt structure on screen. The system also shows an explanation of why it chose those specific materials and measurements, which is especially helpful if you're still learning the vocabulary.


### Interacting With the AI After Enhancement


Once your generic prompt has been enhanced, you can ask the AI questions or request changes conversationally:


- **"Why did you choose brushed fleece?"** — The AI will explain its reasoning based on your description and image.
- **"I don't want to use cotton."** — The AI will swap out cotton for an appropriate alternative.
- **"What is rayon?"** — It'll define unfamiliar terms for you.


You can also hover over measurement names to see their definitions. After you're satisfied with the enhanced prompt, click **Accept and Use** , then **Generate** .


## How to Write a Strict Prompt


Strict prompts were built for teams that already have tech packs — usually stored in Excel or similar tools.


The workflow is straightforward:


1. Open your existing tech pack or spec sheet.
2. Copy the BOM and size chart data.
3. Paste it into the prompt text box in AI Tech Packs.


The pasted data will likely look messy and hard to read. That's expected. Click **Format** , and the AI will clean it up into the structured strict prompt format. This lets you verify that everything copied over correctly before generating.


This feature is most useful for brands with a library of existing styles. You already know those specs work for your fit and your factories — now you can bring them into the system and reuse them with confidence.


## How Locking Works


Below the prompt area, you'll find two toggle buttons: **Lock POM Measurements** and **Lock Bill of Materials** .


Locking prevents the AI from adding new items to a section. Here's exactly what that means:


- **Locked:** The AI will only use the items you provided. If a measurement or material is missing a value, the AI will fill it in — but it won't invent new measurement names or add new materials.
- **Unlocked:** The AI may add measurements or materials it thinks are relevant, based on the garment image and type.


### When to Lock Your POMs


Lock your measurements when you have a complete or nearly complete size chart — say 20 to 30 measurements. In that case, the AI might add unnecessary measurements if left unlocked. Locking keeps your chart clean.


### When to Keep Your BOM Unlocked


We generally recommend keeping your BOM unlocked. This is where the AI adds the most value: it can identify materials and construction details from your garment image that you may have overlooked. It might add a trim, lining, or hardware item that wasn't in your original spec.


That said, experiment with both settings and see what works best for your workflow.


### A Real Example


In our video walkthrough, we generated a tech pack with a strict prompt containing 6 materials and 25 measurements. We locked the POMs but left the BOM unlocked.


The result: exactly 25 measurements (the AI respected the lock) and 8 materials (the AI added 2 that it identified from the garment image).


## Using Grade Rules Instead of a Full Size Chart


If you don't want to write out a complete size chart with values for every size, you can use grade rules instead.


Grade rules are a form of strict prompt. Instead of providing measurements for every size column, you:


1. Provide one column of measurements for your sample size (e.g., M).
2. Specify the grade increment (e.g., +2.5 cm between sizes).
3. Optionally include a tolerance value.


The AI rebuilds the full size chart in the background using your base size and grade rules. This saves time if you work from graded specs rather than full charts.


## Style Blocks: Saving and Reusing Prompts


Once you've built a strict prompt you're happy with — whether you wrote it from scratch, enhanced it from a generic prompt, or imported it from an existing tech pack — you can save it as a **style block** .


Style blocks let you reuse proven specs across future tech packs. For example, if you have a hoodie spec that your factory has already approved, save it as "Hoodie Style A." Next time you need a similar hoodie, load that style block, make minor adjustments for the new product, and generate.


This is especially powerful for brands with 50+ styles. Instead of re-entering specs every time, you build a library of reusable patterns.


## How the AI Handles Your Information


There are two rules that govern how AI Tech Packs processes your prompt:


### 1. Order Is Preserved


The order you submit your measurements and materials is the order they'll appear in the generated tech pack. If your BOM lists main fabric first, then trim, then labels — that's how the output will be structured.


This matters because many factories and suppliers expect a specific ordering. The system respects whatever sequence you provide.


### 2. Your Information Comes First


This is the single most important thing to understand about our prompt system:


**Any missing information is filled in by the AI. But if you provide it, we use your information first.**


If you specify a fabric weight, we use that weight. If you leave it blank, the AI fills it in based on the garment type and image. The AI is a supplement, not an override. You're always in control of what goes into your tech pack.


## Getting Started


Whether you're creating your first tech pack ever or importing hundreds of existing styles, the prompt system is designed to meet you where you are.


- **Brand new to tech packs?** Start with a generic prompt. Describe what you want, upload an image, click Enhance, and let the AI guide you through the rest.
- **Established brand with existing specs?** Paste your BOM and size chart into a strict prompt, click Format, lock your POMs, and generate.
- **Somewhere in between?** Use a generic prompt to get started, then refine the enhanced strict prompt with manual edits or AI chat.


📺


## Need Help?


If you're having trouble loading existing specs into style blocks or have questions about prompting, our team is here to help.


- **Email:** info@aitechpacks.com
- **WhatsApp / Call / Text:** +1(408) 763-1969


## **Ready to Build Your First AI Tech Pack?**


Now that you know how to write prompts, try it yourself. Head to our[AI Tech Pack Generator](https://aitechpacks.com/techpack) to upload a garment image, write your prompt, and generate a factory-ready tech pack in minutes — no design experience required.


[Create Your Tech Pack →](https://aitechpacks.com/techpack)
