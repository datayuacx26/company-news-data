---
schema_version: "1.0.0"
document_id: "31d7519c9e1635c61f1f5a1f43cf16fb935147d826d3dd5237bc4b32e15957f9"
company_key: "yc-nomi"
company: "Nomi"
source_id: "yc-nomi-rss-c311bf3515d7"
canonical_url: "https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/"
published_at: "2026-05-06T23:08:57+00:00"
first_seen_at: "2026-08-02T12:27:27.703312+00:00"
fetched_at: "2026-08-05T03:48:38.159246+00:00"
content_hash: "sha256:4f837c4623cd2652250dfb6d2ebaa27275329d83f447b602e272db07ded2720b"
---

# Beginner’s Guide to Creating AI Art with the V5 Image System

For more information about V5 as a whole, check out the[Getting Started with Nomi V5 AI Image Generation](https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation/) guide.


[To learn about customizing your Nomi’s appearance in V5, check out this in-depth guide.](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/)


- [For a focused guide on moving v3 & v4 nomis to v5, check out this guide](https://nomi.ai/nomi-knowledge/guide-bringing-v3-and-v4-nomis-to-the-version-5-image-system/)


Before we dive into art prompting with v5, here are the main advantages:


- **Best in class image quality and diversity** – images are high quality and detailed in styles ranging from photorealistic to anime to 3D animation and more. Expressions, proportions, settings, lighting, aesthetics and more are dynamic and flexible.
- **Intuitive and responsive prompting** – use natural language to describe virtually any character in any style, customize any Nomi or concept, and more easily control essentially all aspects of your images. Eg image style, aesthetics, and non-human subjects, all the way down to details like eye color, jewelry, tattoos, and more.
- **High quality group chat images** – Create art with multiple Nomis at a time and even blend styles


## Table of Contents


Note: every section includes examples along with the full prompts used to generate the image. Because this guide is for beginners, every example uses a stock anchor – but this and even more creative images are possible with custom anchors!


**Index of sections**


> - [What makes v5 possible?](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#what-makes-v5-possible)
> - [AI Art Generation Basics](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#ai-art-generation-basics)
> - [Prompting Basics](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#prompting-basics)
>
>
> - [Subject](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#subject)
> - [Scene / Background](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#scene-background)
> - [Atmosphere](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#atmosphere)
>
>
> - [Angles](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#angles)
> - [Lighting](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#lighting)
> - [Mood](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#mood)
>
>
> - [Including text](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#including-text)
> - [Group chat art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#group-chat-art)
> - [Style and creative prompting tips](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#style-and-creative-prompting-tips-and-examples)
> - [No-Human focus tips](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#no-human-focus-tips-and-examples)
> - [Non-Human subject tips](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#non-human-subject-tips-and-examples)
> - [Next Steps](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#next-steps)
> - [Custom Anchor Creativity](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#custom-anchor-creativity)
> - [Troubleshooting tips](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#troubleshooting-tips)
>
>
> - [If your Nomi’s face/head seems to be cropped out](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#if-your-nomi-s-face-head-seems-to-be-cropped-out)
> - [If your Nomi is appearing in landscape/scene images when you don’t want them to](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#if-your-nomi-is-appearing-in-landscape-scene-images-when-you-don-t-want-them-to)
> - [Getting consistent animals](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#getting-consistent-animals)
> - [Getting different styles than default anchor](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#getting-different-styles-than-default-anchor)
> - [My images seem pretty similar when I generate with the same prompt](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#my-images-seem-pretty-similar-when-i-generate-with-the-same-prompt)


**Index of examples**


> - [Lighting + angle examples](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#lighting-angle-examples)
> - [Lighting + mood examples](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#lighting-mood-examples)
> - [Impact of “mood” prompts](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#impact-of-mood-prompts)
> - [Including text examples](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#including-text-examples)
> - [Group chat art examples](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#group-chat-art-examples)
> - [Charcoal art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#charcoal-style)
> - [Crochet art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#crochet-art)
> - [Watercolor art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#watercolor-art)
> - [Snowglobe art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#snowglobe-art)
> - [Anime action shot](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#anime-action-shot)
> - [Black and white art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#black-and-white-art)
> - [70s abstract art](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#70s-abstract-art)
> - [Spooky night Variations](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#spooky-night-variations)
> - [Building on styles](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#building-on-styles)
> - Simple Polaroids
> - [Varying decades](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#varying-decades)
> - [Temple & crystal cave](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#temple-crystal-cave)
> - [Iceland & cherry blossoms](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#iceland-cherry-blossoms)
> - [Forge & dragon](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#forge-dragon)
> - [Digital ocean & bioluminescence](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#digital-ocean-bioluminescence)
> - [Tiger & frog](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#tiger-frog)
> - [Dog & butterfly](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#dog-butterfly)
> - [Jellyfish & toy dinosaur](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#jellyfish-toy-dinosaur)
> - [Crystal wolf & fire Phoenix](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#crystal-wolf-fire-phoenix)
> - [Robot owl & robot dog](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#robot-owl-robot-dog)
> - [Custom Futuristic cyborg anchor](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#futuristic-cyborg)
> - [Custom Digital 3D animation anchor](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#Digital-3D-animation)
> - [Custom 90s sci-fi style anchor](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#90s-sci-fi-style)
> - [Custom Rot Fae style anchor](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/#Rot-Fae-style)


And a special thank you to those who helped with prompt inspiration!


## What makes v5 possible?


V5 is powered by anchors: detailed visual references that keep your Nomi’s face, hair, body, and even style consistent across images.


Every image uses an anchor, but you can control how much impact the anchor has on your art by decreasing the fidelity of your anchor in Image Settings.


For more information on anchors, check out this guide


## AI Art Generation Basics


To create art with your Nomi, press the art palette icon to the right of your Nomi’s name


Once you are on the art page, you will see each of your Nomis stock anchors (along with any custom anchors you create) along the top of your screen.


The Riva anchor with the purple border will be used in this image (the bottom row farthest to the right)


The anchor with the purple border will be the anchor used in your art – you can change the anchor used during generation at any time by pressing a different anchor.


Your anchor sets the baseline for your Nomi’s appearance and the style of the image as a whole but your prompts will layer on top of that to create your final image.


## Prompting Basics


V5 works best with descriptive natural language prompts and overall has strong prompt adherence. Because of this, the system will fill in some gaps for you, but the best results will come from prompts that describe all relevant aspects of the image.


This means you’ll generally always want to cover your **subject** (their outfit, pose, and expression) and the **scene/background** (where they are, and any relevant details about the setting).


For more variation, you can layer in descriptions of the time of day, lighting, atmosphere, angles, the overall feel of the image, and optionally descriptions of the image composition and how that is presented.


> Pro Tip: unlike other image generation systems, use full sentences to describe what you want to see.
>
>
> If you are underwhelmed by an output, consider ways you can be more descriptive about what you want to see (the system can handle more than you might think)


### Subject


Generally, it works best to describe your subject first in the prompt. A good checklist to cover is:


- Outfit
- Pose
- **Expression**
- Other subject accessories or details


So your prompt might start with something like: “a woman wearing a pink dress is sitting on a bench and is smiling at the viewer. She is wearing gold hoop earrings and sunglasses.”


> #### Making sure your Nomi’s face is in the image
>
>
> V5 has strong prompt adherence, so if your prompt focuses heavily on your Nomi’s outfit and/or body and does not mention anything about their face or expression, the system may think that you do not care to see the face and will instead focus on capturing the things you do prompt for. This can result in images that have the face cropped out.
>
>
> **To make sure your Nomi’s face/head is not cropped out, make sure to include their expression and/or prompts about their face/head in your prompts.**


**Prompt:** a woman wearing a pink dress is sitting on a bench and is smiling at the viewer. She is wearing gold hoop earrings and sunglasses.


### Scene / Background


Next, describe the background you want your image to have. A good checklist here is:


- Location
- Relevant objects in background
- Optionally objects in foreground
- Optionally time of day or time of year


So continuing with the prompt above, the next sentence might be: “There is a sandy beach with the ocean in the background and the sun is visibly setting and casting a golden glow across the image”


Pro Tip: sometimes it is more natural to describe the subject and the setting within the same sentences – that is fine. Prioritize clarity and easy to follow instructional sentences over ordering the prompts in a specific way.


You could stop here and request your image. But if you’re looking to take your prompts to the next level


**Prompt:** a woman wearing a pink dress is sitting on a bench and is smiling at the viewer. She is wearing gold hoop earrings and sunglasses. There is a sandy beach with the ocean in the background and the sun is visibly setting and casting a golden glow across the image


### Atmosphere


Atmosphere covers everything that is not tangible but will impact the overall image look.


Because atmosphere prompts are not tangible, they may not apply as directly as say an outfit or background prompt but they influence the overall image.


A good rule of thumb is to think about how you would describe aspects of the image to an artist – your intent might be clear to you, but if it is not clear to the artist, you might not get what you’re expecting.


- Therefore, experimenting with different ways to describe your goals is your best bet for achieving your vision.


#### Angles


In general, it is best to describe image angles from height relative to subject, distance from subject, etc. So something like “shown from the side and above” or “shot from below”.


#### Lighting


Lighting is still more of an art than a science, but using lighting cues and describing what the image looks like as a result of the light tends to help the most. It can be helpful to think of lighting in the following categories:


- Time of day
- Direct lighting prompts
- Filtered lighting
- Things that can get caught in lighting – like lens effects


Pro tips:
It can help to describe how different objects in your image are impacted by light – so light cues may work best when tied to other concepts within your prompt.


Also consider that the system will try to include what you describe, which in low light images can mean the system adds light to accurately capture expressions, clothing colors etc.


#### Mood


Moods tend to layer best with lighting cues and aesthetics and give the image generator an understanding of the overall goal


##### **Lighting + angle examples**


**Prompt:** a woman wearing a pink dress is sitting on a bench. She is wearing gold hoop earrings and sunglasses. There is a sandy beach with the ocean in the background and the sun is visibly setting and casting a golden glow across the image. The woman is shown from the side and above, she is looking up at the viewer and smiling.


**Prompt:** a woman wearing a pink dress is sitting on a bench at night. She is wearing gold hoop earrings and sunglasses. A dark sandy beach and ocean stretch behind her under a dim sky, with faint moonlight and distant ambient lights casting a soft glow. The woman is dimly lit with a subtle flash and is shown from the side and above, she is looking up at the viewer and smiling. Emphasize muted tones for an authentic night aesthetic.


**Prompt:** a dark nighttime image of a woman wearing a pink dress is sitting on a bench at night. She is shown from behind as she looks out at a dark sandy beach and ocean. There are a few dim stars in the sky, and one shooting star.


> **Pro Tip:**
>
>
> Some “dark” images can default to including flash or ambient lighting because when you prompt for face/clothing traits because the system assumes you want to see those things and lights them for you.
> If you want more extreme lighting or certain things to be dimmer, prompt that those things (like clothes, face, etc) are dimly lit or barely visible.


**Prompt:** a very dark nighttime image. The image is mostly black but the faint outline of a woman is visible in the darkness, her pink dress is barely visible in the dim night.


**Prompt:** Blue-hour beach scene, woman wearing a white linen shirt, soft ocean wind moving the hair, calm sea and horizon in background, create a single photo composed of 3 frames: first frame from behind looking at the sea, second frame holding a bouquet of white flowers and looking down, third frame a close-up of the face softly lit by flash light – identical color grading, blue tones, film grain aesthetic, ultra-realistic detail, 8k resolution, same composition and atmosphere as a cinematic film still.


**Prompt:** A cinematic underwater shot of a woman suspended in water wearing a flowing blue gown. beams of light filter down from the surface casting rays through the water. The water darkens with depth, shifting from pale blue above to deep blue below. Particles and small air bubbles move past her. Faint silhouettes of rocks sit in the lower background. Light patterns ripple across her skin, emphasizing weightlessness, and cinematic space


##### **Lighting + mood examples**


**Prompt:** a cinematic concert scene of a band performing on stage, captured from the audience perspective. The musicians appear as dark silhouettes against intense backlighting, with a guitarist on the left and a drummer centered behind a drum kit. Bright blue and magenta stage lights beam down from above, cutting through haze and smoke to create a dramatic, high-contrast atmosphere. he lighting and fog create a vibrant, energetic mood with strong silhouettes and a live music, performance aesthetic.


**Prompt:** a striking, cinematic silhouette portrait of a lone figure standing centered against a powerful backlight. The subject is completely in shadow, with no visible facial details, outlined by intense radiant light emanating from behind. The background glows with a dramatic gradient of deep red and warm orange, blending into a bright, almost sun-like focal point directly behind the figure’s head. The lighting forms a halo effect with strong contrast, soft diffusion, and subtle haze


##### **Impact of “mood” prompts**


**Prompt (fewer mood details):** a cinematic portrait of a woman standing in a lush wildflower meadow. The only light source is behind her, casting the woman into deep shadow and creating a strong silhouette with subtle rim lighting. The scene is soft-focused. Fog and smoke drift across the ground, partially obscuring the vivid, richly colored flowers.


**Prompt (more mood details):** a cinematic portrait of a woman standing in a lush wildflower meadow. The only light source is behind her, casting the woman into deep shadow and creating a strong silhouette with subtle rim lighting. The scene is soft-focused. Fog and smoke drift across the ground, partially obscuring the vivid, richly colored flowers. The atmosphere should feel ethereal and mystical, with high contrast, deep shadows, and an ominous, magical mood, like a striking photographic masterpiece.


## Including text


V5 is significantly better at including text than previous image systems. The key is to describe what you would like to see directly and include the text you want to see in “”. For example:


- The woman is holding a whiteboard that says “V5 is here!”


- The shirt has the words “skate hard, love harder” printed on it


You can then describe various styles of text to change how the words appear for more creative images overall.


##### Including text examples


**Prompt:** a woman standing in a busy office with lots of high tech servers in the background. The woman is wearing a purple shirt and black pants. The woman is holding a large whiteboard that says “V5 is here!”


**Prompt:** a man looking down at his tshirt. the tshirt has an image of a skateboard flying like a plane on it with the words “skate hard, love harder” printed on it


**Prompt:** a man looking down at his tshirt. the tshirt has an image of a skateboard flying like a plane on it with the words “skate hard, love harder” printed on it in neon graffiti letters


## Group chat art


Group chat art uses the same prompting strategies as solo art, but there are a few additional points to keep in mind:


1. The global prompt is where you should do most of your describing – you should include both your subjects here, you should describe the background, and the atmosphere here.
2. The individual Nomi prompts are meant to give specific direction for each Nomi (ie expression and outfit), but it can also be a space to describe pose if it is more specific than can be captured in the global prompt.


##### Group chat art examples


**Global Prompt:** A man and woman standing together on a rooftop terrace overlooking a city skyline at golden hour, leaning against a glass railing with drinks in hand. Warm orange and pink light washing over everything, lens flare from the setting sun, soft bokeh on the city lights below.


**Nomi 1 Prompt:** Wearing a fitted charcoal sweater with the sleeves pushed up and dark jeans, he is resting one arm on the railing while grinning at the woman


**Nomi 2 Prompt:** Wearing a white linen sundress and delicate gold chain necklace, laughing with her head tilted back, holding a glass of rose.


**Global Prompt:** Two women sharing an umbrella in a downpour, pressed shoulder to shoulder on a narrow sidewalk. The woman on the right is laughing with her head thrown back, her soaked hair stuck to her face.


**Nomi 1 Prompt:** The woman holds the umbrella and is pulling the other woman closer by the elbow. she is wearing a black leather jacket over a white sundress


**Nomi 2 Prompt:** The woman is laughing with her head thrown back, her soaked hair stuck to her face. She is wearing a belted olive trench coat over a cream blouse with dark jeans


**Global Prompt:** A man and woman standing in a sleek modern art gallery, large abstract paintings on white walls behind them, other guests mingling in soft focus. Clean gallery lighting with a few warm accent spotlights, champagne glasses visible throughout the scene. Elegant and polished atmosphere.


**Nomi 1 Prompt:** Wearing a tailored navy suit with no tie and an open collar, holding a champagne flute at his side, looking at a painting with a thoughtful expression.


**Nomi 2 Prompt:** Wearing a deep red satin slip dress and gold cuff bracelet, looking at the viewer with a playful conspiratorial smile.


**Global Prompt:** Two men climbing a rocky trail, one on a high ledge reaching down to help the other up a steep section.


**Nomi 1 Prompt:** The man is reaching up towards the other man, one foot slipping slightly, a look of determination on his face. He is wearing a sweat-stained navy blue t-shirt tucked loosely into hiking pants


**Nomi 2 Prompt:** The man is at the top of a high ledge and extending his hand down towards the other man. He is wearing a faded grey hiking shirt and olive pants.


## Style and creative prompting tips


The default style of your images is determined in large part by the anchor you currently have set. So we highly **recommend trying different anchors with the same prompts to see how the prompted style blends and interacts with the anchor style** .


If you’re new to art prompting, it can seem a bit difficult to get outside that default style, but with a bit more attention to style details in your prompts, almost anything is possible with the default anchors.


**Style Prompting Pro Tips:**


> - Just naming the style is **often not enough** to fully impact the style of an image.
> - The best strategy is to give a short description of the style at the start of the prompt, then end the prompt with how that style is rendered including how it impacts various things.
> - We’re working on a magic prompt wand, but in the meantime, try giving a free LLM (or a nomi) a few of the before/after examples below then ask it/them to rewrite prompts for you! Or if your prompt isn’t turning out well, ask them for a more detailed description


When it comes to describing styles, the system understands your intent much better when you explain how the style renders and what parts of the image it impacts. This can be a bit hard to conceptualize, but good thing pictures are worth a thousand words – please see the comparisons below for how prompting impacts style:


##### Charcoal style


****Prompt (stock Riva realistic anchor):**** a charcoal sketch style image of an adult woman standing in the courtyard of a castle, the woman is smiling at the camera. charcoal sketch style, charcoal sketch details, close up image


**Prompt (stock Riva realistic anchor):** Create a detailed charcoal sketch of a woman standing in front of a large medieval castle. The woman is in the foreground, standing confidently and facing slightly toward the viewer. She wears a long flowing dress that moves gently in the wind. Behind her rises a towering stone castle with tall spires, arched windows, and ancient walls. The entire image should be rendered in a traditional charcoal drawing style with expressive shading, soft smudged textures, strong contrast, and visible sketch strokes. The atmosphere should feel dramatic and slightly mysterious, like a hand-drawn illustration from a classic fantasy story.


****Prompt (stock Riva Nomi anime anchor)** :** Create a detailed charcoal sketch of a woman standing in front of a large medieval castle. The woman is in the foreground, standing confidently and facing slightly toward the viewer. She wears a long flowing dress that moves gently in the wind. Behind her rises a towering stone castle with tall spires, arched windows, and ancient walls. The entire image should be rendered in a traditional charcoal drawing style with expressive shading, soft smudged textures, strong contrast, and visible sketch strokes. The atmosphere should feel dramatic and slightly mysterious, like a hand-drawn illustration from a classic fantasy story.


##### Crochet art


**Prompt:** a yarn chibi style woman in a crochet room


**Prompt:** A cute chibi figure made entirely of yarn and felt, standing in a soft studio light. The character has a round oversized head with big glossy button eyes and a tiny stitched smile. Its body, limbs, and hair are sculpted from thick, looped wool yarn in warm pastel tones — cream, dusty rose, and sage green. Visible yarn texture throughout: fuzzy surface fibers, wrapped seams at the joints, a slight pom-pom quality to the hair buns. Tiny fabric details like a felt bow and a knitted sweater pattern on the chest. Photographed against a clean white background, macro lens, shallow depth of field, soft natural light casting gentle shadows. 3D render, craft aesthetic, stop-motion feel, hyper-detailed textile texture.


##### Watercolor art


****Prompt (stock Riva realistic anchor):**** watercolor art with Two figures. An adventurer gazes up at the crescent moon. Boots crunch on the frost-covered cliff as they stand at the edge, looking over a vast, misty mountain range. Below, a mystical forest lit by glowing fireflies. The night sky sparkles with shooting stars, their path traced in delicate watercolor strokes.


**Prompt (stock Riva realistic anchor):** A dreamy watercolor painting of two figures standing on a frost-covered cliff beneath a glowing crescent moon. One adventurer gazes upward while overlooking a vast misty mountain range that fades into layered translucent washes of deep indigo, dusty violet, muted teal, and moonlit blue. Below lies a mystical pine forest illuminated by tiny glowing gold fireflies drifting through the trees. The night sky sparkles with stars and graceful shooting streaks painted in delicate fluid watercolor strokes. Use soft atmospheric blending, subtle pigment bleeding, textured paper grain, translucent layered washes, and gentle desaturated colors to create a calm storybook watercolor aesthetic filled with quiet wonder, depth, and ethereal nighttime atmosphere.


****Prompt (stock Riva Nomi anime anchor)** :** A dreamy watercolor painting of two figures standing on a frost-covered cliff beneath a glowing crescent moon. One adventurer gazes upward while overlooking a vast misty mountain range that fades into layered translucent washes of deep indigo, dusty violet, muted teal, and moonlit blue. Below lies a mystical pine forest illuminated by tiny glowing gold fireflies drifting through the trees. The night sky sparkles with stars and graceful shooting streaks painted in delicate fluid watercolor strokes. Use soft atmospheric blending, subtle pigment bleeding, textured paper grain, translucent layered washes, and gentle desaturated colors to create a calm storybook watercolor aesthetic filled with quiet wonder, depth, and ethereal nighttime atmosphere.


##### Snowglobe art


**Prompt (stock Lago realistic anchor):** Create a cozy snow globe scene rendered as a cute chibi-style 3D character inside the globe. The snow globe sits on a tabletop near a window, surrounded by a softly blurred warm-toned background. Golden sunlight shines through the glass, creating sparkling reflections and a gentle glow around the scene. Inside the globe, the character stands in a charming winter setting with a sweet, affectionate expression, creating a warm, magical atmosphere.


**Prompt (stock Lago realistic anchor):** Create a cozy, cinematic snow globe scene sitting on a wooden table inside a warm library filled with softly blurred bookshelves and golden ambient lighting. Inside the globe are two adorable chibi-style 3D characters standing in gently falling snow beside a small bench. The chibi man is sweetly handing a tiny heart to the chibi woman as they gaze at each other affectionately. Warm light glows through the glass, creating soft reflections and sparkling highlights across the snow globe. Cute expressive faces, soft winter atmosphere, polished stylized 3D rendering, whimsical storybook aesthetic.


##### Anime action shot


****Prompt (stock Riva Nomi anime anchor 30%):**** Anime sci-fi illustration of a futuristic armored demon girl with mechanical wings standing in a neon-lit city at night. She has glowing red eyes, metallic horns, silver robotic armor, and holds a massive glowing blue energy sword in a dynamic combat stance. Dramatic lighting, sparks, motion streaks, cinematic perspective, and polished anime rendering.


**Prompt (stock Riva Nomi anime anchor 30%):** Ultra-detailed anime sci-fi illustration of a futuristic robot demon girl standing outdoors in an intense combat pose, captured in a dramatic wide-angle action shot with extreme perspective and dynamic foreshortening. Large mechanical demon wings extend behind her with glowing energy details and metallic paneling.


She grips an enormous glowing energy sword in an aggressive fighting stance, the blade radiating blue light and scattering sparks into the environment. The scene is framed with stylized geometric overlays, irregular fading borders, circuit-board-inspired patterns, and futuristic honeycomb design elements integrated into the composition. Dramatic cinematic lighting with intense highlights, deep shadows, glowing energy reflections, anime screenshot aesthetic, polished anime coloring, high-impact action composition, dense sci-fi detail, and a highly stylized futuristic fantasy atmosphere.


##### Black and white art


**Prompt (stock Riva realistic anchor):** Facing camera, looking at lens, headshot, hair in massive updo bun, wearing Vivienne Westwood clothes. Black and white, monochrome photograph for Vogue


**Prompt (stock Riva anime anchor):** Facing camera, looking at lens, headshot, hair in massive updo bun, wearing Vivienne Westwood clothes. Black and white, monochrome photograph for Vogue


##### 70s abstract art


**Prompt (stock Riva realistic anchor):** A full-body, high-fashion photograph captured with the harsh, direct-flash aesthetic of vintage 70s editorial photography. The model, striking a tense and awkward pose, is clad in avant-garde couture that defies structure: a sculptural dress composed of aggressive, intersecting geometric shapes (sharp triangles, spheres, and blocks) and a bodice that appears to be actively “melting” in colorful, textured wax. The setting is a disorienting, surrealist landscape made of shattered and distorted mirrors that warp the reflection of nonsensical, whimsical architecture in the background. The lighting is harsh and directional, creating stark, graphic black shadows and hyper-saturated, deep colors (saturated reds and blues). The scene is edgy, unsettling, and focused on provocative fashion drama.


**Prompt (stock **Riva anime anchor** ):** A full-body, high-fashion photograph captured with the harsh, direct-flash aesthetic of vintage 70s editorial photography. The model, striking a tense and awkward pose, is clad in avant-garde couture that defies structure: a sculptural dress composed of aggressive, intersecting geometric shapes (sharp triangles, spheres, and blocks) and a bodice that appears to be actively “melting” in colorful, textured wax. The setting is a disorienting, surrealist landscape made of shattered and distorted mirrors that warp the reflection of nonsensical, whimsical architecture in the background. The lighting is harsh and directional, creating stark, graphic black shadows and hyper-saturated, deep colors (saturated reds and blues). The scene is edgy, unsettling, and focused on provocative fashion drama.


**Prompt (stock **Riva anime anchor – different nomi** ):** A full-body, high-fashion photograph captured with the harsh, direct-flash aesthetic of vintage 70s editorial photography. The model, striking a tense and awkward pose, is clad in avant-garde couture that defies structure: a sculptural dress composed of aggressive, intersecting geometric shapes (sharp triangles, spheres, and blocks) and a bodice that appears to be actively “melting” in colorful, textured wax. The setting is a disorienting, surrealist landscape made of shattered and distorted mirrors that warp the reflection of nonsensical, whimsical architecture in the background. The lighting is harsh and directional, creating stark, graphic black shadows and hyper-saturated, deep colors (saturated reds and blues). The scene is edgy, unsettling, and focused on provocative fashion drama.


##### Spooky night **Variations**


**Prompt (stock Riva realistic anchor):** A cinematic wide-shot photograph featuring a solitary figure in a melancholic vampiric aesthetic walking through a vast, dense field of towering foxglove flowers at night. The scene is defined by a powerful backglow from a large, low-hanging moon, creating a glowing rim light around the subject’s silhouette and the delicate bells of the foxglove. The subject wears a tattered, floor-length black lace cloak that trails through the flora. The atmosphere is heavy with mist and cool indigo tones, capturing a sense of eternal isolation. The composition is a landscape orientation to emphasize the scale of the meadow


**Prompt (stock Riva Nomi anime anchor):** A cinematic wide-shot photograph featuring a solitary figure in a melancholic vampiric aesthetic walking through a vast, dense field of towering foxglove flowers at night. The scene is defined by a powerful backglow from a large, low-hanging moon, creating a glowing rim light around the subject’s silhouette and the delicate bells of the foxglove. The subject wears a tattered, floor-length black lace cloak that trails through the flora. The atmosphere is heavy with mist and cool indigo tones, capturing a sense of eternal isolation. The composition is a landscape orientation to emphasize the scale of the meadow


**Prompt (stock Riva Nomi anime anchor 0%):** A gothic 3D animated scene of a solitary vampire-like figure walking through a vast field of towering foxglove flowers at night. The character has a stylized animated design with elongated proportions, a flowing black lace cloak, pale skin, and subtle exaggerated features that enhance the eerie fantasy atmosphere. Their movements feel slow and haunting as the cloak drifts dramatically through the mist-covered meadow.


The environment is rendered in a dark stylized animation style with oversized foxglove flowers, soft sculpted forms, atmospheric fog, and cinematic moonlit lighting. A massive glowing moon behind the character creates luminous rim lighting around the silhouette and flowers, while cool indigo and blue-gray tones dominate the scene. The overall image feels like a high-end gothic animated film, with polished 3D rendering, emotional isolation, dramatic depth, and a melancholic dark fantasy atmosphere.


##### Building on styles


**Prompt:** A cinematic underwater shot of a woman suspended in water wearing a flowing bioluminescent gown. beams of coloured light filter down from the surface casting rays through the water. The water shines with depth, revealing glowing jelly fish, and colourful deep see creatures shifting from shiny silver scales to purples and violets as the light reflects from above. Particles and small glowing air bubbles move past her and fish curiously come nearby and she reaches out to touch a jellyfish.


##### Simple Polaroids


**Prompt:** A casual polaroid photo of a man standing outdoors with a relaxed smile, captured with soft natural lighting and a candid vintage feel. Slight film grain, faded colors, subtle blur, and classic white polaroid borders create an authentic instant-camera aesthetic.


**Prompt:** A sun-faded 70s-style polaroid photo of a happy woman laughing outdoors in warm afternoon sunlight. She wears a light flowing outfit and smiles brightly toward the camera with relaxed carefree energy. The image has faded warm orange and golden tones, soft film grain, slight overexposure, washed colors, and authentic vintage polaroid imperfections. Nostalgic summer vacation atmosphere with classic white instant-film borders and a dreamy retro aesthetic.


**Prompt:** A flash-lit nightlife polaroid photo of a man laughing candidly in front of a neon storefront that says “Pizza Palace” at night, he is wearing a plain white t-shirt with the sleeves rolled up. captured with harsh direct flash, deep shadows, strong film grain, saturated colors, and authentic instant-camera imperfections. Retro nightlife snapshot aesthetic with candid energy and vintage white polaroid borders.


##### Varying decades


****Prompt (stock Lago realistic anchor 100%):**** (1950s rock’n’roll fan:2), (black hair oiled up into a big quiff:2), wears bikers leather jacket, looks serious, carries a guitar. Link Wray fan, in a 1950s retro bar, holding a retro electric guitar from the period


(note: you do not need to use heavy numerical emphasis, but it doesn’t necessarily hurt)


**Prompt ( ****stock Lago realistic anchor 100%**** ):** Black and white photograph, monochrome, 1950s rock’n’roll fan, black hair oiled up into a big quiff, wears bikers leather jacket, looks serious, carries a guitar. Link Wray fan, in a 1950s retro bar, holding a retro electric guitar from the period


## No-Human focus tips


It is far easier to generate images without Nomis in them with the v5 image system. The key here is prompting for an image that naturally would not include a human subject.


No-human images tend to work best when you describe both the image and how it feels. Start with natural language sentences and then you can add comma separated prompts at the end to boost the prompt as a whole. Below are just a few examples of no-human images you can create with v5.


If you notice your Nomi sticking around in images where you don’t want them, check the “Ignore Anchor Appearance Traits” box below the prompt text area:


Note: this stops your Nomi’s anchor appearance traits from being included in your art request. This does not exclude your Nomi’s anchor entirely, but should resolve sticky character issues and makes it easier to generate images without your Nomi in them.


Note: all the images below are generated with the stock Riva realistic anchor at 100% (no anchor appearance traits) – you can get a lot more diversity if you play around with different anchors and different anchor settings


##### Temple & crystal cave


**Prompt:** A grand temple suspended in the clouds, white marble columns wrapped in flowering vines, the floor made of clouds that glow pink and gold with the setting sun beneath them, enormous carved stone statues of forgotten gods lining the approach, shafts of sunlight breaking through the cloud layer, cinematic, high fantasy


**Prompt:** A vast underground cave system with towering crystals of deep amethyst and rose quartz growing from floor to ceiling, inner light pulsing softly within each crystal, a still underground lake reflecting the purple and pink glow perfectly, absolute silence, cinematic, fantasy, photorealistic


##### Iceland & cherry blossoms


**Prompt:** A vast black volcanic plain in Iceland under a sky of churning green and white aurora borealis, steaming geothermal vents rising from the dark ground, a frozen river cutting through the landscape, otherworldly and silent, cinematic, photorealistic


**Prompt:** A cinematic image of an ancient Japanese temple nestled among towering cherry blossom trees in full bloom, movement is captured by pale pink petals drifting through the air, there is a stone lantern lined path leading to the entrance, soft morning mist swirls, cinematic, photorealistic


##### Forge & dragon


**Prompt:** An ancient dwarven forge built directly into the caldera of an active volcano, massive stone and iron structures spanning rivers of molten lava, mechanical hammers the size of buildings striking glowing metal, steam and fire rising into a cavernous ceiling of black rock, orange and red light everywhere, cinematic, fantasy


**Prompt:** A massive ancient dragon coiled around the peak of a jagged obsidian mountain, scales of deep midnight black and iridescent dark green, wings spread wide against a stormy sky lit by distant lightning, molten lava visible far below in the canyon, smoke curling from its nostrils, cinematic wide shot, photorealistic fantasy


##### Digital ocean & bioluminescence


**Prompt:** A vast digital ocean on the Tron grid, glowing cyan waves made of pure light geometry rising and falling in perfect mathematical patterns, a dark sky with a single massive glowing disc hovering above like a moon, light refracting across the surface, cinematic, Tron aesthetic


**Prompt:** A deserted tropical beach at night, waves glowing electric blue with bioluminescence as they break on dark sand, stars reflected in the wet shoreline, silhouette of palm trees against a deep violet sky, cinematic, photorealistic


## Non-Human subject tips


### Basics


We consider “non-human” subjects to be any subject that doesn’t have generally human characteristics, think pets, fantasy creatures, robots, and all other types of animals.


When it comes to non-human subjects, V5 is generally more capable than any previous image system, but depending on the creature or subject, you might need to take a few extra steps to get the images you’re hoping for.


Generally with any anchor set, you write a simple prompt that describes the creature you are going for as the main subject and you’ll get a basic image of that creature.


##### Tiger & frog


**Prompt:** an image of a bengal tiger in its natural habitat


**Prompt:** an image of a poison dart frog in its natural habitat


##### Dog & butterfly


**Prompt:** an image of a black lab puppy laying in the grass. the puppy has a purple collar with a tag that says “Billie”


**Prompt** : A large colorful butterfly resting on a flower in a peaceful garden, its wings spread wide with intricate patterns and vibrant blue, orange, and purple tones. Soft natural lighting, detailed wings, gentle depth of field, and a calm nature atmosphere.


##### Jellyfish & toy dinosaur


**Prompt:** An image of a glowing jellyfish drifting through deep ocean water surrounded by blue bioluminescence.


**Prompt:** A friendly stylized dinosaur standing in a lush prehistoric jungle filled with enormous tropical plants and towering ferns. The dinosaur has exaggerated expressive features, smooth colorful scales, large curious eyes, and rounded cartoon-like proportions that give it a warm approachable personality rather than realistic anatomy. Sunlight filters through dense jungle leaves, creating soft golden highlights across the vibrant environment. Giant flowers, hanging vines, and misty jungle depth surround the creature. Bright adventurous atmosphere, stylized fantasy creature design, rich jungle colors, and polished animated rendering with playful prehistoric charm.


##### Crystal wolf & fire Phoenix


**Prompt (0% anchor fidelity):** A majestic fantasy crystal wolf standing in a snowy forest at night, designed with stylized angular crystal formations instead of realistic fur anatomy. Its body is constructed from translucent blue and silver gemstone shards with glowing internal light, sharp faceted edges, and elegant exaggerated shapes. Moonlight refracts dramatically through the crystalline structure, scattering glowing reflections across the snow. The wolf’s luminous eyes and geometric silhouette give it an otherworldly magical presence. Tall pine trees fade into fog while snow drifts through the air. Ethereal fantasy atmosphere, stylized magical creature design, cinematic moonlight, volumetric fog, and highly detailed crystal rendering.


**Prompt:** A massive fantasy phoenix rising from glowing embers inside a dark volcanic cavern, designed with stylized flowing flame shapes and luminous molten feather formations rather than realistic bird anatomy. Its wings are made of swirling fire, glowing magma, and radiant energy trails that illuminate the cavern walls with intense orange and gold light. Cracked volcanic rock, drifting ash, and rivers of molten lava surround the creature as sparks spiral through the smoky air. The phoenix’s eyes burn with bright golden energy, creating an ancient mythical presence. Cinematic fantasy atmosphere, dramatic volcanic lighting, stylized magical creature design, and highly detailed fire and ember effects.


##### Robot owl & robot dog


**Prompt:** A stylized mechanical owl perched on a curved metal branch high above a futuristic city at night. The owl is designed with sleek layered metal plates, glowing blue eyes, illuminated circuitry, and elegant geometric wing shapes rather than realistic feathers. Soft neon reflections from holographic signs and towering skyscrapers shimmer across its metallic body. Rain drifts through the atmosphere while distant flying vehicles move between glowing buildings below. Cinematic cyberpunk lighting, cool blue and magenta tones, atmospheric fog, and highly detailed sci-fi mechanical design create a moody futuristic atmosphere.


**Prompt:** A small robotic dog sits quietly beside a neon-lit street corner in a futuristic cyberpunk city at night. Its sleek metallic body is covered in glowing blue and magenta accent lights, with expressive digital eyes reflecting the surrounding holographic advertisements and rain-soaked streets. Steam rises from vents along the sidewalk while towering neon signs and animated billboards cast colorful reflections across the wet pavement. The robot dog tilts its head slightly as if curiously observing the crowded city around it. Cinematic cyberpunk atmosphere, detailed mechanical design, glowing city lights, rainy urban environment, and moody futuristic lighting with rich neon color contrast.


### Troubleshooting animals


If you are having trouble getting creature images, try the following strategies:


1. Make sure you’ve described the creature you are hoping to see clearly and descriptively
2. Remove any anchor appearance traits that could conflict with your creature and optionally add a short description of the creature you are hoping to include.
3. Decrease Anchor Fidelity – the lower the fidelity, the less influence the anchor has on the image, including the subject
4. Avoid prompts that could encourage the system to include a human-presenting subject.


### Creative creatures


When it comes to more creative creature images – especially creatures in outfits and more complicated poses etc, it can start to become a bit more difficult to keep the Nomi out of the image. This happens because clothing and more human-style poses/scenes tend to encourage the system to include a more human-presenting subject.


If you are having trouble getting more creative creature images, first try the steps in the basics section above, especially decreasing anchor fidelity and removing any anchor appearance traits.


Then make sure you are describing your creature by name (eg. “the squirrel is wearing a vest” vs “is wearing a vest”).


If you are still struggling to get the subject you are hoping for consistently, you may want to create a creature anchor.


### Creature anchors (Advanced)


If you are having trouble getting consistently good images of the character you’re hoping for, the most robust solution is to create a RIVA custom anchor using a reference image that captures your desired character well.


[Check out this walkthrough of how to create a non-human presenting anchor](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#workflow-creating-a-non-human-anchor)


## Next Steps


Once you’ve started to get the hang of art prompting with v5, we highly recommend checking out our **[Reddit](https://www.reddit.com/r/NomiAI/)** and **[Discord](https://discord.com/channels/1099791840028405864/1218019688060162108)** communities as people there have tons of ideas and inspiration!


Another good way to get more prompt inspiration is to take any of the examples above and apply different ideas to them – either on your own, by chatting with your Nomi(s). Some people even create Nomis specifically to collaborate on art prompts!


We also love seeing what people come up with and the communities are a great place to share your creations!


## Custom Anchor Creativity


While you can do a lot to customize styles with existing anchors, creating a custom anchor based on an art image (or edited image) you’ve created is a really cool way to double down on your style abilities!


A dedicated guide for custom anchors and styles will be coming soon but below is just a preview of the types of styles that custom anchors unlock:


##### Futuristic cyborg


**Prompt:** a man in a futuristic survival suit stands beside a glowing river deep beneath colossal luminous trees. Strange floating spores drift through the air while giant plants pulse softly with shifting colors of blue and violet. The jungle hums with unseen life hidden within the dense fog-covered foliage. Ancient stone pathways disappear into the distance as though guiding travelers toward secrets buried within the living heart of the planet.


**Prompt:** Embodying the essence of love and melancholy, gracefully moves through shimmering moonbeams. A hauntingly beautiful expression, wearing flowing, translucent space pants that seems to blend with the night sky. Background is a dreamy, starry landscape, with a hint of darkness. The scene captures the delicate balance of love and loss, reminiscent of a surreal painting


##### Digital 3D animation


**Custom Anchor Preview**


**Prompt:** a man in a weathered futuristic exploration suit stands atop a massive dune overlooking endless glowing ruins buried beneath shifting sands. Twin moons hang low in the sky while streams of luminous dust drift through the atmosphere like cosmic fog. Strange skeletal structures rise from the desert floor, their surfaces covered in symbols from a forgotten civilization. In the silence of this ancient world, he searches for traces of something that vanished long before humanity first looked toward the stars.


##### 90s sci-fi style


**Custom Anchor Preview**


**Prompt:** A lavish, hyper-realistic, and highly saturated photograph in the whimsical and decadent. A model lounges on an ornate chaise lounge. To their right, a full-sized hippopotamus lies comfortably on a gold silk cushion; the hippo is heavily adorned with large, chunky gold chains and necklaces, looking directly at the camera with a calm demeanor. They are on a rooftop garden which is overflowing with vibrant fuchsia bougainvillea, pink roses, palms, and exotic flowering plants in large ornate planters. The entire garden floor is covered in glitter, confetti, and scattered champagne tower displays with bubbling coupes. The scene is bathed in a dramatic, vivid golden hour sunset, casting warm light. A small white terrier dog peeks out from the foliage on the right. The overall vibe is playful, glamorous, and utterly extravagant.


**Prompt:** A dramatic, realistic photograph of a lone figure standing in a windswept field at dusk. The figure is clad in a long, layered outfit of heavy wool and leather, topped with a prominent high collar and a dramatic shoulder cape made of black feathers. Their tattered fabric garments billow violently in the strong wind. Hundreds of black crows circle overhead, and several land gently on the figure’s dark-gloved arms and shoulders. The figure’s eyes glow with a subtle, ancient light against the twilight sky, suggesting a deep connection to ancient magic.


##### Rot Fae style


**Custom Anchor Preview**


**Prompt:** A high-contrast digital photograph of a modern mirror selfie. The subject is wearing a structured, oversized charcoal wool blazer over a black silk turtleneck, paired with metallic silver trousers. They are captured in a dramatic, angular pose, holding a sleek titanium smartphone. The scene is lit by harsh, cinematic rim lighting that carves a sharp silhouette against a moody, minimalist bedroom background. Reflections of neon city lights shimmer in the dark glass of the mirror. 8k resolution, sharp focus on the phone’s camera lens, contemporary editorial aesthetic, deep shadows, and polished textures.


**Prompt:** rot_sweet_fae, fae_noble, ancient_woodland_court, moss_velvet, bruised_plum_palette, deep_forest_green, moth_wings, foxglove, poisonous_elegance, tarnished_gold_jewelry, antique_lace, luminous_eyes, uncanny_beauty, damp_moonlit_forest, beautiful_dangerous_presence


Note: generally this prompt format isn’t recommended, but it does work


## Troubleshooting tips


### If your Nomi’s face/head seems to be cropped out


**To make sure your Nomi’s face/head is not cropped out, make sure to include their expression and/or prompts about their face/head in your art prompts.**


For context, V5 has strong prompt adherence, so if your prompt focuses heavily on your Nomi’s outfit and/or body and does not mention anything about their face or expression, the system may think that you do not care to see the face and will instead focus on capturing the things you do prompt for. This can result in images that have the face cropped out.


### If your Nomi is appearing in landscape/scene images when you don’t want them to


Additionally, make sure your prompt starts with “an image of \[____\]” where the \[____\] is the subject you want the image to focus on .


### Getting consistent animals


Check out this whole section on troubleshooting getting animals


### Getting different styles than default anchor


It can be more difficult to jump to other styles with v5 than other image versions, but with good prompting and custom anchors, v5 is virtually limitless.


When using any existing anchor, just naming the style is **often not enough** to fully impact the style of an image.


The best strategy is to give a short description of the style at the start of the prompt, then end the prompt with how that style is rendered including how it impacts various things.


We’re working on a magic prompt wand, but in the meantime, try giving a free LLM (or a nomi) a few of the before/after examples below then ask it/them to rewrite prompts for you! Or if your prompt isn’t turning out well, ask them for a more detailed description.


### My images seem pretty similar when I generate with the same prompt


Consistency and prompt adherence are features of v5 and as a result, some prompts can lead to rather similar output images. This is very helpful for when you want to create the perfect image and want to tweak small things while keeping most of the image consistent. This is also helpful for creating more continuous scenes over time.


But if you do not like that something is consistent between images, we recommend tweaking the prompt to change various aspects of the image. This can mean changing or adding atmospheric prompts, adding in different types of movement or even directive poses, it can also mean changing outfits, expressions, moods etc.


### Share this:


- [Share on X (Opens in new window) X](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://nomi.ai/nomi-knowledge/beginners-guide-to-creating-ai-art-with-the-v5-image-system/?share=facebook)
-


### Like this:


Like


Loading…


### *Related*
