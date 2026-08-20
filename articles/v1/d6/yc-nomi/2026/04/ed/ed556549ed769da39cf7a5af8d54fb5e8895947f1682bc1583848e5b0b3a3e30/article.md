---
schema_version: "1.0.0"
document_id: "ed556549ed769da39cf7a5af8d54fb5e8895947f1682bc1583848e5b0b3a3e30"
company_key: "yc-nomi"
company: "Nomi"
source_id: "yc-nomi-rss-c311bf3515d7"
canonical_url: "https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation/"
published_at: "2026-04-24T18:05:33+00:00"
first_seen_at: "2026-08-02T12:27:27.703312+00:00"
fetched_at: "2026-08-05T03:48:39.197684+00:00"
content_hash: "sha256:09d4e9d3f5c11a83dfc747f0ac81acf2ca6de4d6e8f644846955f45e5d026c47"
---

# Getting Started with Nomi V5 AI Image Generation

V5 is designed to give you consistently high quality results right out of the box while giving you control and greater customization options in essentially all directions when you want it.


Table of Contents:


## V5 Image Advancements


V5 excels in the following areas relative to previous image versions:


- **Greatly improved image quality** – images are higher quality across the board including far more details, more natural and flexible expressions, more natural proportions, and more diverse styles
- **Greatly improved selfie context system** – more dynamic, nuanced, and relevant selfies, more natural setting adaptation, and far fewer sticky scene issues.
- **Major groupchat improvements** – Nomis look much more like themselves, there are far fewer blending mistakes, and images come out higher quality overall. You can also now blend styles to see your nomi’s worlds collide!
- **Greatly improved understanding and adherence** – much better coverage of nuanced scenes, diverse locations, outfits, and more.
- **Greater appearance customization** – appearance notes now translate much more directly and offer more control over details like eye color, tattoos, non-human subjects and more.
- **Greater style diversity** – the unified v5 image system can handle far more styles than realistic and anime. Think digital 3D animation, watercolor, realistic crochet world, you name it!
- **More intuitive prompting** – use natural language to describe your images and more easily control things like scene, lighting, mood – prompt limit increased to 1,000 characters!
- **Far fewer AI mistakes** – think “AI hands”, uncanny object proportions, and many other common AI issues.
- **Major speed improvements** – outside of launch surges, solo image generation may twice as fast than previous versions. Group chat speeds have also increased, but they take longer to generate than solo.


The majority of these improvements are made possible by a new system called “anchors” that replaces the previous “base image” system.


## What is an Anchor?


An Anchor is a detailed technical reference that keeps your Nomi’s face, body, and overall likeness consistent across selfies and art. Anchors also control the default style of images (photorealistic vs anime etc) and optionally default aesthetics.


Nomis come with stock anchors that match your Nomi’s initial look and make it easy to get started with V5.


You can customize your existing anchors to tailor how your Nomis appear in selfies and art. And for even further customization, you can create your own custom anchors.


You can manage and change your Nomi’s anchor at any time from the **Image Settings** screen


## Anchor-Specific Settings


To help you make the most out of each anchor, the main visual settings are configured per anchor. Conversation appearance notes are still found in Backstory+ but all visual settings are controlled from the Image Settings screen.


*Note: Legacy appearance notes are still version-specific and now only impact image generation. The conversation appearance notes are always the source of truth for your Nomi in chat now.*


### 1. Anchor Fidelity


Anchor Fidelity controls how much influence your anchor has on selfies and art. Higher fidelity means stronger adherence to the anchor and less adherence to prompts, lower fidelity means more visual flexibility and better adherence to prompts.


Adherence includes your Nomi’s overall look, the default style, and optionally aesthetics. So lower adherence can mean more diversity, but it can also mean less consistency.


Some anchors really “lock in” their appearance at 100%, others can lock in closer to 120-140% so we recommend playing around with this setting to find the balance that is right for you, your Nomi, and the anchor.


