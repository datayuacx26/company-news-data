---
schema_version: "1.0.0"
document_id: "f55ead573d75fc60438c7a32d7ba53b5376c64fe37de04b7a6aca79c7aefe503"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/image-to-video/"
published_at: "2026-06-09T16:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:e37d7fe83ed0c7fa779f9ad469cb2302008b696e476b4216de44ce128421589a"
---

# image-to-video on sync-3

[sync-3](https://sync.so/sync-3) can now[turn a single image into a talking video](https://sync.so/blog/how-to-turn-an-image-into-a-talking-video/) . Upload a still of a person, give it audio, and the model brings them to life, lip synced, expressive, talking. One frame in, a speaking person out.


Lip sync has always had a quiet prerequisite. You needed video of someone already talking. A clip, a take, a source where a face was moving and a mouth was open. The whole category was built on editing motion that already existed. You brought the performance, we re-synced it. That prerequisite is gone. The image is the take now. sync-3 builds the performance off of it, the face, the mouth, the head, every bit of the motion that makes a still feel alive.


## Image-to-video turns one frame into a performance


The obvious question is how a photo becomes a performance. The answer is the same thing that makes sync-3 what it is.


Older lip sync worked in small independent chunks. It looked at a narrow window around the mouth, edited the frames it was handed, and stitched the pieces back together. That approach needs existing motion because it has nothing else to reason from. Take away the video and there’s nothing to edit.


sync-3 doesn’t work that way. It builds a global understanding of a person across an entire shot and generates every frame at once. It reasons about the whole face, what to move and what to hold, instead of patching a region frame by frame. Once a model understands a face that completely, the starting point stops mattering. A full video and a single image are just different amounts of the same information. sync-3 can take the smallest possible amount, one frame, and construct the rest.


So this isn’t a separate tool bolted onto the side of the product. It’s the same model, asked to start from less. Image input runs on sync-3, nothing else, because nothing else understands a face well enough to build one a frame at a time.


## Any still image of a person can now talk


The unlock is that your input no longer has to exist as video. Anything that can be a still of a person can now speak.


A generated character from Midjourney or any image model. A headshot. A painting, an illustration, a single frame pulled from something else. You bring the image and the words, sync-3 handles the lip sync and the motion around it. The output length matches your audio, so a thirty second script gives you thirty seconds of video. No trimming, no padding.


The audio is yours to decide. Clone a voice, generate one, or upload a track you already have. The model syncs to whatever you give it, across 95+ languages, the same coverage as the rest of sync-3. A static portrait can deliver a line in English, then the same line in Hindi, then Japanese, off one image.


For anyone making content from generated stills, this collapses a whole step. You used to need an image model, then a separate motion or video model, then a lip sync pass. Now the image is the only thing you have to make first.


## How to use image-to-video on sync-3


It’s already live for everyone. Open the[studio](https://sync.so/try) , upload an image where you’d normally upload a video, add your audio, generate. That’s the whole flow. Nothing new to learn, the same place you already work.


For best results the face should be clear and reasonably front-facing, the kind of image you’d be happy to see talk. One image, one audio track, and you’re set. If there’s more than one face in the frame, tap the one you want to speak. sync-3 does the rest.


## Start from less, get more


That’s the thread running through every model we’ve shipped, and image-to-video is the clearest version of it yet. Lip sync started as a way to fix footage you already had. It’s becoming a way to make footage you never shot.


The still image is the floor, not the ceiling. The same intelligence that turns one frame into a performance is the thing we keep building on. For now, the practical version is simple. You don’t need a video anymore. You need an image and something to say.


Try it on your next still in the[playground](https://sync.so/try) , or call it through the[API](https://docs.sync.so/introduction) . At sync. labs we believe every story deserves every audience, and image-to-video is one more step toward that.
