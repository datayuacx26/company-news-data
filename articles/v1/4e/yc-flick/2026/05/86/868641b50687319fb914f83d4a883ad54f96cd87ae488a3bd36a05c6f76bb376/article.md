---
schema_version: "1.0.0"
document_id: "868641b50687319fb914f83d4a883ad54f96cd87ae488a3bd36a05c6f76bb376"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/behind-ser-loras"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-21T20:27:22.551209+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:5eedfc434befa0acc6f488e292bb3f173a830f8b6db7e5f56656e4bf81c3adb2"
---

# Behind “Ser Loras”

I have recently been developing a short film project, tentatively titled *Ser Loras* . The narrative centers on a diminutive knight—the creation of a young girl's imagination—who awakens within an empty house. Burdened with a scroll, he navigates a world where blades of grass transform into dense forests and bedframes loom like treacherous cliffs. His quest is singular and intimate: to bridge the gap between a daughter's vibrant drawings and her father's preoccupied reality.


The most compelling aspect of this project is the manipulation of scale. Through the eyes of *Ser Loras* , a mundane bedroom is reimagined as a sprawling landscape. Everyday items lose their domestic utility and gain epic proportions: a picture book serves as a staircase, a crayon box provides structural timber, and a sketch on the floor becomes a vast thoroughfare. The adventure arises not from high-fantasy tropes, but from the radical reinterpretation of the ordinary through a miniature lens.


However, this vision presents a significant technical hurdle when interfacing with AI. Conventional generative models are inherently biased toward real-world proportions, often inadvertently enlarging the protagonist or allowing the spatial logic of the environment to drift between frames. In a project defined by its smallness, even a marginal inconsistency in *Ser Loras* 's height can instantly dissolve the cinematic illusion.


The difficulty, therefore, lies not in the creation of a single aesthetic image, but in enforcing a rigorous continuity of scale across an entire sequence. *Ser Loras* must consistently inhabit a body only a few centimeters tall. A simple drawer pull must retain the presence of a massive iron ring, and a child's artwork must remain as expansive as a rug. Every household object must be believable as both a familiar tool and a formidable terrain to be scaled or traversed.


To address this, I have structured the workflow as an experiment in AI-driven consistency. By first constructing a primitive 3D environment in Blender, I can lock the camera angles and establish a definitive spatial blueprint. These screenshots are then converted into intermediate sketches, providing Nano Banana with a clear structural guide while removing the digital artifacts of the raw CG. This process ensures that the AI respects the underlying spatial logic, breathing life into a world that feels unified and tangibly small.


If the only goal were to master composition and scale, the most straightforward approach would be to feed Blender screenshots directly into Nano Banana for photorealistic rendering. I experimented with this initially, and it certainly succeeds at locking the camera angle, furniture placement, and character proportions. For a project like this, those details are vital. Since *Ser Loras* is only a few centimeters tall, even a minor drift in scale can shatter the illusion of his miniature existence.


However, the difficulty lies in the fact that a Blender screenshot already possesses a heavy 3D-rendered footprint. The materials, lighting, and geometric edges are all partially predetermined. Because I must constrain the prompt to preserve the spatial layout, Nano Banana tends to become overly conservative. It doesn’t truly reinterpret the scene; it merely polishes the existing 3D assets. The structure remains accurate, but the final output often retains a synthetic quality—overly crisp edges and clinical materials that lack the organic imperfections of a real camera lens.


This is precisely why I introduced an intermediate sketch step. The sketch retains the essential structural blueprint—perspective, scale, and object silhouettes—while stripping away the digital weight of the original Blender materials. Nano Banana is no longer trying to fix a CG render; it is instead interpreting a clear visual guide. This allows the AI more creative room to rebuild the lighting, surface textures, and photographic depth from the ground up.


Ultimately, this workflow is about balancing technical accuracy with cinematic realism. Blender provides the necessary spatial logic, the sketch removes the lingering CG footprint, and Nano Banana finally breathes live-action life into the miniature world.


To mitigate the risk of continuity dissolves during complex sequences, I have adopted a methodology of deconstructing expansive movements into discrete, close-up inserts. Rather than attempting to force the generative model to synthesize a singular, exhaustive shot of the knight accessing the drawer and retrieving a sketch, I partition the narrative into manageable action beats. The sequence is reimagined as a series of intimate moments: the initial failure to reach the handle, the tactical positioning of a picture book, the precarious height gained via a crayon box, the organic chaos of spilled wax, and the eventual retrieval of the drawing. This granular approach ensures each frame is defined by a singular, lucid objective and a simplified spatial relationship. By reducing the visual density of each shot, I can more rigorously enforce the project’s essential scale and composition, bypassing the typical AI tendencies toward morphological drift and perspective distortion. Ultimately, this technical necessity has matured into a core component of the film’s visual language, reinforcing the immersive, miniature perspective of *Ser Loras.*


Most of this workflow is currently built inside **flick.art** . I mainly use **Nano Banana** for image generation and refinement, then combine **Kling** and **Seedance** depending on the needs of each shot when turning the images into video. The process is not about entering one prompt and waiting for a final result. It is a constant back-and-forth between composition, scale, character consistency, and controllable motion.


For me, *Ser Loras* is not only a short film project, but also an experiment in AI filmmaking workflow: how to make the model serve a continuous space, a consistent character, and a clear narrative logic, rather than simply producing beautiful individual images. I hope this process can be useful to other creators facing similar challenges in AI filmmaking, especially those working through issues of character consistency, spatial continuity, scale control, and complex action breakdowns.


.
