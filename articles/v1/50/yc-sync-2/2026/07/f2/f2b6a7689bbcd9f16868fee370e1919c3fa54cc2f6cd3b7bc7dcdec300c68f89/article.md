---
schema_version: "1.0.0"
document_id: "f2b6a7689bbcd9f16868fee370e1919c3fa54cc2f6cd3b7bc7dcdec300c68f89"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/the-film-grain-problem/"
published_at: "2026-07-07T16:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:dcc118b1bcf8ec5c2e248869e919f8e33b4397ade25ab2b63af12747448a1335"
---

# The film grain problem nobody has solved

Over the past 10 months, there’s been a problem bugging me, like an itch I can’t scratch. Why is this simple aesthetic look such a challenge to work with?


## Film grain and digital noise are not the same thing


Film grain and digital noise look almost identical, but they come from opposite places. Every camera you record with picks up a certain amount of something called “noise”: a random optical texture pattern of pixels that becomes visible especially in low light. Noise is bad, and filmmakers work to avoid it by keeping scenes well-lit. Grain is different. When you shoot on actual film rather than a digital camera, a similar optical texture gets captured, this time caused by the chemical processing of the film.


When done right, grain actually enhances cinematic media, making it look authentic, like it was shot on film even when it wasn’t. Filmmakers chase this look, sometimes going as far as recreating synthetic grain in post-production for the sake of the “film aesthetic”.


Which is great. But for a VFX artist, grain and noise are both a nightmare: any kind of VFX work becomes nearly impossible when you’re working directly off a shot covered in these random texture patterns.


So the industry works in three steps: remove the grain, do the work, bring the grain back. Sounds simple. In reality both ends of that process are extremely tedious, and each one is ruled by a king that nobody has come close to beating.


But why does this even matter to me?


I work at[sync. labs](https://sync.so/) , where we do masked video generation: taking a video and fully preserving every aspect of it except the small portion you’re looking to modify, whether that’s emotions, appearances, performances, or the lips themselves.


We’ve been exploring these research problems for the past seven years and commercialized our[lip sync pipeline](https://sync.so/blog/how-ai-lip-sync-actually-works/) in 2023.[sync-3](https://sync.so/sync-3) was a breakthrough in preserving a speaker’s unique style while reasoning about the environment they’re in (lighting scenarios, occlusions), which means we already handle grain quite well out of the box.


But as we’ve worked with more studios and taken on more client projects, we’ve seen that removing any kind of noise or grain before lip sync only results in higher quality outputs, no matter the scene or the shot.


## The two kings of the space


Two pieces of software rule grain work: Neat Video for removing it, and DasGrain for putting it back. Neither has real competition, and both are painful in their own way.


Removing noise or grain is generally called denoising. Many people assume that grain and noise are the same, which leads to the first problem: most approaches focus on noise reduction, not grain reduction. Classic denoise algorithms that ship with every editor (including ffmpeg’s) aren’t an option, they lose so much detail that faces and objects come out looking like plastic.


What you actually need is intentional denoise tailored to film grain specifically, across all three channels (red, green, blue). The king of degraining is[Neat Video](https://www.neatvideo.com/) . It’s ancient-looking software where you pay per seat, per application, and worst of all, there’s no bulk processing. A proper degrain means a VFX artist going shot by shot, manually tweaking settings for every single one.


You’d think you could automate Neat. An API, maybe even the accessibility features. Nope. They’ve disabled every single way of automating anything within the app. They did recently add an auto profile setting, but apply it blindly to an entire scene or movie and it’s not going to work.


On the regraining side, the king is a Nuke gizmo named DasGrain, created by Fabian Holtz. Once VFX is done on a shot, you can’t just reapply the original grain, because a portion of the shot has been modified. You need to synthesize grain that matches the original plate as closely as possible while still looking natural on the new version.


DasGrain does this well, and thankfully is free and open-source, but the fundamental issue is that it’s a Nuke gizmo: the workflow only exists inside Nuke. This means every time a VFX artist finishes making changes to their shot, they need to regrain before sharing it, and repeat after every iteration.


So after months of experimentation with these two kings, I decided to tackle this problem once and for all with *onegrain* .


## Ten months of failed degrain experiments


For ten months, almost every automated approach I tried for degraining and regraining fell short. I first came across the concept 10 months ago and experimented with ffmpeg and a whole bunch of other command-line approaches. Safe to say, all of them were very disappointing. Since then, I’ve thrown the same problem at every new AI model launch, and none of them could build the ideas I had in mind.


Two months ago, I spent a long time with GPT-5.5 trying to train a model on a bunch of clips: synthesize fake noise, degrain, compare, then regrain back to that same fake original. That didn’t work either, and it’s an approach that would need to be done at massive scale.


I even built a hacky screen-parsing CLI tool that captured the screen into text coordinates and simulated clicks, literally editing in DaVinci Resolve for me, running Neat Video shot by shot at around 10 seconds each. It automated the whole pass, but OCR and click automation were never going to scale.


Finally, after discussing this with our team at sync. labs, especially our in-house VFX artists who have worked within the industry for over a decade, I decided to see if we could topple these two kings and build something that is foundationally adaptable to different workflows and bridges the gap between degrain and regrain.


## onegrain degrains and regrains in one pipeline


onegrain is a single pipeline that both removes film grain and restores it, built to run outside any one application. Here’s the thing about Neat Video though: it’s not generative AI. There’s no neural network. It’s an extremely specialized algorithm that’s extremely strong at handling all sorts of cases. It detects the noisiest frame within a shot, finds a patch of solid video within that frame with the most noise, and from that patch understands how much noise it can extract without making the video look bad. Then it processes the whole shot, extremely quickly.


Using this same method, plus some experimentation with the help of the latest LLMs including Claude Fable, we built an algorithm that not only meets the quality of Neat Video, but surpasses it, through intentional grain removal that preserves the look of the video.


For regraining, Fabian Holtz’s DasGrain was an incredible resource to draw inspiration from for the onegrain pipeline. This means regraining can now be done in bulk, without artists having to do it manually. And since it’s automated, it can take an iterative approach, running multiple render passes and continuously working until it matches the grain level of the original plate.


To be honest, it’s not perfect yet, but the results are quite promising:


onegrain runs in the simplest of forms directly from the command line, is exposed as an API, and integrates as a native OFX plugin, meaning you can connect it to Nuke, DaVinci Resolve, Baselight, or any other application that supports OFX.


For years, Neat Video and DasGrain have been incredible for unlocking workflows for VFX artists specifically. But what if these workflows didn’t have to be only for VFX artists? What if the entire processing pipeline could work with the same tools VFX has been using all along?


I’m personally very excited to keep learning more in this field and iterating on onegrain, working directly with our VFX artists to ensure we build it with their workflows in mind. We’re just getting started, and I hope to share an update on the next version of onegrain soon.
