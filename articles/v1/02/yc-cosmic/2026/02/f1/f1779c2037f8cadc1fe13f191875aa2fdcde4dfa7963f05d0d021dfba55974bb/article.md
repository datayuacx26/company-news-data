---
schema_version: "1.0.0"
document_id: "f1779c2037f8cadc1fe13f191875aa2fdcde4dfa7963f05d0d021dfba55974bb"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/nano-banana-2-is-now-available-in-cosmic-generate-ai-images-with-google-gemini-image-models"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:0173326b5e7146cf2501c0bd2aae1d2979b1ebd52c6a731eb5d0e5991a45dcfb"
---

# Nano Banana 2 Is Now Available in Cosmic: Generate AI Images with Google Gemini Image Models

AI-powered image generation just took a massive leap forward. Google's Nano Banana 2 models are now available directly through the Cosmic dashboard and API, bringing professional-grade image creation to your content workflows. We tested the new models with a demanding architectural prompt and the results speak for themselves.


## What Is Nano Banana?


Nano Banana is the name Google gives to the native image generation capabilities built into its Gemini models. Rather than treating image generation as a separate system, Nano Banana models understand context, follow complex instructions, and produce visuals that align closely with detailed text prompts.


Two Nano Banana models are now available in Cosmic:


- **Gemini 3.1 Flash Image** (` gemini-3.1-flash-image-preview` ): The recommended default for most use cases. Fast, efficient, and supports resolutions from 512px up to 4K with 14 aspect ratios and up to 14 reference images.
- **Gemini 3 Pro Image** (` gemini-3-pro-image-preview` ): A premium tier designed for professional assets. It uses advanced "thinking" to reason through complex prompts, delivers high-fidelity text rendering, and supports up to 4K output.


Both models are available through the[Cosmic AI API](https://www.cosmicjs.com/docs/api/ai#generate-image) and the Cosmic dashboard.


## We Put It to the Test


To see what Nano Banana 2 can really do, we ran the following prompt through both models:


> "Create a highly detailed 4K architectural rendering of a modern sustainable home with floor-to-ceiling windows, integrated into a hillside landscape."


### Gemini 3.1 Flash Image Result


The Flash Image model delivered a striking result in seconds. The rendering captures a multi-level modern home with dark timber cladding, expansive glass walls, a living green roof, and a wrapping deck with an outdoor fire feature. The hillside integration feels natural, with stone retaining walls and native plantings blending the structure into the mountainous terrain. The sunset lighting across the distant peaks adds cinematic depth to the composition.


### Gemini 3 Pro Image Result


The Pro Image model took a different creative direction with the same prompt. This rendering presents a warm, earth-toned home with rammed earth walls, solar panels, green roofs, and a natural swimming pool surrounded by drought-tolerant landscaping. The flagstone pathways, raised garden beds, and open interior visible through floor-to-ceiling glass create an inviting, lived-in quality. The composition feels more grounded and organic compared to the dramatic mountain setting of the Flash result.


Both outputs are impressive, but they showcase different strengths. Flash Image excels at speed and dramatic visual impact. Pro Image brings a more considered, design-forward approach that feels closer to what a human architect might envision.


## How to Generate Images with Cosmic


Getting started takes just a few lines of code. Here is how to generate an image using the Cosmic SDK:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'BUCKET_SLUG'  ,
readKey  :     'BUCKET_READ_KEY'  ,
writeKey  :     'BUCKET_WRITE_KEY'
}  )


// Using Gemini 3.1 Flash Image (default, recommended)
const   imageResponse   =     await   cosmic  .  ai  .  generateImage  (  {
prompt  :     'Create a highly detailed 4K architectural rendering of a modern sustainable home with floor-to-ceiling windows, integrated into a hillside landscape.'  ,
model  :     'gemini-3.1-flash-image-preview'  ,
size  :     '4096x4096'  ,
folder  :     'ai-generated'  ,
alt_text  :     'AI-generated sustainable hillside home rendering'
}  )


