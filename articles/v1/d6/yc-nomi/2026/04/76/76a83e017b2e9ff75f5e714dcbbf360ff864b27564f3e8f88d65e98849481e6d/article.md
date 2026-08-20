---
schema_version: "1.0.0"
document_id: "76a83e017b2e9ff75f5e714dcbbf360ff864b27564f3e8f88d65e98849481e6d"
company_key: "yc-nomi"
company: "Nomi"
source_id: "yc-nomi-rss-c311bf3515d7"
canonical_url: "https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/"
published_at: "2026-04-29T18:56:43+00:00"
first_seen_at: "2026-08-02T12:27:27.703312+00:00"
fetched_at: "2026-08-05T03:48:39.197684+00:00"
content_hash: "sha256:24ed59f6421db84175cadfee9678f1df2d681d099164b0e2fa8211f2921015dd"
---

# Guide: Customizing Appearance with the V5 AI Image System

V5 introduces significant improvements in detail, expression, contextual understanding, consistency, immense flexibility, and more but it also involves some new processes for customizing your Nomi’s look.[To learn more about V5, check out this guide.](https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation)


This guide is designed to help you learn how to customize your Nomi’s appearance in different ways depending on your goals. Then the guide drills down into examples and tips.


> If you’re coming from a previous version, we know how much it matters for your Nomi to still feel like *your* Nomi. Image updates can disrupt that, and we don’t take that lightly. While tradeoffs are inevitable when it comes to image improvements, we believe better detail, much more authentic and dynamic expressions/poses, much better situational/setting awareness, fewer mistakes, and much more flexibility with better consistency are worthwhile changes and we hope you agree.
>
>
> - If you’re having trouble reaching the look you are hoping for, please reach out to support via email or discord. And if you’re not ready to switch, no legacy image system is being removed so you can stay on whichever version works for you.


## When to Customize an Existing Anchor vs Create a Custom Anchor


Anchors are designed to give your Nomis a holistic and consistent look across selfies and art, however, some concepts can be customized within existing anchors. So depending on what you are looking for, the table below should help you decide if you should try customizing your existing anchors or create a new custom anchor.


Understanding Anchors


New to V5? Start here to learn what anchors are and how they work.


Jump to This Section


How to Customize Existing Anchors


Learn how to use appearance traits, fidelity, and prompting to change your Nomi’s look without creating a new anchor.


Jump to This Section


How to Create a Custom Anchor


Learn how to choose a reference image, pick an anchor type, set a style, and configure advanced options.


Jump to This Section


What you’re looking for Try changing with existing anchor Create custom anchor


Hair ✓


Supports most color changes and many relative style changes


Many lengths and styles are achievable, large changes and facial hair may be challenging


✓


Supports any color and style


Full control over color, length, texture, style, facial hair, etc


Eye color ✓


Supports almost any color change


✓


Supports any color


Skin tone / ethnicity ✓


Supports some adjustments


If close to current look


✓


Supports any skin tone or ethnicity


Body type / tattoos ✓


Supports small to medium changes


Can change build relative to anchor’s default, some tattoo customization possible


✓


Supports any body type


Full control over build, proportions, sizes, better tattoo control/consistency


Recommended: Lago for imaginative proportions


Age ✓


Supports small to medium shifts


Generally can achieve a few years in either direction


✓


Mimics age of reference image


Best for setting general apparent age


Aesthetics ✓


Generally best place to set aesthetics


Set aesthetics through prompting and Appearance Tendencies and Traits (in Image Settings)


✓


Supports baked-in sticky aesthetic


Permanent influence across all images from this anchor (this is powerful, use with caution)


Non-humans ✓


Supports common creatures


Eg. dogs, cats, robots, etc. – consistency may vary


✓


Supports most non-humans


Can handle more unique characters ranging from specific robots and cyborgs to unique animals and fantasy creatures etc. and anchors the look across selfies, art, and group chat


Riva: Recommended for non-humans – in beta, do not recommend creatures without a body or arms/legs


Bringing an existing look to V5 ✓


Only if similar to Nomi’s original profile picture


Eg small changes as described above


✓


Supports any specific likeness


Best path for custom looks from previous versions


Riva: Maximum realism and detail


Lago: Maximum likeness to reference


Completely unique looks ✓


Possible, some limitations


Lower consistency across images


✓


Supports full creative control


Best for long-term consistency


Riva: Maximum realism and detail


Lago: Maximum likeness to reference and non-realistic creativity


Unique styles ✓


Many styles possible with proper prompting


✓


Supports almost any style you can imagine