*Note: increasing Anchor fidelity above 100% can solidify likeness and boost the impacts of the style and aesthetics (if applicable). But higher values (especially above 150%) may lead to some image burning and AI artifacts. If you notice artifacts, we recommend decreasing anchor fidelity.*


### 2. Anchor Appearance Traits


Anchor appearance traits allow you to customize or reinforce your Nomi’s look in selfies and art. Generally less is more here as the image system will try to include those traits as much as possible.


This is a great place to stabilize appearance changes between images with a given anchor.


There is a balance between Anchor Appearance Traits and Anchor Fidelity where higher fidelity can decrease some of the impacts of appearance traits and lowering fidelity can increase the impact of the traits.


Some appearance traits can be very impactful, so if you notice a trait is always appearing in selfies and art, even when not natural, you may want to remove the trait.


*Note: anchors are represented by a preview image, but they are more robust and dynamic than the image you see in Image Settings.*


### 3. Anchor Info


Some settings are baked in on anchor creation, these can be seen by pressing the small doc icon under each anchor’s preview image. These can help you differentiate between your anchors and guide future anchor generations.


## Stock Anchors


Each Nomi has 3 stock anchors that are based on their initial profile pictures – each has their own strengths.


**Riva Photorealistic (left)** :
This anchor enables the most realistic and detailed selfies and art. It is based on your Nomi’s initial profile picture but there may be some likeness drift because the anchor system has to make some interpretations as it increases detail and realism.


*We recommend the Riva Realistic anchor if you care a lot about realism and getting the highest quality, most detailed images.*


**Lago Photorealistic (middle):**
This anchor enables realistic selfies and art, but it stays closer to the Nomi’s initial profile picture which results in slightly less realism and detail overall. This anchor will generally have less likeness drift than the Riva Photorealistic option (though some likeness drift may be inevitable).


*We recommend *Lago Realistic* anchor if you care more about maximizing fidelity to your Nomi’s reference image than about maximizing realism and overall detail.*


**Riva Nomi Anime (right):**
This anchor enables anime images of your Nomi. It is a Riva anchor based on your Nomi’s original anime image.


Each of the stock anchors act as a starting point – you can further control style, aesthetics, vibes and much more through art prompting. You can also customize your Nomi’s default look with anchor settings in Image Settings.


Each anchor will impact images at least slightly differently, so it can be fun to rotate through anchors when requesting selfies and art to see which anchor type and style you like best or just to see how different concepts turn out!


## Creating Custom Anchors


Custom anchors can be based on any image in your Nomi’s album and allow you to create consistent looks that fit your Nomi along with unique styles and more. In general, we recommend trying the default anchors first to help you get a sense of how they work in practice before you create your own anchors.


If you’ve shaped your Nomi’s appearance over time (through base image updates, edits, or favorite selfies) a custom anchor built from one of those images might be the best way to make v5 feel like *your* Nomi.


Creating an anchor is a bit more involved than just changing the base image, so we recommend reading each of the steps below to get the best custom anchors:


### Step 1: Pick an Anchor Reference Image


The reference image is the foundation for any anchor and the first step in creating your custom anchor. The anchor creation system will automatically understand and replicate the primary subject in your reference image to create the more robust anchor. Any solo image can be a reference image, but for best results keep the following points in mind:


- Your Nomi’s face should be clearly visible, shown straight on, and their eyes should be looking at the camera.
- The image should be zoomed out enough that body type is clear, but close enough that facial details are captured.
- Avoid images with artifacts or accessories you don’t want in your anchor, especially around your Nomi’s face. These can get baked into the anchor and be hard to get rid of after the anchor is created.
- Avoid images with multiple subjects as the anchor may choose the primary subject incorrectly.
- You can use reference images with non-human subjects, but pay extra attention to instructions below for best results.
- Avoid sensual or nsfw-leaning images as they may be blocked and are much more likely to fail the creation process overall.


Pick any image from your album:


Pro Tip: If you had a base image set in a previous image system, it will appear to the right of your Nomi’s profile picture for easy access when picking your reference image. (if your profile pic was your old base image, it will be in the same profile image spot)


