---
schema_version: "1.0.0"
document_id: "f8fbd1d185cfea693771c29917bee135606cb15e7bade849122bc7ddf6079e28"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids"
published_at: "2025-09-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:ad79c1663b01726525f3aa264ea1522ae10696f6ec5af2a4bb3e3deeb83b74fe"
---

# Level layout and terrain workflows in Survival Kids

**[Survival Kids](https://www.nintendo.com/en-ca/store/products/survival-kids-switch-2/?srsltid=AfmBOooHSChjib3uPi7ZjGkEBNMEZQhTBxNt0vyQzhaC0zK6VXZcCfpt) *is Unity’s first end-to-end development project, created in collaboration with KONAMI. The small team that built it tapped into decades of gamedev experience, and this blog series dives into how they approached this ambitious project. The[first instalment](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids) explored the team’s frame and rendering work, while this edition looks at[level and environment design](https://unity.com/resources/introduction-to-level-design-in-game-development-and-in-unity) .***


While mostly we’ve talked about how we achieved the looks you see onscreen, we wanted to dig into what we did to improve our workflows, making them faster so we could deliver the project on time.


Due to the team’s size and the scope of the project, we put a lot of consideration into the level layout and terrain workflows. We needed something that was easy for a level designer to iterate ideas on, but our approach also had to need minimal-to-no rework once the levels were handed over to the art team. We opted for an approach that has a very white box-like workflow for the level designers while we were already working with final game assets.


*Survival Kids* uses a set of reusable building blocks to create the main layout of the levels. It is a set of prefabs with cliff rock sides and a mesh terrain patch on top, configured in different sizes and shapes.


Terrain building blocks for level creation in Survival Kids


The cliff sides are eight separate meshes, sides, and corners, each with a simplified mesh collider. They all share the same material, which speeds up their rendering.


The exposed meshes of a cliff


This setup allows the level designer to use these building blocks to create most of the levels using the final game assets but with a fast workflow that’s very close to white-boxing, to allow quick iteration on ideas.


This terrain module “building block” approach resulted in many overlapping modules due to the organic shapes of the levels. It also led to most of the cliff sides being inside or behind other modules and never seen by the game camera. To solve this, we created an Editor tool that went through each module one by one, checking to see if any part of them could ever be seen from the game camera’s angle and removing all of the unseen ones.


Removing unseen meshes underneath terrains


We used a Houdini Engine tool to create continuous terrain meshes based on the different sections of the terrain modules.


Creating continuous terrain meshes from five separate patches


A modified version of Unity’s[Polybrush](https://unity.com/features/polybrush) package was used to paint the different materials on the terrain meshes. The modification was that Polybrush would paint a control texture, similar to how Unity’s terrain system does, instead of painting values to the vertices of the mesh.


Using Polybrush to paint materials onto terrain meshes


Ambient Occlusion is then baked into the vertex color of the terrain meshes to better visually ground the objects placed on it.


Baking in Ambient Occlusion


A custom tool was then used to distribute small objects such as grass, flowers, and small rocks onto the terrain meshes. The distribution of the objects is controlled individually using parameters like density, underlying material, terrain curvature, or proximity to larger objects. Throughout the project, we stayed with very low density to minimize the performance impact of small vegetation.


Distributing low-density small vegetation in the environment using the Mesh Scatter Tool


We then used another custom tool to split the mesh colliders of the different terrains. We did this in two ways:


- As a grid, to reduce the triangle count and volume of each mesh collider and improve the game’s physics performance


- By material, so that the game can generate different sounds and footstep VFX based on which material the characters are walking on (grass, sand, snow, ice, etc.)


Using the Terrain Mesh Splitter to distribute the mesh colliders of different terrains


***Check out the other instalments of our blog series deep dive into*** **Survival Kids** ***production:***
***-***[" Graphics and rendering tips from](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)[Survival Kids"](https://unity.com/blog/graphics-and-rendering-tips-from-survival-kids)
-["Level layout and terrain workflows in](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)[Survival Kids "](https://unity.com/blog/level-layout-and-terrain-workflows-in-survival-kids)
-["Inside the Survival Kids multiplayer network infrastructure"](https://unity.com/blog/inside-the-survival-kids-multiplayer-network-infrastructure)


***To learn more about projects made with Unity, visit the***[Resources](https://unity.com/resources) ***page.***
