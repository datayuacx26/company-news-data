---
schema_version: "1.0.0"
document_id: "bcec8cb23004ab46f7030958d936857927fb35f1d195678c75e8a7954922132e"
company_key: "yc-mocha"
company: "Mocha"
source_id: "yc-mocha-rss-d0ffed2c2227"
canonical_url: "https://getmocha.com/how-to-generate-images-ai"
published_at: "2026-01-19T00:00:00+00:00"
first_seen_at: "2026-07-24T11:28:42.148680+00:00"
fetched_at: "2026-08-20T02:22:16.718062+00:00"
content_hash: "sha256:091217a51fc0aa3250c781c3ff08742457e6e2fd4b66abcf10883a99361a7e4a"
---

# How to Add AI Image Generation to Your App

import PromptBox from '@/components/PromptBox';


In this guide, we're going to step through the process of setting up a Mocha app that has image generation capabilities using Google's Gemini API. You'll learn how to create an app that generates professional product mockups, logos, or any other images from text descriptions.


This is a powerful feature that can greatly enhance your app's functionality. Using Gemini's image generation API, we're going to set up a practical app that generates product mockups for entrepreneurs and marketers to use in their pitch decks and marketing materials.


## What is Nano Banana Pro?


Google's Gemini API provides powerful image generation capabilities through models like Gemini 2.5 Flash (Nano Banana) and Gemini 3 Pro Preview (Nano Banana Pro). These models can generate high-quality, photorealistic images from text descriptions, making them perfect for:


- **Product mockups** : Create professional product shots for e-commerce and marketing
- **Logo design** : Generate logos with accurate text rendering
- **Commercial photography** : Studio-lit product photographs
- **Conceptual artwork** : Visualize ideas and concepts


Gemini image generation supports text-to-image, image editing, and multi-image composition. All generated images include SynthID watermarks for authenticity tracking.


For more information, visit[ai.google.dev](https://ai.google.dev/gemini-api/docs/image-generation) .


## Starting with the Right Prompt


The initial prompt you give Mocha is crucial for getting your project started on the right track. When building an app that uses image generation, you need to be specific about:


- **What types of images you want to generate** : Clearly describe the image types and use cases
- **How the images should be used** : Explain how generated images fit into your app's functionality
- **Key features** : List the main features you want, including image storage, download capabilities, and style options
- **Technical requirements** : Mention any specific models or services you want to use (like Gemini Nano Banana Pro)
- **Design preferences** : Describe the UI/UX style you're aiming for


Here's the example prompt used to build the mockup generator app:


<PromptBox client:load prompt={\`Create a product mockup generator app for entrepreneurs and marketers.


I want to describe a product idea and get professional-looking mockup images I can use in pitch decks and marketing materials.


Features needed:


- Text input where I describe my product (what it is, style preferences, setting/context)
- Generate multiple mockup variations from a single description
- Choose from different mockup styles (minimalist, lifestyle, studio shot, in-context)
- Save generated images to a gallery
- Download images in high resolution
- Ability to regenerate or refine with additional instructions


Use Gemini Nano Banana Pro for image generation. The UI should feel premium and professional since this is a tool for business users creating presentation materials.\`} />


Notice how this prompt includes:


- A clear description of the app's purpose and target users
- Specific features broken down into bullet points
- Technical requirements (Gemini Nano Banana Pro)
- Design preferences (premium and professional UI)
- Use case context (pitch decks and marketing materials)


This level of detail helps Mocha understand exactly what you're building and sets a solid foundation for the rest of the development process.


This example will show you how to get the image generation technology working and how to troubleshoot common issues that may arise during setup.


## Setting Up the Gemini API Key


During the build, you should see a prompt to add a secret for the Gemini API Key. Click the walkthrough button if you need guidance on how to add this key.


To get your API key:


1. Visit[aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with your Google account
3. Click "Create API Key" to generate a new key
4. Copy the API key


Paste this API key into the secret field in your Mocha app. The app will store it securely and use it to make requests to the Gemini API.


## Enabling Billing for Image Generation


**Important** : Unlike some other APIs, Gemini's image generation features require a billing account to be linked to your API key, even for free tier usage. After adding your API key, you may encounter quota errors when trying to generate images.


If you see an error indicating that the quota limit has been exceeded or that the API key doesn't have access to image generation, you'll need to enable billing:


1. Go to[ai.google.dev/usage](https://ai.google.dev/usage) to check your API usage and billing status
2. Link a billing account to your Google Cloud project (even if you stay within free tier limits)
3. Ensure your API key has access to the image generation models


Once your billing account is connected, image generation should work properly. You can still stay within the free tier limits, but having billing enabled is required for the feature to function.


## Testing Image Generation


Wait until the app finishes building its initial version. This may take several minutes to complete the first loop of implementations. When done, test it out by generating your first image with a descriptive prompt.


Try a prompt like:


```text
A minimalist ceramic coffee mug on a clean white background with soft studio lighting. The mug has a modern design with subtle texture. Professional product photography style.


```


The app should generate a high-quality mockup image that you can view, save, and download.


## Debugging Image Generation Issues


If you encounter issues with image generation, here are common problems and solutions:


### Issue: Images Not Displaying Correctly


You may find that generated images show as broken or display "invalid data". This usually indicates that the image data isn't being extracted correctly from the API response.


> **Example prompt to the AI:**
>
>
> "I tried again after connecting my billing account and the image that it generates has` <img src="data:image/png;base64,undefined">` and the image shows invalid data as a label. Downloading it resulted in a 'check internet connection' in my downloads. Please make sure the image is correctly rendered from the API. Add console logs to help with the debug process."


The AI can help debug these issues by:


1. Adding console logging to inspect the API response structure
2. Checking how the image data is extracted from the response
3. Verifying the base64 encoding and data URI formatting


Open the logs in the Mocha interface by pressing ⌘ B (or Ctrl + B on Windows) to see what the API is returning and help diagnose the issue.


### Issue: Quota Exceeded Errors


If you see quota errors, check:


1. Your billing account is properly linked (as mentioned above)
2. You haven't exceeded the free tier limits at[ai.google.dev/usage](https://ai.google.dev/usage)
3. Your API key has the correct permissions enabled


The app should display helpful error messages when quota issues occur, making it easier to diagnose what's wrong.


### Issue: Wrong Model Being Used


You may need to specify which Gemini model to use. You might need to switch from` gemini-2.5-flash-preview-image` to` gemini-3-pro-image-preview` (also known as Nano Banana Pro) for better quality results.


> **Example prompt to the AI:**
>
>
> "Let's use the gemini-3-pro-image-preview model for image generation instead of the current one."


Make sure your app is configured to use the model that best fits your needs. Nano Banana Pro generally provides higher quality images, while the Flash model is faster.


## Wrapping Up


In this guide, we walked through the process of setting up a Mocha app that has image generation capabilities using Google's Gemini API. We covered:


- How to craft an effective initial prompt
- Setting up the Gemini API key
- The importance of enabling billing (even for free tier)
- Debugging common image generation issues


You can see the full source code for the app we built by visiting the[MockupForge Example App](https://mockupgen.mocha.app/) .


---


**Ready to add AI image generation to your app?**


- [Start building for free with Mocha →](https://getmocha.com/)
- [Read the Mocha documentation →](https://docs.getmocha.com/)
