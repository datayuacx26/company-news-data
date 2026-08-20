---
schema_version: "1.0.0"
document_id: "fc78cb96ed82e01bdd805d1f8efe5ae027fa0d9799db36332230420dc7f4e1ff"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids"
published_at: "2025-08-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:a6198941cb3bd00a24d71cfcab7e8479c250658bf28777081b18a2c28fdd3e70"
---

# Graphics and rendering tips from Survival Kids

This summer, Unity released the first game developed end-to-end in-house, an update on the co-op family game *[Survival Kids](https://www.konami.com/games/survival_kids/) ,* in partnership with KONAMI. The game was built by a small internal team of about 20 people at its max, so the team had to find innovative ways to stay within the scope of the project and release timeline with limited resources, just like any[indie studio](https://unity.com/campaign/indie) . In this post, we dig into how we created the game’s visual frame and rendering.


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


Defining the visual identity


We wanted to achieve something visually interesting. Our goals were very artistic, but we also wanted to make it very cheap in terms of performance since we didn’t know what kind of device capabilities we’d be working with at first.


The first part of the project was just visually exploring – we had an art diorama that we were using to show how we imagined the art to be. Part of that is a very stylized lighting setup, including customized shadows.


We went with the[Universal Render Pipeline (URP)](https://unity.com/features/srp/universal-render-pipeline) since it has a great track record for performance on a wide range of devices, and it’s relatively easy to create any new features we need to make it hit the game’s visual targets. The rendered frame is very close to vanilla URP in Forward mode, since the game mostly has only one light source, the sun. We have a few modifications here and there, like the custom shadows, ambient occlusion, and a couple of other custom render features, but overall it’s vanilla URP onscreen.


Gameplay in Survival Kids


The biggest addition was to the shaders to support the very specific look of the art direction since we needed to make modifications to how lighting was calculated. Making custom shaders isn’t particularly new, however we wrote our own custom Shader Graph targets to ensure that anyone could contribute. Using[AssemblyDefinitionReference](https://docs.unity3d.com/6000.0/Documentation/Manual/class-AssemblyDefinitionReferenceImporter.html) s allowed us to add project specific Shader Graph targets without needing to have a completely custom URP version. This let us stick to the vanilla URP with just our local Shader Graph targets, which worked really well for our project.


Lighting and global illumination


One of our aims was to have dynamic lighting – we wanted the option to be able to change the lighting color, intensity, etc. That meant we couldn’t easily bake lighting information using lightmaps, so we would be missing out on some of the lighting detail you would get from baking in bounce lighting / global illumination. We needed to think of different ways to balance high visual quality and good performance with a dynamic lighting approach, since it is normally more expensive. This led us to use LightProbes initially and also relying more heavily on Ambient Occlusion (AO) to help ground objects.


Lighting and global illumination information


Because we knew that global illumination was going to be very important for this project, we initially implemented a custom solution that would update LightProbes at runtime. But then when we moved to Unity 6, the team really wanted to switch over to Adaptive Probe Volumes (APVs) because the visual quality was considerably better than the system we’d knocked together while having comparable performance impact. When you have the option of upgrading from something good to something *really* good that’s high quality and performant, you just switch.


Ocean


The ocean was heavily based on a Unity URP demo project[Boat Attack](https://github.com/Unity-Technologies/BoatAttack) , but with a more stylized look. One of the things we really wanted to do was have wake coming off from the island and other elements in the water. This is usually implemented by using the depth buffer to work out the coastline by distance – but we don’t have a coastline, really, we have a Whurtle-island.


Signed Distance Field (SDF) creates wakes at a set distance from the coastline.


With the Whurtle-island, you have a sudden dropoff, and there’s not enough depth falloff for the effect, especially taking into account the terrain submerged under the water. The best idea we came up with was to use a signed distance field, or SDF – it’s basically a texture that encodes the signed distance of an object, or, in our case, the coastline. This way, we can start the wake at a certain distance from the coastline, then use sine wave and some distortion textures to give it an interesting look.


In the end, we had an Editor tool that bakes the signed distance for the coastline based on four set water heights. Then we did some blending and lerping between them for a rough approximation of where the coastline actually was, since the water level in most levels changes depending on the player’s progress. We relied on this pre-baked SDF information for several different effects, from adjusting ocean wave height to adding foam, wake, and caustics.


Breakdown of a frame


High-level breakdown of a rendered frame, where the passes marked in red are customized


Visual interaction


For visual interactions, a capsule is rendered from a top-down view around anything we needed to track the position of, like players, carriable objects, tools, etc., into a RenderTexture. The texture is based in world space with a sliding window as the player’s camera moves around.


We generate an offset (red, blue) from the center of the capsule, as well as worldspace height information (green). In the alpha channel, we store a falloff value for the strength. That’s then used by different shaders for creating effects such as vegetation bending, animated ripples on water surfaces, or darkening the terrain a bit to create a very soft shadow effect.


Shaders are applied to a RenderTexture “capsule” around each player and carriable object to create effects that make them seem more grounded in the world, such as shadows or wake in the water.


Depth and dither pre-pass


For a performance optimization, we used a depth prepass, which fills the depth buffer before we render objects normally, reducing the cost of rendering those objects due to early depth test rejection.


A stencil used for blurring in the SMAA Pass and pre-fill depth with dither pattern to see behind geometry


We dealt with dithered objects separately in a custom pass because we need to render them differently depending on their state and which player is viewing them. They’re in a different GameObject layer that is excluded from Opaque Layer Mask in the renderer so they’re not automatically rendered, and this means we need to render them in a custom pass. We used MaterialPropertyBlocks to set individual values for objects and applied stencils to mark up the objects that are dithered so we can blur those sections later on. However, since this breaks SRP batching, we needed to limit its use. We decided to only apply MaterialPropertyBlocks as needed and remove them when done, restoring objects to a batchable state.


In the end, we have a whole pass that just deals with how we render that particular layer into the depth buffer. Next, we apply a stencil on the depth buffer to mark off which pixels are part of the objects that we’re fading away, and then that gets used later when we’re doing anti-aliasing.


Gradient shadows


Part of our art style was to have colored shadows with a gradient along the direction of the shadow. To achieve this, we had a custom screenspace texture generated from a RenderFeature that would sample the shadow map in world space, but also look ahead in the XZ plane to determine a shadow blend value. This is similar to a[PCF](https://developer.nvidia.com/gpugems/gpugems/part-ii-lighting-and-shadows/chapter-11-shadow-map-antialiasing) filter used in soft shadows, but in one direction. This was rendered into a downsized texture about quarter the size of the screen, and we then blended the shadow color between three colors.


A low-resolution screen space pass on the shadows generates the edge gradient colors and creates a gradient in the light direction. It then relies on bilinear sampling later on.


MSVAO (multi-scale volumetric ambient occlusion)


Unfortunately for us, the SSAO provided with URP wasn’t quite suited to our needs. While it’s a mobile-friendly implementation, for the look we were going for we needed to set the radius value quite high, which took a significant chunk of our frame budget (~4ms). Instead, we reused the MSVAO implementation from the old PostProcessing Stack v2 package, with some minor changes to make it more efficient and integrate our shadow color.


A customized version of the MSVAO from the PostProcessing Stack v2 Package, with minor adjustments to make it more performant on a target platform


Drawing the scene


*Survival Kids* has the standard rendering passes you expect in URP (Opaque, Skybox, Transparency), but we also have an additional pass to handle our dithered objects, just after the opaque pass. This is where we will actually render our dithered geometry due to the fact that geometry in this layer is not rendered in the opaque pass. We also do a depth equals test in this pass to ensure we only render where we prefilled the depth buffer.


Drawing of the geometry (depth equals test) for our dithered objects. Ambient Occlusion is disabled in this state.


For objects that are dithered, we need to disable Ambient Occlusion on them due to the artefacts that will occur due to MSVAO treating the “holes” in the depth buffer as occlusion.


SMAA + blur on dither


After the scene is rendered, we apply our anti-aliasing. Unfortunately, the areas that are dithered will trip up the algorithm (SMAA), causing visual artefacts. To avoid this, we need to deal with these areas separately. Areas that are dithered (determined by the stencil) are blurred, producing an alpha blend effect on those areas, and then SMAA is processed in the areas that aren’t dithered. This is skipped in certain circumstances, but we end up with a cleaned-up final image ready for post processing.


Post-processing and UI


We kept our[post-processing effects](https://docs.unity3d.com/Manual/urp/integration-with-post-processing.html) as cheap as possible, using just a bit of Tonemapping, Bloom, and Color Correction.


At one point, we used URP’s Blur in post-processing to soften the game behind the UI, but we replaced that with a cheaper Kawase blur RenderFeature later on. Our UI system is built on UGUI with a bit of custom rendering for the fading.


UI in Survival Kids


The way we initially set up our UI, we were fading menus in and out, but this approach caused some issues due to how the alpha is done for the UI. At first, we started rendering the UI into a separate texture via a camera, then blit that correctly so we can fade the UI into the main image, we changed this so it could be achieved using a RenderFeature rather than using an entire extra camera.


***Check out the other instalments of our blog series deep dive into*** **Survival Kids** ***production:***
***-***[" Graphics and rendering tips from](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)[Survival Kids"](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)
-["Level layout and terrain workflows in](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)[Survival Kids "](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)
-["Inside the Survival Kids multiplayer network infrastructure"](https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure)
