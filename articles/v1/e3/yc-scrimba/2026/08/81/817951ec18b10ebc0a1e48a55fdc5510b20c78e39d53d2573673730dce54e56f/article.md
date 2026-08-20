---
schema_version: "1.0.0"
document_id: "817951ec18b10ebc0a1e48a55fdc5510b20c78e39d53d2573673730dce54e56f"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/text-to-video-ai-how-it-works/"
published_at: "2026-08-10T09:41:43+00:00"
first_seen_at: "2026-08-10T18:07:24.658795+00:00"
fetched_at: "2026-08-10T18:07:26.369992+00:00"
content_hash: "sha256:ff2d51c2726e7f741b3d83f05b67d70b300ad03962570a7ef99377cfbfc3296a"
---

# Text to Video AI: How It Actually Works [2026]

"Text to video AI" is one phrase covering two unrelated technologies. One family generates footage that never existed, denoising compressed video latents with a diffusion transformer. The other assembles a script into video from pieces that already exist: an avatar, a synthetic voice, a template. They fail differently, they cost differently, and they are good at opposite jobs. Most articles blur them together, which is why people buy the wrong one.


## What Does "Text to Video AI" Actually Mean?


Text to video AI refers to any system that turns written input into moving pictures, but the term spans two different architectures with different outputs and failure modes.


The first family is **generative video models** . You write a prompt, the model produces pixels that did not exist before, and nothing in the output came from a library. Runway, Google's Veo, and open-weights models like LTX belong here.


The second family is **video assembly tools** . You write a script, and the platform renders it through components that already exist: a digital presenter, a cloned voice, a branded template. Synthesia and HeyGen are the reference examples.


Both are marketed as "text to video." Only one is generating video in the sense researchers mean.


Generative video model Video assembly tool


What you type A prompt describing a shot A script to be spoken


What the system does Denoises latents into new frames Renders an avatar and voice over a template


What comes out Footage of something that never happened A presenter delivering your words


Typical clip length 5 to 10 seconds per generation Minutes, limited by script length


Where it breaks Physics, on-screen text, long takes Visual sameness, low information density


The practical consequence is simple. If you need *footage* , you want the first family. If you need an *explanation* , neither family is automatically right, and the reason is worth the rest of this article.


## How Generative Video Models Work


Generative video models compress video into a latent space, cut it into patches spanning space and time, then repeatedly denoise those patches while conditioned on your text prompt.


The clearest public description of this architecture comes from OpenAI's technical report on Sora, which explains that the team trained "text-conditional diffusion models jointly on videos and images of variable durations, resolutions and aspect ratios" using "a transformer architecture that operates on spacetime patches of video and image latent codes" ([OpenAI](https://openai.com/index/video-generation-models-as-world-simulators/?ref=scrimba.com) ). Most current models follow some version of that recipe.


Broken into stages, the pipeline runs like this:


1. **Compress.** A video autoencoder squeezes raw frames into a much smaller latent representation, because working on pixels directly is far too expensive at video scale.
2. **Patch.** The latent volume is cut into *spacetime patches* , chunks covering a small region of the frame across a short span of time. This is what lets one model handle different durations, resolutions, and aspect ratios without retraining.
3. **Denoise.** The transformer starts from noise and removes it step by step, steered at each step by an embedding of your prompt.
4. **Decode.** The cleaned latents go back through the decoder into frames, and in current models the audio comes out of that same pass rather than being dubbed on afterwards.


That last stage is what changed the category most recently. Google's Veo generates sound effects, ambient noise, and dialogue natively instead of as a separate step ([Google DeepMind](https://deepmind.google/models/veo/?ref=scrimba.com) ). On the open side, Lightricks ships LTX-2.3, a 22B diffusion transformer built to "generate synchronized video and audio within a single model," with open weights, distilled checkpoints, and an explicit focus on local execution ([Lightricks](https://huggingface.co/Lightricks/LTX-2.3?ref=scrimba.com) ).


It also explains the characteristic weirdness of AI video. These models are not editing footage or simulating a scene. They predict what a plausible video of your description would look like.


> "Our results suggest that scaling video generation models is a promising path towards building general purpose simulators of the physical world." - OpenAI, *Video generation models as world simulators*


