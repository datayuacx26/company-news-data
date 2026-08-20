---
schema_version: "1.0.0"
document_id: "d40bcbce16d5f05b20fbd01c1a3f5739169737b14ea4aa4966995562a532333d"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/introducing-ai-video-generation"
published_at: "2025-12-23T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:1593c00a7b6408a805d435938f908040015f0397a45845ce03fc9d679224f34b"
---

# Introducing AI Video Generation

We're excited to announce **AI video generation** is now available on all accounts in the Cosmic dashboard and API. Powered by Google's Veo 3.1 you can create powerful, dynamic video content using natural language.


This release also includes **more efficient token usage** for all media generation features enabling you to create even more AI generated images and videos.


## Check it out


You can now create compelling video content using only natural language. For example you can create something like this:


> Prompt: "Cinematic close-up of raindrops falling on a window at night, city lights blurred in background"


### 📹 AI Video Generation with Veo 3.1


Google's state-of-the-art Veo 3.1 models deliver:


- **Native audio generation** - Videos include automatically generated audio that matches your scene
- **Two quality tiers** - Fast (30-90s generation) or Standard (60-180s, premium cinematic quality)
- **Flexible options** - Generate 4, 6, or 8-second videos at 720p or 1080p
- **Image-to-video mode** - Use 1 reference image as the starting frame for precise control
- **Video extension** - Extend any Veo-generated video to create longer sequences
- **Instant storage** - Videos are automatically saved to your Media Library with global CDN delivery


## What can you create?


**🎬 Professional Video Content**


- Create product showcases, social media content, and marketing videos in seconds
- Cinematic quality suitable for hero content and campaigns
- Reference images ensure brand consistency across all generated content


**⚡ Seamless Integration**


- Videos are automatically uploaded to your Media Library
- Global CDN delivery with real-time image optimization via Imgix
- Complete API and SDK access for programmatic generation


## Bring Your Images to Life


Transform static images into dynamic videos with image-to-video generation. Start with any photo and add motion, sound, and cinematic effects.


**Reference Image:**


> Prompt: "Animate this image. Drum solo sounds. Zoom out to show other band members and audience."


Image-to-video mode gives you precise control over your video content by using your existing images as the starting frame. Perfect for bringing product photos, marketing materials, and brand assets to life with motion and sound.


## How to Use


### Using Video Generation in the Dashboard


1. Navigate to your **Project Dashboard** → **Media**
2. Click **"Create"**
3. Select **"Video Generation"** mode
4. Choose **"Veo 3.1 Fast"** or **"Veo 3.1 Standard"** from the model dropdown
5. Enter your video prompt
6. **(Optional)** Add 1 reference image to use as the starting frame (image-to-video mode)
7. Choose duration (4, 6, or 8 seconds) and resolution (720p or 1080p)
8. Click **"Generate"**


### Using the API


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'YOUR_BUCKET_SLUG'  ,
readKey  :     'YOUR_READ_KEY'  ,
writeKey  :     'YOUR_WRITE_KEY'
}  )


// Generate a video with reference image (image-to-video mode)
const   video   =     await   cosmic  .  ai  .  generateVideo  (  {
prompt  :     'Product rotates smoothly revealing all angles with soft studio lighting'  ,
duration  :     8  ,
resolution  :     '720p'  ,
reference_images  :     [
'https://cdn.cosmicjs.com/product-hero.jpg'
]  ,
folder  :     'ai-videos'
}  )


