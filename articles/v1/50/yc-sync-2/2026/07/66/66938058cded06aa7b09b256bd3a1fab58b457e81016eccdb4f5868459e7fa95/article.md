---
schema_version: "1.0.0"
document_id: "66938058cded06aa7b09b256bd3a1fab58b457e81016eccdb4f5868459e7fa95"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/why-ai-lip-sync-looks-fake/"
published_at: "2026-07-14T16:00:00+00:00"
first_seen_at: "2026-07-24T03:27:11.170382+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:5ce69dc55b2972e6e426fc3f4f25e73bb9d76e07eeb10fda2a180fdfb61708ee"
---

# Why AI lip sync still looks fake

You can always tell. You might not have the words for it, but three seconds into a badly synced clip your brain has already flagged it and quietly stopped believing the person on screen. That reaction is not fussiness, and it is not something audiences will ever be trained out of. Humans spend their whole lives reading faces, so a mouth that fights the sound coming out of it registers as wrong before you consciously notice why.


Most AI lip sync looks fake because the model solved the mouth and ignored everything around it. It found a mouth, read the audio, and painted a plausible mouth shape back on. That is enough to pass on a still frame or a clean studio talking head, and it is why so many demo reels look convincing. It falls apart the moment a real scene shows up, because a real face is attached to a jaw, a neck, changing light, and a camera that moves. Every visible failure in AI lip sync traces back to that one mistake, and once you know what to look for, the tells are remarkably consistent.


## Your brain is a lip sync detector


