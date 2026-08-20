---
schema_version: "1.0.0"
document_id: "fe45e4c6280405e1f35c97336fd6cd23d97bcabfbb65d227a721f660b923e900"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/building-voice-enabled-content-experiences-ai-speech-headless-cms"
published_at: "2026-02-23T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:1a0324aabf60049e531889e1afeb9b2d452f54bb34b1d9e6fc2542c31e1ff4f2"
---

# Building Voice-Enabled Content Experiences: How to Add AI Speech Features to Your Headless CMS

Audio content is no longer optional. Major publishers like The Guardian have added "Listen to this article" features, and accessibility requirements are driving demand for multi-modal content delivery. With Cosmic's built-in AI audio generation, developers can now add voice capabilities to any Cosmic-powered application in minutes, not weeks.


This guide walks you through using Cosmic's native text-to-speech capabilities, covering the dashboard workflow, API integration, content modeling, implementation patterns, and cost optimization.


## Why Voice-Enabled Content Matters


Voice-enabled content serves multiple audiences:


- **Accessibility compliance** : WCAG 2.1 guidelines (1.2.1, 1.2.3) establish requirements for audio alternatives to text content
- **Multi-modal consumption** : Users increasingly want to listen while commuting, exercising, or multitasking
- **Content reach expansion** : Audio versions open your content to visually impaired users and auditory learners
- **Engagement metrics** : Publications report increased time-on-site when audio options are available


## Cosmic AI Audio Generation


Cosmic provides built-in text-to-speech powered by OpenAI's TTS models, available directly from the[dashboard](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) and[API](https://www.cosmicjs.com/docs/api/ai#generate-audio) . No third-party API keys or external services required.


### Key Features


- **9 natural-sounding voices** : Choose from feminine voices (nova, shimmer, coral, sage, alloy) and masculine voices (echo, onyx, fable, ash)
- **Two quality tiers** : Standard (tts-1) for fast, low-latency generation or HD (tts-1-hd) for higher quality output
- **Long text support** : Texts over 4,096 characters are automatically chunked at paragraph boundaries and concatenated into a single file
- **Instant CDN delivery** : Generated audio is saved as MP3 to your Media Library and served through Cosmic's global CDN


### Voice Selection Guide


Voice Style Best For


nova Warm and bright, friendly narration Podcasts, explainers


shimmer Soft and intimate, gentle delivery Meditation, ASMR, bedtime stories


coral Clear and polished, professional tone Product demos, business content


sage Calm and steady, thoughtful pace Education, tutorials


alloy Neutral and balanced, versatile General purpose, articles


echo Deep and authoritative, confident Announcements, trailers, news


onyx Bold and commanding, strong presence Intros, branding, dramatic reads


fable Animated and expressive, natural storyteller Storytelling, audiobooks


ash Warm and approachable, conversational Conversational, interviews


## Generating Audio from the Dashboard


The fastest way to get started is directly from the Cosmic dashboard:


1. Navigate to **Media** in your project
2. Click **Create** and select **Audio**
3. Select a voice from the dropdown (default: Nova)
4. Paste or type the text you want to convert to speech
5. Click **Generate** to create the audio file


Audio files are automatically saved to your Media Library in MP3 format, ready for use in your applications. For full details on dashboard usage, see the[AI dashboard docs](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) .


## Content Modeling for Audio


Before implementing programmatic TTS, extend your content model to support audio metadata. In Cosmic, add these metafields to your blog post or article object type:


```text
const   audioMetafields   =     [
{
title  :     "Audio File"  ,
key  :     "audio_file"  ,
type  :     "file"  ,
value  :     ""
}  ,
{
title  :     "Audio Duration"  ,
key  :     "audio_duration"  ,
type  :     "text"  ,
value  :     ""
}  ,
{
title  :     "Voice ID"  ,
key  :     "voice_id"  ,
type  :     "select-dropdown"  ,
options  :     [
{     key  :     "nova"  ,     value  :     "Nova"     }  ,
{     key  :     "sage"  ,     value  :     "Sage"     }  ,
{     key  :     "coral"  ,     value  :     "Coral"     }  ,
{     key  :     "echo"  ,     value  :     "Echo"     }  ,
{     key  :     "onyx"  ,     value  :     "Onyx"     }  ,
{     key  :     "fable"  ,     value  :     "Fable"     }  ,
{     key  :     "shimmer"  ,     value  :     "Shimmer"     }  ,
{     key  :     "alloy"  ,     value  :     "Alloy"     }  ,
{     key  :     "ash"  ,     value  :     "Ash"     }
]
}  ,
{
title  :     "Audio Language"  ,
key  :     "audio_language"  ,
type  :     "text"  ,
value  :     "en-US"
}
]
```


