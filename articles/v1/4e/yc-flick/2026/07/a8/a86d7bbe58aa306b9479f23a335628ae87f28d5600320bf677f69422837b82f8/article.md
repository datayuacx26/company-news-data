---
schema_version: "1.0.0"
document_id: "a86d7bbe58aa306b9479f23a335628ae87f28d5600320bf677f69422837b82f8"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/img2img-consistent-character/stable-diffusion"
published_at: null
first_seen_at: "2026-07-23T08:28:30.776088+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:1a3c1d634072d34d3c6aafff4bd9bff170ba938cb600c18e92c145eeadcdcff1"
---

# How to Keep a Character Consistent in Stable Diffusion

## HOW IT WORKS


1.


---


STEP 1


CHOOSE YOUR PATH


For a fully repeatable identity, plan a LoRA. For a fast face from one image, use IPAdapter FaceID or InstantID on SDXL.


2.


---


STEP 2


TRAIN A LORA OR LOAD AN ADAPTER


Train an SDXL LoRA on 20 to 50 images with a unique trigger token, or wire an IPAdapter FaceID node and feed it your reference face.


3.


---


STEP 3


LOCK POSE WITH CONTROLNET


Add a ControlNet OpenPose node so the pose and composition stay fixed while you change the scene around the character.


4.


---


STEP 4


GENERATE AND REFINE


Call the trigger token or reuse the reference in each prompt. A face-restore or detailer pass tightens the likeness shot to shot.


5.


---


STEP 5


BRING IT TO LIFE IN FLICK


Stable Diffusion makes stills only. Drop your locked character into Flick and animate it with Veo 3, Kling, or Seedance.
