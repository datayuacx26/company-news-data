---
schema_version: "1.0.0"
document_id: "9e76b2e842f2f8fd3f31b7165445831d42520b30dd3f17eedf2a59c7dc2863e1"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/img2img-consistent-character/flux"
published_at: null
first_seen_at: "2026-07-23T08:28:30.776088+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:b60ac0b0232c2f76f0ad94df5eafb9e5b046a8b7f8b79b49c92ada42f5c6fc49"
---

# How to Keep a Character Consistent in Flux

## HOW IT WORKS


1.


---


STEP 1


START FROM ONE CLEAN CHARACTER IMAGE


Generate or choose a clear image of your character. With Kontext, this single image is the subject you preserve.


2.


---


STEP 2


EDIT IN CONTEXT WITH KONTEXT


Load Flux Kontext, provide the character image plus an instruction like a new pose or scene. Kontext keeps the subject while changing the setting.


3.


---


STEP 3


TRAIN A LORA FOR FULL REPEATABILITY


For a character you reuse constantly, train a Flux LoRA on 15 to 30 varied images and a unique trigger token, then call it in any prompt.


4.


---


STEP 4


TIGHTEN THE FACE WITH PULID


When the face is small or drifting, add PuLID to preserve the specific identity on top of your prompt or LoRA.


5.


---


STEP 5


BRING IT TO LIFE IN FLICK


Flux makes stills only. Drop your locked character into Flick and animate it with Veo 3, Kling, or Seedance so it carries into video.
