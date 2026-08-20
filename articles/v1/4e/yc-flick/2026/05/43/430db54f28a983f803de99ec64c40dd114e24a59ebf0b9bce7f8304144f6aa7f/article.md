---
schema_version: "1.0.0"
document_id: "430db54f28a983f803de99ec64c40dd114e24a59ebf0b9bce7f8304144f6aa7f"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/echoes-of-the-sun"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-21T20:27:22.551209+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:176517ba0f1ade2a6f065e2a396f9d3b51bc023e9caae0e0437e6c93a016e6df"
---

# Echoes of the Sun — How We Made It

*Notes from the making of a two-part AI short film.*


There's a moment in any production where you stop asking "can we do this?" and start asking "how do we do this?" With *Echoes of the Sun* , that shift happened early. The question was never whether AI could generate the images. The question was whether we could make something that held together as a film — with real structure, real emotional logic, and a visual language disciplined enough to carry forty-plus shots without drifting.


This is how we got there.


## Where It Started


The project came from a conversation with Anh, my co-director and the screenwriter. He carries a notebook. Dozens of ideas, half-developed, waiting. This one surfaced from that pile and immediately felt different — complete enough to start. The premise arrived fully formed: a lone warrior crosses an alien desert, finds a dying boy, leads him deeper into a buried cave, and awakens something ancient. A myth-origin story disguised as a survival story. Clean descent structure. A final image strong enough to carry everything that came before it — ancient guards standing still in the chamber as the titan rises, not to attack, but to answer.


What we didn't know yet was that the film would become something else entirely. Part of that shift happened through conversation — sharing early work with other residents, getting outside eyes on what we were building. That kind of friction is underrated in an AI workflow where you can disappear into generation cycles for days without surfacing. Because as we developed the visual language, a second narrative layer emerged: the warrior is a girl in a coma, dreaming. The animated world is her dream. The boy she carries is her father, young. The film that starts as fantasy ends in a hospital room, in the rain, with a father reading to his daughter until she wakes up. The same story, refracted.


That structural decision changed everything about the production. We now had two films to make simultaneously — one anime, one photorealistic — that had to feel like halves of the same thing.


## The Stack


Before a single frame was generated, we had to decide what tools we were actually using and how they'd work together.


### Flick as the hub — where all the generation happened.


Inside that: **Nano Banana 2** (Gemini 3.1 Flash) for stills, prose-based prompting. **Nano Banana Pro** (Gemini 3 Pro) for intent-driven creative direction when we needed the model to reason rather than just execute. **Kling 3.0** for all the realistic hospital shots — it accepts character references via` @element` syntax. **Seedance 2.0** for the animated/hybrid manga part — it accepts references via` @image` syntax, and proved stronger on the physical consistency of ink motion and environment.


Then: **ElevenLabs** for the voiceover. **Nano Banana** for the book cover design as well — the hardcover manga "Echoes of the Sun" that appears in both worlds, the physical object that connects the two halves.


One rule that held across every tool, every session: each platform requires a structurally different prompt format. Not a tone shift. A structural one. What works in Seedance fails in Kling. What Nano Banana Pro rewards — intent, creative direction, room for the model to reason — is nothing like what Seedance needs. We built a prompt architecture for each model and never mixed them.


## Finding the Visual Language — Part One: The Animation


This took longer than anything else.


We knew we wanted manga. We knew we wanted hand-drawn aesthetics, not CGI. But "manga" covers an enormous range, and the wrong register would have made the whole film feel generic. So we went through the library.


**Berserk noir et blanc** was the first test. Kentaro Miura's linework is extraordinary — heavy precise inking, dense crosshatching, aplats of black that carry real weight. But black and white presented an obvious problem: two worlds that needed to feel distinct, and no color to carry the distinction. We moved on.


**Berserk colorisé** — the same graphic structure but with a semi-realistic digital painting layer over the inking. This was closer. The palette naturally aligned with what we needed: terracotta-sienna, pewter-grey, bleached bone. But the semi-realism worked against the dreamlike register. Too grounded.


