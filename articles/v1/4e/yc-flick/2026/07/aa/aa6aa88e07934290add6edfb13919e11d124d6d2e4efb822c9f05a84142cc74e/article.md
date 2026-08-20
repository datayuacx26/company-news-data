---
schema_version: "1.0.0"
document_id: "aa6aa88e07934290add6edfb13919e11d124d6d2e4efb822c9f05a84142cc74e"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/img2img-consistent-character/veo-3"
published_at: null
first_seen_at: "2026-07-23T08:28:30.776088+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:00881ee75f83be12ce232fbc0396e8768b6a67cefeeec5ea0f8c512158687575"
---

# How to Keep a Character Consistent in Veo 3

## HOW IT WORKS


1.


---


STEP 1


LOCK YOUR CHARACTER FIRST


Veo 3.1 in Flick animates a starting image, it does not read character references itself. So build a reusable character first: generate it with a reference-capable model like Nano Banana Pro, or lock it as a Character in Flick.


Made in Flick
2.


---


STEP 2


ANIMATE THE STILL WITH VEO 3.1


Open Veo 3.1 in Flick and set your character still as the start frame, then generate image to video. Veo animates that exact frame, so the identity carries straight into the clip.


Made in Flick
3.


---


STEP 3


PROMPT THE ACTION, NOT THE FACE


Describe the scene and what the character does. The start frame fixes the look, so prompt the motion and setting, not the identity.


Made in Flick
4.


---


STEP 4


REUSE THE SAME STILL EACH SHOT


For every new shot, start from the same character still, or bracket the motion with first and last frames. Veo has no memory between generations, so the shared still is what keeps the character consistent.


Made in Flick
5.


---


STEP 5


EXTEND INTO A SEQUENCE


Use scene extension or first and last frame to chain clips into a longer take, carrying the same still across the cut.


Made in Flick
