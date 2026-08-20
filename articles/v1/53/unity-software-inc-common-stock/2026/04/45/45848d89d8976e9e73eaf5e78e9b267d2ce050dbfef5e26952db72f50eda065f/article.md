---
schema_version: "1.0.0"
document_id: "45848d89d8976e9e73eaf5e78e9b267d2ce050dbfef5e26952db72f50eda065f"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/unity-ai-cubemap-generator"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8b346e311b874b7c72a1b4560b0cbf2d23960773dad5f84e3c91b493a623deb4"
---

# Unity AI Cubemap Generator: Create skyboxes and environment reflections from a text prompt

***Today’s entry in our AI tools in beta series explores how you can quickly create cubemaps for your prototype environments using the cubemap generator.***


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


Unity's cubemap generator is a generative AI tool built into the Unity Editor as part of the AI tools in beta. It lets you create seamless, 360º cubemaps from text-based prompts or reference images – ready to use as skyboxes, environmental reflections, or lighting sources directly in your scene – without leaving the Editor or opening a third-party tool.


This blog covers every stage of the cubemap generator workflow: opening the window, choosing an AI model, writing prompts, using reference images, managing generations, upscaling, and applying the result to your scene.


The Unity Editor with the cubemap generator open


## **What is a cubemap and why generate one during prototyping?**


A cubemap is a collection of images mapped to the six faces of a cube that surrounds a scene. Unity uses cubemaps to render skyboxes – the sky and distant environment visible around a scene – and to drive environmental reflections on materials and surfaces. Reflection probes, skybox materials, and many lighting workflows all rely on cubemaps.


During the prototyping stage, you often need placeholder skyboxes and reflection environments to validate lighting mood, test material responses, and block out the visual feel of a scene before final assets are ready. The cubemap generator can help accelerate that process by producing usable placeholder cubemaps from a text description.


As with all AI tools’ generators, the assets produced are prototyping tools for placeholder content. Generated cubemaps are replaced with human-made final artwork before a project ships. They are a starting point for iteration, not a substitute for the work of artists and environment designers.


## **Opening the cubemap generator**


The cubemap generator opens automatically when you create or select a cubemap asset configured for AI generation.


To open it:


1. In the Project window, right-click an empty area.


2. Select **Create** > **Rendering** > **Generate** **Cubemap** .


3. A new cubemap asset appears in the Assets folder. Rename it.


The cubemap generator window opens alongside the new asset.


## **The Generate window – controls and options**


The Generate window is the main interface for defining what the cubemap generator produces. The following options are available:


### **Model selection**


Select the **Change** button to open the Select AI Model window and choose the model that best suits your cubemap type. Models are named to describe their output style – for example, Cinematic or Cartoon. Choosing the right model for the intended environment (a photoreal forest versus a stylized fantasy world) affects the look of every generation.


### **Prompt**


Enter a text description of the environment you want the cubemap to represent. Be specific about the setting, time of day, weather, and mood. For example: "Medieval path in a forest next to a castle."


### **Negative Prompt**


The Negative Prompt field lets you describe elements you want to exclude from the result. If the model tends to include elements that do not fit your scene – such as people, buildings, or certain color tones – entering those terms here steers the generation away from them.


### **Count**


The Count slider sets how many cubemap variations to generate in a single run. Generating multiple variations in one pass lets you compare results and select the most suitable one without re-entering the same prompt.


### **Custom seed**


Enabling Custom Seed and entering a specific number locks the generation to a repeatable starting point. This is useful when you want to produce consistent results across multiple prompts – for example, when iterating on the same environment with small prompt changes.


The Generate window


### **Add More Controls To Prompt**


Selecting **Add More Controls To Prompt** opens a panel for attaching reference image operators that guide the generation. See the "Generating from a reference image" section below.


## **Generating from a reference image**


In addition to text prompts, the cubemap generator supports reference image inputs that influence the visual character of what is produced. This is useful when you have an existing concept image, a painted sky reference, or a screenshot of a scene with a mood you want to match.


To add a reference image:


1. In the Generate window, select **Add More Controls To Prompt** .


2. In the **Select which operator to Add** window, choose one of the following reference types:


**Image** **Reference** – Uses the reference image to define the base visual appearance of the cubemap – color palette, lighting tone, and overall aesthetic.