console  .  log  (  video  .  media  .  url  )     // Ready to use!
```


## Examples


### E-Commerce Product Videos


Generate product showcase videos starting from your product hero image:


```text
const   productVideo   =     await   cosmic  .  ai  .  generateVideo  (  {
prompt  :     'Product rotates smoothly with elegant studio lighting'  ,
duration  :     6  ,
reference_images  :     [  'https://cdn.cosmicjs.com/product-hero.jpg'  ]
}  )
```


### Social Media Content


Create engaging short-form content for Instagram Reels, TikTok, and Stories:


```text
const   reelVideo   =     await   cosmic  .  ai  .  generateVideo  (  {
prompt  :     'Time-lapse of sunrise over city skyline with vibrant colors'  ,
duration  :     6  ,
resolution  :     '1080p'
}  )
```


### Marketing Campaigns


Build premium video ads starting from your hero image:


```text
const   heroVideo   =     await   cosmic  .  ai  .  generateVideo  (  {
prompt  :     'Camera slowly zooms in with dramatic lighting and depth of field'  ,
model  :     'veo-3.1-generate-preview'  ,     // Premium quality
duration  :     8  ,
resolution  :     '1080p'  ,
reference_images  :     [  'https://cdn.cosmicjs.com/hero-frame.jpg'  ]
}  )
```


## Extend Your Videos


Take your Veo-generated videos further with **video extension** . Continue from the last frame to create longer, more complex sequences without starting from scratch.


**How it works:**


1. Generate or select any Veo-generated video in your Media Library
2. Click **"Extend video"** from the media details panel
3. Describe how you want the video to continue
4. Veo 3.1 seamlessly extends from the last frame


Video extension is perfect for:


- **Building longer narratives** - Chain multiple generations into cohesive stories
- **Iterative creation** - Generate, review, and extend until you achieve your vision
- **Scene transitions** - Extend with camera movements, zooms, or new action


Each new generation automatically becomes your active video for subsequent extensions, making it easy to build longer sequences step by step.


### Extending a Video in the Dashboard


1. Open any Veo-generated video from your **Media Library**
2. Click **"Extend video"** in the Cosmic AI section
3. Describe how you want the video to continue
4. The new extended video replaces the previous one in context for chaining


Video extensions are always 8 seconds at 720p, and each extended video can be extended again for unlimited chaining.


### Video Extension via API


Chain multiple video segments together programmatically to build longer narratives:


```text
// Generate initial video
const   video   =     await   cosmic  .  ai  .  generateVideo  (  {
prompt  :     'A calico kitten sitting peacefully in golden sunlight'  ,
duration  :     8
}  )


// Extend it with a continuation
const   extended   =     await   cosmic  .  ai  .  extendVideo  (  {
media_id  :   video  .  media  .  id  ,
prompt  :     'The kitten stands up and walks away into the garden'
}  )


console  .  log  (  extended  .  media  .  url  )     // Your extended video!
```


### More examples


> Prompt: "Birds eye view of Lake Tahoe at winter. Zoom down to the water and fly over a cabin with smoking chimney."


> Prompt: "A drummer playing a drum solo at a concert. Shot panes across the stage."


> Prompt: "A time-lapse video of a serene mountain landscape at sunset with gentle wind."


## Where to Find It


Video generation is now available in the dashboard, API, and SDK:


- **Dashboard Media Library** - Generate and extend videos directly in your[project dashboard](https://app.cosmicjs.com/login)
- **API & SDK** - Use` cosmic.ai.generateVideo()` and` cosmic.ai.extendVideo()` in your code, now available in the latest[Cosmic JavaScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk) .


### 💰 72% More Efficient Media Token Usage


We've restructured how tokens are used for media generation to give you even more AI generated images and videos. See[AI usage and limits in the Cosmic docs](https://www.cosmicjs.com/docs/api/ai#usage-and-limitations) for more details.


## Get Started


Ready to create stunning AI-generated videos?[Log in to the dashboard](https://app.cosmicjs.com/login) and update to the latest SDK version and start generating:


```text
npm     install   @cosmicjs/sdk@latest
```


All users automatically receive the improved token efficiency - no action required. Your existing token allocations now deliver 3-4x more media generation capacity.


📚[View Complete AI API Documentation](https://www.cosmicjs.com/docs/api/ai)
📹[Video Generation Reference](https://www.cosmicjs.com/docs/api/ai#generate-video)
🔗[Video Extension Reference](https://www.cosmicjs.com/docs/api/ai#extend-video)
💰[Pricing Information](https://www.cosmicjs.com/pricing)


*Video generation powered by Google's Veo 3.1 models. Token efficiency improvements automatically applied to all accounts starting December 23, 2025.*
