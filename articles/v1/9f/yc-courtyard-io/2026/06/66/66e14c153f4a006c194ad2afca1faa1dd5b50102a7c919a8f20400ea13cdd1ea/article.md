---
schema_version: "1.0.0"
document_id: "66e14c153f4a006c194ad2afca1faa1dd5b50102a7c919a8f20400ea13cdd1ea"
company_key: "yc-courtyard-io"
company: "Courtyard.io"
source_id: "yc-courtyard-io-news-import-299396b692d9"
canonical_url: "https://courtyard.io/blog/post/from-a-slack-link-to-production-in-under-48-hours-cloning-shopifys-quick-with-devin"
published_at: "2026-06-12T21:40:08.883+00:00"
first_seen_at: "2026-07-25T00:50:41.643845+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:9a4b249b475e65baeef5d88c6f5138633acc485b49570392ff964586cd8a3e53"
---

# From a Slack link to production in under 48 hours: cloning Shopify's "Quick" with Devin

On a Wednesday night, Katherine Champagne dropped a link in our #ai-adventures Slack channel about[Quick](https://shopify.engineering/quick) , Shopify's internal tool that lets anyone — engineer or not — ship a static site in seconds. By Thursday night we had our own version live on an internal domain, behind SSO, fully git-backed, built in Go, deployed on Cloud Run via Bazel and Terraform. Total elapsed time from "look at this cool thing" to "it's live in production": **about 21 hours.** A full slate of follow-on features — a redesigned UI with sound, a quick push/pull CLI, auto-generated site thumbnails — landed within the next day **.**


We didn't staff a team for it or slow down on our (many) existing priorities. We pointed[Devin](https://www.google.com/aclk?sa=L&pf=1&ai=DChsSEwiY-Za-zIKVAxX2Un8AHR9zJgQYACICCAEQABoCb2E&co=1&ase=2&gclid=Cj0KCQjw3K7RBhDJARIsAKRtP5Q0fTAYh1uitPbgpHb9VuuVdxV_EOrsFVhGJXmdavk8p8hjvy0gfPIaAqNYEALw_wcB&cid=CAAS0wHkaPTBNRxdtewCcZzA-pb-0HJezpRCwIqcw28hkPb-LNfpv1LcFU1jtqqaz6Zrt_KcaAQn7yUan1xX6tMpCfvGkGINdeoM1k2VwX9Ydvkxu36PSiZlEbEq1wrY4096IA-YfxKFYJd0-_wGxolzgBv1vq6969iNMmFrUd2I1Jr7co5NAIhh_KtmYwI7Y1tn7yABoPxlVcqqq9x75ovqREvMkimftxCUdEln8sWHLz0V1fl5Kitb1muCBuDED10T12ntwVwnFrsW09R3LkRknbU0yiNY&cce=2&category=acrcp_v1_32&sig=AOD64_3MhfPPbK_qdysQxC3bYo-qbe3iUA&q&nis=4&adurl=https://devin.ai/?utm_keyword%3Ddevin%26utm_term%3Ddevin%26utm_campaign%3DDevin_Brand_Search%26utm_source%3Dgoogle%26utm_medium%3Dcpc%26hsa_acc%3D2306956870%26hsa_cam%3D23521478533%26hsa_grp%3D199099789371%26hsa_ad%3D803150813646%26hsa_src%3Dg%26hsa_tgt%3Dkwd-43720640%26hsa_kw%3Ddevin%26hsa_mt%3De%26hsa_net%3Dadwords%26hsa_ver%3D3%26gad_source%3D1%26gad_campaignid%3D23521478533%26gbraid%3D0AAAAA_ehtCvvIKCBk8uwxm0BQnN7iKkyp%26gclid%3DCj0KCQjw3K7RBhDJARIsAKRtP5Q0fTAYh1uitPbgpHb9VuuVdxV_EOrsFVhGJXmdavk8p8hjvy0gfPIaAqNYEALw_wcB&ved=2ahUKEwjtipG-zIKVAxXiJNAFHWvBLI8Q0Qx6BAgNEAE) **with**[Fable 5](https://platform.claude.com/docs/en/about-claude/models/introducing-claude-fable-5-and-claude-mythos-5) at the idea, told it our constraints, and reviewed its work collaboratively as it went.


## The spark


It started the way a lot of good ideas do — someone noticed something and shared it:


*"\[this is\] how Shopify lets anyone from any team deploy static html (with option for some simple backend functionality too). The claimed adoption is amazing!!!"*


Shopify's Quick is a genuinely clever piece of internal tooling: drag in some HTML, get a live URL. No tickets, no pipelines, no waiting on an engineering team. The adoption numbers in the thread were the eye-catching part - when you make shipping trivial, *everyone* ships.


The obvious question: could we have that at Courtyard? This idea had come up in the past, but usually with caveats like "make the deployment relatively easy" or "give non-engineers easy-to-use git tools" not "make deployments not a thing" or "make version control invisible and painless."


So we asked Devin to build it.


## The constraints (and how they shaped the thing)


This is the part that makes the story worth telling. We didn't hand over a spec. We handed over a few principles and let the design fall out of a back-and-forth in Slack. Every one of these started as a comment in the thread:


