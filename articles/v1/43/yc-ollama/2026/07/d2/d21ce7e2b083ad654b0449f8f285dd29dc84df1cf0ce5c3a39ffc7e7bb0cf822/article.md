---
schema_version: "1.0.0"
document_id: "d21ce7e2b083ad654b0449f8f285dd29dc84df1cf0ce5c3a39ffc7e7bb0cf822"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/image-generation"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:446e75da0380e2b7f8d110e44c234ace9b29fa1b42e8e9a80a7911d5f43a1fa9"
---

# Image generation (experimental)

Ollama now supports image generation on macOS, with Windows and Linux coming soon.


```text
ollama run x/z-image-turbo "your prompt"


```


Images save to your current directory. Terminals that support image rendering (Ghostty, iTerm2, etc.) can preview images directly inline.


## Models


### [Z-Image Turbo](https://ollama.com/x/z-image-turbo)


```text
ollama run x/z-image-turbo


```


Z-Image Turbo is a 6 billion parameter text-to-image model from Alibaba’s Tongyi Lab. It generates photorealistic images and handles bilingual text rendering in both English and Chinese.


- **Photorealistic output:** Strong at generating realistic photographs, portraits, and scenes
- **Bilingual text rendering:** Accurately renders both English and Chinese text in images
- **Apache 2.0:** Open weights available for commercial use


#### Examples


Photorealistic portraits:


```text
Young woman in a cozy coffee shop, natural window lighting, wearing a cream knit sweater, holding a ceramic mug, soft bokeh background with warm ambient lights, candid moment, shot on 35mm film


```


Chinese calligraphy:


```text
Traditional Chinese calligraphy brush painting style, the characters "山高水长" written in elegant black ink on rice paper, red seal stamp in corner, minimalist composition


```


Creative composition:


```text
Surreal double exposure portrait, woman's silhouette filled with blooming cherry blossom trees, soft pink and white petals floating, dreamy ethereal atmosphere


```


[Z-image turbo model page](https://ollama.com/x/z-image-turbo)


### [FLUX.2 Klein](https://ollama.com/x/flux2-klein)


Black Forest Labs’ fastest image-generation model to date, available in 4B and 9B parameter sizes.


FLUX.2 Klein handles readable text in images well, useful for UI mockups and designs with typography.


- **4B model:** Apache 2.0, fully open for commercial use
- **9B model:** FLUX Non-Commercial License v2.1


```text
ollama run x/flux2-klein


```


#### Examples


Text rendering:


```text
A neon sign reading "OPEN 24 HOURS" in a rainy city alley at night, reflections on wet pavement


```


Product photography:


```text
Matte black coffee tumbler on wooden desk, morning sunlight casting long shadows, steam rising, commercial product shot


```


[FLUX.2 Klein model page](https://ollama.com/x/flux2-klein)


## Configuration


Customize image generation with these parameters:


### Image location


Generated images save to your current directory. Change directories in your terminal to save images elsewhere.


### Image sizes


Modify width and height using the` /set width` and` /set height` commands. Smaller images generate faster and use less memory.


### Number of steps


Steps control how many iterations the model runs. Fewer steps = faster but less detailed. Too many steps can cause artifacts. Ollama defaults to the recommended step count for each model.


### Random seed


Set a seed for reproducible results, useful for iterating on a subject or sharing exact outputs. Different seeds produce different images, even with the same prompt.


### Negative prompts


Negative prompts guide the model on what you don’t want in the image.


## What’s next


- Windows and Linux support
- Additional image generation models, and image editing