console  .  log  (  imageResponse  .  media  .  imgix_url  )
```


Or use the Pro Image model for premium quality:


```text
const   proResponse   =     await   cosmic  .  ai  .  generateImage  (  {
model  :     'gemini-3-pro-image-preview'  ,
size  :     '4096x4096'
}  )
```


Every generated image is automatically saved to your Cosmic Media Library with a CDN-hosted URL, ready to use in your content.


## Reference Images for Brand Consistency


One of the most powerful features of Nano Banana in Cosmic is reference image support. You can pass existing images as context, and the model will analyze their style, composition, and visual language to inform the generated output. This is ideal for:


- Maintaining a consistent visual style across marketing campaigns
- Creating variations of existing product photography
- Applying a specific aesthetic from one image to an entirely new scene


Flash Image supports up to 14 reference images, while Pro Image supports up to 5.


```text
const   styledResponse   =     await   cosmic  .  ai  .  generateImage  (  {
prompt  :     'A modern sustainable office building in the same architectural style'  ,
model  :     'gemini-3.1-flash-image-preview'  ,
reference_images  :     [
'https://imgix.cosmicjs.com/4559d440-1337-11f1-9e28-d5fea3b8af7e-ai-gemini-3-1-flash-image-preview.png?w=1200'
]  ,
size  :     '2048x2048'
}  )
```


## Available Sizes and Aspect Ratios


Both models support multiple output sizes, giving you flexibility for different use cases:


Model Supported Sizes Aspect Ratios


Gemini 3.1 Flash Image 512x512, 1024x1024, 2048x2048, 4096x4096 14 ratios including 1:1, 16:9, 9:16, 21:9


Gemini 3 Pro Image 1024x1024, 2048x2048, 4096x4096 10 ratios including 1:1, 16:9, 9:16, 21:9


The 512px option on Flash Image is perfect for generating quick thumbnails and icons, while 4K output on either model delivers publication-ready assets.


## When to Use Each Model


**Choose Gemini 3.1 Flash Image when you need:**


- Fast turnaround for content at scale
- Quick thumbnails and social media visuals
- Batch generation across large product catalogs
- Budget-conscious workflows where speed matters


**Choose Gemini 3 Pro Image when you need:**


- Professional marketing assets and hero images
- Complex compositions with precise text rendering
- Maximum detail and photorealistic quality
- Images that require advanced reasoning about the prompt


## Generate Images from the Dashboard Too


You do not need to write code to use Nano Banana in Cosmic. From the[Cosmic dashboard](https://www.cosmicjs.com/docs/dashboard/ai) , navigate to your Media Library, click "Create," and describe the image you want to generate. The dashboard supports both Flash Image and Pro Image models, and generated images are instantly available in your library.


## Beyond Images: Text, Video, and Audio


Nano Banana image generation is part of Cosmic's broader AI capabilities. The same API also supports:


- **Text generation** with models from Anthropic Claude, Google Gemini, and OpenAI
- **Video generation** with Google's Veo 3.1 models, including native audio and image-to-video mode
- **Audio generation** with OpenAI's text-to-speech models across 9 natural voices


Explore the full[AI API documentation](https://www.cosmicjs.com/docs/api/ai) to see all available models and capabilities, or check the[available models reference](https://www.cosmicjs.com/docs/api/ai#available-models) for the complete list.


## Get Started Today


Nano Banana 2 is available now on all Cosmic plans.[Log in to your Cosmic dashboard](https://app.cosmicjs.com/login) , generate your first image, and see what these models can do for your content pipeline. Whether you are building product catalogs, marketing pages, or automated content workflows, AI image generation is now a single API call away.


*Image from[Nano Banana 2 announcement](https://blog.google/innovation-and-ai/technology/ai/nano-banana-2/) .*
