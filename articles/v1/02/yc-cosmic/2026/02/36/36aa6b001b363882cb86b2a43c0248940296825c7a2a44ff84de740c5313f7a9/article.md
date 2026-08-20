---
schema_version: "1.0.0"
document_id: "36aa6b001b363882cb86b2a43c0248940296825c7a2a44ff84de740c5313f7a9"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/introducing-ai-audio-generation"
published_at: "2026-02-23T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:85af33eba87dd70fefa3b5c92c94d4ce79064c93de8321126aad8ec418679f7c"
---

# Introducing AI Audio Generation

Cosmic AI now includes text-to-speech generation, powered by OpenAI's TTS models. Convert any text into natural-sounding audio and save it directly to your media library as an MP3, ready for CDN delivery.


Whether you're creating audio versions of blog posts, podcast intros, product walkthroughs, or accessibility-focused content, audio generation is available in the dashboard, API, and SDK.


## 9 Natural-Sounding Voices


Choose from a range of voices to match your content's tone:


**Feminine voices:**


- **Nova** (default): Warm and bright, great for friendly narration
- **Shimmer** : Soft and intimate, ideal for meditation or bedtime stories
- **Coral** : Clear and polished, professional tone for product demos
- **Sage** : Calm and steady, thoughtful pacing for education and tutorials
- **Alloy** : Neutral and balanced, versatile for general-purpose use


**Masculine voices:**


- **Echo** : Deep and authoritative, great for announcements and news
- **Onyx** : Bold and commanding, strong presence for intros and branding
- **Fable** : Animated and expressive, a natural storyteller for audiobooks
- **Ash** : Warm and approachable, conversational for interviews


## Two Quality Tiers


- **Standard (tts-1)** : Fast, low-latency generation. Recommended for most use cases.
- **HD (tts-1-hd)** : Higher quality audio with richer detail. 2x token cost.


## Long Text Support


Texts over 4,096 characters are automatically split at paragraph boundaries and concatenated into a single seamless audio file. No manual chunking required.


## How to Use in the Dashboard


1. Navigate to **Media** in your project
2. Click **Create** and select **Audio**
3. Select a **voice** from the dropdown
4. Paste or type the text you want to convert
5. Click **Generate**


The audio file is saved to your media library in MP3 format, available instantly via CDN.


## API and SDK Access


Generate audio programmatically using the API or JavaScript SDK:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'BUCKET_SLUG'  ,
readKey  :     'BUCKET_READ_KEY'  ,
writeKey  :     'BUCKET_WRITE_KEY'
}  )


const   audio   =     await   cosmic  .  ai  .  generateAudio  (  {
prompt  :     'Welcome to our product walkthrough.'  ,
voice  :     'nova'
}  )


console  .  log  (  audio  .  media  .  url  )
```


Or with cURL:


```text
curl   https://workers.cosmicjs.com/v3/buckets/  ${BUCKET_SLUG}  /ai/audio   \
-d   '{"prompt":"Your text here","voice":"nova"}'     \
-H   'Content-Type: application/json'     \
-H   "Authorization: Bearer   ${BUCKET_WRITE_KEY}  "
```


Optional parameters include` model` (` tts-1` or` tts-1-hd` ),` voice` ,` folder` , and` metadata` .


## Pricing


Audio generation tokens scale with text length:


Model Cost per 1,000 characters


TTS Standard 3,600 tokens


TTS HD 7,200 tokens


## Get Started


Audio generation is available now on all Cosmic plans. Open your project, head to Media, and try it out. For full API documentation, visit the[AI API reference](https://www.cosmicjs.com/docs/api/ai#generate-audio) .


For dashboard usage details, see the[AI dashboard docs](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) .
