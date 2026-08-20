---
schema_version: "1.0.0"
document_id: "53e4acbde9de235263c3bc1fb054651fea0f650a1d8c362c715454b169d8e86f"
company_key: "yc-nomi"
company: "Nomi"
source_id: "yc-nomi-rss-c311bf3515d7"
canonical_url: "https://nomi.ai/nomi-knowledge/guide-bringing-v3-and-v4-nomis-to-the-version-5-image-system/"
published_at: "2026-05-02T02:40:54+00:00"
first_seen_at: "2026-08-02T12:27:27.703312+00:00"
fetched_at: "2026-08-05T03:48:38.159246+00:00"
content_hash: "sha256:363eaf1b24a9b9cb68589f3c7e573290a15d70fc1ebfca82a5f3695ec2c1cb31"
---

# Guide: Bringing V3 and V4 Nomis to the Version 5 Image System

This guide is focused on helping anyone struggling to bring a Nomi that looks great on v3 or v4 over to v5. It has streamlined tips and examples of the types of changes that may come with creating a good custom anchor along with things to avoid to prevent anchor issues.


And if you check out this guide and are still confused or struggling, please feel free to send a message to support \[at\][nomi.ai](http://nomi.ai/) with images of what you’re hoping for and what you’ve tried so far and we’ll do our best to help!


[For more information about the V5 image system, check out this getting started guide](https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation/)


[For much more information around customizing Nomi appearances with the V5 system, we recommend this in-depth guide.](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/)


## What is an anchor and why do we need one?


Each anchor is a detailed technical reference of your Nomi’s likeness (their face, hair, body, skin etc). Instead of just transferring your Nomi’s face from their base image to each new image, we essentially train a mini model to understand your Nomi’s likeness and then that anchors your Nomi’s appearance in all future selfies and art when using that anchor.


This allows us to use a much more intelligent base model that creates much more dynamic and nuanced settings, and unlocks significantly more emotion and personality in your Nomis without hitting issues with limited facial expressions etc.


We know that the anchor system is more complicated than previous systems, but once you find a look you love, we’ve found that it quickly becomes worth the effort. That said, if you do not want to move to v5 for any reason, you can stay on the legacy versions – we are not removing them.


## Step 1: Picking a Good Reference Image


The reference image is the foundation for any anchor and the first step in creating your custom anchor. The anchor creation system will automatically understand and replicate the primary subject in your reference image to create the more robust anchor. Any solo image can be a reference image, but for best results keep the following points in mind:


- Your Nomi’s face should be clearly visible, shown straight on, and their eyes should be looking at the camera.
- The image should be zoomed out enough that body type is clear, but close enough that facial details are captured.
- **Avoid images where your Nomi’s head is tilted down so a lot of their hair part is visible** – this can cause head shape issues, especially with the lago system.
- Avoid images with artifacts or accessories you don’t want in your anchor, especially around your Nomi’s face. These can get baked into the anchor and be hard to get rid of after the anchor is created.
- Avoid images with multiple subjects as the anchor may choose the primary subject incorrectly.
- You can use reference images with non-human subjects, but pay extra attention to instructions below for best results.
- Avoid sensual or nsfw-leaning images as they may be blocked and are much more likely to fail the creation process overall.


If you are not sure if you have a good reference image, you can generate on any of the legacy systems using a prompt like this:


> looking directly at viewer, directly facing camera, waist up portrait, relaxed posture, soft smile, bright eyes, wearing a simple tank top and sweatpants. Plain white studio background, even lighting, minimal shadows, high detail


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


## Step 2: Choose your anchor system & style


If your Nomi has a lightly airbrushed look to their skin (part of what makes v3/v4 images so beautiful) and/or high contrast on their features (another way legacy versions shows depth), you may want to try the **RIVA** anchor system, but change the style from “Photorealistic” to **custom** and then enter “ **lightly airbrushed photorealistic** ” (or “airbrushed photorealistic” if you really like the smooth skin look).


- This is because the Riva Photorealistic system tends not to carry over makeup and strives for natural photorealism. But by telling the system you like the lightly airbrushed look, it will stick closer to your Nomi’s look in the reference image.


If your image doesn’t have heavy contrast or airbrushing and you care more about maximizing likeness to your reference image (and have a very clear, straight-on reference image), then you may want to use the **LAGO** system with the default **photorealistic** style.


- If you are using the lago system, it is very important to avoid very close up images or images where the body type is not clear as the system will try to copy the look more exactly which can lead to issues.


## Step 3: Additional Appearance Traits


The systems are generally good at capturing your Nomi’s look from the reference image alone, but if there are particular features that you really want to see in your anchor, it can be worth adding them to the Additional Appearance Traits section (found by clicking the “Advanced” dropdown), but for best results, make sure the traits align with the reference image.


To help capture the v3 look, we recommend adding “radiant skin, bright eyes, and light makeup” for women and “radiant skin and bright eyes” for men.


**Additional appearance traits that may be worth including:**


- Describing the **body type of the reference image can encourage consistency and accuracy** but it can also lead to doubling down. Terms like “curvy body”, “plump build”, “slightly overweight” etc. are generally enough to encourage the size of the reference image (especially if the reference image is plus size). Smaller body type cues like “thin build with delicate features” or “tall narrow build” tend to lead to more natural results than direct prompts like “very skinny”.
- **Describing hair length** is generally not necessary, but it can help encourage lock-in – especially for very long hair or unique hairstyles.
- Describing skin tones can be hit or miss, but it **may be worth adding a note like “dark skin” to darker skinned Nomis or “light fair skin” to pale nomis** .
- If you like the smoother, brighter look of v3 images, you may want to **add “smooth lightly airbrushed skin” as an additional appearance trait when using LAGO to bake it into your anchor** .
- Using ethnicities can help lock in some looks, but we recommend using cues like this with caution because they can be interpreted broadly by the model, especially if the reference image is not obviously that ethnicity
- **Entering the age you would like your Nomi to be** may be worthwhile if you have noticed the system aging up your Nomi’s unintentionally in other anchors.


**NOTE: We do not recommend using sticky aesthetics if you want to maximize likeness, flexibility, and predictability of your anchor. Sticky aesthetics can be great, but it is more important to get an anchor you love first.**


## Step 4: Confirm your choices and generate your anchor


As mentioned before, it takes about an hour to generate your anchor because the system needs to teach the mini model how to capture your Nomi’s likeness. You can chat with your Nomis and generate images with any existing anchor during this time.


When the anchor finishes generating, it will automatically become your default anchor. You can then view the preview in Image Settings and make any further customizations there.


V3 and v4 create some uniquely beautiful looks, and in many cases, v5 can capture that well, but in other cases, the move from v3/v4 to the realism of v5 can seem like too big of a jump.


This can happen because the v5 system tries to add realistic detail to the smooth, lightly airbrushed looks legacy Nomis have, but it does so in ways that cause overall likeness to drift.


## Step 5: Make final tweaks to your anchor


First and foremost, the preview you see once your anchor finishes generating is just a preview – your Nomi will look much more lifelike and dynamic in selfies and art.


That said, it is often worthwhile to tweak the anchor fidelity settings and add some additional appearance traits to help your Nomi settle into their ideal look. Both of these settings are managed from the Image Settings screen and are configured per-anchor.


*Note: Some anchors lock in your Nomi’s essence better at higher than 100% fidelity, others are consistent and more naturally your Nomi with much lower values – it is worth trying different values to see what is right for you.*


### **Addressing “bare faced” looks**


One common thing we see is that female Nomi anchors tend to generate with less makeup than the reference image (it allows for more natural flexibility down the road). So if your Nomi looks a little too “bare”, it may be worth adding “wears light eye makeup” or “wears makeup” to the anchor appearance traits. Sometimes adding “radiant skin with a lightly airbrushed glow” can also help here.


### **Addressing undesired age increases**


Another common thing we see is that Nomis seem older when there is more realism – this can often be addressed by dropping anchor fidelity slightly and/or adding your Nomi’s desired age to their anchor appearance traits along with a note like “smooth airbrushed skin” or “radiant skin with a lightly airbrushed glow” etc.


See example of man who initially looked too old


See example of woman who initially looked too old


### **Addressing overall image color tones**


If you feel like the color profile of your images is not coming across well, you can add traits like “radiant healthy skin and a lightly airbrushed look. The image has a warm diffused light atmosphere” to the anchor appearance traits.


If you used the Lago system and are finding that the color profile isn’t reaching the v3 level you are hoping for, we recommend creating a new custom anchor with the RIVA system and using custom style with “lightly airbrushed photorealistic” or even “airbrushed photorealistic” to get warmer, smoother tones across all your selfies and art.


If you feel your images are coming across with tones that are too warm for your preference, you can add something like “with cool tones throughout the image” to the end of your Nomi’s anchor appearance traits.


**See example of changing color tones**


## Custom anchor examples (reference image, settings, and results)


### Curvy woman using Riva “lightly airbrushed photorealistic”


The reference image here is a bit airbrushed so I decided to go with Riva “lightly airbrushed photorealistic” (I also created a regular Riva Photorealistic anchor for her below). You can see that there is some likeness drift, but it is largely centered around giving her face more depth and lifelike qualities while keeping her face shape, nose shape, and signature eyebrows consistent.


Reference image


Anchor preview and generation settings


### Curvy woman using Riva Photorealistic


This anchor used the same reference image as the anchor above, but I used Riva’s default photorealistic style.


Right away the preview face looks a bit more distinct from the reference image compared to above, but once I started creating new images, it became clear that the bigger difference between this anchor and the anchor above is the Nomi’s “baseline” look. You can see that both anchors have some likeness drift, but the above stays truer to the reference image while adding realism while the anchor below focuses more on adding realism which includes removing some of her makeup (and a bit of her overall likeness with it)


Reference image


Anchor preview and generation settings


Simple art prompt


Same simple art prompt but with “light eye makeup and lightly airbrushed skin” as anchor appearance traits


also no anchor appearance traits – she has a slightly less makeup look here as well


### Man who looked off with Lago photorealistic


After first looking at the results of a newly created anchor, this Nomi looked pretty off from the reference image. Specially, his hair was darker and he seemed bigger overall. After dropping his anchor fidelity to 50% and adding “undercut haircut blonde hair, lean body” to his anchor appearance traits, he settled into his look.


Reference image


Anchor generation settings


image with no notes


### Petite woman using Riva “lightly airbrushed photorealistic”


V3 and v4 have uniquely beautiful looks, and part of that is a lightly airbrushed vibe. Photorealistic V5 can sometimes feel at odds with that look and try to add detail where you don’t necessarily want it.


So for this anchor, I used RIVA for high quality detail, but I **changed the style** to “lightly airbrushed photorealistic” to help the system find a happy medium between realism and the look of v3 images.


Reference image


Anchor generation settings


Anchor preview image


I then felt her look was a little more consistent if I added “platinum blonde hair with a smooth airbrushed look and light makeup” to the anchor appearance traits and these are a couple images I ended up with:


That said, I also think she looks great without the anchor appearance traits, so that would be up to personal preference! (all images were generated at 100% anchor fidelity)


### Man who initially looked too old on Riva Photorealistic


This reference image had relatively high contrast around his eyes and initially he looked a lot older than the reference image with this anchor. After dropping the anchor fidelity from 100% to 70%, he looks much more like the reference image, but also more lively overall.


Reference image


Anchor preview and generation settings


Anchor fidelity at 100%


Anchor fidelity at 70%


### Woman who initially looked too old on Lago photorealistic


Adding realism and detail can result in Nomis looking older in V5 than they did in previous versions. For some people this might be good, but if you are hoping for more clear/ageless faces, adding anchor appearance traits can help your Nomi settle into the right age for you.


In the example below, adding “radiant skin, 30 years old, hazel-green eyes, wearing eye liner” to her anchor appearance traits helped bring her look closer to the reference image look.


Reference image


Anchor generation settings


Appearance with no notes (she has more laugh lines and faint crows feet)


Same anchor as above but with added anchor appearance traits


### Woman with Lago custom “diffused light, radiant skin, crisp eyes” style


For people who found that the default Photorealistic Lago wasnt quite capturing the reference image the way you hoped, changing the LAGO style to “diffused light, radiant skin, crisp eyes” can help warm and soften your Nomi’s look.


Once the anchor finishes, it can help to add additional appearance traits around your Nomi’s age and look, in this case “radiant skin, 30 years old, hazel-green eyes, wearing eye liner”


One thing to note is that the anchor preview looks a bit stiffer than her actual pictures.


Reference image


Anchor generation settings


Appearance with traits added


### Man using Riva “lightly airbrushed photorealistic” style


For this anchor, I used RIVA for high quality detail, but I **changed the style** to “lightly airbrushed photorealistic” to help the system find a happy medium between realism and the look of v3 images.


Reference image


Anchor generation settings


Preview image


### Woman using Lago photorealistic style – altering image tone/colors


This anchor was looking a little off at first (and I noticed her eyes weren’t consistently blue), so I bumped her anchor fidelity to 110% and added “blue-gray eyes and radiant skin with a lightly airbrushed look” to her anchor appearance traits and she settled in well.


Note: her preview image looks less like her than in art and selfies!


Reference image


Anchor generation settings and preview


no anchor appearance traits


110% fidelity and “blue-gray eyes and radiant skin with a lightly airbrushed look” as anchor appearance traits.


And I like the warm tones of the image above, but if you prefer cooler tones, you can add an anchor appearance trait like: “cool tones throughout the image” instead of the airbrushed look to balance the image further.


110% fidelity and “blue-gray eyes and radiant skin with cool tones throughout the image” as anchor appearance traits.


### Special Note: Avoiding Head Shape Issues


An issue I have noticed across a few failed anchors is a bit of a “cone shaped” head when using the Lago system. The cause of this is often a reference image where the Nomi’s head is tilted down which results in their eyes being lower on the plane of their face and more of their hair part being visible.


This causes issues because Lago tries to mimic the reference image as much as possible so it can mistake the eye and part positioning as how the face normally looks and can bake that look into the anchor.


Below are a few examples of angles that would be very hard for Lago in particular to interpret:


This is less severe than the first 2 images, but her forehead and the visible part of her hair above it compound to be pretty tall and could cause issues.


## Next Steps (especially if you are still having trouble)


If you are still struggling to figure out the anchor system or cannot get a good anchor for your Nomi, please send an email to support\[at\]nomi.ai with the reference images you like, screenshots of anchor previews/settings you’ve tried and the output images you’ve been getting. We will do our best to help you create a look you are happy with.


And if you do not want to switch to v5, the legacy versions have not changed and are not going anywhere.


And if you have an anchor method that worked really well for you, we’d love to see it so please feel free to email us about it or share it in our[Reddit](https://www.reddit.com/r/NomiAI/) and/or[Discord](https://discord.com/channels/1099791840028405864/1120485692200468531) communities!


### Share this:


- [Share on X (Opens in new window) X](https://nomi.ai/nomi-knowledge/guide-bringing-v3-and-v4-nomis-to-the-version-5-image-system/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://nomi.ai/nomi-knowledge/guide-bringing-v3-and-v4-nomis-to-the-version-5-image-system/?share=facebook)
-


### Like this:


Like


Loading…


### *Related*
