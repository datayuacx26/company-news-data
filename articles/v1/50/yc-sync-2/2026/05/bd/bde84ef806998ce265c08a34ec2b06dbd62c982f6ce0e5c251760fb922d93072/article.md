---
schema_version: "1.0.0"
document_id: "bde84ef806998ce265c08a34ec2b06dbd62c982f6ce0e5c251760fb922d93072"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/how-ai-lip-sync-actually-works/"
published_at: "2026-05-28T16:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:8aa2c0c313164b19b08f19cb7d2f4eaba7b1d116c9198dbf98004105b714c0c1"
---

# How AI lip sync actually works

Lip sync sounds trivial until you actually try to do it. You have audio of someone saying something, a video of their face, and you want the mouth to match what they’re saying. Easy right? Except humans are wildly good at noticing when a mouth doesn’t fit the sound coming out of it, and drifting it by even a few frames is enough to make the whole thing feel wrong. There’s a name for the perceptual machinery behind that. It’s called the McGurk effect, and it’s why you can hear the wrong syllable just by seeing the wrong mouth shape. Bad lip sync isn’t something you can quietly ignore, it’s the kind of thing your nervous system flags before your conscious mind catches up. Watch any pre-2020 attempt at AI dubbing for ten seconds and you’ll feel exactly what I mean.


So what is a model actually doing when it lip syncs a video?


## The shape of the problem


You’re trying to learn a function. Given an audio clip and a face, produce a face that’s the same face. Same identity, same lighting, same head pose. Just with a mouth that matches the audio. Same person, different words.


It looks like a translation problem. It sort of is. But unlike translating text, you can’t just swap one piece for another. The mouth doesn’t live in isolation. It connects to the chin, the cheeks, the throat. Light has to fall on it the way it falls on the rest of the face. Teeth show up and disappear. The jaw drops. The adam’s apple moves. None of that can break, or you’ve made an obvious deepfake instead of a lip sync.


Before 2020, the field could already produce realistic lip sync. The catch was that you had to train the model specifically on the person you wanted to sync, and the moment you handed it a face it had never seen before, the whole thing collapsed. There was another, less obvious problem too. The audio-to-mouth side of those systems was typically built around restrictive 3D models that produced a kind of open-and-close jaw motion rather than the continuous, expressive movement you actually get when a person talks. Subtle phonetic transitions got flattened. Emotion didn’t really come through. The mouth would technically track the audio, but it felt mechanical the moment you watched it for more than a few seconds.


So the first real question any modern AI lip sync model has to answer is this. How do you produce fluent, expressive mouth motion for any person, without retraining on every new face?


## Two streams meeting in the middle


Every modern AI lip sync system has two encoders. One for audio, one for visual.


The audio encoder takes your speech and turns it into a sequence of vectors, one per short chunk of audio, usually around 200 milliseconds. These vectors don’t store the audio itself. They store what the model has learned to extract from it. Roughly, the phonemes being spoken. A phoneme is the smallest unit of speech. “Cat” is three phonemes, /k/, /æ/, /t/. The encoder doesn’t necessarily label them explicitly. It just learns that certain audio patterns correspond to certain mouth shapes. This mapping has a name. Phoneme to viseme mapping. A viseme is the visual equivalent of a phoneme, the mouth position that corresponds to a sound.


The visual encoder takes the video frames around the moment you’re trying to sync. It pulls out identity (whose face is this), pose (which way are they looking), and context (what’s the rest of the face doing). The mouth region itself gets masked out. The model isn’t allowed to see what the mouth was doing in the original, because that’s what it has to generate.


Then the two streams meet. The audio tells the model what the mouth should be doing. The visual tells it whose mouth it is and how it’s oriented in space. A generator takes both and produces a new mouth that fits the face it’s attached to.


That’s the basic shape. The interesting question is how you train this thing.


## The trick that changed the field


The big problem with training a lip sync model used to be the loss function. How do you tell the model whether it’s right?


You can compare the generated frame to the ground truth frame pixel by pixel, but pixel loss doesn’t actually care about lip sync. A model that produces a blurry mouth roughly in the right place will score better than a model that produces a sharp mouth slightly off, which means the loss function ends up quietly rewarding the wrong thing.


You can use a perceptual loss instead. Train a separate network to judge whether the output looks right. But that doesn’t specifically care whether the mouth matches the audio either, it just cares whether the face looks like a face.


