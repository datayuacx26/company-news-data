---
schema_version: "1.0.0"
document_id: "432a30a34602c682e926b843127e8e9d7f2ac8a121a0ae5d914ab53d69b0c50a"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/cuda-compiler"
published_at: "2026-02-03T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:68ed27763739620cae847bf4fd27e04ec08666ff5b5e8b19a37469ce051acda1"
---

# Cloud Compiler Analyzer (PTX/SASS) Inside Your IDE

## TLDR


- Launching our Cloud Compiler: Analyze CUDA remotely to show PTX/SASS directly from VS Code or Cursor
- No local CUDA toolkit or GPU required
- PyTorch and CUTLASS headers included
- 12+ GPU architectures supported (sm_80 to sm_120a)
- Enterprise onboarding available


## The Problem


As @gau.nernst mentioned, if you’re doing kernel optimization, you need to see what the compiler actually produced. PTX shows you the intermediate representation. SASS shows you the machine instructions hitting the GPU.


But getting there is annoying. Godbolt is great for quick experiments, but it doesn’t have PyTorch or CUTLASS headers. Setting up a local CUDA toolkit just to inspect assembly is overkill. And copying code back and forth between your editor and a browser breaks your flow.


You want to change a line, see the assembly, iterate. Fast.


## What We Built


Wafer now includes cloud CUDA compilation. You can write CUDA in your editor, do Command + Shift + P, and type: “Compile CUDA” and get PTX and SASS back. No local CUDA install required. No GPU required either.


The compiler button appears in your editor title bar when you’re editing a` .cu` file. Click it, pick your target architecture, and see the output. That’s it.


We support 12+ GPU architectures: sm_80 (Ampere) through sm_120a (Blackwell). Whatever you’re targeting, we’ve got it.


## Built-in Headers


We include PyTorch headers and CUTLASS by default.


You can` #include <ATen/ATen.h>` or pull in CUTLASS 4.3.5 and compile against them directly.


Custom nvcc flags work too. Want` -O3` ?` --maxrregcount=32` ?` -lineinfo` for debugging? Pass them through.


## Try Out Today


If you’d like to use this on your codebase, talk to us and we’ll help you get set up.


- [Book a Demo](https://cal.com/wafer/20min)
- [Contact the team](https://www.wafer.ai/cdn-cgi/l/email-protection#3058597047515655421e5159)


## Give Us Feedback


If there’s something else that would make your kernel development faster, let us know.


Reach out at[\[email protected\]](https://www.wafer.ai/cdn-cgi/l/email-protection#244c4d6453454241560a454d) or find us on[Twitter/X](https://x.com/wafer_ai) .