- **"It should be git-backed."** Every deploy is a real git commit. That gives you full history and one-command rollback for free, instead of an opaque blob sitting in a storage bucket.
- **"This shouldn't live in experimental/. It should be real code."** The first prototype was a quick FastAPI app in a sandbox folder. We threw that away and made Quick a real, production-reviewed service, one whose *whole job* is to let people publish sites by dropping files into their own experimental/<name>/sites/ folder.
- **"If I'm going to review it, it should be in Go. And we prefer Bazel."** Devin rewrote the service in dependency-free Go (standard library only, single binary), studied our existing repo to match our conventions, and wired it into Bazel with rules_go + Gazelle.
- **"Non-engineers need a drag-and-drop UI but it still has to be git-backed."** This was the most interesting tension. The answer: the web UI takes your identity from the SSO header that our IAP proxy injects (X-Goog-Authenticated-User-Email), writes your files into the repo, commits with *you* as the author, pushes a branch, and opens a PR that our existing auto-approver merges. A non-technical user never sees git. But every single deploy is still a reviewed-by-policy commit with a full audit trail.
- **"Big files can't just fail."** Our repo's CI blocks large binaries from ever entering git history — which would make Quick useless for anyone with an image-heavy site. So Devin built an invisible, git-LFS-style split: text files go to git as normal; anything large gets content-addressed (sha256), offloaded to a GCS bucket, and served back transparently. The person uploading never knows it happened.


Notice what's *not* in that list: nobody wrote a design doc. The architecture emerged from a handful of one-line product opinions, with Devin proposing the trade-offs (git-canonical vs. serving from GCS, how "commit as you" actually works, where the security boundaries sit) and us picking.


## Working *with* the guardrails, not around them


The thing that could have turned this into a multi-week project is our own governance. Our monorepo has real teeth: an isolation check that bans PRs from mixing sandbox and production files, history-hygiene checks that reject oversized blobs, CODEOWNERS, and a human approval rule on production code.


Instead of asking us to relax any of that, Devin audited the constraints up front and designed within them. When a deploy needs to merge a PR on a non-technical user's behalf, it taught our existing experimental auto-approver a narrow, auditable carve-out. When we pushed back that the carve-out had to preserve the approver's core invariant ( *you can only auto-merge changes to your own folder* ), Devin tightened it so a bot deploy is only ever approvable for the deployer's own subdirectory, verified independently by the approver. Defense in depth, with BDD tests, matching the patterns already in the repo.


Devin has been our most-used coding agent for months, but the Fable 5 release has really made it shine. Our engineering team has thrown a variety of more complex and ambiguous problems at it, and its architecture instincts and focus on the goal have differentiated it from the previous state-of-the-art frontier models. This was also the case with Quick - we were able to give Devin principles and constraints, and, without being too prescriptive, saw it utilize Fable to arrive at elegant solutions that satisfy our software design principles without compromise.


Every PR went through our normal review. Devin worked through XSS hardening, zip-slip and path-traversal tests, a deploy mutex for concurrency, request-size limits, and keeping the deploy token out of process command lines — most of it surfaced and resolved in review, the way it's supposed to work.


## The unglamorous middle: infrastructure


Shipping the *service* was maybe a third of the work. The rest was getting it actually running in our cloud. This is where having an agent that just keeps going earns its keep.


Over the course of the night, Devin opened and iterated a stack of Terraform PRs to stand up Cloud Run behind IAP, a GCS assets bucket, a build trigger, and the deploy pipeline. Along the way it hit — and worked through — exactly the kind of stuff that makes infra miserable:


- a Cloud Run secret that existed but had no version yet
- a timeout_sec that GCP silently disallows on serverless-NEG backends
- a Terraform import to adopt a secret we'd created by hand
- a "chicken-and-egg" between workspaces that we asked it to eliminate by splitting the PRs (it did)
- a CGO/stdlib.h build failure that only showed up in the deploy image, which it fixed *and* then closed the gap so the same mismatch fails at PR time instead of at deploy time
- the GitHub fine-grained PAT permissions (Contents + Pull requests, read **and** write) and the IAP service agent's run.invoker binding


None of these are interesting individually. Collectively they're the reason "just clone Shopify's tool" usually takes a sprint. Here, the human side was mostly clicking *approve* , signing commits, and answering "what kind of token do I need?"


## Live — then better


**~21 hours in** , Quick was live on our internal dev domain behind SSO. Shortly after, it was in production. Idea to production, well under a day.


But the thread didn't stop there, and this is the part that actually shows what the workflow unlocks. Once it was live, requests came in like we were talking to a teammate, and they shipped the within minutes:


- **"Make the home page juicy — look up current design trends, no AI slop, real animations… and don't forget sounds."** Devin redesigned the home page (dark theme, kinetic logo, staggered card grid, confetti on deploy) with an entire audio layer synthesized in the browser via the Web Audio API — hover ticks, a deploy chime, ambient hum behind a mute toggle — with zero asset files, so it stays a single embedded file in the Go binary.
- **"Give us a CLI for the nerds: quick push / quick pull."** Done, stdlib-only, authenticated through IAP with gcloud auth login.
- **"Auto-generate a screenshot of each site for the homepage grid and the link unfurl."** Devin added headless Chromium to the container, rendered each site's index.html on deploy, cached the PNG keyed to the latest commit, and served it as the og:image.


Plus the real-world papercuts you only find once people use a thing: a missing git committer identity in the container, "ghost" sites that served from disk after a failed commit, a folder-drop-instead-of-zip that died in the browser before it ever reached the server. Each one root-caused and fixed.
