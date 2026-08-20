---
schema_version: "1.0.0"
document_id: "dbd7750a600d8eaab11cdccf748c3c186e9e9d5f9c6fae598cd4c90f052d2a6a"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/new-ai-models-google-gemini-3-pro-and-gemini-3-pro-image-nano-banana-pro"
published_at: "2025-12-10T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:d147b0bc6710615940b08f982a09d7172cb3b085604fd727b66cfe5140b1859f"
---

# New AI Models: Google Gemini 3 Pro & Gemini 3 Pro Image (Nano Banana Pro)

We're excited to announce the addition of **Google Gemini 3 Pro** models to Cosmic AI. These cutting-edge models bring state-of-the-art reasoning and advanced image generation capabilities to your projects.


## What's New?


### 🍌 Gemini 3 Pro


Google's most intelligent model with exceptional reasoning capabilities, specifically built for agentic workflows and complex tasks.


**Key Features:**


- **1M token context window** - Process massive amounts of information
- **64K max output tokens** - Generate extensive content in a single response
- **Advanced thinking levels** - Choose between 'low' for speed or 'high' for complex reasoning
- **Multimodal capabilities** - Text, vision, tools, and thinking support


### 🍌 Gemini 3 Pro Image (Nano Banana Pro)


Native image generation optimized for speed, flexibility, and contextual understanding. The first model on Cosmic to support **reference images** for style-consistent generation.


**Key Features:**


- **Up to 4K image generation** - Create stunning high-resolution images (1024×1024, 2048×2048, 4096×4096)
- **Reference images support** - Provide existing images to guide style, composition, and aesthetic
- **Contextual understanding** - AI analyzes reference images to inform new generations
- **Fast generation** - Optimized for speed without compromising quality


## Why You'll Love It


**🎯 Better Reasoning**


- Superior performance on complex, multi-step tasks
- Built specifically for agentic workflows
- Handles sophisticated problem-solving with ease


**🎨 Style Consistency**


- Use reference images to maintain visual brand consistency
- Create variations based on existing artwork
- Apply the aesthetic of one image to new scenes


**⚡ Powerful & Flexible**


- Massive context windows for processing large documents
- High-resolution image generation up to 4096×4096
- Competitive pricing for enterprise-grade capabilities


## How to Use


### Using Gemini Models in the Dashboard


#### Text Generation


1. Navigate to any AI feature in your dashboard (Content Generation, Repository Updates, or AI Chat)
2. Click the **model dropdown** menu
3. Select **"Gemini 3 Pro 🍌"** from the available models
4. Enter your prompt and start generating


#### Image Generation


1. Go to your **Project Dashboard** → **Media**
2. Click **"Generate with AI"**
3. Select **"Gemini 3 Pro Image 🍌"** from the model dropdown
4. Enter your image prompt
5. **(Optional)** Add reference images to guide the style and composition
6. Choose your desired size (1024×1024, 2048×2048, or 4096×4096)
7. Click **"Generate"**


## Examples


Prompt: "Create a highly detailed 4K architectural rendering of a modern sustainable home with floor-to-ceiling windows, integrated into a hillside landscape."


Referencing this image, "Put this house on a beach in Costa Rica."


### Text Generation with Gemini 3 Pro in the Cosmic API


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'BUCKET_SLUG'  ,
readKey  :     'BUCKET_READ_KEY'  ,
writeKey  :     'BUCKET_WRITE_KEY'
}  )


const   response   =     await   cosmic  .  ai  .  generateText  (  {
prompt  :     'Analyze this content and suggest improvements'  ,
model  :     'gemini-3-pro-preview'  ,
max_tokens  :     8000
}  )
```


### Image Generation with Reference Images


```text
const   styledImage   =     await   cosmic  .  ai  .  generateImage  (  {
prompt  :     'Create a mountain landscape in the same artistic style'  ,
model  :     'gemini-3-pro-image-preview'  ,
size  :     '2048x2048'  ,
reference_images  :     [
'https://cdn.cosmicjs.com/your-style-reference.jpg'
]  ,
folder  :     'ai-generated'
}  )
```


## Where to Find It


Both Gemini models are now available across all AI features in Cosmic:


- **API & SDK** - Use` gemini-3-pro-preview` or` gemini-3-pro-image-preview` as your model parameter
- **Dashboard AI Chat** - Select Gemini models from the model dropdown
- **Repository Updates** - Choose Gemini for intelligent code generation
- **Content Generation** - Leverage advanced reasoning for content creation


## Get Started


Ready to experience Google's most advanced AI models?[Log in to your Cosmic dashboard](https://app.cosmicjs.com/login) and check out the[AI API documentation](https://www.cosmicjs.com/docs/api/ai) to begin creating extraordinary images today.


*Image created in the Cosmic dashboard using Gemini 3 Pro Image (Nano Banana Pro), in honor of the approaching[3I/ATLAS comet](https://en.wikipedia.org/wiki/3I/ATLAS) .*
