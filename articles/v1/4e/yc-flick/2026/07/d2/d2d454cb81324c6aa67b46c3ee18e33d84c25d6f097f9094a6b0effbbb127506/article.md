---
schema_version: "1.0.0"
document_id: "d2d454cb81324c6aa67b46c3ee18e33d84c25d6f097f9094a6b0effbbb127506"
company_key: "yc-flick"
company: "Flick"
source_id: "yc-flick-news-import-00342d05d871"
canonical_url: "https://flick.art/blog/img2img-consistent-character/comfyui"
published_at: null
first_seen_at: "2026-07-23T08:28:30.776088+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:48391ddb9e220aaa76ff4c4142aed9e173100b17c130cd8a9397acc2182cbeb1"
---

# How to Keep a Character Consistent in ComfyUI

## HOW IT WORKS


1.


---


STEP 1


LOAD AN SDXL CHECKPOINT


Start from a photoreal SDXL checkpoint like RealVisXL or Juggernaut; they hold anatomy and identity far better than SD 1.5 for character work. Run DPM++ 2M Karras at 25 to 30 steps and keep the base at 1024, since IPAdapter and InstantID were trained at SDXL scale and drift at smaller sizes.


2.


---


STEP 2


ADD AN IPADAPTER FACEID NODE


Wire in an IPAdapter FaceID Plus v2 node, load the CLIP-Vision and FaceID LoRA it expects, and feed it one clean reference face. Hold the weight around 0.7 to 0.8: lower and the prompt takes over and the face drifts, higher and identity locks but the expression stiffens. Stack InstantID beside it when the face has to hold at hard angles.


3.


---


STEP 3


LOCK POSE WITH CONTROLNET


Feed a pose skeleton into a ControlNet OpenPose node so the body and framing stay fixed while only the setting changes. Add a Depth or Canny ControlNet at low weight (0.3 to 0.5) to also hold composition, but keep total ControlNet strength under about 1.0 so it does not fight the IPAdapter.


4.


---


STEP 4


REFINE WITH FACEDETAILER


Route the output through a FaceDetailer node (Impact Pack) to crop, re-diffuse, and re-anchor the face at full resolution, fixing the melting eyes and mouth SDXL gives you at distance. Keep the same reference and a fixed seed across generations, and hold FaceDetailer denoise around 0.4 so it sharpens without inventing a new face.


5.


---


STEP 5


BRING IT TO LIFE IN FLICK


ComfyUI stops at stills, and the graph, the custom nodes, and the GPU are yours to maintain. Drop your locked character into Flick and animate it with Veo 3, Kling, or Seedance, and Character Reference carries the same face into video with no adapter stack to wire.
