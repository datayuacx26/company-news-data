---
schema_version: "1.0.0"
document_id: "abbbd5c3bb200c365d577485b9f45f4c8301acc6d9175d72cdf814aff3417c08"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/img2img-consistent-character/midjourney"
published_at: null
first_seen_at: "2026-07-23T08:28:30.776088+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:6e8d97e878a7df0084f99d92cc75b44c605b0e3e9d80821a197d5f4900af846f"
---

# How to Keep a Character Consistent in Midjourney

WHAT IS --OREF IN MIDJOURNEY?


Omni Reference. You pass an image URL with` --oref` and Midjourney matches the character's identity in the new generation, rather than re-rolling a face from scratch.


WHAT --OW VALUE SHOULD I USE?


Start around 200 to 400. That keeps the character recognizable while still following your scene prompt. Push toward 500 or higher only when the face is drifting and you need a harder lock.


CAN MIDJOURNEY KEEP A CHARACTER CONSISTENT IN VIDEO?


No. Midjourney is image-only. Lock the character in a still, then animate it with a reference-to-video model like Veo 3 or Kling, both available in Flick.


DO I NEED TO TRAIN A LORA?


Not for most projects. A single good reference with --oref is enough. Train a LoRA in ComfyUI only when you are generating hundreds of shots and drift becomes expensive to fix.


WHY DOES MY MIDJOURNEY CHARACTER KEEP CHANGING?


Each generation is created from scratch, so small differences accumulate into drift. Attach the same` --oref` image, keep the written description identical, and raise` --ow` toward 400 when the face slips.


HOW MANY REFERENCE IMAGES CAN I USE?


Omni Reference works from a single strong reference. If you need several angles, generate a clean turnaround first and use the clearest front-facing frame as your --oref anchor.


DOES THE OLDER --CREF STILL WORK?


Character Reference (` --cref` ) was the previous system. Omni Reference (` --oref` ) supersedes it with stronger identity matching, so prefer --oref on current versions.


HOW DO I KEEP THE OUTFIT AND PROPS CONSISTENT TOO?


Wardrobe is where --oref drifts most. Describe the outfit explicitly in every prompt, raise --ow, and for exact continuity lock the look in Flick's Character Reference and carry it across shots.
