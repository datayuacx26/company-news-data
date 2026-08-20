---
schema_version: "1.0.0"
document_id: "2446826446f4ac2ae4fcd1ddba19222cdf7ee32770529f28be894e83da084354"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/unity-ai-3d-object-generator"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:aa8e573b007cae8e1efe9303a59b58f06d35cb8dbf4060c0e3b24cd952a96976"
---

# Unity's AI tools in beta: Create props with the 3D Object Generator

***Unity’s AI tools are officially in open beta. Today we’re covering how to generate 3D meshes from text prompts and reference images – directly inside the Unity Editor.***


Creating 3D assets for prototyping can be a time-consuming and costly process. Reference art takes time to find, model, and optimize – and by the time it exists, the design has often already changed.


The 3D Object Generator included with Unity’s AI tools in beta removes that step. Describe what you need, optionally drop in a reference image, and the generator returns a placeholder asset in seconds.


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


## **What the 3D Object Generator produces**


The 3D Object Generator creates static 3D mesh prefabs. It’s well-suited for simple, single-part props: household items, environment details, decorative objects, and any asset that needs to exist in three dimensions but doesn't need to move independently. The output is a standard Unity prefab containing a mesh and materials, and it drops into any Scene the same way as any other asset.


3D-generated assets for a fantasy game prototype


## **Two ways to generate a 3D object**


You can generate a 3D object directly from the Generators window – for more control over inputs and model selection – or using the in-editor AI Assistant using natural language.


### **Using the Generators window**


**1.** Open the 3D Object Generator. In the Unity Editor, select **AI** > **Generate** **New** > **3D** **Object** . A new mesh file is created in the **Assets** folder.


**2** .Open the generator. Double-click the new file to open the 3D Object Generator window.


**3. (Optional)** Select a model. Select **Change** to choose your preferred AI model.


**4.** Provide a reference image. You can:


**i.** Select an existing image from your project


**ii.** Import an image from disk


**iii.** Generate one using another Generator (for example, using the Sprite or Texture Generator)


**5.** Drag the reference image to the Image Reference field.


**6.** Select **Generate** .


Drag a reference image into the Image Reference field and select Generate to produce a 3D object prefab.


### **Using the in-editor AI Assistant**


The in-editor AI Assistant can generate 3D objects from a natural-language prompt alone. For example: “Create a 3D model of a cereal box.”


If you don't provide an image, the asisstant first generates a reference image and then uses it to produce the 3D object. If you include an image in the conversation, the assistant uses it directly – handling any required preprocessing, such as background removal, before generating.


The in-editor AI assistant can handle the full pipeline – from reference image generation to 3D object output – from a single prompt.


## **Reference images and output quality**


For higher visual quality and more predictable results, use a reference image rather than text alone. A clear, isolated subject on a plain or transparent background tends to produce the best output.


An AI-generated model of a pirate ship on a white background


## **Managing generated assets**


All assets produced by Unity’s AI Generators carry embedded metadata flagging them as AI-generated. This makes them easy to identify in the Project window and ensures you can track them for app store declarations. Generated 3D objects used for prototyping can be replaced with final art at any point without disrupting your scene layout – the prefab reference stays in place.


Generators can be disabled entirely in the Unity Dashboard.


A collection of assets generated with Unity's AI tools in beta and tagged with an "AI-generated" label


## **More on Unity’s AI tools in beta**


If you’re interested in reading more about what’s available in the beta test of Unity’s AI tools, we invite you to read other articles in this ongoing series:


- [Introducing Unity’s AI tools in beta](https://unity.com/blog/unity-ai-how-to-get-started)
- [The in-editor AI assistant, explained](https://unity.com/blog/unity-ai-assistant-ask-plan-agent-mode-explained)
- [Using the UI Generator](https://unity.com/blog/unity-ai-ui-generator)
- [How to get started with Unity MCP](https://unity.com/blog/unity-ai-mcp-how-to-get-started)
- [Create PBR materials from a text prompt using the Material Generator](https://unity.com/blog/unity-ai-material-generator)
- [Create skyboxes and environment reflections with the Cubemap Generator](https://unity.com/blog/unity-ai-cubemap-generator)
- [Using Sprite Generator to create 2D sprites, icons, and spritesheets](https://unity.com/blog/unity-ai-sprite-generator)


## **Try Unity’s AI tools in beta today**


The AI tools beta is available now for all Unity 6 developers. Sign up for a free trial, explore the in-editor AI Assistant, connect your preferred tools via the AI Gateway, and start experimenting with what your development workflow looks like with a project-aware AI agent built in.


Sign up and learn more about plans, pricing, and data privacy at[unity.com/features/ai](https://unity.com/features/ai)


Full documentation is available in the AI docs linked from the Editor or at[docs.unity3d.com](https://docs.unity3d.com/) .


***Unity’s in-editor AI assistant is currently in open beta.** As such, features, behavior, and availability described in this post are under active development and may change, be limited, or be discontinued without notice.*
