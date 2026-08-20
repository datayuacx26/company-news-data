---
schema_version: "1.0.0"
document_id: "6401edffecc9cd8a76f39d162b67707c6fadbe685d9609175d20941ee945b205"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/recreate-movie-scenes-with-ai"
published_at: null
first_seen_at: "2026-08-14T05:10:32.259992+00:00"
fetched_at: "2026-08-14T05:10:33.709328+00:00"
content_hash: "sha256:b8204804a4f4517f59a32ff81d4b6b35f1c8cee3748de10e319e512b17290705"
---

# How to Recreate Any Movie Scene with AI | Flick

The fireplace scene in Call Me by Your Name was one of the scenes that I just couldn’t forget after seeing it. I wanted to recreate the blocking, framing, and camera movement that Guadagnino brilliantly implements here. So here’s how I did it, for free, with AI.


1. Find the ideal stills from classic film scenes.
2. Edit the image with Nano Banana Pro to create a realistic human.
3. Generate the video.


## Step 1: Choose your frame


Find the right still. Find a frame where the camera isn't moving, the subject is alone in the composition, and the light has one clear source.


One thing to be clear about: this frame is a reference, not an asset. It never appears in your output. It's there so you can answer to it, the way a DP answers to a lighting diagram.


Rebuild your favorite scene


One still, one new face, one short clip.


## Step 2: Rebuild the still with a new person in it


Now recreate it. In Flick this is Nano Banana Pro edit work against your reference. Describe your person, then correct one thing at a time the way you'd direct a set: I changed the sofa to blue, and swapped out my protaganist.


## Step 3: Generate the video


Run image to video with your finished still pinned as the first frame, and write the motion prompt as a list of refusals. Camera locked. No zoom, no drift. The fire is the only real motion. The person breathes; they don't act. Five seconds, one take.


This restraint is the entire trick. The original scene is famous because nothing moves except what the fire does to a face. Every extra motion the model invents (a head turn, a camera push, flicker in the room light) breaks the spell, so budget motion like money and spend almost none of it.


## The result


Look at the frame again. Long dark hair, red sweater, blue sofa, the hearth burning at the edge of a cool, book-lined room. Not one pixel comes from the film, and you knew what it was before you read a caption. That makes for a great homage.


## Frequently Asked Questions


Can AI recreate a scene from a real movie?


As a rebuilt homage, yes: same composition, lighting, and camera grammar, with a new person in the frame. That's the four-step method behind the published Flick TV recreation in this post.


What tools do I need?


The clip used Nano Banana Pro for the still edits and Flick's image-to-video models for motion, all on one canvas. The still carries the scene; the video pass only animates the fire.


Why does the fireplace scene work so well for this?


Locked camera, single subject, one motivated light source. Those three properties are exactly what image models reproduce reliably, which makes this scene the best first project before you attempt anything with camera movement.


Why not just prompt the director's name?


Describe the aesthetics instead: warm single-source firelight, cool Italian interior, film grain, a held static frame. Models execute described light better than name-drops, and style-by-name requests for living directors are where content filters get twitchy.


## Related guides


- [Keep characters consistent](https://flick.art/blog/img2img-consistent-character)