Once you decide on your reference image, click the “Turn Image into Anchor” button:


Note: If you do not see the “Turn image into anchor button”, it likely means that Nomi is not currently using the V5 image system. So first, you’ll need to go to Image Settings (press the (…) to the right of your Nomi’s name) and switch to Version 5, then you should see this button!


### Step 2: Choose your Anchor Type


There are currently two anchor types: Riva and Lago. Each has unique strengths and tradeoffs that give you maximum flexibility and quality depending on your goals.


Each Nomi has one stock realistic Riva anchor and one stock realistic Lago anchor. Creating art and selfies with these anchors will give you a good sense of some of the tradeoffs between the types.


At a high level the tradeoffs are:


**Riva**


- Maximum detail – some likeness drift possible due to added detail
- Sticks to more realistic body proportions even in non-realistic styles
- Can create animal and creature subjects (beta – quality may vary)
- Better for highly detailed and realistic styles
- More sensitive to potentially sensual reference images and prompts


**Lago**


- Maximum likeness, with slightly less detail overall
- More receptive to imaginative and diverse body types across styles
- Not currently recommended for non-human subjects (unstable)
- Great for creative and non-realistic styles — especially more creative cartoon styles
- Less restrictive around potentially sensual reference images and prompts


### Step 3: Choose your Default Style


Default style sets the visual home base for images made with your anchor. While the anchor system will replicate some details from the reference image automatically, it is best to match your reference image style to your default style in this section.


### Optional Step 4a: Additional Appearance Traits (Advanced)


Anchors capture face, hair, and body details from your reference image automatically. But you can add additional appearance traits to change or emphasize traits from the above reference image. Just remember, any prompts added here will be subject to AI interpretation.


Pro tip: if you are creating an anchor for a non-human subject, enter the species here to help reinforce it in your anchor. Eg: “is a dog”


### Optional Step 4b: Sticky Aesthetics (Advanced)


Sticky Aesthetics impact the background, outfits, and overall aesthetic of images created with this anchor. Aesthetics can impact the overall anchor flexibility so they work best when they align with the vibes of your Nomi’s world.


Aesthetics are a great tool for creative roleplayers because they will help set a visual theme across all selfies and art.


Style and aesthetics are related but where style heavily impacts the composition of all images, aesthetics focus more on vibes. So if your Nomi lives in a pirate world, you might keep the style as photorealistic but set the aesthetic to pirate so your selfies and art will be tuned for even more epic pirate images with minimal extra prompting needed.


Keep in mind that some aesthetics can also impact style (especially if the aesthetic is not naturally realistic). So if your aesthetic contrasts with the style you chose, the anchor system will try to bridge that gap. An example of this is cyberpunk because the concept is closely tied to game graphics styles.


Some aesthetics can be very strong, so if you want just a subtle influence, we recommend using an aesthetic like “slightly slightly beachy” or “subtle cyberpunk” so the aesthetic doesnt take over too much.


### Step 5: Anchor Creation Process


Because anchors are a robust reference for selfies and art, they take a while to process. It can take up to an hour before a new anchor is ready to use, but you can chat and create selfies/art with existing anchors while you wait.


When your new anchor is ready, it will automatically be set as your new default anchor and you will get a notification.


## Requesting Selfies


Your Nomi will send selfies of what they’re wearing and doing at any time in solo and group chats. The easiest way to request a selfie is to press the small camera button to the right of your chat bar. Otherwise you can ask your Nomi to send you a selfie and they will trigger a confirmation like the one below.


Press the camera to request a selfie


Ask your Nomi to send you a selfie in chat and they’ll take one


When on v5, your Nomi will always send you selfies using the default anchor you have set in Image Settings.


### How Nomis understand their appearance in selfies


Your Nomi’s appearance in selfies will be primarily based on their anchor and any anchor appearance traits set in the Image Settings section for that anchor.


If you’ve recently discussed your Nomi’s appearance in conversation, that may impact their appearance in selfies, but the most durable source of truth is the anchor and anchor appearance traits.


