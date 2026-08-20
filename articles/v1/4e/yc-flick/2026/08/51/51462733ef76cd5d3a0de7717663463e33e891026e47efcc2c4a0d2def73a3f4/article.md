---
schema_version: "1.0.0"
document_id: "51462733ef76cd5d3a0de7717663463e33e891026e47efcc2c4a0d2def73a3f4"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/how-i-made-a-viking-film-with-ai"
published_at: null
first_seen_at: "2026-08-14T05:10:32.259992+00:00"
fetched_at: "2026-08-14T05:10:33.709328+00:00"
content_hash: "sha256:d8a198f408f763ca30311e4f9a6446a351365bf11dc6b4a23d1df325a93adc84"
---

# How I Made a Photorealistic Viking Film with AI (Full Workflow)

Low-angle cinematic portrait of a weathered Norse man against a stone monolith and blue sky, from the AI short film Ulysses


I made a photorealistic Viking short film called *Ulysses* entirely with AI. No camera, no crew, no actors, no set. Just over four hundred shots, built in about eight days, that look like real footage. This is the full workflow, start to finish, exactly the way it happened on the canvas.


It is a short film about two seemingly ordinary game NPCs waiting in an empty holding space, only for us to gradually realize that they may be more alive than the player who controls them. Instead of seeing NPCs as inactive background code, we imagine that they continue to exist even when unseen, carrying their own awareness, uncertainty, and presence. The story begins as an absurd waiting game, but slowly becomes a reflection on what it means to be visible, to perform, and to have inner life.


## 1. Story first, then shots


Ulysses is a Viking-age story: a mead-hall keeper, a young guard, an armored warrior, and a dragon. Before generating anything, I broke it into shots, the way you would storyboard any film. Every shot got a rough plan for who is in it, where the camera sits, and what happens.


Make your own AI film


Start with one image and build a whole film, shot by shot.


## 2. Cast the characters as reference sheets


This is the single most important step. For each character I generated a clean three-view reference sheet, front, profile, and three-quarter, on a plain grey background, like a casting photo.


Three-view full-body reference sheet of a bearded Norse man in a wool tunic on a grey background


The sheet becomes the character's “actor.” Every later shot of that person is built from it, so the face never drifts. My lead's sheet was reused as the base in seventy-nine separate shots. Wardrobe and armor get the same treatment: I sketched the armor by hand, then converted the sketch into a full photoreal turnaround.


Three-view reference sheet of an armored Viking warrior in a horned helmet holding an axe


## 3. Sketch every shot by hand


This is the step that surprises people most. Before generating a frame, I drew a rough sketch of the composition, just shapes: where the character stands, where the horizon sits, where the camera looks. On this film I did that for hundreds of shots.


The sketch is the blocking and the camera in one. It is fast, it is disposable, and it gives the model something exact to follow instead of guessing at a composition from words.


## 4. Turn the sketch into a photoreal frame


Then I combined three things in Nano Banana, the composition sketch, the character's reference sheet, and a short instruction, and asked for a photorealistic live-action frame. The prompt forbids the model from wandering:


```text
Use the provided sketch as the only composition reference, use the provided guard
reference as the guard character reference. Convert the scene into a photorealistic
live-action cinematic image. Composition 100% exactly matches the provided sketch.
Camera position 100% unchanged.
```


A rough sketch plus a locked character comes out the other side as a shot that looks filmed.


The same Norse man from the reference sheet, running along a blue-painted wall on white sand, a finished cinematic frame


## 5. Refine by editing, not re-rolling


Almost nothing in this film came from a blank text prompt. Of my image generations, 371 were edits of an existing frame and only 21 were text-to-image. When a frame was slightly off, a heavy jaw, an odd shadow, a wrong detail, I edited that frame rather than starting over. Consistency comes from editing on top of a locked base, not from perfect wording.


## 6. Animate, and let them speak


With the stills locked, I animated selected frames into video with Kling. Some shots are simple motion; others are dialogue, with the character talking straight to camera. Those prompts are specific about the performance:


```text
Handheld camera, extreme close-up of his face. His eyes stay locked on the camera,
no drift. He says, "Still, here." Then a brief pause, still staring, with visible
strain, as if forcing the words out.
```


The still holds the identity; the video adds the breath and the voice.


## 7. The numbers behind it


For a sense of scale, here is what the finished project held:


From the Ulysses canvas Value


Images generated / failures 424 / 20


Reference-image links 653 (92% of all connections)


Image edits vs text-to-image 371 vs 21 (95% img2img)


One reference sheet reused 79 shots


Build time about 8 days, one person


## 8. What I would tell you before you start


- **Build your reference sheets first.** They are the whole game. A character without a sheet will drift.
- **Sketch your shots.** A bad drawing with the right composition beats a beautiful paragraph.
- **Edit, do not re-roll.** Lock a base image and change as little as possible.
- **Expect misses.** Twenty shots failed outright and many more needed fixing. That is normal; the pipeline is built to absorb it.


## Make your own


Start with one image and build a film shot by shot. For the deep dives on the two steps that matter most, see how to make a character reference sheet and our[guide to consistent AI characters](https://flick.art/blog/img2img-consistent-character) .