**Composition** **Reference** – Guides the spatial arrangement of scene elements. Use this when you want generated content to reflect a particular layout – for example, the horizon line, a dominant landmass, or a specific distribution of sky versus ground.


**Depth** **Reference** – Adds depth data for generating environments that have a clear sense of near, mid, and far elements. This is particularly suited to reference images of natural environments with layered depth.


3. Select a reference image from your Assets by browsing the Select Texture 2D window.


4. Adjust the Strength slider to set how much the reference image influences the result. A lower value means the text prompt has more control; a higher value means the reference image has more influence.


5. Select **Generate** .


Reference images are saved with the generation data and can be reused in later sessions.


## **The Generations panel**


Every cubemap produced by a generation run appears in the Generations panel. Thumbnails show a 360º preview of each result alongside the prompt and model used.


Hovering over a thumbnail displays the generation details.


Right-clicking a thumbnail opens a context menu with the following options:


- **Select** – Highlights the cubemap in the Project window.
- **Promote to current asset** – Replaces the current cubemap asset with the selected generation. Use this when you have found a result you want to commit to the asset slot.
- **Promote to new asset** – **** Creates a new independent asset from the selected generation, leaving the original cubemap asset unchanged.
- **Reveal in Finder** – Opens the file location for the selected generation on your computer.


**Show Generation Data** displays the model, prompt, and reference image settings used to produce this cubemap. From the Generation Data window you can:


- Select **Use All** to copy every setting back into the Generate window for a follow-up generation.
- Select **Use** to copy only the selected individual settings.
- Select **Copy** to immediately generate a new cubemap using those settings.


## **Upscaling a generated cubemap**


Initial generations are saved as .jpg preview files. To produce a high-resolution version suitable for use in skyboxes, reflection probes, or large-scale environments, use the Upscale feature.


To upscale:


1. In the Generate window, select the **Upscale** tab.


2. Review the cubemap shown in the Base Image section.


3. Select **Upscale** .


The upscaled version is saved as a .png file. Upscaling enhances resolution and clarity while preserving the content of the image.


The cubemap generator window with the Upscale tab active


## **Applying a cubemap to your scene**


Once you have promoted a generated cubemap to an asset, you can apply it to your scene as a skybox or assign it to a reflection probe.


For instructions on assigning a cubemap as a skybox, refer to the[Unity Manual Sky documentation](https://docs.unity3d.com/6000.0/Documentation/Manual/sky-landing.html) .


Generating a cubemap asset


## **Using the cubemap generator with the in-Editor AI assistant**


The cubemap generator can also be invoked and guided through the Unity AI Assistant window using natural-language prompts. In Agent mode, the Unity AI Assistant understands your full project context – including your scene setup, active GameObjects, existing materials, and installed packages – and can use that context to suggest or initiate cubemap generation workflows.


For example, you can prompt the Unity AI Assistant: “Generate a night-time forest skybox cubemap for this scene” and it will open the appropriate workflow, pre-populate relevant settings, and propose a generation based on the context of your project.


## **More on AI tools**


If you’re interested in reading more about what’s available in the Unity AI Open Beta, we invite you to read other articles in this ongoing series:


- [Introducing AI tools in beta](https://unity.com/blog/unity-ai-how-to-get-started)
- [The in-Editor AI assistant, explained](https://unity.com/blog/unity-ai-assistant-ask-plan-agent-mode-explained)
- [How to get started with Unity MCP](https://unity.com/blog/unity-ai-mcp-how-to-get-started)
- [Using Unity AI’s UI Generator](https://unity.com/blog/unity-ai-ui-generator)
- [Create props with Unity AI’s 3D Object Generator](https://unity.com/blog/unity-ai-3d-object-generator)
- [Using Sprite Generator to create 2D sprites, icons and spritesheets](https://unity.com/blog/unity-ai-sprite-generator)


## **Try AI tools today**


AI tools in beta are available now for all Unity 6 developers. Sign up for a free trial, explore the AI Assistant, connect your preferred tools via the AI Gateway, and start experimenting with what your development workflow looks like with a project-aware AI agent built in.


Sign up and learn more about plans, pricing, and data privacy at unity.com/features/ai


Full documentation is available in the Unity AI docs linked from the Editor or at[docs.unity3d.com](https://docs.unity3d.com/) .


***The in-Editor AI assistant is currently in open beta.** As such, features, behavior, and availability described in this post are under active development and may change, be limited, or be discontinued without notice.*
