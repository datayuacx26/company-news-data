---
schema_version: "1.0.0"
document_id: "215a2022d0800ddca6b380961050b1cf5cf1f5c2fb7cdf4c51dde2bb97c1cfd3"
company_key: "yc-reality-defender"
company: "Reality Defender"
source_id: "yc-reality-defender-news-import-247f3b081c39"
canonical_url: "https://www.realitydefender.com/insights/red-teaming-googles-new-selfie-video-sign-in"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T09:38:33.194451+00:00"
fetched_at: "2026-07-30T09:38:35.624984+00:00"
content_hash: "sha256:ba2278e26e983435021082a8ac535506fc2063a502252410d2f60b6f6391a498"
---

# Red-teaming Google’s new selfie video sign-in proves liveness is not enough

**A live person completed Google’s liveness test using a deepfake. Under the conditions we tested, the deepfake enrollment succeeded.**


On July 23, 2026,[Google introduced selfie video](https://blog.google/innovation-and-ai/technology/safety-security/selfie-video-sign-in/) as an account-access and recovery option for eligible personal Google Accounts. During setup, a user records a short video while following guided head-movement prompts. Google stores that video as a reference and may compare it with a new recording if the user later needs help signing in.


Google says the feature uses multiple layers of security to help prevent impersonation with fake photos, videos, and deepfakes. For Reality Defender’s red team, that raised a straightforward question:


Can a system confirm that an interaction is live without confirming that the face it sees is authentic?


We conducted a controlled assessment to find out.


## What we tested


Our red team used an eligible personal Google Account owned and controlled by Reality Defender. No third-party accounts were involved.


A team member entered the selfie-video enrollment flow and responded to Google’s instructions in real time. When prompted to center or move their head, the operator performed the requested movement.


At the same time, commercially available real-time face-swap software transformed the operator’s appearance into that of another person. The manipulated stream was then presented to the enrollment flow as camera input.


The distinction is important: the operator was real, the interaction and movements were live, but the visible identity was synthetic.


We did not develop a custom deepfake model, exploit Google infrastructure, or build purpose-made attack software. Because this research may expose a security weakness, we are not publishing the names of the tools, their configuration, or the precise method used to deliver the transformed stream.


## What happened


The real-time transformation followed the operator’s movements as they completed the on-screen challenge. Google’s enrollment flow accepted the presentation and registered the synthetic face as the selfie-video reference associated with the company-controlled test account.


We conducted two complete enrollment attempts, and both succeeded. The tests were performed in Microsoft Edge on a Windows 11 laptop equipped with an NVIDIA GPU, using commercially available real-time face-swap software and camera-input manipulation techniques.


Under the conditions tested, a live operator was able to enroll a face that was not their physical face in two out of two attempts.


This demonstrates repeatability in that environment, but it does not establish a universal success rate across other accounts, devices, browsers, regions, or account-risk conditions.


## Why it matters: liveness is not authenticity


Liveness checks are designed to establish that an interaction is happening in real time. Guided movements can help reject basic attacks such as a static photograph or simple replay.


Real-time face transformation changes that threat model. A live operator can respond to unpredictable instructions while software changes the identity presented to the system. The resulting video can contain genuine, prompt-responsive movement without being an authentic representation of the person performing it.


That leaves identity systems with two separate questions:


1. Is a person responding in real time?
2. Is the media an authentic representation of that person?


In our test, the answer to the first question was yes. The answer to the second was no.


The finding also highlights the importance of capture provenance: understanding where media originated and what happened to it before reaching the verification service.


The security boundary does not end at the face. It includes the camera, operating system, media-processing layer, browser or application, and the path used to deliver the video. Manipulation introduced anywhere along that path can change what the system ultimately receives.


## What this result does—and does not—show


Our finding is specific:


Under the tested conditions, Google’s selfie-video enrollment flow accepted a real-time synthetic facial presentation while a live operator completed the required movement challenge.


It does not demonstrate arbitrary takeover of Google Accounts, recovery of an account belonging to another person, or defeat of every security and risk signal Google may apply.


We did not access another person’s account, steal credentials, obtain private user information, or interfere with Google services. This is also an account-access and recovery feature, not a KYC process that verifies a government identity.


Enrollment integrity still matters because the saved video becomes a reference the system may rely on later. If manipulated media can be registered at that stage, the system may anchor future comparisons to a synthetic representation rather than an authentic physical capture.


We have not established that a different operator could later use that representation to recover the account. That would require separate controlled testing.