This schema captures essential audio metadata for playback controls and accessibility features. The` audio_file` metafield uses Cosmic's file type, which stores a reference to a media object in your Media Library, giving you access to the full media properties including the CDN URL, file name, and size.


## Implementation: Cosmic SDK Audio Generation


Here's a complete implementation pattern for generating audio from Cosmic content using the built-in[AI audio API](https://www.cosmicjs.com/docs/api/ai#generate-audio) :


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY
}  )


async     function     generateAudioForArticle  (  articleSlug  )     {
// Fetch article content from Cosmic
const     {     object  :   article   }     =     await   cosmic  .  objects
.  findOne  (  {
type  :     'blog-posts'  ,
slug  :   articleSlug
}  )
.  props  (  'id,title,metadata'  )


// Use Cosmic AI to generate a clean, narration-ready version of the article
const     {     text  :   narratorText   }     =     await   cosmic  .  ai  .  generateText  (  {
prompt  :     `  Convert the following markdown article into clean, narration-ready plain text suitable for text-to-speech. Remove all markdown formatting, code blocks, tables, URLs, and image references. Convert lists into natural flowing sentences. Do not add any commentary or introduction that is not in the original content. Just return the cleaned narration text.\n\n  ${  article  .  metadata  .  markdown_content  }  `  ,
model  :     'claude-haiku-4-5-20251001'  ,
max_tokens  :     4000
}  )


// Generate audio using Cosmic's built-in AI
const   audioResponse   =     await   cosmic  .  ai  .  generateAudio  (  {
prompt  :   narratorText  ,
voice  :   article  .  metadata  .  voice_id     ||     'nova'  ,
model  :     'tts-1-hd'  ,
folder  :     'article-audio'
}  )


// Update article with the generated audio file reference
await   cosmic  .  objects  .  updateOne  (  article  .  id  ,     {
metadata  :     {
audio_file  :   audioResponse  .  media  .  name  ,
audio_duration  :     calculateDuration  (  narratorText  )
}
}  )


return   audioResponse  .  media  .  url
}


function     calculateDuration  (  text  )     {
// Average speaking rate: 150 words per minute
const   words   =   text  .  split  (  /  \s+  /  )  .  length
const   minutes   =     Math  .  ceil  (  words   /     150  )
return     `  ${  minutes  }   min  `
}
```


This approach uses Cosmic AI's text generation with a lightweight model (Claude Haiku 4.5) to intelligently convert markdown content into narration-ready text before passing it to the audio generator. Unlike simple HTML stripping, the AI understands context and converts structured elements like tables and code blocks into natural, speakable prose. There is no need for a separate OpenAI client or API key. Cosmic handles the text cleanup, TTS generation, file storage, and CDN delivery all within the platform. The generated MP3 is automatically available in your Media Library. When updating the article, we set the` audio_file` metafield to the media object's` name` value, which links the file metafield to the corresponding media asset in your library.


## Using the API Directly with cURL


You can also generate audio directly via the[REST API](https://www.cosmicjs.com/docs) :


```text
# Using default settings (nova voice, standard quality)
curl   https://workers.cosmicjs.com/v3/buckets/  ${BUCKET_SLUG}  /ai/audio   \
-d   '{"prompt":"Welcome to our product walkthrough."}'     \
-H   'Content-Type: application/json'     \
-H   "Authorization: Bearer   ${BUCKET_WRITE_KEY}  "


# Using HD model with a specific voice
curl   https://workers.cosmicjs.com/v3/buckets/  ${BUCKET_SLUG}  /ai/audio   \
-d   '{"prompt":"Your article text here...","voice":"sage","model":"tts-1-hd"}'     \
-H   'Content-Type: application/json'     \
-H   "Authorization: Bearer   ${BUCKET_WRITE_KEY}  "
```


The response includes the full media object with a CDN URL you can use immediately in your application.


## Batch Audio Generation


For generating audio across multiple articles at once, use a batch processing approach:


```text
async     function     generateAudioForAllArticles  (  )     {
const     {     objects  :   articles   }     =     await   cosmic  .  objects
.  find  (  {     type  :     'blog-posts'     }  )
.  props  (  'id,slug,title,metadata'  )


const   results   =     [  ]


for     (  const   article   of   articles  )     {
// Skip articles that already have audio
if     (  article  .  metadata  .  audio_file  )     {
console  .  log  (  `  Skipping   ${  article  .  title  }   - audio already exists  `  )
continue
}


// Use Cosmic AI to convert markdown to narration-ready text
model  :     'claude-haiku-4-5-20251001'  ,
max_tokens  :     4000
}  )