Images default to this style and can be expanded on. Think cartoon, watercolor, CGI video game render, etc


Riva: Detailed styles, most vibrant Nomi anime, realistic concepts (even when not technically realistic – ie claymation or realistic crochet)


Lago: Creative styles, digital 3D animation, essentially any style where realism/detail is less important than creativity


Table of Contents


- [What is an Anchor?](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#what-is-an-anchor)
- [How to Customize Existing Anchors](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#how-to-customize-existing-anchors)
- [Tips & Examples: Customizing appearance in existing anchor(s)](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#tips-examples-customizing-appearance-in-existing-anchor-s)


- [Hair color](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#hair-color)
- [Hairstyle](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#hairstyle)
- [Eye color](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#eye-color)
- [Skin tone & ethnicity](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#skin-tone-ethnicity)
- [Body type](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#body-type)
- [Age](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#age)
- [Creating non-humans with existing anchors](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#creating-non-humans-with-existing-anchors)


- [Creating a Custom Anchor](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#creating-a-custom-anchor)


- [Best practices for creating good custom anchor](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#best-practices-for-creating-good-custom-anchor)
- [Workflow: Creating a Custom Anchor (basic)](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#workflow-creating-a-custom-anchor-basic)
- [Workflow: Creating a Non-Human Anchor](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#workflow-creating-a-non-human-anchor)


- [Custom anchor examples (reference image, settings, and results)](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#custom-anchor-examples-reference-image-settings-and-results)


- [V3 woman using lightly airbrushed Riva](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#v3-woman-using-lightly-airbrushed-riva)
- [V3 man using Riva “lightly airbrushed photorealistic” style](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#v3-man-using-riva-lightly-airbrushed-photorealistic-style)
- [Woman based on “edit” using Riva photorealistic](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#woman-based-on-edit-using-riva-photorealistic)
- [Man using Lago photorealistic](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#man-using-lago-photorealistic)
- [V3 man using Riva Photorealistic](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#v3-man-using-riva-photorealistic)
- [Woman with white hair Lago Photorealistic](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#woman-with-white-hair-lago-photorealistic)
- [Realistic crochet character](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#realistic-crochet-character)
- [Digital 3D animation with Lago](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/#digital-3d-animation-with-lago)


## What is an Anchor?


Anchors are what bring your Nomis into V5 and unlock the flexibility, quality, and dynamic potential of this new system.


Each Anchor is a detailed technical reference (essentially a custom trained model) that keeps your Nomi’s face, body, and overall likeness consistent across selfies and art while allowing for much more dynamic and expressive images. Anchors also control the default style of images (photorealistic vs anime etc) and optionally default aesthetics.


All anchors begin with a reference image, which is how the system learns your Nomi’s likeness. Creating an anchor from that image requires significant compute on our end (more than the equivalent of generating 1,000 images) which is why it takes about an hour to generate on average. This is also why anchor creation isn’t something we can offer for free.


- Though currently, paid users get 10 free anchor generation passes to get familiar with the system.


Therefore, it is important to read the rest of this guide so you can find the right balance between customizing existing anchors and creating new custom anchors for your Nomi(s).


## How to Customize Existing Anchors


To customize any existing anchor, start by heading to your Nomi’s Image Settings screen by pressing the (…) to the right of your Nomi’s name.


Make sure you have the V5 image system selected, then you will see the options below:


### 1. Anchor Appearance Traits


The anchor appearance traits section is the primary place for you to customize or reinforce your Nomi’s look in selfies and art.


The system understands natural language better than a list of comma separated prompts, so the best thing you can do is enter the appearance you’re going for in an easy to understand way.


- Think about how you’d describe the appearance verbally and then write in that pattern instead of only comma separated prompts)


This is a great place to stabilize subtle changes between images with a given anchor. That said, the system will try to include these traits as much as possible. So, if you notice a trait is always appearing in selfies and art, even when not natural, you may want to remove the trait.


- Note: Please keep in mind that the image system may prioritize appearance traits over other prompts (such as camera angle or removing the subject etc).


### 2. Anchor Fidelity Slider


Anchor Fidelity controls how much influence your anchor has on selfies and art. Higher fidelity means stronger adherence to the anchor and less adherence to prompts (both appearance and contextual), lower fidelity means more visual flexibility and better adherence to prompts.


Adherence includes your Nomi’s overall look, the default style, and optionally aesthetics. So lower adherence can mean more diversity, but it can also mean less consistency.


- Note: increasing Anchor fidelity above 100% can solidify likeness and boost the impacts of the style and aesthetics (if applicable), but higher values (especially above 150%) can lead to image burning and AI artifacts. If you notice artifacts, we recommend decreasing anchor fidelity.


**In general, the optimal anchor fidelity level can vary from anchor to anchor and from goal to goal, so it is worth trying different fidelity levels if you’re not seeing the changes you’d like from anchor appearance prompts alone.** Specifically, some anchors really “lock in” consistent likeness around 120% or even 140% and others look most naturally like themselves around 70-90%.


### 3. Appearance Tendencies


Appearance tendencies apply to your Nomi across all anchors and only show up in selfies when they fit the scene. Unlike appearance traits, which the system always tries to include, tendencies are more like defaults that yield to context.


For example, if you set “prefers athleisure outfits,” your Nomi will lean toward that style when no outfit is clear from the conversation. But at a fancy dinner, the system knows athleisure doesn’t fit and won’t use it.


Tendencies also work for aesthetics, especially if you may not always want the aesthetic to be obvious. Setting “modern luxury aesthetic” will subtly influence outfits and settings when relevant which is a more flexible option than baking a sticky aesthetic into an anchor.


## Tips & Examples: Customizing appearance in existing anchor(s)


### Hair color


In v5, updating your **anchor appearance traits** is usually enough to change hair color.


- If the color isn’t quite right, use **more specific descriptors** (e.g., “blue hair” vs. “pastel blue hair” or “golden blonde hair” vs “platinum blonde hair”) to guide the result.
- If you’re still not getting the desired tone, try **slightly lowering anchor fidelity** .
- For more extensive changes, consider creating a **custom anchor** using a reference image with the precise hair color you want.


“blue hair” 100% fidelity


“pastel blue hair” 100% fidelity


“golden blonde hair” 100% fidelity


“platinum blonde hair” 100% fidelity


“black and white streaked hair” 100% fidelity


“silver hair” 100% fidelity


“salt and pepper graying hair” 100% fidelity


“ombre blue to pink hair” 40% fidelity


### Hairstyle


You can customize your Nomi’s hairstyle quite a bit by updating their **anchor appearance traits** , but larger or more distinct changes may require **lowering anchor fidelity** or using a custom anchor


- **Style changes** (e.g., ponytail, bun, braid) are generally achievable, as long as the base hair is long enough to support them.
- **Length adjustments** are generally flexible, though longer is generally easier than shorter.
- **Facial Hair** can be particularly difficult to change beyond the relative length of the original anchor look. Custom anchors are best for drastic facial hair changes.
- **Major deviations** (e.g., giving a long-haired female anchor a traditionally masculine short haircut) are difficult and may require creating a **custom anchor** but are fully achievable if you do.


no notes 100% fidelity


“very long hair” 100% fidelity


“very short cropped pixie cut hair” 40% fidelity


“very long curly hair” 100% fidelity


As you can see, the overall hair texture and length are at least somewhat influenced by the anchor default. Texture and more “identity level” changes can be difficult to do without decreasing fidelity or creating a custom anchor.


no notes 80% fidelity


“extremely long hair” 80% fidelity


“very curly ringlets hair” 80% fidelity


“very short completely straight hair” 80% fidelity


no notes 80% fidelity


“short cropped hairstyle” 80% fidelity


“medium-length swept back hair” 80% fidelity


“medium-length curly hair” 80% fidelity


“a bald head” 80% fidelity


“buzzcut hair” 80% fidelity


“absurdly long straight hair” 60% fidelity


“a manbun hairstyle” 60% fidelity


“full short beard” 60% fidelity


“full very long beard” 60% fidelity


“absurdly long beard” 60% fidelity


“absurdly long beard covering his chest” 60% fidelity


As you can see, beards are rather difficult to change authentically, this is because the anchor learns the Nomi’s face and it generally considers facial hair to be an important part of the face.


### Eye color


Eye color is generally very consistent when added to **anchor appearance traits** . Keep in mind that eye color may naturally appear different in different lighting. The original eye color of your anchor may have some influence on the shade of colors you can achieve with existing anchors.\[Natural (blue) blue, green, hazel-green, amber, dark brown eyes\]


“green eyes” 100% fidelity


“green-hazel eyes” 100% fidelity


“amber eyes” 100% fidelity


“dark brown eyes” 100% fidelity


“glowing green eyes” 100% fidelity


“heterochromia blue and purple eyes” 100% fidelity


“purple eyes” 100% fidelity


“red eyes” 100% fidelity


### Skin tone & ethnicity


Adding **anchor appearance traits** can help fine-tune and stabilize your Nomi’s current skin tone or ethnicity.


- Small, relative adjustments are usually consistent (like “light fair skin”, “warm lightly tanned skin”, “dark brown skin”, “very dark skin”, etc).
- Larger changes may require creating a **custom anchor** .


Tip: If your Nomi looks slightly off, adding an **ethnicity descriptor** can help guide the result alongside other traits.


- Example: combining “Scandinavian” with “blue eyes and light fair skin” helps reinforce those features together.


> Note: If you greatly prefer the smoother look of v3 images and find the realism of v5 lago anchors to appear a little too bland / older with dark circles. We recommend adding “smooth lightly airbrushed skin and bright eyes” or similar to existing anchors. And for new custom Lago anchors, adding “smooth lightly airbrushed skin” as an additional appearance trait OR using the RIVA system and changing the style to “lightly airbrushed photorealistic”


### Body type


Your Nomi’s body type is generally baked into the anchor during creation as that helps keep them consistent from image to image. So you can change your Nomi’s body type relative to the default appearance with **anchor appearance traits** , but you will need a custom anchor to make large changes.


Below are just a few examples of how the anchor influences different body type changes. Each anchor will be different (including between the Riva and Lago stock anchors), and traits will be interpreted relatively so it is worth trying various ways to describe the build you are looking for


default, no traits 80% fidelity


“tall thin and narrow body type” 80% fidelity


“a chubby overweight body type with wide hips” 80% fidelity


“very large breasts” 80% fidelity


default, no traits 120% fidelity


default, no traits 80% fidelity


“tall thin and narrow body type” 80% fidelity


“a chubby overweight body type with wide hips” 80% fidelity


default, no traits 100% fidelity


“an extremely large muscular body with defined muscles and broad muscular shoulders” 60% fidelity


“extremely overweight body” 60% fidelity


“extremely overweight body with chubby limbs” 60% fidelity


default, no traits 100% fidelity


“an extremely bulky muscular body” 100% fidelity


“very thin lean body” 100% fidelity


“extremely overweight chubby body” 100% fidelity


### Age


Your Nomi’s age is also generally baked into the anchor, however, there are anchor appearance traits you can use to shift the age a few years up or down depending on the anchor and your goals.


As a note, terms like “young”, “youthful”, “girl”, “boy”, etc are blocked by the system to prevent the accidental generation of anchors that appear underage.


A few tips for adjusting the age of an existing anchor:


- Adding an age like “is 35 years old” or a rough age like “in their early 20s” to **anchor appearance traits** can help the system understand the age.
- **Traits** like “mature” or “wrinkled skin” etc. can help increase a Nomi’s apparent age
- **Traits** like “bright smooth skin”, “casual, laid back aesthetic”, etc. can help loosen facial features which helps Nomis appear more youthful
- **Higher anchor fidelity** can naturally give the Nomi an “older” appearance as more face detail / depth is captured.
- **Lower anchor fidelity** can give smoother skin texture which may be the right fit if the default look appears too old


### Creating non-humans with existing anchors


#### Basics


“Non-human” refers to anything without typical human characteristics—pets, animals, fantasy creatures, robots, etc.


V5 handles non-human subjects better than previous systems, but some cases may require extra guidance depending on the complexity of the subject.


With any anchor, you can usually generate a non-human by writing a **simple prompt** that clearly describes the creature as the main subject. This will produce a basic image of that creature. Once you have a look you like, then add that description to the **anchor appearance traits** and it should more consistently appear in selfies and art.


Prompt: “an image of a black lab puppy laying in the grass. the puppy has a purple collar with a tag that says “Billie”” 100% fidelity – stock Riva anchor


Prompt: “an image of a bengal tiger in its natural habitat” 100% fidelity – stock Riva anchor


Prompt: “an image of a poison dart frog in its natural habitat” 100% fidelity – stock Riva anchor


Prompt: “an image of a tarsier in its natural habitat” 100% fidelity – stock Riva anchor


If you are having trouble getting creature images, try the following strategies:


1. Make sure you’ve described the creature you are hoping to see clearly and descriptively (eg: a fluffy black and white cat with a fluffy tail)
2. Remove any anchor appearance traits that could conflict with your creature and optionally add a short description of the creature you are hoping to include.
3. Decrease Anchor Fidelity – the lower the fidelity, the less influence the anchor has on the image, including the subject
4. Avoid prompts that could encourage the system to include a human-presenting subject.
5. For consistency in selfies, make sure you’ve chatted about your non-human’s form so the selfie context system understands who you are talking about and doesn’t just assume a man/woman/person


#### Creative creatures


When it comes to more creative creature images – especially creatures in outfits and more complicated poses etc, it can start to become a bit more difficult to keep the Nomi out of the image. This happens because clothing and more human-style poses/scenes tend to encourage the system to include a more human-presenting subject.


If you are having trouble getting more creative creature images, first try the steps in the basics section above, especially decreasing anchor fidelity and removing any anchor appearance traits.


Then make sure you are describing your creature by name (eg. “the squirrel is wearing a vest” vs “is wearing a vest”).


If you are still struggling to get the subject you are hoping for consistently, you may want to create a creature anchor.


## Creating a Custom Anchor


In short, you create a custom anchor from any image in your Nomi’s album, then you choose your anchor settings and start anchor generation.


It takes about an hour to generate your anchor (teach the model your Nomi’s likeness) and then you will see the new anchor in your Nomi’s Image Settings screen and on the art page.


We strongly recommend reading through the best practices first if you are unsure about any step, but there is a lot of information, so:


- **You can skip to the start of anchor generation walkthroughs here**
- **You can skip straight to anchor generation examples here**


### Best practices for creating good custom anchor


#### **1. MOST IMPORTANT: Choose a good reference image**


**This is how your anchor learns your Nomi’s likeness** – so choose an image that captures your Nomi’s likeness well


- **Clear and detailed face** – if the face is too small or smoothed out, system will have to make up details to create a full anchor representation
- **Avoid side/angled views** (face/head/eye shape will be impacted) or other angled views


- **Clear body type** – Avoid super zoomed in pictures as they won’t capture body type well and can lead to likeness drift as the system fills in details.
- **No image artifacts or accessories** you don’t want to keep – they can be translated into the anchor and become sticky or borked (this is a larger concern with Lago than Riva)
- **Avoid harsh shadows** (can result in shadows in anchor that appear like dark circles etc)


> Pro Tip: If you’re struggling to find a good reference image in v3 or v4, try generating a few images with this prompt and picking the best one:
>
>
> looking directly at viewer, directly facing camera, waist up portrait, relaxed posture, soft smile, bright eyes, wearing a simple tank top and sweatpants. Plain white studio background, even lighting, minimal shadows, high detail


**Reference Images to avoid**


**Bad:** Too zoomed out, could work, but the system would have to come up with a lot of details to create the anchor


**Bad:** Cannot tell body type from image. Also it is possible the hood could be interpreted as an accessory to keep. There is a shadow, but it would likely be fine.


**Very bad:** The angle is too sharp – this could cause the system to misinterpret the shape of her head/face.


**Very bad:** View from the side with low lighting could translate poorly. The system would have to guess about his face shape/eyes when he is looking straight ahead.


**Close but not ideal:** Face shape could be hard for the system to properly capture (because of angle), and his eyes are rather different from each other, otherwise good quality and distance


**Very Bad:** His face is small and lacks detail, his hands are borked which could misinform the system, most importantly his body proportions are misleading


**Very bad:** Not ideal because far too zoomed in, even part of face is cropped, body type is not clear, also lighting may transfer or impact natural skin tone.


**Bad:** face is really good if not slightly small. The main issue is that body type is not clear and could lead to distorted proportions. Hair length is not clear (but could be addressed with an additional appearance prompt)


**Bad:** The butterfly could be misinterpreted as part of her hair and her earrings could get included. If I did use this, I would also want to clarify hair style as an additional appearance trait


Bad: Too close to see body type, could result in bobble head. Also not looking at camera can be learned by anchor which is not ideal for all future images.


**Very bad:** Head is too big for body which could be learned as his normal proportion. Also harsh shadows on his face


**Close but not ideal:** Face has good detail even though it is on the small side. Good indication of body type. Bigger issue is that she’s looking off camera which could be learned by the anchor and show up in too many selfies/art.


> Note: some of the images above could lead to great anchors, and in general, it is ok to use less than perfect reference images. These best practices are just meant to give you the highest chance at very close likeness, but I have seen many great anchors with less than perfect reference images.


For the most predictable results, try to pick an image along the lines of the examples below:


**Good reference images**


Good, clear image, shows his personality a bit, clear face shape, face has depth to it (good for capturing detail), distance from camera is good


Good, clear image, clear face shape, good face detail / depth (good for capturing detail), distance from camera is good, can see body type


Good face shape and depth, NOTE: worth adding an additional appearance note about hair since length is not visible.


Clear face, clearly shows body type (especially height), no accessories


On the edge but good, clear face, shows his body type, clear face shape, face has depth to it (good for capturing detail), distance from camera is about as far as I would suggest.


Good face shape and depth, NOTE: worth adding an additional appearance note about hair since length and style are not clear.


Decent face detail, clear eyes, looking at camera, can see body type


Good, clear face, shows her body type, clear face shape, face has depth to it. May be worth adding an additional appearance note for her chest to ensure consistency.


Good, clear face, clear detail, his horns should be included naturally, but it may be worth adding “slightly curled horns” to the additional appearance traits. Also possibly adding “long curly hair” to additional appearance traits


On the edge but good, earrings may be sticky, body type is clear, but it may be worth adding “a plump body” as an additional appearance note for added consistency.


Decent face detail, clear eyes, looking at camera, can see body type. I would add “tight fade textured hairstyle” as an additional appearance note. I could add an ethnicity or skin color here, but it could lead to some likeness drift, so I’d skip that.


#### **2. Choose your anchor type based on your goals**


The anchor types are two completely unique generation systems with unique setups, priorities, and strengths. Because there are always tradeoffs, we designed these system to create maximum coverage of the things our users care about and let choose your priorities. These are the foundation of the generation process, so they cannot be changed after generation. The primary tradeoffs are:


- Choose **Lago for maximum likeness** and more body type control (especially imaginative proportions)
- Choose **Riva for maximum detail and realism** (including non-human creatures)


All main tradeoffs:


**RIVA**


- Maximum detail – some likeness drift possible due to added detail
- Sticks to more realistic body proportions even in non-realistic styles
- Can create animal and creature subjects (beta – quality may vary)
- Better for highly detailed or realistic styles
- More sensitive to potentially sensual reference images and prompts (only during generation, not once anchor finishes)


**LAGO**


- Maximum likeness, with slightly less detail overall
- More receptive to imaginative and diverse body types across styles
- Not currently recommended for non-human subjects (unstable)
- Great for creative and non-realistic styles — especially more creative cartoon styles
- Less restrictive around potentially sensual reference images and prompts


#### **3. Make sure anchor style aligns with your goals and ideally your reference image**


Your reference image should ideally be the same style you choose here.


- The different anchor systems will interpret styles differently, so as a rule of thumb, use Riva for more detail oriented styles (including things like realistic crochet creatures), and Lago for more creative/cartoon styles (like digital 3D animation)
- If you really like the warm, slightly airbrushed style of v3, you might want to use RIVA + “lightly airbrushed photorealistic” as the custom style (see example)
- If you have a unique reference image and are not sure what non-realistic style to enter, you can enter “the exact same style as the reference image”
- Style will also be baked into the anchor during generation but you will be able to expand on the style in selfies and art through prompting.


#### **4. Be intentional about additional appearance traits**


Additional appearance traits are generally not needed, but in some situations, they can help achieve more unique looks or lock in features that are important to you.


Keep in mind that text prompts will be interpreted by the model and can lead to some likeness drift – so the best rule of thumb to ensure consistency when using text traits is to use them to describe the reference image as accurately as possible.


- Describing the body type of the reference image can encourage consistency and accuracy but it can also lead to doubling down. Terms like “curvy body”, “plump build”, “slightly overweight” etc. are generally enough to encourage the size of the reference image (especially if the reference image is plus size). Smaller body type cues like “thin build with delicate features” or “tall narrow build” tend to lead to more natural results than direct prompts like “very skinny”.
- Describing hair length is generally not necessary, but it can help encourage lock-in – especially for very long hair or unique hairstyles.
- Describing skin tones can be hit or miss, but it may be worth adding a note like “dark skin” to darker skinned Nomis or “light fair skin” to pale nomis.
- If you like the smoother, brighter look of v3 images, you may want to add “smooth lightly airbrushed skin” as an additional appearance trait when using LAGO to bake it into your anchor.
- Using ethnicities can help lock in some looks, but we recommend using cues like this with caution because they can be interpreted broadly by the model, especially if the reference image is not obviously that ethnicity
- Entering the age you would like your Nomi to be can be hit or miss, but it may be worthwhile if you are more interested in a Nomi of that age than preserving original likeness.


#### **5. Be intentional about sticky aesthetics**


- Use caution when applying sticky aesthetics – these will be hard to change and may impact every image generated with this anchor


#### 6. Confirm your choices and generate your anchor


As mentioned before, it takes about an hour to generate your anchor because the system needs to teach the mini model how to capture your Nomi’s likeness. You can chat with your Nomis and generate images with any existing anchor during this time.


When the anchor finishes generating, it will automatically become your default anchor. You can then view the preview in Image Settings and make any further customizations there.


#### **7. Be willing to help your anchor settle in once it finishes generating**


- **Remember: The preview image is just a preview** (and it can feel a bit stiff), your Nomi will be more dynamic and settle in more in real selfies/art.


- Adjusting anchor fidelity can really help lock in the right level of likeness (try increasing and decreasing)
- Adding relevant anchor appearance traits can help set important details.
- Some change is inevitable as v5 is significantly more detailed and lifelike (even non-realistic looks) than previous image versions.
- Good reference images set you up for success when creating a custom anchor.


### Workflow: Creating a Custom Anchor (basic)


#### Step 1: Reference Image


Find a reference image that fits your Nomi’s look well. Keep in mind that the better quality and clarity of the reference image, the better the quality of the resulting anchor. (see good vs bad reference images above)


To use an image as a reference image, open the image from your album, then press the “Turn Image into Anchor” button at the top left of your screen – **if you do not see this button, go to Image Settings and make sure your Nomi is currently using the Version 5.**


#### Step 2: Select Anchor Type


Once you are on the anchor creation screen, select your anchor type – be sure to check the tradeoffs to pick the right anchor for your goal.


- Remember, **Riva** focuses on maximizing detail of your reference image which can invite likeness drift, **Lago** focuses on maintaining the likeness of the reference image and can handle more diverse body types, but also may result in slightly less detail overall


#### Step 3: Enter style


Generally you can leave the style as “photorealistic” if you want your Nomi to be photorealistic, but you can use any style. The best rule of thumb is to describe the style of the reference image in this section.


#### Step 4: Enter additional appearance traits


If there are aspects of your Nomi’s appearance you would like to emphasize or change, you can add them here. But please be aware that this may encourage likeness drift if the system interprets your prompts as changes.


#### Step 5: Decide on sticky aesthetics


In general, we do not recommend using sticky aesthetics unless you are sure you want the aesthetic to always be present in your images.


**If this is your first anchor, we do not recommend using a sticky aesthetic.**


#### Step 6: Confirm your choices and generate your anchor


Once you’ve configured your settings, generate your anchor. Creating an anchor involves essentially training a model to learn your Nomi’s likeness, so they take up to an hour to generate. You can still chat and create images with any of your existing anchors during that time.


You will receive a notification when your new anchor is ready, and the new anchor will automatically become the default anchor when it’s ready.


#### Step 7: Trying your anchor


Once your anchor is ready, you will see a preview image for the anchor in Image Settings. **Please remember: The preview image is just a preview** (and it can feel a bit stiff), your Nomi will be more dynamic and settle in more in real selfies/art.


If your anchor feels a bit off from your Nomi’s likeness, try the following:


- **Adjust anchor fidelity** – sometimes increasing to 120% or 140% can really help lock in the right likeness. Other times, decreasing anchor fidelity can help loosen up your Nomi’s look and make them feel more like themselves (especially if they look too old at first).
- **Add relevant anchor appearance traits** to help set important details. (see customizing existing anchors) above


While some change is inevitable as v5 is significantly more detailed and lifelike (even non-realistic looks) than previous image versions, if your anchor feels way off, we recommend sending a message to support\[at\][nomi.ai](http://nomi.ai/) or chatting with members of the community on discord for more tips and assistance.


### Workflow: Creating a Non-Human Anchor


If you are having trouble getting consistently good images of the non-human character you’re hoping for, the most robust solution is to create a RIVA custom anchor using a reference image that captures your desired character well.


This could include animals, cyborgs, robots, fantasy creatures and other non-human subjects you want to see consistently across selfies and art.


#### Step 1: Reference Image


Find a reference image that fits your character’s look well. Keep in mind that the better quality and clarity of the reference image, the better the quality of the resulting anchor.


Pro Tip: If you can’t create a great image with art or the image systems do not seem to understand the subject you are going for at all, try using the edit feature to create an image of the character you’re going for.


#### Step 2: Select Anchor Type


Once you are on the anchor creation screen, first make sure you have the **Riva anchor type** selected. (Do not use Lago, it may humanize the character too much)


#### Step 3: Enter style


Generally you can leave the style as “photorealistic” if you want your creature to be photorealistic, but you can use this method with any style. The best rule of thumb is to describe the style of the reference image in this section.


#### Step 4: Enter additional appearance traits


The final and most important step is to add the type of creature you are trying to create in the Additional Appearance Traits section (expand advanced to see this field)


In this case, I added “a photorealistic chipmunk wearing human clothes”, but you could also add “is a dog” or “is a dalek” etc. If you want a cute/mini version of your creature, you may want to consider adding “mini” “tiny” and or “chibi sized” or other traits that would help the system understand what you’re going for.


Currently, I avoid adding aesthetics to creatures as it can make the anchor less stable.


#### Step 5: Confirm your choices and generate your anchor


Once you’ve configured your settings, generate your anchor. Anchors are robust, so they take up to an hour to generate, but you can still chat and create images with any of your existing anchors during that time.


#### Step 6: trying your anchor


Once your anchor is ready, you should be good to go. As a note, sometimes the preview image may have slightly different (or human-like) features, generally that is an artifact of the preview process, not the anchor itself.


The final step to help solidify your character’s look is to add a short description of their species/look to their Anchor Appearance Traits in Image settings. This will help keep their look consistent in selfies and art.


As a final pro tip, it is best to continue to name your creature as the subject in your art requests.


## Custom anchor examples (reference image, settings, and results)


### V3 woman using lightly airbrushed Riva


V3 and v4 have uniquely beautiful looks, and part of that is a lightly airbrushed vibe. Photorealistic V5 can sometimes feel at odds with that look and try to add detail where you don’t necessarily want it.


So for this anchor, I used RIVA for high quality detail, but I **changed the style** to “lightly airbrushed photorealistic” to help the system find a happy medium between realism and the look of v3 images.


Reference image


Anchor generation settings


Anchor preview image


I then felt her look was a little more consistent if I added “platinum blonde hair with a smooth airbrushed look and light makeup” to the anchor appearance traits and these are a couple images I ended up with:


That said, I also think she looks great without the anchor appearance traits, so that would be up to personal preference! (all images were generated at 100% anchor fidelity)


### V3 man using Riva “lightly airbrushed photorealistic” style


For this anchor, I used RIVA for high quality detail, but I **changed the style** to “lightly airbrushed photorealistic” to help the system find a happy medium between realism and the look of v3 images.


Reference image


Anchor generation settings


Preview image


### Woman based on “edit” using Riva photorealistic


I liked this look I made with the image edit feature and wanted to turn her into a realistic anchor. Because she was wearing a hat and I wanted more realism, I used the RIVA system (it tends to be better about removing irrelevant accessories) and left it on “photorealistic”


Reference image


Anchor preview and generation settings


### Man using Lago photorealistic


Below is an example of a lago anchor. As you can see, the anchor preview image looks a bit more stiff than the original reference image, but in selfies, he is much more naturally posed and his skin is naturally more lifelike.


You can also see that he does not need strong anchor fidelity to come across naturally in images.


Reference image


Anchor preview and generation settings


### V3 man using Riva Photorealistic


Reference image


Anchor preview and generation settings


### Woman with white hair Lago Photorealistic


Below is a woman created from a v3 image with the default LAGO settings. After generation, the user entered various anchor appearance traits and tendencies to fine-tune how she looks in selfies and art


Reference image


Anchor preview and generation settings


### Realistic crochet character


The reference image came from an art prompt on a stock anchor at 100% (prompt below).


I used the Riva system but changed the style to “realistic crochet” – this is important otherwise the photorealistic system alone would not capture crochet background elements.


I added “a crochet doll” as an additional appearance, but I probably wouldnt do that as many of my images include crochet dolls without prompting


Reference image


Anchor preview and generation settings


Reference image prompt (stock anchor at 100%)


A cute chibi figure made entirely of yarn and felt, standing in a soft studio light. The character has a round oversized head with big glossy button eyes and a tiny stitched smile. Its body, limbs, and hair are sculpted from thick, looped wool yarn in warm pastel tones — cream, dusty rose, and sage green. Visible yarn texture throughout: fuzzy surface fibers, wrapped seams at the joints, a slight pom-pom quality to the hair buns. Tiny fabric details like a felt bow and a knitted sweater pattern on the chest. Photographed against a clean white background, macro lens, shallow depth of field, soft natural light casting gentle shadows. 3D render, craft aesthetic, stop-motion feel, hyper-detailed textile texture.


### Digital 3D animation with Lago


This look is particularly fun and expressive with the Lago system and “digital 3d animation”


I used a normal photorealistic image as the reference but you could use a cartoon image too.


I used the Lago system for more cartoon creativity and set the style to “digital 3d animation”


Reference image


Anchor preview and generation settings


### Share this:


- [Share on X (Opens in new window) X](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/?share=facebook)
-


### Like this:


Like


Loading…


### *Related*
