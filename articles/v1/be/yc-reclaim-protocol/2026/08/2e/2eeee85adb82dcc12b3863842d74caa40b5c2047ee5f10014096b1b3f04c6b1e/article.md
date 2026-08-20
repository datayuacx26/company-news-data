---
schema_version: "1.0.0"
document_id: "2eeee85adb82dcc12b3863842d74caa40b5c2047ee5f10014096b1b3f04c6b1e"
company_key: "yc-reclaim-protocol"
company: "Reclaim Protocol"
source_id: "yc-reclaim-protocol-news-import-8b5fb76919cb"
canonical_url: "https://blog.reclaimprotocol.org/posts/why-we-built-popcorn"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T18:56:07.223230+00:00"
fetched_at: "2026-08-06T18:56:09.186964+00:00"
content_hash: "sha256:7f9edee2f2343796af56441615fba599678dc02f0b0ae4da056cb37ef82b67af"
---

# Why We Built Popcorn: An Attestable Browser Cloud for Reclaim

# Why We Built Popcorn: An Attestable Browser Cloud for Reclaim


Abdul Rashid Reshamwala


Aug 5, 2026


engineering


popcorn


TEE


[Reclaim](https://reclaimprotocol.org/) lets people prove facts about themselves from websites they already use. **Popcorn is the attestable browser cloud we built to make those proofs easier to integrate.** Users complete the flow in a live, mobile-native browser running inside a[Trusted Execution Environment (TEE)](https://cloud.google.com/confidential-computing/docs/confidential-computing-overview) . Popcorn generates the proof, then destroys the session. You can[try Popcorn](https://popcorn.reclaimprotocol.org/) or[explore the code on GitHub](https://github.com/reclaimprotocol/popcorn-oss) .


In the latest 30-day window, Popcorn created **102,859 browser sessions** and allocated ready browsers in **470 ms at p50** and **889 ms at p95** .


The story began with a WebView on the user's phone. That approach kept the work close to the user, but it asked every app integrating Reclaim to carry a surprising amount of infrastructure in its pocket: native cryptography, WebView orchestration, proof generation, and platform-specific glue. We rebuilt our proof-generation engine and cut proof time from roughly 40 seconds to 4–5 seconds, but the integration was still substantial.[That migration is a story of its own.](https://blog.reclaimprotocol.org/posts/gnark-migration)


The performance problem had improved. The integration problem had not.


We built both an[App Clip](https://developer.apple.com/documentation/appclip) and an Android Instant App to avoid a full SDK integration. They worked, but partners did not want verification to send users into a separate app experience.[Google Play Instant was eventually discontinued](https://developer.android.com/topic/google-play-instant) . We needed an integration that stayed inside the product flow without putting the full browser and proof workload on the phone.


The product idea sounded almost suspiciously simple: open a browser remotely, stream its live view to the user, let them sign in and navigate, then generate the proof. Moving the work off the phone, however, also moves the hardest part of the trust model into our infrastructure.


The integration starter pack


### Some assembly no longer required.


same proof · less inside the app


Before · ship the machinery


WebView


Native crypto


Proof engine


Platform glue


With Popcorn · open the flow


https://…/verify


Open


**One verification URL** The browser and proof workload run remotely.


🍿


## Starting with Kernel


We evaluated Browserbase and other hosted-browser platforms.[Kernel](https://www.kernel.sh/) worked best for our flows, especially because we needed **headful** browsers.


Users needed to operate a real, visible Chrome session. Some sites behave differently in headless mode, while some login flows block automation. Kernel gave us interactive Chrome sessions, a live view, and browser-control APIs we could build on.


Kernel got us to a working product quickly. Another important point was that its[browser image was open source](https://github.com/kernel/kernel-images) . We could inspect the image, make the changes our flows needed, and contribute useful fixes upstream, including[parent-frame status events](https://github.com/kernel/kernel-images/pull/117) and[a video-sync anti-echo fix](https://github.com/kernel/kernel-images/pull/166) .


Once the remote-browser experience worked, the harder question became where that browser could safely run.


Kernel did not have plans to support the hardware-backed security model we needed. That meant the browser had to run on infrastructure we controlled. We started building our own TEE-based deployment, using Kernel's open-source browser image as the base instead of rebuilding a headful Chromium container from scratch.


## The browser still had to be trusted


Browser sessions handle credentials, cookies, and private account data. Encryption protects that data while it travels across the network and while it is stored, but not while the browser is actively using it. Inside an ordinary VM, the host can still access that memory.


A[Trusted Execution Environment, or TEE](https://cloud.google.com/confidential-computing/docs/confidential-computing-overview) , gave us the missing protection. It is designed to isolate the browser's memory from the rest of the host, including the surrounding cloud infrastructure.


**A very short hardware heist** attempt #404


Suspicious host process


⌁


` steal(browser.memory)`


TEE


BONK!


Protected memory


🍿


browser: unbothered


MEM?


nope


Result: memory not found. Dignity also missing.


It cannot directly inspect the TEE-protected browser memory.


A TEE does not hide the page from the browser; the browser still needs to render it and generate the proof. It protects that work from the surrounding host. In production, each browser runs on a[Confidential GKE Node](https://cloud.google.com/kubernetes-engine/docs/how-to/confidential-gke-nodes) with a small attestor service beside it.


In simple terms, a verifier can check that the session ran on genuine confidential hardware and used approved code. It does not have to take our word that the server was configured correctly.


Here is how it works in production. The verifier sends the attestor a fresh nonce, or one-time random challenge. The attestor returns a response tied to that challenge, the cryptographic fingerprints of the browser and attestor images, and a[Google Cloud hardware attestation](https://cloud.google.com/confidential-computing/docs/attestation) .


A TEE still does not replace TLS, authentication, authorization, or code review; it narrows what must be trusted.


This was not a security feature we could bolt on after the browser worked. TEE availability now determined where we could run Popcorn.


We first self-hosted on AWS with[AMD SEV-SNP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sev-snp.html) , but too few nearby regions supported it. Google Cloud offered[AMD SEV-backed Confidential VMs](https://cloud.google.com/confidential-computing/confidential-vm/docs/supported-configurations) and Confidential GKE in more of the regions we needed, so we moved the fleet.


## The browser was only half the system


At this point, we had a browser image that could run inside a TEE. But an image is not a browser cloud. We still needed a system that could:


- keep isolated browsers ready before users asked for them;
- route each request to a nearby, healthy region;
- connect the user securely to the assigned browser; and
- replace used browsers, add TEE machines when needed, and clean up finished sessions.


We split the work across a few focused components. The **control plane** authenticates the request and selects a region. A **pool manager** in that region assigns a ready browser, while the **gateway** securely carries the live view between the user and that browser. The control plane coordinates the session without sitting in the streaming path.


[Agones](https://agones.dev/) is an open-source system for managing fleets of multiplayer game servers on Kubernetes. That sounds unrelated until you look at the lifecycle. A browser waits for a user, belongs to that user for a short session, and is then discarded. Agones already knows how to manage that pattern.


Each regional pool keeps a warm buffer of browsers that are already running and marked` Ready` . A request only needs to claim one, so scheduling, image pulls, and browser startup stay off the user-facing path. Agones tracks each browser from waiting to assigned to finished. When one is assigned, its **FleetAutoscaler** starts a replacement. If the current TEE machines are full, our **node prescaler** adds another machine.


The flow is simple: a user starts a session, and the control plane chooses a pool close to them. Each regional pool contains several TEE nodes, and each node runs many isolated browser pods. The pool manager asks Agones for a ready browser and returns signed connection details. The live view then travels through the regional gateway directly to that browser. Behind the scenes, the scalers replace the assigned pod and add another TEE node when the pool needs more room.


### From request to browser


USER REQUEST


**Control plane**


POP!


**Pool manager** asia-south1


**Pool manager** southamerica-east1


**Pool manager** us-central1


**Selected regional pool**


TEE node 1


TEE node 2


TEE node 3


NEW


**FleetAutoscaler** •


**Node prescaler**


ROUTED


The implementation lives in the regional[pool manager](https://github.com/reclaimprotocol/popcorn-oss/tree/main/services/pool-manager) , the Agones[browser-fleet chart](https://github.com/reclaimprotocol/popcorn-oss/tree/main/charts/browser-fleet) , and the GKE node prescaler.


Together, these pieces gave us a repeatable way to bring up a secure browser cluster in a new region, keep browsers ready, and serve users from a location close to them.


## Owning the experience


Once the secure regional fleet was running, our focus shifted from where the browser ran to how it felt. Phones and slow networks exposed rough edges in the live stream, runtime, and viewer. We worked through them one by one.


## From WebRTC to VNC


The first major change was the live view. In our iOS testing, Low Power Mode sometimes prevented the remote video from autoplaying, while backgrounding the app could lead to slow ICE reconnection. Some mobile and corporate networks also blocked or degraded WebRTC's preferred UDP paths. TURN, including relay over TCP, improved reachability but added another network hop and, in our measurements, more latency.


We replaced the WebRTC video stream with VNC's[Remote Framebuffer protocol](https://datatracker.ietf.org/doc/html/rfc6143) , carried over a secure WebSocket and rendered with[noVNC](https://github.com/novnc/noVNC) . RFB represents display updates as rectangles, which suited our mostly static verification pages: users often pause to read or type, so the display changes only intermittently. This is a workload-specific trade-off; WebRTC remains better suited to high-motion content.


### When the page barely moves


continuous frames · changed rectangles


Before


**WebRTC**


video stream


frame · frame · frame


The whole view stays in motion


After


**VNC**


pixel updates


quiet page · tiny update


Only changed rectangles travel


For Popcorn, the change reduced the live-view path to one WebSocket connection, shortened recovery after app switches, and produced more predictable behavior on slow mobile and corporate networks.


## A smaller image for a narrower job


Changing the transport exposed another problem: the runtime carried far more than our live-view path needed. It still depended on the Kernel Images API and the Neko/WebRTC runtime, along with the supporting machinery around them. That made browser startup, image builds, and our development loop slower.


So we rebuilt the runtime image from scratch around one job: running Popcorn's LiveView in production. The[new image](https://github.com/reclaimprotocol/popcorn-oss/tree/main/images/minimal-vnc-desktop) contains the browser, lightweight desktop, VNC/noVNC viewer, Reclaim proof endpoint, proxying, and attestation. It no longer depends on the Kernel Images API or the Neko/WebRTC runtime.


Runtime metric Before After


Browser boot ~15 seconds ~2 seconds


Unpacked image size 2.6 GB 0.9 GB


Image build ~30 minutes ~10 minutes


These are internal before-and-after measurements for the runtime image, separate from the request-to-allocation latency discussed later. The smaller stack also gave us a cleaner build loop and much less software to operate.


## Running the mobile gauntlet


Most Reclaim sessions happen on phones. The viewers we tested could show a desktop browser on a small screen, but we wanted Popcorn to feel like a native mobile browser.


Typing had to feel immediate. Zoom had to follow your fingers. Prompts and forms had to fit the screen, and switching apps could not lose the session. We built those behaviors into the viewer itself.


### Same phone. Different browser.


from remote desktop to native-feeling


01 · Type


#### Typing should feel immediate


Before


9:41


Remote view


Enter verification code


8


4


?


9


delayed input


waiting…


keys can arrive late or incorrectly


Now


9:41


Popcorn · secure


Enter verification code


8


4


2


9


instant feedback


connected


stays responsive on a slow network


types like a local keyboard


02 · Pinch and zoom


#### The page should follow your fingers


Before


9:41


Remote view


cropped


cropped, blurry, and delayed


Now


9:41


Popcorn · secure


full width


smooth zoom handled on the phone


03 · Use the page


#### Controls should fit the phone


Before


9:41


Remote view


Verify your account


Focused field


Are you sure?


field hidden


fields and prompts can leave the screen


Now


9:41


Popcorn · secure


Verify your account


Focused field


Are you sure?


Cancel


Continue


forms behave like mobile controls


04 · Switch apps


#### Come back where you left off


Before


9:41


Remote view


Verification in progress


Messages


Your verification code is


8429


stale viewer


return to a black frame


Now


9:41


Popcorn · secure


Verification in progress


8429


Messages


Your verification code is


8429


back where you left off


the session stays with you


Popcorn now owns the mobile experience. Integrators get native-feeling typing, zoom, gestures, dialogs, and reconnection without rebuilding them inside their app.


## What it looks like in production


By **August 5, 2026** , Popcorn had moved well beyond a prototype. Over the previous 30 days, the fleet created **102,859 sessions** and ended **102,809** . Those sessions accounted for **10,605 hours and 11 minutes** of browser time, with an average lifetime of **6 minutes 11 seconds** . The fleet handled an average of **2.38 new sessions per minute** .


Live from the last 30 days


### Production pulse!


Ending Aug 5, 2026


Now with
more traffic!


✦


✦


✦


102,859sessions created


10,605ended-session hours


2.38sessions per minute


Sessions created


Actual results!


07/06: 2,825 sessions


07/08: 3,297 sessions


07/10: 2,921 sessions


07/12: 6,868 sessions


07/14: 8,322 sessions


07/16: 7,276 sessions


07/18: 5,533 sessions


07/20: 6,815 sessions


07/22: 5,900 sessions


07/24: 5,860 sessions


07/26: 6,286 sessions


07/28: 9,214 sessions


07/30: 9,181 sessions


08/01: 11,502 sessions


08/03: 11,059 sessions


11,059


07/06


07/12


07/18


07/24


07/30


08/03


Traffic grew from 2,825 to 11,059 sessions per interval. Operators are standing by.


The speed users feel first is allocation. We measure it from the moment the control plane receives a request until the regional pool manager returns the signed connection details for an assigned browser. That took **470 ms at p50** and **889 ms at p95** .


On provisioned capacity, the minimal image can start a fresh browser pod in roughly **2–3 seconds** . Once the cloud prerequisites are available, we can bring an entire regional Popcorn cluster to` Ready` in roughly **20 minutes** . That lets us add capacity close to users wherever compatible TEE infrastructure is available.


## What we learned


**Start managed. Fork when the requirements sharpen. Specialize after learning.**


The path to Popcorn was not “build versus buy.” Using Kernel let us validate the product before building browser infrastructure. Forking its open-source image taught us which parts of the stack mattered. Operating that fork exposed the limits of a general-purpose image for our workload. Only then did a minimal image, a mobile-native viewer, and our own scaler become the sensible choice.


**Let real constraints choose the technology.**


Cloud regions, mobile networks, and phone keyboards chose more of this architecture than fashion did. Each change came from watching the verification flow fail under a real constraint, then fixing that constraint at the layer that owned it.


**Security becomes infrastructure topology.**


Once every browser must run inside a TEE, confidential-computing availability becomes part of scheduling. It shapes cloud selection, regional expansion, node pools, images, attestation, capacity planning, and failure handling. A security requirement no longer stays in the security layer.


Popcorn now gives apps a much lighter way to integrate Reclaim while keeping the sensitive browser workload inside an attestable environment. It took a hosted service, an open-source fork, a cloud migration, a new streaming path, and an unreasonable amount of attention to mobile keyboards to get there. The[runtime image](https://github.com/reclaimprotocol/popcorn-oss/tree/main/images/minimal-vnc-desktop) ,[fleet configuration](https://github.com/reclaimprotocol/popcorn-oss/tree/main/charts/browser-fleet) ,[attestation design](https://github.com/reclaimprotocol/popcorn-oss/blob/main/docs/attestation.md) , and[full repository](https://github.com/reclaimprotocol/popcorn-oss) are open source.


[Try Popcorn](https://popcorn.reclaimprotocol.org/) or[start with the repository](https://github.com/reclaimprotocol/popcorn-oss) .


Questions, feedback, or want to deploy Popcorn? EmailAbdul orSrivatsan , or reach[Abdul on X](https://x.com/abdul_rashid_r) .