A simulator that learned physics by watching, rather than by being told, gets physics wrong in ways no 3D renderer ever would. For the wider picture of where generative models sit in a developer's toolkit, Scrimba's guide to[AI tools and courses for learning to code](https://scrimba.com/articles/best-ai-tools-and-courses-for-learning-to-code/) covers the adjacent territory.


## How Script to Video Assembly Tools Work


Assembly tools take a different path. Nothing is invented frame by frame, so the output is predictable in a way generated footage is not.


HeyGen's API makes the mechanism unusually legible. A request carries an avatar identifier, a script string, and a voice identifier, plus a choice of which renderer animates the result ([HeyGen](https://developers.heygen.com/generate-avatar-video?ref=scrimba.com) ). There is no prompt, because there is nothing to imagine.


Synthesia works the same way at product level, offering more than 240 stock avatars, a personal avatar generated from a single photo, and brand-controlled outfits and backgrounds ([Synthesia](https://www.synthesia.io/features/avatars?ref=scrimba.com) ). Its avatars deliver scripts in over 160 languages ([Synthesia](https://www.synthesia.io/?ref=scrimba.com) ), and that is the category's real value: localizing a training video is otherwise a reshoot.


The two families are converging at the edges. Synthesia now prompts avatars into AI-generated scenes powered by Veo 3, so a script-to-video product has a generative model running inside it ([Synthesia](https://www.synthesia.io/features/avatars?ref=scrimba.com) ).


The tradeoff is consistent. Assembly tools do not hallucinate and they do not morph. What they produce instead is *sameness* : a person in a frame, talking, for as long as the script runs.


## What AI Video Can and Cannot Do in 2026


Generated video in 2026 is genuinely good at short, cinematic, non-verbal shots, and consistently weak at length, physics, speech, and anything with text on screen.


**Length is the hardest ceiling.** Veo 3.1 generates eight seconds at a time. Extending a clip adds seven seconds per call up to a maximum of 148 seconds, and extension only works at 720p ([Google](https://ai.google.dev/gemini-api/docs/veo?ref=scrimba.com) ). "Make me a three minute video" is a stitching project, not a generation.


Physics is approximated rather than computed, and OpenAI's own report is blunt about it: the model "does not accurately model the physics of many basic interactions, like glass shattering," and interactions like eating food "do not always yield correct changes in object state" ([OpenAI](https://openai.com/index/video-generation-models-as-world-simulators/?ref=scrimba.com) ). The same report names incoherencies that develop in long duration samples, and objects that appear from nowhere.


Speech is the other soft spot. Google states directly that producing natural and consistent spoken audio, particularly across shorter segments, "remains an area of active development" ([Google DeepMind](https://deepmind.google/models/veo/?ref=scrimba.com) ). Anything where a viewer has to follow spoken reasoning is the worst case for a generative model.


Then there is text on screen. Labels, code, diagrams, and UI copy are exactly what an explanatory video needs, and exactly what these models render least reliably.


Beneath those sit operational details that rarely reach product marketing:


- Generated videos are removed from Google's servers after two days, so anything worth keeping has to be downloaded ([Google](https://ai.google.dev/gemini-api/docs/veo?ref=scrimba.com) ).
- Latency runs from 11 seconds at best to six minutes at peak.
- Referencing multiple videos in one prompt is unsupported and degrades output.
- English is fully supported; other languages are documented as unevaluated.
- Output carries SynthID watermarking for provenance.


The last limitation is not technical. This market removes products with little notice. OpenAI discontinued the Sora web and app experiences on April 26, 2026, and the Sora API shuts down on September 24, 2026 ([OpenAI Help Center](https://help.openai.com/en/articles/20001152-what-to-know-about-the-sora-discontinuation?ref=scrimba.com) ). The deprecation notice lists no recommended replacement ([OpenAI](https://developers.openai.com/api/docs/deprecations?ref=scrimba.com) ). Build any pipeline so a vendor shutdown is an inconvenience rather than a rewrite.


## What Does Text to Video AI Cost?


Text to video AI is priced per second of output, and published 2026 rates run from roughly five cents to sixty cents per generated second depending on tier and resolution.


Google publishes exact figures, checked in August 2026. Veo 3.1 Standard with audio costs $0.40 per second at 720p and 1080p, and $0.60 at 4K. The Fast tier drops to $0.10 per second at 720p and the Lite tier to $0.05. Gemini Omni Flash bills video output at 5,792 tokens per second of 720p video, which Google notes works out to roughly $0.10 per second ([Google](https://ai.google.dev/gemini-api/docs/pricing?ref=scrimba.com) ).


Run the arithmetic the vendor pages avoid. At $0.40 per second, one finished minute costs $24 in generation alone, and that assumes every take is usable. It never is. The number that matters is **cost per usable second** , and the gap between it and the list price is set by how often you throw a take away.


Open weights change the shape of the bill rather than removing it. LTX-2.3 is built for local execution, so the cost moves into GPU time and setup effort ([Lightricks](https://huggingface.co/Lightricks/LTX-2.3?ref=scrimba.com) ). Scrimba's overview of[how web developers can use AI](https://scrimba.com/articles/how-web-developers-can-use-ai/) covers the broader workflow picture.


## Which Text to Video Tool for Which Job?


The right tool follows from the job, not from the leaderboard. Match the output you need against what each family is architecturally capable of producing.


The job What you need Reasonable options Why


Cinematic b-roll, social clips Generative model Runway Gen-4.5, Veo 3.1 Short, visual, no speech dependency


Corporate training, internal comms Assembly tool Synthesia, HeyGen Predictable, on-brand, script-driven


Localizing existing video Assembly tool Synthesia, HeyGen Language coverage is the entire value


Local or offline generation Open weights LTX-2.3 No per-second bill, full control


Explaining how something works Explanation video See below Neither family knows your subject


Two attributions are worth keeping straight. Runway's own research post claims the top spot on the Artificial Analysis Text-to-Video benchmark at 1,247 Elo, a vendor claim rather than an independent finding ([Runway](https://runway.com/research/introducing-runway-gen-4.5?ref=scrimba.com) ). Google now recommends Gemini Omni Flash as its default generation model, with Veo 3.1 reserved for scene extension and last-frame control ([Google](https://ai.google.dev/gemini-api/docs/video?ref=scrimba.com) ).


### Explanation Video Is a Different Job


Marketing video optimizes for how it looks. Explanation video optimizes for whether the viewer understood. That difference is architectural, not stylistic.


A prompt cannot supply the thing being explained. When the subject is a codebase, a research paper, or a concept someone just got stuck on, the substance lives outside the prompt box, and prompt engineering does not move it inside. A text-to-video model cannot explain *your* system, because it has never seen it.


The format itself holds up. A study of 500 adult learners across four conditions found "no statistically significant difference amongst conditions on recall and recognition performance" between synthetic video, human-recorded video, and text, while participants preferred video formats over text materials ([Li, Barry and Cukurova](https://arxiv.org/abs/2412.10384?ref=scrimba.com) ). Video did not teach better. It was preferred and it was not worse, which is a reasonable bar.


This is the category Scrimba Explain sits in. It is an MCP plugin: you ask your coding agent a question, the agent researches it against the files and conversation it already has, and Explain turns that answer into a narrated video walkthrough. It is free during open beta and works with Claude Code, Codex, ChatGPT, and any agent supporting MCP ([Scrimba](https://explain.new/?ref=scrimba.com) ). Non-code subjects work too, and its FAQ names biology, mathematics, philosophy, and language learning.


What it is not is equally worth stating. There are no avatars, no stock footage, and no brand templates, so it is not an alternative to Synthesia or HeyGen, and it does not generate footage from an arbitrary prompt. Its own FAQ is candid about accuracy: "Like any AI tool, it can make mistakes, so double-check anything important" ([Scrimba](https://explain.new/?ref=scrimba.com) ). If the plugin model is unfamiliar, Scrimba's roundup of[MCP tutorials and courses](https://scrimba.com/articles/best-mcp-tutorials-and-courses/) covers the protocol itself.


## How to Write a Prompt That Actually Works


Prompting a video model is closer to writing a shot list than writing a request. Describe the frame, not the intention.


1. **Subject.** Name what is in frame concretely. "A rusted metal bucket" beats "an old container."
2. **Motion.** Say what moves and how fast, because models default to drifting.
3. **Camera.** Specify the shot and the movement: static wide, slow push in, handheld pan.
4. **Lighting.** Time of day, source, direction. This carries more of the final look than anything else on the list.
5. **One event.** A prompt containing two events becomes a cut, and cuts are where consistency breaks.


Prompting cannot reach the structural limits. Clip length, readable on-screen text, hands mid-interaction, and any fact the model was never given sit outside what wording can fix. Scrimba's list of[prompt engineering courses](https://scrimba.com/articles/best-prompt-engineering-courses-2026/) transfers reasonably well.


## Frequently Asked Questions


### How does text to video AI actually work?


Generative models compress video into a latent space, split it into patches covering space and time, then denoise those patches step by step while conditioned on your text prompt. Assembly tools work differently, rendering a script through a pre-built avatar and synthetic voice rather than generating new footage.


### What is the best text to video AI in 2026?


There is no single best, because the two families do different jobs. Runway and Veo lead on generated footage, Synthesia and HeyGen lead on script-driven presenter video, and LTX-2.3 leads among open-weights models you can run locally. Pick by output type first.


### Is there a free text to video AI?


Some tools offer free tiers with limits, and open-weights models like LTX-2.3 are free to download and run on your own hardware, though you pay in GPU time. API access to the leading hosted models is paid, typically billed per second of generated video.


### How long can AI-generated videos be?


Most models generate five to ten seconds per request. Veo 3.1 produces eight-second clips and can extend them seven seconds at a time up to 148 seconds, at reduced resolution. Longer videos are stitched from many generations rather than produced in one pass.


### Can AI video generators make explainer videos?


Not well, for anything specific. Generative models render text on screen unreliably and cannot know your codebase, document, or product. Explanation video needs a system that reads the actual source material and narrates it, which is a different mechanism from prompt-to-footage generation.


## Key Takeaways


- **"Text to video AI" describes two architectures.** Generative models denoise new footage into existence; assembly tools render a script through an existing avatar and voice.
- **The core mechanism is a diffusion transformer over spacetime patches** , trained jointly on videos and images of varying duration, resolution, and aspect ratio.
- **Eight seconds is the practical generation unit** in 2026. Longer output is extension and stitching, usually at reduced resolution.
- **Physics, speech, and on-screen text are the reliable failure modes** , documented by the model providers themselves.
- **Cost is per second and adds up fast.** At $0.40 per second, one usable minute of Veo 3.1 Standard runs $24 before a single rejected take.
- **The market is unstable.** OpenAI shut down the Sora app in April 2026 and retires its API in September 2026.
- **Explanation video is a separate job** that needs a system reading real source material, not a prompt describing a shot.


## Sources


- OpenAI. "Video generation models as world simulators."[https://openai.com/index/video-generation-models-as-world-simulators/](https://openai.com/index/video-generation-models-as-world-simulators/?ref=scrimba.com)
- OpenAI Help Center. "What to know about the Sora discontinuation." 2026.[https://help.openai.com/en/articles/20001152-what-to-know-about-the-sora-discontinuation](https://help.openai.com/en/articles/20001152-what-to-know-about-the-sora-discontinuation?ref=scrimba.com)
- OpenAI. "Deprecations." 2026.[https://developers.openai.com/api/docs/deprecations](https://developers.openai.com/api/docs/deprecations?ref=scrimba.com)
- Google. "Video generation in the Gemini API." 2026.[https://ai.google.dev/gemini-api/docs/video](https://ai.google.dev/gemini-api/docs/video?ref=scrimba.com)
- Google. "Generate videos with Veo." 2026.[https://ai.google.dev/gemini-api/docs/veo](https://ai.google.dev/gemini-api/docs/veo?ref=scrimba.com)
- Google. "Gemini Developer API pricing." 2026.[https://ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing?ref=scrimba.com)
- Google DeepMind. "Veo." 2026.[https://deepmind.google/models/veo/](https://deepmind.google/models/veo/?ref=scrimba.com)
- Runway. "Introducing Runway Gen-4.5." 2025.[https://runway.com/research/introducing-runway-gen-4.5](https://runway.com/research/introducing-runway-gen-4.5?ref=scrimba.com)
- Lightricks. "LTX-2.3 model card." 2026.[https://huggingface.co/Lightricks/LTX-2.3](https://huggingface.co/Lightricks/LTX-2.3?ref=scrimba.com)
- Li, Z. R., Barry, C., and Cukurova, M. "Adult learners recall and recognition performance and affective feedback when learning from an AI-generated synthetic video." 2024. arXiv:2412.10384.[https://arxiv.org/abs/2412.10384](https://arxiv.org/abs/2412.10384?ref=scrimba.com)
- Synthesia. "AI Avatars." Self-reported.[https://www.synthesia.io/features/avatars](https://www.synthesia.io/features/avatars?ref=scrimba.com)
- HeyGen. "Generate an avatar video."[https://developers.heygen.com/generate-avatar-video](https://developers.heygen.com/generate-avatar-video?ref=scrimba.com)
- Scrimba. "Scrimba Explain." Self-reported.[https://explain.new/](https://explain.new/?ref=scrimba.com)
