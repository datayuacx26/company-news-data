---
schema_version: "1.0.0"
document_id: "b0a946e710e36d750e6e13309c138572ea04c5a64118e5af966af3aff8239205"
company_key: "yc-ncompass-technologies"
company: "nCompass Technologies"
source_id: "yc-ncompass-technologies-rss-a6b0cf73499b"
canonical_url: "https://community.ncompass.tech/t/public-launch-post/21"
published_at: "2025-11-24T10:04:19+00:00"
first_seen_at: "2026-07-25T15:45:43.728961+00:00"
fetched_at: "2026-07-28T22:25:08.967255+00:00"
content_hash: "sha256:87cd14cb664bce9b6033484034a18bab6ac246525799e15b3a0fc60b7dafc6ac"
---

# :compass: Public Launch Post

[LinkedIn Post](https://www.linkedin.com/posts/adityarajagopal_machinelearning-ai-gpu-activity-7397741499821658115-VE2W?utm_source=li_share&utm_content=feedcontent&utm_medium=g_dt_web&utm_campaign=copy)


If you’re building AI models and working with PyTorch, GPU kernels, or performance tuning… you probably know the pain of profiling.


- Running a trace on a remote GPU…
- Downloading 200MB+ files…
- Opening Nsight or Perfetto locally…
- Still not knowing which line of code triggered the slowdown.
- Then branching your code to sprinkle profiling markers, re-running everything, and waiting 10–30 minutes for each iteration.


The whole workflow feels like wading through butter.


We’ve felt this firsthand at[nCompass](https://www.linkedin.com/company/ncompass-technologies?trk=public_post-text) — and it slowed us down more than it should. So we built the tool we always wished existed: 𝗻𝗰𝗽𝗿𝗼𝗳.


𝗻𝗰𝗽𝗿𝗼𝗳 𝗯𝗿𝗶𝗻𝗴𝘀 𝗚𝗣𝗨 + 𝗔𝗜 𝗽𝗲𝗿𝗳𝗼𝗿𝗺𝗮𝗻𝗰𝗲 𝗽𝗿𝗼𝗳𝗶𝗹𝗶𝗻𝗴 𝗱𝗶𝗿𝗲𝗰𝘁𝗹𝘆 𝗶𝗻𝘁𝗼 𝗩𝗦𝗖𝗼𝗱𝗲/𝗖𝘂𝗿𝘀𝗼𝗿:
Add TorchRecord / NVTX markers 𝘄𝗶𝘁𝗵𝗼𝘂𝘁 𝗲𝗱𝗶𝘁𝗶𝗻𝗴 𝘆𝗼𝘂𝗿 𝘀𝗼𝘂𝗿𝗰𝗲 𝗰𝗼𝗱𝗲
View traces 𝗶𝗻𝘀𝗶𝗱𝗲 𝘁𝗵𝗲 𝗜𝗗𝗘 with an integrated Perfetto viewer
Jump from 𝗮𝗻𝘆 𝘁𝗿𝗮𝗰𝗲 𝗲𝘃𝗲𝗻𝘁 → 𝘁𝗵𝗲 𝗲𝘅𝗮𝗰𝘁 𝘀𝗼𝘂𝗿𝗰𝗲 𝗹𝗶𝗻𝗲
View traces where they’re stored — no more shuttling files around


If you’ve ever thought, “Why is this GPU op so slow?” or “Where is this memory allocation coming from?” — ncprof makes the answer visible.


𝗜𝗻𝘀𝘁𝗮𝗹𝗹 𝗶𝗻 𝗩𝗦𝗖𝗼𝗱𝗲 / 𝗖𝘂𝗿𝘀𝗼𝗿: Search for nCompass in the Extensions Marketplace
[𝗦𝗗𝗞 & 𝗲𝘅𝗮𝗺𝗽𝗹𝗲𝘀](https://github.com/nCompass-tech/ncompass) ,[Pip](https://pypi.org/project/ncompass/)
[𝗗𝗼𝗰𝘀](https://docs.ncompass.tech/)
[𝗗𝗶𝘀𝗰𝘂𝘀𝘀𝗶𝗼𝗻](https://community.ncompass.tech/)


We would love to hear what you think as you try it out!
