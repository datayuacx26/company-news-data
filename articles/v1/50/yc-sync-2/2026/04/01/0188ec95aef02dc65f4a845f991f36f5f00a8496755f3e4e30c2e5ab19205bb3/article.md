---
schema_version: "1.0.0"
document_id: "0188ec95aef02dc65f4a845f991f36f5f00a8496755f3e4e30c2e5ab19205bb3"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/sync-3-most-intelligent-lipsync-model/"
published_at: "2026-04-06T16:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:c622607488b7e5f7a66809055a12e83e571448e09fcee3dfd05a5c3078f1f3cf"
---

# sync-3: the most intelligent lipsync model

[sync-3](https://sync.so/sync-3) is our most advanced AI lip sync model, and the closest we’ve ever gotten to a lip sync your eye never catches. Every model we’ve shipped has chased the same thing. The kind of lip sync where you stop noticing the mouth and just watch the scene. That’s the whole job, and most of the work hides behind it. sync-3 gets there by changing what the model actually looks at.


For years, lip sync has been treated like a mouth problem. Find the mouth, read the audio, paint a plausible mouth back onto the face. That works on clean studio footage. It falls apart the moment a real scene shows up. A head turns. The light shifts. A second person walks into frame. The camera shakes for half a second. Suddenly the model is guessing, and guessing is where every artifact you’ve ever winced at comes from.


sync-3 doesn’t start with the mouth. It starts with the scene.


## sync-3 reads the scene, not just the mouth


Before sync-3 touches a single frame, it builds a wide spatial read of the whole shot. Where the face sits. How it’s lit. Which way it’s turned. Who else is in the frame, and which of those people is actually talking. That’s what drives the lip sync, not a square cropped around someone’s mouth. You’re syncing a scene the model understands instead of a mouth it’s been forced to invent around.


That shift is why sync-3 holds up where older models start to guess. Sharp angles and side profiles, where half the mouth is hidden. Low or uneven lighting, where the shape of the lips is ambiguous from one frame to the next. Multiple speakers in a single shot, where you have to know who’s talking before you touch a face. Close-ups, where there is nowhere on the screen to hide a soft edge, a missing tooth, or a frame that doesn’t quite belong.


And then there’s acting. The lip sync has to carry the original performance instead of flattening it into something generic. The little pauses, the half-smile, the breath before a line. sync-3 was built to keep all of that, and that’s the piece I’m most proud of because it’s the piece nobody asks about until it’s missing.


## sync-3 holds where older models break


The clean cases were never really the problem. The hard cases are the ones that show up in actual edits. Handheld shake. Partial shadow from a boom or a window. Heavy background depth. An extreme angle change in the middle of a sentence because the editor cut to a reaction. These are the moments where older models would fall apart, the kind of shot a colorist or editor flags back at us and says, “yeah, that one.” sync-3 is the version of the model where those shots stop being the exception.


sync-3 runs up to 4K at 60fps, so you don’t have to downscale a shot just to hide a seam. The output holds at the resolution real projects ship at. And because it preserves performance across languages, the same take survives translation. The timing, the delivery, the emotion stay intact while only the words change. That’s what makes a visually dubbed scene feel like the original performance instead of a cover of it.


## fewer retakes, fewer fixes


sync-3 cuts retakes and manual fixes across dubbing, dialogue editing, and animation. The best generation we can produce on whatever you hand us, not just the polished pieces. That holds for localizing a feature, fixing a line in post, animating a character that never stood in front of a camera, or shipping a campaign in twelve markets in a week.


We’ve always seen lip sync as the first surface, not the final one. Record once, edit forever. Once any take can be rewritten later without losing what made it good in the first place, the camera stops being a constraint. The performance becomes the asset, and the words can keep moving around it.


We at sync. labs are the team behind[wav2lip](https://sync.so/blog/what-is-wav2lip/) , the first zero-shot lip sync model, open-sourced years ago now. Everything we’ve shipped since has been chasing the same idea, and sync-3 is the version of it I’d hand to someone without caveats for the first time.


Read more about[sync-3](https://sync.so/sync-3) , try it in the[playground](https://sync.so/try) , or call it through the[API](https://docs.sync.so/introduction) . At sync. labs we believe every story deserves every audience, and sync-3 is how we get a little closer to that.