What we arrived at was a hybrid: **manga/ink painting animation** . Not a reference that exists cleanly anywhere, which was exactly the point. The key principle was that nothing could be static. Every ink line had to be alive. Brushstrokes animate continuously. Shadows breathe. Ink splatters move. The desert itself is in constant motion.


That last insight took the longest to arrive at: AI video tools will not animate what you don't explicitly describe. You cannot assume motion. You have to write it into existence — every brushstroke, every particle, every shift of shadow — or the image freezes like a painting pretending to be a film.


The prompt header that locked the style:


*Hybrid manga/ink painting animation style — ink lines remain visible and alive. Terracotta-ochre brushstrokes animate continuously with the wind. Nothing is ever still. Maintain the hand-drawn ink line aesthetic throughout.*


From that point, every animation prompt opened with that block. Consistency came from the discipline of repetition, not from any single perfect image.


---


## The Characters


Four characters. Each one required its own locking process — generating contact sheets, identifying the exact features that needed to hold across dozens of shots, then writing those features into every prompt with surgical precision.


**The warrior woman** is the center of the animated world. Bone-cream poncho, chainmail, dark braids, runic scarification on her face, pale green eyes. Her sword: long, narrow blade, asymmetric geometric guard, amber runic glow. Right hand, always, unless the shot explicitly specifies otherwise. The specificity wasn't decorative — a sword in the wrong hand breaks screen geography continuity and forces a re-generation.


**The boy** : disheveled black hair, pale skin, torn tunic, simple sandals. The defining character note was that he had to read as older than his age. And when carried — limp. Dead weight. If you don't write the physics explicitly, the model generates bounce. Rigidity. A child held like a parcel rather than like a body that has surrendered.


**The girl patient** : honey-brown dreadlocks, an Ilizarov external fixator on her left leg. The fixator is the visual bridge between the two worlds — her real injury echoes in the boy's inability to walk in the dream. We learned early to name the device precisely. Generic terms gave generic results. "Ilizarov external fixator" produced the correct medical hardware, which is what the film needed to feel real.


**The father** is the grown-up version of the boy. Same bone structure, same quality of stillness. Different scale. The casting logic was internal to the film's metaphor: the girl dreams of herself as a warrior carrying her young father. She saves him. He's been at her bedside reading to her every night. Same person, different time.


## The World: Part One (The Desert)


The exterior world took a complete environmental prompt system to stabilize. Without it, every generation drifted — different sky temperatures, different rock formations, inconsistent haze. With it, the world held.


The universal environment block:


*Alien desert landscape, flat rust-terracotta sand floor, scattered dark grey-pewter rock formations, buried ancient metal structures with runic-geometric engravings partially consumed by rock and sand, massive planetary arc visible on horizon through heavy atmospheric haze. Color grade: warm desaturated terracotta ground, cool grey rock and metal, bleached warm-grey sky, heavy sandy haze dissolving distance, soft directionless diffuse lighting, very low saturation, warm-cool ground-to-sky split, fine dusty grain.*


One sun, low and bleached. No blue sky. No Saharan sand. The specific word choices mattered — "rust-terracotta" produces different results than "red." "Warm-grey sky" produces different results than "grey." The vocabulary was calibrated, not descriptive.


Every exterior prompt opened with this block, then added the shot-specific action. The environment became a container, not a description rebuilt from scratch each time.


## The World: Part Two (The Hospital)


Very low saturation. Cool blue-teal ambient light — the kind hospitals actually have at night. A single warm amber bedside lamp as the only warm source. One lamp, not two. Getting that wrong in a prompt produces a different color relationship entirely, and you feel it.


Fine film grain. Glacial dolly movements — physical camera movement through space, not zoom. Never "push-in." The distinction between dolly and optical zoom is real and the models respond to it differently.


The 14-shot waking sequence was built as a full découpage before a single prompt was written: wide establishing shot, rain on the window, the father disoriented at the bedside, the ECG at normal rhythm, the book on the table, a plastic glass with water spilled on the floor, a finger moving, the ECG accelerating, lips forming "dad," the father turning. Cut.


Each shot then got its own prompt. But they were written as a sequence, not as isolated images — which meant every choice about camera height, character position, and light temperature had to hold across the entire 14-shot arc.