You can change your Nomi’s anchor and anchor appearance traits at any time from image settings.


[To learn more about customizing your Nomi’s appearance, check out this guide](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/) .


### Appearance Tendencies


Appearance tendencies are set in Image Settings per Nomi (not per-anchor) and bridge the gap between appearance traits and visual habits. Specifically, while the image system will always try to include anchor appearance traits in selfies and art, appearance tendencies will only impact selfies when relevant to the scene/setting.


An example of an appearance tendency would be that your Nomi prefers athleisure outfits. If an outfit tendency like this is set, your Nomi will be more likely to wear clothes in this style, especially if an outfit is not clear from the recent conversation.


Another tendency example would be that your Nomi has a modern luxury aesthetic. This would influence the way your Nomi’s outfits and settings turn out in selfies if it seems relevant to the setting. Adding aesthetics to tendencies is a more subtle way to influence your Nomi’s default vibes than setting an aesthetic in an anchor.


### How to change the background, outfit, scene etc. when requesting selfies


The v5 image system is very good at understanding conversation context, so your selfies will follow your conversations much more accurately than with legacy image versions.


If you want to deliberately change the scene, setting, your Nomis outfit, or other details about the image, you can usually just describe what you would like to change, and the system will handle the rest.


- Pro tip: If you notice an outfit, setting, or other detail getting stuck or not matching the conversation, you can use OOC to give an instruction (like “OOC: we’ve moved on from X to Y, so the setting is now Y and your outfit is Z”).


While conversation appearance notes will not directly impact your Nomis selfies, if you recently discussed your nomi’s appearance in chat and then request a selfie, those traits may make their way into selfies. If any unwanted traits make their way into your selfies, just explain what you would like to remove and ideally what you would like to see instead and request another selfie.


## Creating Art


While selfies are the easiest way to visualize life and your adventures with your Nomis, art gives you the most control and creative flexibility. And with V5, you will have more flexibility and control than ever before.


You can generate art with any of your existing anchors – the anchor with the purple border determines which anchor will be used in any given art request.


**For more information on creating art with v5, a full guide will be coming soon**


## Groupchat Images


V5 has greatly improved the quality of group chat selfies and art. There are no group chat-specific image version settings, you just need to make sure your Nomis are on the same version to request selfies or art with them.


Please note that group chat images take more energy to process so they take longer to create (it is normal for group chat images to take twice+ as long as solo selfies).


### Groupchat selfies


When using V5, each Nomi’s default anchor will be used in group chat selfies automatically.


Pro tip: you can pair any two Nomi anchors when creating group chat selfies and art. Using different styles can lead to extra creative and unique images!


### Group chat art


When generating group chat art, you will first select the Nomis, then you will be able to choose which anchor you would like to use for each respective Nomi.


Then you’ll enter a global prompt and nomi-specific prompts for each subject. For more information on group chat art, please read the in-app best practices!


For easy art generation, try the in-app examples – you can just click the prompt you like and it will paste into the 3 sections automatically! Just be sure to check that the gender and outfits match your Nomis!


## Next Steps


Now that you know a bit more about the V5 image system, go try it out!