The breakthrough in[Wav2Lip](https://sync.so/blog/a-lip-sync-expert-is-all-you-need-for-speech-to-lip-generation-in-the-wild/) , which our team built in 2020, was using a pre-trained lip sync expert as the discriminator. The expert was a separate model called SyncNet, trained specifically to detect whether a piece of audio and a piece of video are in sync. You point SyncNet at your generator’s output and ask the only question that matters. Is this lip sync good? If it isn’t, the generator gets penalized hard. The generator’s only job becomes to fool a discriminator that already knows exactly what good lip sync looks like.


The key word is “expert.” Most GAN-style training co-evolves the generator and discriminator together. You start with a bad generator and a bad discriminator, and they push each other to improve. It’s slow. It’s unstable. And for something as specific as lip sync, the discriminator never really specializes. It spends half its capacity learning what faces look like at the same time. A frozen, pre-trained lip sync expert sidesteps that entirely. The discriminator is already an expert in the one thing you care about. The generator just has to catch up.


The result was the first AI lip sync technology that worked on arbitrary in-the-wild video, not just the curated speakers it was trained on. That paper is now the foundation most modern lip sync models build on. The specifics have moved a lot since. Newer architectures use diffusion instead of GANs. Attention mechanisms instead of pure convolutions. Bigger backbones. But the SyncNet-as-discriminator pattern is still everywhere.


One thing worth flagging. The standard metric for evaluating lip sync, LSE-D and LSE-C, is computed using the same SyncNet model that most modern systems are trained against. That’s a separate story, and not a flattering one for the field. More on that in another post.


## Why zero-shot is harder than it looks


A model that can lip sync any person on any video, without ever having seen them before, is what the field calls zero-shot. It sounds like an incremental improvement on the regular case. It is not.


The hard part of zero-shot is identity preservation. If you train a model on millions of faces and ask it to lip sync someone new, the natural tendency is for the output to drift toward the “average” face in the training data. The mouth ends up looking generic. Teeth change shape. Lips lose their distinctive curve. The skin around the mouth subtly stops matching the skin everywhere else. It’s not catastrophic. You’d miss it on a single frame. But watch the video and something feels off.


Solving this takes two things. First, a strong identity branch baked into the architecture, usually a separate face recognition network like ArcFace, with a loss term that forces the generated mouth to score as the same identity as the source. Second, training data diverse enough that the model has actually seen mouths like the one you’re asking it to generate. Faces from underrepresented demographics still tend to come out worse, because the training distribution is what it is.


Talking head AI is the same problem in a harder form. Take a single photo or short clip of someone and produce a long video of that person speaking, with way less reference data and a lot more that the model has to invent. Modern zero-shot models, including sync. labs’, vozo’s, lipdub’s, and the open source efforts like[LatentSync](https://sync.so/blog/what-is-latentsync/) and[MuseTalk](https://sync.so/blog/what-is-musetalk/) , all handle this with varying degrees of success. The general arc over the last few years has been bigger backbones, better identity losses, and more diverse training data, and the gap between the best and worst output is mostly explained by how well a given team has executed against that arc.


## What still doesn’t work


It’s worth being honest about the failure modes, because they tell you where the field actually is.


Teeth, weirdly, are one of the hardest things to get right. A mouth that opens has to reveal teeth, and teeth turn out to be oddly person specific - gap, color, alignment, whether the canines stick out, all of it. Models that generate mouths well still routinely fail on the teeth inside them, and if you look closely you’ll see generated teeth that are slightly too clean, too uniform, vaguely stock photo. Extreme angles are a similar story. Profiles, three-quarter views, anything where the mouth isn’t roughly facing the camera. Most training data is frontal, so the model has seen fewer mouths from the side and it shows.


Occlusions get tricky too. A microphone in front of the lips, a hand drifting up to the chin, hair falling across the mouth. The model has to decide whether to preserve the thing in the way or quietly remove it, and most of the time it does neither cleanly. Emotion is its own category of pain. Yelling doesn’t look like talking, crying doesn’t look like talking, and laughing-while-talking is its own thing entirely. Most models are trained predominantly on calm conversational speech, so the moment you hand them anything operatic the output falls apart.


The hardest version of the problem is cross-lingual lip sync. Take footage shot in English and produce a version where the same person appears to be speaking Japanese. The model has to invent mouth movements that this specific person, who was actually speaking English when the camera rolled, would plausibly make if they were speaking Japanese instead. The mouth shapes are different, the rhythm is different, and there’s no ground truth to lean on because the moment in time you’re trying to generate never happened. When it works, this is the entire value proposition of AI dubbing. When it doesn’t, it’s just the uncanny valley with subtitles.


## Where it’s going


The next few years of lip sync, from where we sit, are about two things.


The first is moving past the mouth-only bottleneck. Most current models still treat lip sync as a local problem. Change the mouth, leave the rest alone. But humans don’t move just our mouths when we talk. We move our eyebrows, our cheeks, our whole head. The next generation generates full facial behavior from audio, not just the mouth region. Some of this is already shipping. More of it is in research preview.


The second is real time. Most modern AI lip sync models still need a few seconds of compute per second of video, and getting the same quality to run at 30fps with no perceptible latency is what unlocks the next set of use cases: live translation, video calls, gaming, anything where you can’t ask the user to wait. It’s a tractable problem, but it’s not a solved one.


If you take one thing away from all of this, it’s that lip sync is a deceptively hard problem dressed up as a simple one. The reason it works as well as it does today comes down to a small number of architectural decisions made in the last five years, most of which trace back to the same handful of papers and the same handful of researchers. The field has been moving fast. It’s also still wrong about a lot of things, and the people working on it are usually the first to say so.


If you want to feel where AI lip sync technology actually is, instead of where the demos suggest, the[sync. playground](https://sync.so/try) lets you run any of our models against your own video. Bring something hard. Long sentences, weird angles, emotional delivery, a language you don’t speak. You’ll learn more in ten minutes there than from any explainer, including this one.