## The Book


The hardcover manga "Echoes of the Sun" is the film's central object. Present in the hospital room on the bedside table. The physical object the father is reading from. The spine readable in the dream world. The prop that makes both worlds true simultaneously.


The author credit on the cover: **Ann Blacksmith** . Ann for Anh, my co-director. Blacksmith for Faure — which means *forgeron* in French. A small private cipher inside a fictional book inside a real film.


Getting it right took several passes. The title treatment went through iterations inside Nano Banana — gold foil was too decorative, spot UV black lacquer was closer, but neither landed exactly where I wanted. Eventually I finished the title design in Affinity Designer, combining AI-generated elements with classic tools. That turned out to be the right approach for anything where you have a precise image in your head: AI gets you 80% of the way there, then a traditional tool closes the gap. Trying to force the last 20% through prompting is where you lose hours.


## The Voiceover


The voice is the father's. A man of around 40, reading aloud from a book. Not performing, not narrating in the epic register — reading. The way someone reads to a person who may not be able to hear them anymore, hoping anyway.


That constraint shaped every line of the text. Nothing could feel written. Everything had to feel spoken, live, slightly imperfect in the way that real speech is imperfect.


For the moment when the titan bows — the climax of the animated sequence — we needed something beyond English. A single phrase in an invented ancient language. The phrase we locked: *"Sol'varath... anaï kaïm."* Short enough to feel carved rather than composed. Sonically alien but somehow readable as command. A sound you'd engrave in stone.


The voiceover covers approximately 2 minutes 30 seconds — the entire first part. Almost no silence. The voice holds the animated world together the way music holds a film together: always present, slightly below the surface, keeping you inside the register.


## What We Learned


A few things that changed how we work, permanently.


**Describe only what isn't already visible.** If the character reference shows the sword, don't describe the sword again unless something about it is different in this shot. Redundant description doesn't reinforce — it introduces noise.


**Motion must be explicit.** Nothing animates without being described as animating. Brushstrokes, water, fabric, particles — write the motion or it freezes.


**Physics require explicit description.** A body being carried has weight. Resistance. A limp, not a posture. If you don't describe the physics, you get a different physics — usually wrong.


**Style reference images carry more information than text.** Locking a master image early and using it as a style anchor is more reliable than text-only prompting for maintaining visual consistency across a long production.


**The instinct to cut is the most valuable thing you bring.** The AI doesn't know when the image falls apart. It doesn't feel when the warrior's face tips from believable to false, when the scale of the titan breaks the frame, when the hospital light has drifted from what the film needs. That instinct — knowing where fake begins — is still entirely human. It's the part of the process that no tool replaces.


## Where Things Stand


The film is complete.


Two parts. One animated, one photorealistic. A daughter's dream and her waking. Hybrid manga and clinical hospital. A warrior who carries a boy across a buried world, and a father who reads to a girl who can't yet answer.


The edit, color, and SFX integration came last — and honestly, this is where everything actually happens. Generation produces material. Editing makes a film. It's where the emotional logic gets built or broken, where a cut at the wrong frame kills a scene and a cut at the right one makes you feel something you couldn't have predicted. The SFX layer — the ink splatters, the particle systems, the ECG pulse — none of it exists in the generation. It gets placed in post, frame by frame, the same way it would in any other production. The AI doesn't know about rhythm. It doesn't know about what the viewer is carrying into a cut. That knowledge is 25 years of work, and it belongs to the edit, not the prompt.


## Credits


*Echoes of the Sun*


- Directed by: Arno Faure & Nguyen-Anh Nguyen
- Based on an idea by: Nguyen-Anh Nguyen
- Adapted for the screen by: Arno Faure
- Creative direction / AI generation · Editing · Sound Design: Arno Faure
- Music: Quinten Coblentz · Rotem Moav · Eleven Tales
- AI platforms: Flick (Nano Banana 2, Nano Banana Pro, Kling 3.0, Seedance 2.0) Voice: ElevenLabs
- The directors wish to thank: Zoey Zhang, Ray Wang, Katherine Zhao, Hannah Lim and all the residents of the the Spring 26 program.