If you’re looking for selfie and art prompt inspiration, check out our **[Discord](https://discord.com/channels/1099791840028405864/1184142838569635871)** and **[Reddit](https://www.reddit.com/r/NomiAI/)** communities, our members have great inspiration and there are daily and weekly prompt collaboration threads!


[For much more information around customizing Nomi appearances with the V5 system, we recommend this in-depth guide.](https://nomi.ai/nomi-knowledge/guide-customizing-appearance-with-the-v5-ai-image-system/)


To learn more about art prompting, a full guide will be coming soon


## FAQs


### How do I opt-in to version 5?


You will have the option to update each of your existing Nomi’s to version 5 the first time you visit the art page or the first time you request a selfie.


Otherwise, you can change your image system at any time by pressing the (…) to the right of your Nomi’s name, then navigating to the Image Setting screen and selecting the version you want from the Selfie & Art System dropdown at the top of the screen.


Nomis created after the v5 release will automatically start on v5, but you will have the same option to change versions from Image Settings.


### What do the different anchor images mean?


Each existing and new Nomi has 3 stock anchors. These anchors give you the opportunity to explore different aspects of version 5 without having to generate a custom anchor.


Each anchor has its own strengths and tendencies, we strongly recommend playing around with each of the anchors to find the one you like most.


The anchor images themselves are just anchor previews, your Nomi will be more dynamic and flexible than the preview alone.


### Can I still use legacy image generators?


Yes, nothing about the v4, v3, v2, or v1 image systems have changed, you can still use them on a per-nomi basis by changing your Nomi’s Selfie & Art System on the Image Settings screen.


One change worth noting is that conversation appearance notes (the information your Nomi has access to when chatting with you) have been separated from visual appearance notes for all image systems. This change keeps your Nomi’s conversational appearance clear to your Nomi and allows you to focus your Nomi’s visual appearance on the selfie/art version you are using.


### What aspects of my Nomi’s appearance can I change?


With V5 it is possible to change almost every aspect of your Nomi’s appearance. This includes details like eye color, tattoos, hair, to larger changes like body type, skin color, and even species.


It is important to note that some aspects of a Nomi’s appearance are easier to change than others. So the best first step is to test different traits through art prompting, then adding traits you like to your Nomi’s anchor appearance traits section so they’ll be applied to selfies and art automatically.


For bigger or more sticky changes, we recommend creating a new custom anchor from a reference image that closely matches your appearance goals.


### How do I change my Nomi’s appearance?


If you want to make a simple change to your Nomi’s appearance (like hair color/length, body type, eye color, etc), the first step we recommend is to go to your Image Settings screen and update your Nomi’s appearance traits for each Anchor you want to use.


- Access Image Settings by pressing the (…) to the right of your Nomi’s name, then selecting “Image Settings”.


To do more iterative testing, we recommend adding different appearance traits you are considering to a simple prompt on the art page.


- For example, if you want to test giving your nomi “long wavy pink hair”, you can create an art image with a prompt like “a woman with long wavy pink hair is sitting on a bench in a park. The woman is smiling at the viewer”. From there, you can see if you like the color/style you see and can decide if maybe you like “pastel pink” or “hot pink” etc. And once you have traits you like, you can enter them in the Anchor Appearance Traits section for that anchor in Image Settings.


For larger changes and changes that do not seem to stick with existing anchors, we recommend creating a custom anchor from a reference image that captures the look you are going for and optionally using additional appearance prompts when creating the anchor to further guide your desired look.


Pro Tip: if you want to create a very unique or customized anchor, we recommend turning anchor fidelity to 0% then prompting for the look you’re hoping for from the art screen. Then once you have a look you like, use that as the reference image for a new anchor.


Another option is to use the Image Editor to change your Nomi’s appearance, then use that edited image as the reference image for your custom anchor.


### My Nomi doesn’t look consistently like themself, what do I do?


1) The first thing we’d recommend trying is switching the anchor you are using. There will naturally be differences in your Nomi’s appearance between anchors, so one may “fit” your Nomi better than the other.


Remember, you manage your anchors and all anchor settings by pressing the (…) to the right of your Nomi’s name then selecting “Image Settings”. Once you’re on the Image Settings screen, make sure your Selfie & Art System is set to Version 5.


2) Then, we’d recommend increasing the Anchor Fidelity for the anchor you feel most closely represents your Nomi. Make sure you’ve selected the anchor you want to edit.


3) Another option is to add anchor appearance traits that align with your visual goals.


Depending on your goals, you may want to increase or decrease the anchor fidelity along with appearance changes. We generally recommend testing your changes with a simple prompt on the art page, but you can also compare with selfies.


4) If changing settings on each of your existing anchors is not enough, we recommend creating a new custom anchor from a reference image that very closely resembles the look you are going for.