Humans are wired to catch bad lip sync, and the wiring runs deeper than taste. In 1976, psychologists Harry McGurk and John MacDonald published a strange finding: when people watch a mouth making one sound while hearing another, many don’t notice a mismatch. They hear a third sound entirely, one that was never spoken. The[McGurk effect](https://www.nature.com/articles/264746a0) showed that speech perception runs on two channels at once. Your brain fuses what it sees on the lips with what arrives in the ears, constantly and involuntarily, and you cannot switch it off.


That fusion is why lip sync errors feel wrong before they look wrong. When the mouth and the audio disagree, your perceptual system is caught mid-merge with two signals that refuse to reconcile. Nobody pauses a video and says “the viseme timing drifted by four frames.” They say the video felt off, or dubbed, or cheap, and then they scroll past. The judgment happens in seconds and it is almost never articulated, which is exactly what makes it dangerous for anyone publishing synced video.


The stakes are not abstract. Dubbing already wins when it is done well: when Netflix released the German drama Dark,[81% of its English-speaking viewers watched it dubbed](https://sync.so/blog/the-future-of-dubbing-is-visual/) rather than subtitled. Audiences do not actually prefer reading their entertainment. They prefer immersion, and they will take a translated performance over subtitles the moment the translation stops reminding them it exists. Lip sync is the part that decides whether it does. The gap between synced video that builds trust and synced video that quietly burns it comes down to a handful of visible failures.


## AI lip sync gives itself away in five predictable ways


Bad AI lip sync fails in the same few places almost every time, and each failure has a specific cause. None of them require a trained eye to spot. Once you have seen each one, you cannot unsee it, and that is what makes the tells useful when you are judging a tool.


**Teeth are the first tell.** Teeth are oddly specific to a person: the gap, the color, the alignment, whether the canines sit forward. Generative models have a documented tendency to drift toward the average of their training data, so when a model that has seen millions of mouths invents a new one, the teeth it fills in are a little too even, a little too bright, faintly stock-photo. The person’s mouth opens and someone else’s teeth are inside it. Watch the moment a mouth opens on a wide vowel. If the teeth look borrowed, the sync is generated and losing.


**Side profiles and sharp angles are the second.** Most training footage is people facing the camera, because that is what most footage of people talking looks like. Turn a head to a three-quarter view or a full profile and a weak model is suddenly operating far from home: it either smears the mouth, hallucinates lip shapes that belong on a frontal face, or snaps the whole mouth region back toward the camera while the head keeps turning. For a fraction of a second the face stops being the same face. A single head turn mid-sentence is one of the fastest ways to break a tool, and real footage is full of them.


**Timing drift is the third, and it is the one your gut catches even when your eyes do not.** Lip sync only has to be off by a few frames for the whole clip to feel dubbed. The mouth is technically moving, the sounds are technically there, but they arrive slightly apart and the McGurk machinery starts grinding. Consonants are the stress test. Sounds like /b/, /p/ and /m/ need the lips to visibly close, and they are gone in a few frames. A model that lands its vowels but blurs through its plosives produces that unmistakable rubber-mouth feel: motion without articulation. Cheap tools drift. It is the difference between watching a person talk and watching a puppet.


**The seam is the fourth.** Most lip sync pipelines regenerate only a patch around the mouth and blend it back into the original frame. That blend is where the crime scene is. Skin texture inside the patch renders slightly softer than the skin outside it. Compression artifacts and sensor noise in the original footage stop at the patch border, because the generated pixels never went through the camera that produced them. Film grain is the harshest judge here:[grain is a per-frame fingerprint](https://sync.so/blog/the-film-grain-problem/) , and a regenerated region that does not carry it reads as a smooth rectangle floating on a textured face. On heavily compressed social video the seam often hides. In a close-up, a color grade, or anything that ships at real resolution, it is the giveaway that separates a demo from a deliverable.


**A flattened performance is the last, and the most expensive.** This is the one nobody asks about until it is missing. A person talking is a performance, and the mouth is the smallest part of it: the pause before a hard line, the half-smile that undercuts a sentence, the breath. Models are trained overwhelmingly on calm conversational speech, so that is what they reproduce: the words land but the acting does not. Hand one a yelling take, a crying take, or laughing-while-talking and it collapses into that same neutral, careful mouth. The sync can be frame-accurate and the clip can still feel dead, because what got lost was not timing. It was the performance, which is the only reason the footage was worth syncing in the first place.


## Every tell is the same mistake wearing five disguises


All five tells share one root cause: the model is looking at a cropped square of mouth and nothing else. That framing made sense historically, because it makes the problem tractable. Crop the mouth, condition on audio, generate, paste back. But a cropped mouth has no idea the head is mid-turn, no idea the key light is coming from the left, no idea the person is furious, and no idea whose teeth it is supposed to draw. Every disguise the failure wears, borrowed teeth, broken profiles, floating seams, flattened acting, is the model guessing at context it was never shown.


Fixing it means inverting the order of operations: read the scene first, then sync the mouth. That is the design bet behind[sync-3](https://sync.so/sync-3) , our most advanced AI lip sync model. Before generating anything, it builds a spatial understanding of the shot itself: where the face sits in the frame, how it is lit, which way it is pointed, and when there is more than one person on screen, who is actually speaking. Generation happens inside that understanding, so the lips it produces belong to this face, in this light, at this angle, carrying this performance. The result is footage that holds where mouth-crop models break: side profiles, low and uneven light, multiple speakers, close-ups at up to 4K 60fps where there is nowhere for a seam or a wrong tooth to hide. And because the model treats the original take as something to preserve rather than something to paint over, the delivery survives translation. The pause and the half-smile stay, only the words change.


None of this is magic, and it is worth being honest that no model, ours included, is perfect on every tell in every shot yet. We have been working on this specific problem longer than almost anyone: our team built[wav2lip](https://sync.so/blog/a-lip-sync-expert-is-all-you-need-for-speech-to-lip-generation-in-the-wild/) , the first zero-shot lip sync model, and open-sourced it back in 2020, and a lot of what the field knows about the failure modes above came from watching that generation of models break. If you want the mechanics under the hood, the two-encoder architecture, the training trick that made zero-shot possible, and why teeth and cross-lingual sync remain genuinely hard research problems, we wrote that up separately in[how AI lip sync actually works](https://sync.so/blog/how-ai-lip-sync-actually-works/) . This post is about what your audience sees. That one is about why.


## How to test any AI lip sync tool in ten minutes


The fastest way to judge an AI lip sync tool is to feed it the footage its demo reel avoided. Vendor examples are frontal, well-lit, calm, and lightly compressed for a reason. You now know the five tells, so build a test kit that aims one shot at each:


1. **The vowel close-up.** A tight shot of someone talking, close enough to count teeth. Watch the wide vowels. Do the teeth stay theirs, or did the model issue a replacement set?
2. **The head turn.** A shot where the speaker turns from camera to profile mid-sentence. Does the mouth survive the turn, or does the face briefly become someone else on the way around?
3. **The consonant run.** A line dense with b’s, p’s and m’s, played at full speed and then frame by frame. Do the lips actually close on the plosives, or mush through them?
4. **The textured shot.** Something with visible grain, or a moody grade, viewed at full resolution rather than in a compressed preview. Look for the rectangle. Matching skin is easy; matching grain is not.
5. **The emotional take.** A shout, a laugh mid-sentence, a voice breaking. Did the performance come through, or did everything get ironed into the same polite conversational mouth?


Then add the control that removes your own bias: a language you do not speak. When you know the words, your brain helpfully fills in sync that is not there. In an unfamiliar language you judge purely on fusion, the same way a native speaker of that language will.


If a tool holds up across that kit, it will hold up on your easy footage too. If it only looks good on the frontal, well-lit, calmly spoken clip, you are looking at a gimmick, and your audience will feel it in three seconds even if they never once say why. You can run the whole test against our models right now in the[sync. playground](https://sync.so/try) . Bring the shot you think will break it. That is the shot we built for.