const   audioResponse   =     await   cosmic  .  ai  .  generateAudio  (  {
prompt  :   narratorText  ,
voice  :   article  .  metadata  .  voice_id     ||     'nova'  ,
model  :     'tts-1'  ,
folder  :     'article-audio'
}  )


await   cosmic  .  objects  .  updateOne  (  article  .  id  ,     {
metadata  :     {
audio_file  :   audioResponse  .  media  .  name  ,
audio_duration  :     calculateDuration  (  narratorText  )
}
}  )


results  .  push  (  {
title  :   article  .  title  ,
audioUrl  :   audioResponse  .  media  .  url
}  )


console  .  log  (  `  Generated audio for:   ${  article  .  title  }  `  )
}


return   results
}
```


## Automating Audio with Cosmic AI Agents


For a fully hands-off approach, use[Cosmic AI Agents](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) to automatically generate audio whenever new content is published. Set up an event-triggered Content Agent that listens for publish events:


1. Navigate to the **AI Agents** page in your project
2. Click **Create Agent** and select **Content Agent**
3. Set the prompt: "When a blog post is published, generate an audio version using the nova voice and update the audio_file metafield with the result."
4. Enable **Event Triggers** and select **Object Published** for your blog posts object type


You can also create a[Workflow](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) that chains audio generation with other tasks, such as generating social media content or translating the article into other languages, all triggered by a single publish event.


## Deployment and Caching Strategies


Audio generation consumes AI tokens, so implement these optimizations:


**1. Pre-generate on publish** : Use Cosmic webhooks or event-triggered agents to generate audio when content is published, not on user request.


**2. CDN caching** : Generated audio files are stored in Cosmic's Media Library and delivered through the global CDN for edge caching. No additional CDN configuration is needed.


**3. Selective generation** : Not all content needs audio. Add a toggle metafield to let editors choose which articles get audio versions.


**4. Choose the right quality tier** : Use standard (tts-1) for most content. Reserve HD (tts-1-hd) for premium content where higher audio fidelity justifies the 2x token cost.


**5. Automatic chunking** : Cosmic handles long texts automatically. Articles over 4,096 characters are split at paragraph boundaries and concatenated into a single MP3, so you do not need to manage chunking logic yourself.


## Accessibility Compliance


When implementing voice features, ensure WCAG 2.1 compliance:


- **Provide transcripts** : Audio content must have text alternatives (WCAG 1.2.1). Since your source content is already text, link to it alongside the audio player.
- **Player controls** : Include play, pause, volume, and playback speed controls
- **Keyboard navigation** : Ensure the audio player is fully keyboard accessible
- **Visual indicators** : Show current playback position and remaining time


## Understanding Token Costs


Cosmic AI audio generation uses tokens as the unit of usage. In general, 1 token equals roughly 1 word or 4 characters. Audio generation tokens are counted as output tokens, which reflect the computational requirements of producing speech.


You can monitor your AI token usage from the **Usage** section of your project settings. For details on plan limits and token add-ons, visit the[pricing page](https://www.cosmicjs.com/docs) .


To optimize costs:


- Use the standard model (tts-1) for routine content
- Reserve HD (tts-1-hd) for high-visibility or premium content
- Skip audio generation for short-form content like product updates or changelogs
- Track usage patterns monthly and adjust your generation strategy accordingly


## Putting It All Together


Voice-enabled content transforms how users interact with your applications. With Cosmic's built-in AI audio generation, you do not need to manage separate TTS API keys, handle file storage, or configure CDN delivery. Everything is handled within the platform.


Start by generating a few audio files from the dashboard to test voice options. Then integrate the SDK into your publish workflow for automated generation. For fully autonomous audio production, set up an event-triggered agent that generates audio every time new content goes live.


Your content becomes instantly more accessible, engaging, and versatile with just a few lines of code.[Log in](https://www.cosmicjs.com/docs) to your Cosmic account and start generating audio today.


**Resources**


- [Introducing AI Audio Generation](https://www.cosmicjs.com/blog/introducing-ai-audio-generation) - Announcement blog post
- [AI API Reference](https://www.cosmicjs.com/docs/api/ai#generate-audio) - Full API documentation for audio generation
- [AI Dashboard Docs](https://www.cosmicjs.com/docs/dashboard/ai#generate-audio) - Dashboard usage guide
- [API Reference](https://www.cosmicjs.com/docs) - Complete Cosmic API documentation