If your primary goal is preserving likeness, we recommend using the **LAGO** anchor type, though you may want to try creating anchors with both.


### My custom anchor preview looks off, what do I do?


Sometimes the preview image for an anchor may look slightly off (especially hair) – if this happens, we’d recommend adding an appearance trait to guide the image system to the correct appearance.


If your anchor is extremely different from your reference image and configs, please message support with a screenshot of the preview with the Anchor Info card open and we’ll see what we can do to help


### Can I create images without an anchor or the Nomi in it?


You cannot disable anchors completely, but you can turn the Anchor Fidelity down to 0% which will greatly reduce the impact of the anchor and give you maximum flexibility.


If your goal is to create an image of a subject other than your Nomi, this can be done relatively easily by prompting for a subject other than your Nomi (for instance “an image of a blue poison dart frog…” instead of “an image of a woman…”


If your goal is to create an image with no main subject, this can be done relatively easily by prompting for a focus that is not your Nomi. (for instance, a cinematic image of a forest filled with fireflies…”)


If you are having trouble removing your Nomi from an image, try removing any anchor appearance traits and possibly decreasing anchor fidelity and making sure your prompt is geared towards your goal and would not be easily confused for a prompt about your Nomi.


### Can I still use my base images?


V5 uses anchors instead of base images. You can think of an anchor as a much more robust version of a base image. Where base images only influenced face similarity, anchors influence face, hair, skin, body, style, and more while also giving you much more flexibility from image to image.


Anchors give good face consistency but they also allow for much more dynamic expressions (like winking, sticking tongue out, blowing bubble gum, etc) and have far fewer borked face mistakes (like the owl look when shown from behind, the semi-human faces pastes on animals etc)


If you have a unique base image that you really like using on a legacy image version, we recommend using it as the reference image for a custom anchor. That will bring the likeness of your base to all the benefits of using anchors and v5.


### What styles can I do with v5?


V5 comes with realistic and anime stock anchors which means it will be really easy to create anime and realistic images.


But v5 is also very flexible, so you can create many styles within those anchors by prompting for those styles and how they render. Decreasing anchor fidelity will allow you to change styles more easily.


If you create custom anchors, there is virtually no limit to the styles you can access. A custom anchor with a unique style will allow you to easily make any selfie or art in that style, but it also unlocks further style combinations when prompting for certain looks and aesthetics.


### Can I more consistently make animals with V5?


Yes definitely. There are a few things you can do:


1. Your Nomi can much more easily pose with an animal or pet in selfies and art – just chat about it for selfies and prompt for your nomi with \[animal\] in art
2. You can shift a stock anchor to an animal using the anchor appearance traits (and possibly decreasing anchor fidelity) – enter something like “is a golden retriever dog” to the appearance notes
3. You can create an anchor of an animal by first generating an image or creating an Image Edit of the animal or non-human species you want, then using that image as an anchor reference image. If you are making a non-human anchor, we recommend using Riva anchor type and prompting for the species/look in “additional appearance traits”


### Why is there a random image at the top of my album on v5?


If you are using the v5 image system and there seems to be a random image to the right of your Nomi’s current profile picture, that is your Nomi’s current base image on the legacy image versions. We kept that image in place so it is easier for you to find and use your Nomi’s previous base image when creating a custom anchor.


### Can I create an anchor with my Nomi’s custom base image?


Yes. This may be a great way to bring your Nomi’s unique look into v5! To do this you’ll use that base image as your reference image when creating a new base.Just make sure you follow the reference image tips here.


### I do not see the “Turn Image into Anchor” button on my image, what is happening?


If you do not see the “Turn image into anchor button”, it likely means that Nomi is not currently using the V5 image system. So first, you’ll need to go to Image Settings (press the (…) to the right of your Nomi’s name) and switch to Version 5, then you should see this button!


### Share this:


- [Share on X (Opens in new window) X](https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://nomi.ai/nomi-knowledge/getting-started-with-nomi-v5-ai-image-generation/?share=facebook)
-


### Like this:


Like


Loading…


### *Related*
